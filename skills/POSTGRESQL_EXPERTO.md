# 🐘 ARCHIVO DE CONOCIMIENTO EXPERTO — POSTGRESQL COMPLETO
> Versión: Máxima. Cubre PostgreSQL completo + integración con Node.js (pg) + diseño de bases de datos + optimización.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en PostgreSQL. Sabes diseñar esquemas, escribir queries complejos, optimizar con índices, manejar transacciones y conectar desde Node.js de forma segura y eficiente.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en PostgreSQL. Conoces:
- Diseño de esquemas relacionales y normalización
- SQL completo — SELECT, JOIN, subqueries, CTEs, window functions
- Tipos de datos de PostgreSQL (incluyendo los especiales: JSONB, UUID, arrays, etc.)
- Índices y optimización de queries
- Transacciones y control de concurrencia
- Funciones, triggers y procedimientos almacenados
- Conexión desde Node.js con la librería `pg`
- Pool de conexiones y buenas prácticas
- Migraciones de base de datos
- Backup y restauración
- Seguridad y roles
- Queries parametrizadas para prevenir SQL injection

---

## 1. INSTALACIÓN Y CONFIGURACIÓN

### Instalación en Ubuntu Server
```bash
# Instalar PostgreSQL
sudo apt update
sudo apt install -y postgresql postgresql-contrib

# Iniciar y habilitar el servicio
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo systemctl status postgresql

# Acceder como superusuario
sudo -u postgres psql

# Versión instalada
psql --version
SELECT version();
```

### Configuración inicial de seguridad
```sql
-- Cambiar password del superusuario postgres
ALTER USER postgres WITH PASSWORD 'password_muy_seguro';

-- Crear usuario de la aplicación (nunca usar postgres directamente)
CREATE USER mi_app_user WITH PASSWORD 'password_app_seguro';

-- Crear base de datos
CREATE DATABASE mi_base_datos
  WITH OWNER = mi_app_user
  ENCODING = 'UTF8'
  LC_COLLATE = 'es_ES.UTF-8'
  LC_CTYPE = 'es_ES.UTF-8'
  TEMPLATE = template0;

-- Dar permisos al usuario
GRANT ALL PRIVILEGES ON DATABASE mi_base_datos TO mi_app_user;
GRANT ALL ON SCHEMA public TO mi_app_user;

-- Conectar a la base de datos
\c mi_base_datos
```

### postgresql.conf — Configuración principal
```bash
# Ubicación: /etc/postgresql/15/main/postgresql.conf

# Conexiones
max_connections = 100
superuser_reserved_connections = 3

# Memoria (ajustar según RAM disponible)
shared_buffers = 256MB          # 25% de la RAM
effective_cache_size = 768MB    # 75% de la RAM
work_mem = 4MB                  # Por operación de sort/hash
maintenance_work_mem = 64MB     # Para VACUUM, CREATE INDEX

# WAL (Write-Ahead Logging)
wal_level = replica
max_wal_size = 1GB
min_wal_size = 80MB

# Query planner
random_page_cost = 1.1          # Para SSDs (4.0 para HDDs)
effective_io_concurrency = 200  # Para SSDs

# Logging
log_destination = 'stderr'
logging_collector = on
log_directory = 'log'
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
log_min_duration_statement = 1000  # Logear queries > 1 segundo
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
log_checkpoints = on
log_connections = on
log_disconnections = on
log_lock_waits = on

# Reiniciar PostgreSQL después de cambios
sudo systemctl restart postgresql
```

### pg_hba.conf — Control de acceso
```bash
# Ubicación: /etc/postgresql/15/main/pg_hba.conf

# TYPE  DATABASE    USER         ADDRESS         METHOD
local   all         postgres                     peer
local   all         all                          md5
host    all         all          127.0.0.1/32    scram-sha-256
host    all         all          ::1/128         scram-sha-256

# Para acceso remoto (ajustar IP)
host    mi_base_datos  mi_app_user  0.0.0.0/0   scram-sha-256

# Reiniciar después de cambios
sudo systemctl reload postgresql
```

---

## 2. CONEXIÓN DESDE NODE.JS — LIBRERÍA PG

### Instalación
```bash
npm install pg
npm install -D @types/pg  # Si usas TypeScript
```

### Pool de conexiones (configuración correcta)
```javascript
// src/config/database.js
import pg from 'pg';
import { config } from './env.js';

const { Pool } = pg;

// El pool maneja múltiples conexiones automáticamente
// NUNCA crear un cliente nuevo por cada request
export const pool = new Pool({
  host:     config.db.host,
  port:     config.db.port,
  database: config.db.name,
  user:     config.db.user,
  password: config.db.password,
  ssl: config.db.ssl ? {
    rejectUnauthorized: false, // true en producción con certificado válido
  } : false,

  // Pool settings
  min: config.db.pool.min,          // Conexiones mínimas activas (2)
  max: config.db.pool.max,          // Conexiones máximas (10)
  idleTimeoutMillis: 30000,         // Cerrar conexión inactiva después de 30s
  connectionTimeoutMillis: 5000,    // Timeout para obtener conexión del pool
  maxUses: 7500,                    // Recrear conexión después de 7500 usos

  // Statement timeout (seguridad — cancelar queries lentos)
  statement_timeout: 30000,         // 30 segundos máximo por query
  query_timeout: 30000,
  application_name: 'mi-api',       // Visible en pg_stat_activity
});

// Eventos del pool
pool.on('connect', (client) => {
  // Configurar cada nueva conexión
  client.query("SET timezone = 'America/Mexico_City'");
  console.log(`🔗 Nueva conexión al pool (total: ${pool.totalCount})`);
});

pool.on('error', (err, client) => {
  console.error('❌ Error inesperado en cliente del pool:', err);
});

pool.on('remove', () => {
  console.log(`🔌 Conexión removida del pool (total: ${pool.totalCount})`);
});

// Conectar y verificar
export async function conectarDB() {
  let cliente;
  try {
    cliente = await pool.connect();
    const { rows } = await cliente.query('SELECT NOW() as ahora, current_database() as db');
    console.log(`✅ PostgreSQL conectado - DB: ${rows[0].db} - Hora: ${rows[0].ahora}`);
  } catch (error) {
    console.error('❌ Error conectando a PostgreSQL:', error.message);
    throw error;
  } finally {
    if (cliente) cliente.release(); // SIEMPRE liberar el cliente
  }
}

// Cerrar pool al apagar el servidor
export async function cerrarDB() {
  await pool.end();
  console.log('✅ Pool de conexiones cerrado');
}

// ===== HELPERS DE QUERY =====

// Query básico — para SELECT, INSERT, UPDATE, DELETE
export async function query(texto, params = []) {
  const inicio = Date.now();
  try {
    const resultado = await pool.query(texto, params);
    const duracion = Date.now() - inicio;

    if (duracion > 1000) {
      console.warn(`⚠️ Query lento (${duracion}ms):`, texto.substring(0, 100));
    }

    return resultado;
  } catch (error) {
    console.error('❌ Error en query:', { texto: texto.substring(0, 200), params, error: error.message });
    throw error;
  }
}

// Query que retorna un solo registro
export async function queryUno(texto, params = []) {
  const { rows } = await query(texto, params);
  return rows[0] || null;
}

// Query que retorna múltiples registros
export async function queryVarios(texto, params = []) {
  const { rows } = await query(texto, params);
  return rows;
}

// Transacción — ejecuta múltiples queries atómicamente
export async function transaccion(callback) {
  const cliente = await pool.connect();
  try {
    await cliente.query('BEGIN');
    const resultado = await callback(cliente);
    await cliente.query('COMMIT');
    return resultado;
  } catch (error) {
    await cliente.query('ROLLBACK');
    throw error;
  } finally {
    cliente.release();
  }
}

// Uso de transacción:
// await transaccion(async (cliente) => {
//   await cliente.query('INSERT INTO ...', [...]);
//   await cliente.query('UPDATE ...', [...]);
//   return await cliente.query('SELECT ...', [...]);
// });
```

---

## 3. TIPOS DE DATOS DE POSTGRESQL

```sql
-- === NUMÉRICOS ===
SMALLINT          -- -32768 a 32767 (2 bytes)
INTEGER / INT     -- -2147483648 a 2147483647 (4 bytes)
BIGINT            -- ±9 quintillones (8 bytes)
SERIAL            -- INTEGER autoincremental (legacy)
BIGSERIAL         -- BIGINT autoincremental (legacy)
DECIMAL(p,s)      -- Exacto: p dígitos totales, s decimales
NUMERIC(p,s)      -- Idéntico a DECIMAL
REAL              -- Flotante 4 bytes (impreciso)
DOUBLE PRECISION  -- Flotante 8 bytes (impreciso)

-- === TEXTO ===
CHAR(n)           -- Longitud fija — rellena con espacios
VARCHAR(n)        -- Longitud variable con límite
TEXT              -- Longitud ilimitada — PREFERIDO en PostgreSQL
CITEXT            -- TEXT case-insensitive (extensión)

-- === FECHA Y TIEMPO ===
DATE              -- Solo fecha: 2024-01-15
TIME              -- Solo hora: 14:30:00
TIMETZ            -- Hora con timezone
TIMESTAMP         -- Fecha y hora sin timezone
TIMESTAMPTZ       -- Fecha y hora CON timezone — PREFERIDO
INTERVAL          -- Duración: '1 year 2 months 3 days'

-- === BOOLEANO ===
BOOLEAN           -- true / false / null

-- === UUID ===
UUID              -- 'a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11' — PREFERIDO para IDs

-- === BINARIOS ===
BYTEA             -- Datos binarios (imágenes, archivos)

-- === RED ===
INET              -- Dirección IP: '192.168.1.1'
CIDR              -- Red IP: '192.168.1.0/24'
MACADDR           -- Dirección MAC

-- === JSON ===
JSON              -- Texto JSON validado (sin indexación eficiente)
JSONB             -- JSON binario — PREFERIDO (indexable, más rápido)

-- === ARRAYS ===
INTEGER[]         -- Array de enteros
TEXT[]            -- Array de textos
-- Ejemplo: ARRAY['rojo', 'verde', 'azul'] o '{"rojo","verde","azul"}'

-- === ENUM ===
CREATE TYPE estado_pedido AS ENUM ('pendiente', 'procesando', 'enviado', 'entregado', 'cancelado');

-- === RANGOS ===
INT4RANGE         -- Rango de integers
DATERANGE         -- Rango de fechas
TSTZRANGE         -- Rango de timestamps con timezone

-- === GEOMÉTRICOS ===
POINT             -- Punto (x,y)
LINE              -- Línea
POLYGON           -- Polígono

-- === FULL TEXT SEARCH ===
TSVECTOR          -- Vector de búsqueda de texto
TSQUERY           -- Query de búsqueda de texto
```

---

## 4. DISEÑO DE ESQUEMAS — MEJORES PRÁCTICAS

### Esquema base profesional
```sql
-- Extensiones útiles
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";    -- Generar UUIDs
CREATE EXTENSION IF NOT EXISTS "pgcrypto";     -- Funciones criptográficas
CREATE EXTENSION IF NOT EXISTS "unaccent";     -- Quitar acentos para búsqueda
CREATE EXTENSION IF NOT EXISTS "pg_trgm";      -- Búsqueda por similitud (ILIKE eficiente)
CREATE EXTENSION IF NOT EXISTS "citext";       -- TEXT case-insensitive

-- === TABLA DE USUARIOS ===
CREATE TABLE usuarios (
  -- ID como UUID (no secuencial — más seguro, portable)
  id            UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

  -- Datos básicos
  nombre        VARCHAR(100) NOT NULL
                  CHECK (LENGTH(TRIM(nombre)) >= 2),
  email         CITEXT NOT NULL UNIQUE,  -- CITEXT = case-insensitive
  password_hash TEXT NOT NULL,

  -- Enum para roles
  rol           VARCHAR(20) NOT NULL DEFAULT 'usuario'
                  CHECK (rol IN ('admin', 'moderador', 'usuario')),

  -- Estado
  activo        BOOLEAN NOT NULL DEFAULT true,
  email_verificado BOOLEAN NOT NULL DEFAULT false,

  -- Metadatos de auditoría
  created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  deleted_at    TIMESTAMPTZ,                    -- Soft delete
  ultimo_login  TIMESTAMPTZ,

  -- Datos adicionales flexibles
  metadata      JSONB DEFAULT '{}'::jsonb
);

-- === TABLA DE PRODUCTOS ===
CREATE TABLE productos (
  id            UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  nombre        VARCHAR(200) NOT NULL,
  descripcion   TEXT,
  precio        DECIMAL(10, 2) NOT NULL CHECK (precio >= 0),
  stock         INTEGER NOT NULL DEFAULT 0 CHECK (stock >= 0),
  categoria_id  UUID REFERENCES categorias(id) ON DELETE SET NULL,
  imagenes      TEXT[],                   -- Array de URLs
  atributos     JSONB DEFAULT '{}'::jsonb, -- Datos flexibles
  activo        BOOLEAN NOT NULL DEFAULT true,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),

  -- Índice de búsqueda de texto completo
  search_vector TSVECTOR GENERATED ALWAYS AS (
    to_tsvector('spanish', COALESCE(nombre, '') || ' ' || COALESCE(descripcion, ''))
  ) STORED
);

-- === TABLA DE PEDIDOS ===
CREATE TABLE pedidos (
  id              UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  usuario_id      UUID NOT NULL REFERENCES usuarios(id) ON DELETE RESTRICT,
  estado          VARCHAR(20) NOT NULL DEFAULT 'pendiente'
                    CHECK (estado IN ('pendiente', 'procesando', 'enviado', 'entregado', 'cancelado')),
  total           DECIMAL(10, 2) NOT NULL CHECK (total >= 0),
  direccion       JSONB NOT NULL,
  notas           TEXT,
  created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- === TABLA DE ITEMS DE PEDIDO ===
CREATE TABLE pedido_items (
  id              UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  pedido_id       UUID NOT NULL REFERENCES pedidos(id) ON DELETE CASCADE,
  producto_id     UUID NOT NULL REFERENCES productos(id) ON DELETE RESTRICT,
  cantidad        INTEGER NOT NULL CHECK (cantidad > 0),
  precio_unitario DECIMAL(10, 2) NOT NULL CHECK (precio_unitario >= 0),
  subtotal        DECIMAL(10, 2) GENERATED ALWAYS AS (cantidad * precio_unitario) STORED
);

-- Restricción única para no duplicar producto en mismo pedido
ALTER TABLE pedido_items
  ADD CONSTRAINT uq_pedido_producto UNIQUE (pedido_id, producto_id);

-- === TABLA DE AUDITORÍA ===
CREATE TABLE auditoria (
  id          BIGSERIAL PRIMARY KEY,
  tabla       VARCHAR(100) NOT NULL,
  operacion   CHAR(1) NOT NULL CHECK (operacion IN ('I', 'U', 'D')),
  registro_id UUID,
  datos_antes JSONB,
  datos_despues JSONB,
  usuario_id  UUID REFERENCES usuarios(id),
  ip_address  INET,
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Particionamiento por mes para tablas grandes
CREATE TABLE auditoria (
  -- mismos campos --
) PARTITION BY RANGE (created_at);

CREATE TABLE auditoria_2024_01 PARTITION OF auditoria
  FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');
```

### Trigger para updated_at automático
```sql
-- Función que actualiza updated_at
CREATE OR REPLACE FUNCTION actualizar_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Aplicar a todas las tablas necesarias
CREATE TRIGGER tr_usuarios_updated_at
  BEFORE UPDATE ON usuarios
  FOR EACH ROW EXECUTE FUNCTION actualizar_updated_at();

CREATE TRIGGER tr_productos_updated_at
  BEFORE UPDATE ON productos
  FOR EACH ROW EXECUTE FUNCTION actualizar_updated_at();

CREATE TRIGGER tr_pedidos_updated_at
  BEFORE UPDATE ON pedidos
  FOR EACH ROW EXECUTE FUNCTION actualizar_updated_at();
```

### Trigger para auditoría automática
```sql
CREATE OR REPLACE FUNCTION registrar_auditoria()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO auditoria (tabla, operacion, registro_id, datos_antes, datos_despues)
  VALUES (
    TG_TABLE_NAME,
    CASE TG_OP
      WHEN 'INSERT' THEN 'I'
      WHEN 'UPDATE' THEN 'U'
      WHEN 'DELETE' THEN 'D'
    END,
    COALESCE(NEW.id, OLD.id),
    CASE WHEN TG_OP = 'DELETE' OR TG_OP = 'UPDATE' THEN row_to_json(OLD)::jsonb ELSE NULL END,
    CASE WHEN TG_OP = 'INSERT' OR TG_OP = 'UPDATE' THEN row_to_json(NEW)::jsonb ELSE NULL END
  );
  RETURN COALESCE(NEW, OLD);
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER tr_auditoria_usuarios
  AFTER INSERT OR UPDATE OR DELETE ON usuarios
  FOR EACH ROW EXECUTE FUNCTION registrar_auditoria();
```

---

## 5. ÍNDICES — OPTIMIZACIÓN COMPLETA

```sql
-- === TIPOS DE ÍNDICES ===

-- B-Tree (por defecto) — para comparaciones =, <, >, BETWEEN, LIKE 'texto%'
CREATE INDEX idx_usuarios_email ON usuarios(email);
CREATE INDEX idx_usuarios_created_at ON usuarios(created_at DESC);

-- Índice compuesto — para queries que filtran por múltiples columnas
CREATE INDEX idx_pedidos_usuario_estado ON pedidos(usuario_id, estado);
-- Regla: poner primero la columna con mayor cardinalidad (más valores únicos)

-- Índice parcial — para queries con condición frecuente (más pequeño y rápido)
CREATE INDEX idx_usuarios_activos ON usuarios(email) WHERE activo = true;
CREATE INDEX idx_pedidos_pendientes ON pedidos(created_at) WHERE estado = 'pendiente';

-- Índice único
CREATE UNIQUE INDEX idx_usuarios_email_unico ON usuarios(email);

-- GIN — para JSONB, Arrays, Full Text Search
CREATE INDEX idx_productos_search ON productos USING GIN(search_vector);
CREATE INDEX idx_productos_atributos ON productos USING GIN(atributos);
CREATE INDEX idx_usuarios_metadata ON usuarios USING GIN(metadata);

-- GIN para pg_trgm (búsqueda ILIKE eficiente)
CREATE INDEX idx_usuarios_nombre_trgm ON usuarios USING GIN(nombre gin_trgm_ops);
CREATE INDEX idx_productos_nombre_trgm ON productos USING GIN(nombre gin_trgm_ops);

-- GiST — para rangos, geométricos, JSONB con containment
CREATE INDEX idx_disponibilidad_rango ON disponibilidad USING GIST(periodo);

-- BRIN — para columnas con correlación natural con el orden físico (fecha de inserción)
CREATE INDEX idx_auditoria_created_at ON auditoria USING BRIN(created_at);

-- Hash — solo para igualdad exacta (=)
CREATE INDEX idx_sesiones_token ON sesiones USING HASH(token);

-- Índice de expresión
CREATE INDEX idx_usuarios_email_lower ON usuarios(LOWER(email));
CREATE INDEX idx_usuarios_nombre_sin_acento ON usuarios(unaccent(nombre));

-- === ANALIZAR ÍNDICES ===

-- Ver todos los índices de una tabla
SELECT indexname, indexdef FROM pg_indexes WHERE tablename = 'usuarios';

-- Ver uso de índices
SELECT
  schemaname,
  tablename,
  indexname,
  idx_scan AS veces_usado,
  idx_tup_read AS tuplas_leidas,
  idx_tup_fetch AS tuplas_obtenidas,
  pg_size_pretty(pg_relation_size(indexrelid)) AS tamaño
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;

-- Índices no usados (candidatos a eliminar)
SELECT schemaname, tablename, indexname, idx_scan
FROM pg_stat_user_indexes
WHERE idx_scan = 0 AND indexname NOT LIKE '%pkey%';

-- EXPLAIN ANALYZE — ver plan de ejecución de un query
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM usuarios WHERE email = 'usuario@ejemplo.com';

-- Tamaño de tablas e índices
SELECT
  tablename,
  pg_size_pretty(pg_total_relation_size(tablename::regclass)) AS total,
  pg_size_pretty(pg_relation_size(tablename::regclass)) AS tabla,
  pg_size_pretty(pg_total_relation_size(tablename::regclass) - pg_relation_size(tablename::regclass)) AS indices
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(tablename::regclass) DESC;
```

---

## 6. SQL AVANZADO — QUERIES COMPLETOS

### SELECT con todas las técnicas
```sql
-- === JOIN COMPLETO ===

-- INNER JOIN — solo registros que coinciden en ambas tablas
SELECT u.nombre, p.id AS pedido_id, p.total, p.estado
FROM usuarios u
INNER JOIN pedidos p ON p.usuario_id = u.id
WHERE p.estado != 'cancelado';

-- LEFT JOIN — todos los usuarios, aunque no tengan pedidos
SELECT u.nombre, COUNT(p.id) AS total_pedidos, SUM(p.total) AS monto_total
FROM usuarios u
LEFT JOIN pedidos p ON p.usuario_id = u.id AND p.estado = 'entregado'
WHERE u.activo = true
GROUP BY u.id, u.nombre
ORDER BY monto_total DESC NULLS LAST;

-- JOIN múltiple
SELECT
  u.nombre AS cliente,
  p.id AS pedido,
  prod.nombre AS producto,
  pi.cantidad,
  pi.subtotal
FROM pedidos p
JOIN usuarios u ON u.id = p.usuario_id
JOIN pedido_items pi ON pi.pedido_id = p.id
JOIN productos prod ON prod.id = pi.producto_id
WHERE p.created_at >= NOW() - INTERVAL '30 days'
ORDER BY p.created_at DESC;

-- SELF JOIN — tabla con sí misma (ej: empleado-supervisor)
SELECT
  e.nombre AS empleado,
  s.nombre AS supervisor
FROM empleados e
LEFT JOIN empleados s ON s.id = e.supervisor_id;

-- === CTEs (Common Table Expressions) ===
-- Más legibles que subqueries anidados

-- CTE básico
WITH pedidos_recientes AS (
  SELECT usuario_id, COUNT(*) AS total, SUM(total) AS monto
  FROM pedidos
  WHERE created_at >= NOW() - INTERVAL '90 days'
    AND estado = 'entregado'
  GROUP BY usuario_id
)
SELECT
  u.nombre,
  u.email,
  pr.total AS pedidos_trimestre,
  pr.monto AS monto_trimestre,
  CASE
    WHEN pr.monto > 1000 THEN 'VIP'
    WHEN pr.monto > 500  THEN 'Premium'
    ELSE 'Regular'
  END AS categoria_cliente
FROM usuarios u
JOIN pedidos_recientes pr ON pr.usuario_id = u.id
ORDER BY pr.monto DESC;

-- CTE recursivo — para jerarquías (categorías, empleados)
WITH RECURSIVE categorias_arbol AS (
  -- Caso base: categorías raíz
  SELECT id, nombre, padre_id, 0 AS nivel, nombre::TEXT AS ruta
  FROM categorias
  WHERE padre_id IS NULL

  UNION ALL

  -- Caso recursivo
  SELECT c.id, c.nombre, c.padre_id, ca.nivel + 1,
         ca.ruta || ' > ' || c.nombre
  FROM categorias c
  INNER JOIN categorias_arbol ca ON ca.id = c.padre_id
)
SELECT * FROM categorias_arbol ORDER BY ruta;

-- CTE múltiple
WITH
  ventas_mes AS (
    SELECT DATE_TRUNC('month', created_at) AS mes, SUM(total) AS monto
    FROM pedidos WHERE estado = 'entregado'
    GROUP BY 1
  ),
  promedio AS (
    SELECT AVG(monto) AS promedio_mensual FROM ventas_mes
  )
SELECT
  TO_CHAR(mes, 'Month YYYY') AS mes,
  monto,
  promedio_mensual,
  ROUND(((monto - promedio_mensual) / promedio_mensual) * 100, 2) AS variacion_pct
FROM ventas_mes, promedio
ORDER BY mes;

-- === WINDOW FUNCTIONS ===

-- ROW_NUMBER — numeración
SELECT
  nombre,
  total,
  ROW_NUMBER() OVER (ORDER BY total DESC) AS posicion
FROM pedidos;

-- RANK y DENSE_RANK — con empates
SELECT
  u.nombre,
  SUM(p.total) AS ventas,
  RANK() OVER (ORDER BY SUM(p.total) DESC) AS ranking,
  DENSE_RANK() OVER (ORDER BY SUM(p.total) DESC) AS ranking_denso
FROM usuarios u
JOIN pedidos p ON p.usuario_id = u.id
GROUP BY u.id, u.nombre;

-- LAG / LEAD — acceder a filas anteriores/siguientes
SELECT
  mes,
  ventas,
  LAG(ventas, 1) OVER (ORDER BY mes) AS ventas_mes_anterior,
  ventas - LAG(ventas, 1) OVER (ORDER BY mes) AS diferencia,
  LEAD(ventas, 1) OVER (ORDER BY mes) AS ventas_mes_siguiente
FROM ventas_mensuales;

-- SUM / AVG con PARTITION — acumulado por grupo
SELECT
  usuario_id,
  created_at::DATE AS fecha,
  total,
  SUM(total) OVER (PARTITION BY usuario_id ORDER BY created_at) AS total_acumulado,
  AVG(total) OVER (PARTITION BY usuario_id) AS promedio_usuario,
  COUNT(*) OVER (PARTITION BY usuario_id) AS total_pedidos
FROM pedidos
WHERE estado = 'entregado';

-- NTILE — dividir en grupos
SELECT
  nombre, ventas,
  NTILE(4) OVER (ORDER BY ventas DESC) AS cuartil
FROM resumen_ventas;

-- FIRST_VALUE / LAST_VALUE / NTH_VALUE
SELECT
  nombre,
  ventas,
  FIRST_VALUE(nombre) OVER (ORDER BY ventas DESC) AS mejor_vendedor,
  NTH_VALUE(nombre, 2) OVER (
    ORDER BY ventas DESC
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS segundo_mejor
FROM resumen_ventas;

-- === AGGREGATIONS AVANZADAS ===

-- FILTER — agregar con condición
SELECT
  COUNT(*) AS total_pedidos,
  COUNT(*) FILTER (WHERE estado = 'entregado') AS entregados,
  COUNT(*) FILTER (WHERE estado = 'cancelado') AS cancelados,
  COUNT(*) FILTER (WHERE estado = 'pendiente') AS pendientes,
  AVG(total) FILTER (WHERE estado = 'entregado') AS ticket_promedio
FROM pedidos
WHERE created_at >= DATE_TRUNC('month', NOW());

-- GROUPING SETS — múltiples agrupaciones en un query
SELECT
  COALESCE(categoria, 'TOTAL') AS categoria,
  COALESCE(estado, 'TODOS') AS estado,
  COUNT(*) AS cantidad,
  SUM(total) AS monto
FROM pedidos p
JOIN productos prod ON prod.id = ANY(
  SELECT producto_id FROM pedido_items WHERE pedido_id = p.id LIMIT 1
)
GROUP BY GROUPING SETS (
  (categoria, estado),
  (categoria),
  (estado),
  ()
);

-- ROLLUP — totales acumulados
SELECT año, mes, SUM(ventas) FROM resumen
GROUP BY ROLLUP (año, mes);

-- CUBE — todas las combinaciones
SELECT año, mes, region, SUM(ventas) FROM resumen
GROUP BY CUBE (año, mes, region);

-- STRING_AGG — concatenar valores
SELECT
  pedido_id,
  STRING_AGG(prod.nombre, ', ' ORDER BY prod.nombre) AS productos,
  STRING_AGG(pi.cantidad::TEXT, ', ') AS cantidades
FROM pedido_items pi
JOIN productos prod ON prod.id = pi.producto_id
GROUP BY pedido_id;

-- ARRAY_AGG — agrupar en array
SELECT
  usuario_id,
  ARRAY_AGG(DISTINCT estado ORDER BY estado) AS estados_pedidos,
  ARRAY_AGG(total ORDER BY created_at DESC) AS historico_montos
FROM pedidos
GROUP BY usuario_id;

-- JSON_AGG — agrupar en JSON
SELECT
  u.id,
  u.nombre,
  JSON_AGG(
    JSON_BUILD_OBJECT(
      'id', p.id,
      'total', p.total,
      'estado', p.estado,
      'fecha', p.created_at
    ) ORDER BY p.created_at DESC
  ) AS pedidos
FROM usuarios u
LEFT JOIN pedidos p ON p.usuario_id = u.id
GROUP BY u.id, u.nombre;
```

### JSONB — Queries avanzados
```sql
-- Estructura del JSONB en usuarios.metadata:
-- { "preferencias": { "idioma": "es", "notificaciones": true }, "tags": ["vip", "recurrente"] }

-- Acceder a valores
SELECT metadata -> 'preferencias' AS preferencias FROM usuarios;
SELECT metadata ->> 'preferencias' AS preferencias_texto FROM usuarios;  -- Como texto
SELECT metadata -> 'preferencias' ->> 'idioma' AS idioma FROM usuarios;
SELECT metadata #>> '{preferencias,idioma}' AS idioma FROM usuarios;     -- Path

-- Filtrar por valor JSONB
SELECT * FROM usuarios WHERE metadata @> '{"preferencias": {"idioma": "es"}}';
SELECT * FROM usuarios WHERE metadata -> 'preferencias' ->> 'idioma' = 'es';
SELECT * FROM usuarios WHERE metadata ? 'tags';           -- Tiene la clave
SELECT * FROM usuarios WHERE metadata ?| ARRAY['vip', 'premium']; -- Tiene alguna
SELECT * FROM usuarios WHERE metadata ?& ARRAY['vip', 'recurrente']; -- Tiene todas

-- Verificar si array JSONB contiene valor
SELECT * FROM usuarios WHERE metadata -> 'tags' ? 'vip';
SELECT * FROM usuarios WHERE metadata @> '{"tags": ["vip"]}';

-- Actualizar campos JSONB específicos (sin reemplazar todo)
UPDATE usuarios
SET metadata = metadata || '{"ultimo_login": "2024-01-15"}'::jsonb
WHERE id = $1;

UPDATE usuarios
SET metadata = jsonb_set(metadata, '{preferencias,idioma}', '"en"')
WHERE id = $1;

-- Eliminar clave de JSONB
UPDATE usuarios
SET metadata = metadata - 'campo_obsoleto'
WHERE metadata ? 'campo_obsoleto';

-- jsonb_each — expandir JSONB a filas
SELECT key, value FROM usuarios, jsonb_each(metadata) WHERE id = $1;

-- jsonb_array_elements — expandir array JSONB
SELECT tag.valor FROM usuarios, jsonb_array_elements_text(metadata -> 'tags') AS tag(valor);
```

### Búsqueda de texto completo
```sql
-- Full Text Search
SELECT nombre, descripcion,
       ts_rank(search_vector, query) AS relevancia
FROM productos,
     to_tsquery('spanish', 'laptop & gaming') AS query
WHERE search_vector @@ query
ORDER BY relevancia DESC;

-- Búsqueda con múltiples palabras y operadores
-- &  = AND
-- |  = OR
-- !  = NOT
-- <-> = seguidas (phrase search)
SELECT * FROM productos
WHERE search_vector @@ to_tsquery('spanish', 'laptop | computadora & !usada');

-- Highlight de resultados
SELECT
  ts_headline(
    'spanish',
    descripcion,
    to_tsquery('spanish', 'laptop'),
    'StartSel=<mark>, StopSel=</mark>, MaxWords=30, MinWords=15'
  ) AS descripcion_resaltada
FROM productos
WHERE search_vector @@ to_tsquery('spanish', 'laptop');

-- Búsqueda por similitud (con pg_trgm — ILIKE eficiente)
SELECT nombre, similarity(nombre, 'lapto') AS similitud
FROM productos
WHERE similarity(nombre, 'lapto') > 0.3
ORDER BY similitud DESC;

-- Búsqueda combinada (texto completo + similitud)
SELECT nombre, precio,
       ts_rank(search_vector, query) AS rank_fts,
       similarity(nombre, $1) AS rank_trgm
FROM productos,
     to_tsquery('spanish', plainto_tsquery('spanish', $1)::TEXT) AS query
WHERE search_vector @@ query
   OR similarity(nombre, $1) > 0.3
ORDER BY (ts_rank(search_vector, query) + similarity(nombre, $1)) DESC;
```

---

## 7. TRANSACCIONES Y CONCURRENCIA

```javascript
// Transacción en Node.js — patrón completo
async function crearPedidoCompleto(usuarioId, items) {
  return transaccion(async (cliente) => {
    // 1. Verificar stock de todos los productos
    for (const item of items) {
      const { rows: [producto] } = await cliente.query(
        'SELECT id, nombre, stock FROM productos WHERE id = $1 FOR UPDATE',
        // FOR UPDATE — bloquea la fila para evitar race conditions
        [item.productoId]
      );

      if (!producto) throw new Error(`Producto ${item.productoId} no encontrado`);
      if (producto.stock < item.cantidad) {
        throw new Error(`Stock insuficiente para ${producto.nombre}: ${producto.stock} disponibles`);
      }
    }

    // 2. Crear el pedido
    const total = items.reduce((sum, item) => sum + item.precio * item.cantidad, 0);
    const { rows: [pedido] } = await cliente.query(
      `INSERT INTO pedidos (usuario_id, total, estado, direccion)
       VALUES ($1, $2, 'pendiente', $3)
       RETURNING *`,
      [usuarioId, total, JSON.stringify(items[0].direccion)]
    );

    // 3. Crear items y actualizar stock
    for (const item of items) {
      await cliente.query(
        `INSERT INTO pedido_items (pedido_id, producto_id, cantidad, precio_unitario)
         VALUES ($1, $2, $3, $4)`,
        [pedido.id, item.productoId, item.cantidad, item.precio]
      );

      await cliente.query(
        `UPDATE productos SET stock = stock - $1 WHERE id = $2`,
        [item.cantidad, item.productoId]
      );
    }

    // 4. Registrar en auditoría
    await cliente.query(
      `INSERT INTO auditoria (tabla, operacion, registro_id, datos_despues)
       VALUES ('pedidos', 'I', $1, $2)`,
      [pedido.id, JSON.stringify(pedido)]
    );

    return pedido;
  });
  // Si CUALQUIER paso falla → ROLLBACK automático
}
```

```sql
-- Niveles de aislamiento de transacciones
BEGIN ISOLATION LEVEL READ COMMITTED;    -- Por defecto — lee datos confirmados
BEGIN ISOLATION LEVEL REPEATABLE READ;   -- No ve cambios de otras transacciones durante la TX
BEGIN ISOLATION LEVEL SERIALIZABLE;      -- Más estricto — como si fueran secuenciales

-- Savepoints — puntos de retorno parcial
BEGIN;
  INSERT INTO tabla1 VALUES (1);
  SAVEPOINT mi_savepoint;
  INSERT INTO tabla2 VALUES (2); -- Esto falla
  ROLLBACK TO SAVEPOINT mi_savepoint; -- Deshace solo hasta el savepoint
  INSERT INTO tabla2 VALUES (3); -- Intentar de nuevo
COMMIT;

-- Bloqueos explícitos
SELECT * FROM productos WHERE id = $1 FOR UPDATE;           -- Bloqueo exclusivo
SELECT * FROM productos WHERE id = $1 FOR SHARE;            -- Bloqueo compartido
SELECT * FROM productos WHERE id = $1 FOR UPDATE NOWAIT;    -- Falla si no puede bloquear
SELECT * FROM productos WHERE id = $1 FOR UPDATE SKIP LOCKED; -- Salta filas bloqueadas
```

---

## 8. QUERIES DESDE NODE.JS — PATRONES SEGUROS

```javascript
// === QUERIES PARAMETRIZADOS (OBLIGATORIO — previene SQL injection) ===

// ✅ CORRECTO — siempre usar $1, $2... para valores dinámicos
const { rows } = await pool.query(
  'SELECT * FROM usuarios WHERE email = $1 AND activo = $2',
  ['usuario@ejemplo.com', true]
);

// ❌ NUNCA concatenar strings — SQL injection!
// const query = `SELECT * FROM usuarios WHERE email = '${email}'`; // PELIGROSO

// === CONSULTAS DINÁMICAS SEGURAS ===
// Cuando los NOMBRES de columnas son dinámicos (no los valores)

function construirQueryActualizacion(tabla, campos, id) {
  // Whitelist de tablas y columnas permitidas
  const tablasPermitidas = ['usuarios', 'productos', 'pedidos'];
  const columnasPermitidas = {
    usuarios:  ['nombre', 'email', 'rol', 'activo'],
    productos: ['nombre', 'descripcion', 'precio', 'stock'],
    pedidos:   ['estado', 'notas'],
  };

  if (!tablasPermitidas.includes(tabla)) {
    throw new Error(`Tabla no permitida: ${tabla}`);
  }

  const columnasFiltradas = Object.keys(campos).filter(campo =>
    columnasPermitidas[tabla]?.includes(campo)
  );

  if (columnasFiltradas.length === 0) {
    throw new Error('No hay campos válidos para actualizar');
  }

  // Construir SET de forma segura
  const sets = columnasFiltradas.map((campo, i) => `${campo} = $${i + 1}`).join(', ');
  const valores = [...columnasFiltradas.map(c => campos[c]), id];

  return {
    texto: `UPDATE ${tabla} SET ${sets}, updated_at = NOW() WHERE id = $${columnasFiltradas.length + 1} RETURNING *`,
    valores,
  };
}

// === PAGINACIÓN ===
async function obtenerPaginado({ tabla, pagina = 1, limite = 20, ordenPor = 'created_at', orden = 'DESC', filtros = {} }) {
  const offset = (pagina - 1) * limite;

  // Construir WHERE dinámico seguro
  const condiciones = [];
  const valores = [];
  let contador = 1;

  Object.entries(filtros).forEach(([campo, valor]) => {
    if (valor !== null && valor !== undefined && valor !== '') {
      condiciones.push(`${campo} = $${contador}`);
      valores.push(valor);
      contador++;
    }
  });

  const where = condiciones.length > 0 ? `WHERE ${condiciones.join(' AND ')}` : '';

  const [{ rows: datos }, { rows: [{ total }] }] = await Promise.all([
    pool.query(
      `SELECT * FROM ${tabla} ${where} ORDER BY ${ordenPor} ${orden} LIMIT $${contador} OFFSET $${contador + 1}`,
      [...valores, limite, offset]
    ),
    pool.query(`SELECT COUNT(*) AS total FROM ${tabla} ${where}`, valores),
  ]);

  return {
    datos,
    paginacion: {
      pagina,
      limite,
      total: parseInt(total),
      totalPaginas: Math.ceil(parseInt(total) / limite),
    },
  };
}

// === UPSERT (INSERT o UPDATE) ===
async function upsertUsuario(datos) {
  return queryUno(
    `INSERT INTO usuarios (id, email, nombre, rol)
     VALUES ($1, $2, $3, $4)
     ON CONFLICT (email) DO UPDATE SET
       nombre = EXCLUDED.nombre,
       updated_at = NOW()
     RETURNING *`,
    [datos.id, datos.email, datos.nombre, datos.rol || 'usuario']
  );
}

// === BULK INSERT (inserción masiva eficiente) ===
async function insertarMasivo(registros) {
  if (registros.length === 0) return [];

  // Construir placeholders: ($1,$2,$3), ($4,$5,$6), ...
  const valores = [];
  const placeholders = registros.map((registro, i) => {
    const base = i * 3;
    valores.push(registro.nombre, registro.email, registro.rol || 'usuario');
    return `($${base + 1}, $${base + 2}, $${base + 3})`;
  });

  const { rows } = await pool.query(
    `INSERT INTO usuarios (nombre, email, rol)
     VALUES ${placeholders.join(', ')}
     ON CONFLICT (email) DO NOTHING
     RETURNING id, nombre, email`,
    valores
  );

  return rows;
}

// Con COPY para millones de registros (más rápido)
import { from as copyFrom } from 'pg-copy-streams';

async function insertarConCopy(stream) {
  const cliente = await pool.connect();
  try {
    const copyStream = cliente.query(
      copyFrom('COPY usuarios (nombre, email) FROM STDIN WITH CSV HEADER')
    );
    await pipeline(stream, copyStream);
  } finally {
    cliente.release();
  }
}
```

---

## 9. MIGRACIONES

```javascript
// Estructura de migración simple sin librería
// migrations/001_crear_tablas_base.sql

const migraciones = [
  {
    version: 1,
    nombre: 'crear_tablas_base',
    up: `
      CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

      CREATE TABLE usuarios (
        id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
        nombre VARCHAR(100) NOT NULL,
        email CITEXT NOT NULL UNIQUE,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
      );
    `,
    down: `DROP TABLE IF EXISTS usuarios;`,
  },
  {
    version: 2,
    nombre: 'agregar_campo_rol',
    up: `
      ALTER TABLE usuarios
      ADD COLUMN rol VARCHAR(20) NOT NULL DEFAULT 'usuario'
      CHECK (rol IN ('admin', 'moderador', 'usuario'));

      CREATE INDEX idx_usuarios_rol ON usuarios(rol);
    `,
    down: `ALTER TABLE usuarios DROP COLUMN rol;`,
  },
];

// src/config/migrate.js
export async function ejecutarMigraciones() {
  // Crear tabla de control si no existe
  await pool.query(`
    CREATE TABLE IF NOT EXISTS migraciones (
      version     INTEGER PRIMARY KEY,
      nombre      VARCHAR(200) NOT NULL,
      ejecutada_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    )
  `);

  // Obtener versión actual
  const { rows } = await pool.query(
    'SELECT MAX(version) AS version FROM migraciones'
  );
  const versionActual = rows[0].version || 0;

  // Ejecutar migraciones pendientes
  const pendientes = migraciones.filter(m => m.version > versionActual);

  for (const migracion of pendientes) {
    console.log(`⏳ Ejecutando migración ${migracion.version}: ${migracion.nombre}`);

    await transaccion(async (cliente) => {
      await cliente.query(migracion.up);
      await cliente.query(
        'INSERT INTO migraciones (version, nombre) VALUES ($1, $2)',
        [migracion.version, migracion.nombre]
      );
    });

    console.log(`✅ Migración ${migracion.version} completada`);
  }

  if (pendientes.length === 0) {
    console.log('✅ Base de datos actualizada - no hay migraciones pendientes');
  }
}
```

---

## 10. MANTENIMIENTO Y PERFORMANCE

```sql
-- === VACUUM Y ANALYZE ===
-- PostgreSQL hace esto automáticamente (autovacuum), pero a veces es necesario manualmente

-- VACUUM — recuperar espacio de filas eliminadas
VACUUM usuarios;
VACUUM VERBOSE usuarios;   -- Con detalles
VACUUM FULL usuarios;      -- Reescribe la tabla completa (bloquea la tabla)

-- ANALYZE — actualizar estadísticas para el query planner
ANALYZE usuarios;
ANALYZE VERBOSE;            -- Todas las tablas

-- Ambos juntos
VACUUM ANALYZE usuarios;

-- REINDEX — reconstruir índices corruptos o fragmentados
REINDEX TABLE usuarios;
REINDEX INDEX idx_usuarios_email;
REINDEX DATABASE mi_base_datos; -- Toda la DB (cuidado en producción)

-- Concurrente (sin bloquear)
REINDEX TABLE CONCURRENTLY usuarios;

-- === MONITORING ===

-- Queries activos en este momento
SELECT pid, now() - pg_stat_activity.query_start AS duracion,
       query, state, wait_event_type, wait_event
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 seconds'
  AND state != 'idle'
ORDER BY duracion DESC;

-- Cancelar un query (pid de pg_stat_activity)
SELECT pg_cancel_backend(pid);     -- Cancela el query
SELECT pg_terminate_backend(pid);  -- Termina la conexión

-- Locks activos
SELECT
  pg_locks.pid,
  pg_stat_activity.query,
  pg_locks.mode,
  pg_locks.granted
FROM pg_locks
JOIN pg_stat_activity ON pg_locks.pid = pg_stat_activity.pid
WHERE NOT pg_locks.granted;

-- Tamaño de la base de datos
SELECT
  datname AS base_datos,
  pg_size_pretty(pg_database_size(datname)) AS tamaño
FROM pg_database
ORDER BY pg_database_size(datname) DESC;

-- Tablas con más registros
SELECT
  tablename,
  n_live_tup AS registros_vivos,
  n_dead_tup AS registros_muertos,
  ROUND(n_dead_tup::NUMERIC / NULLIF(n_live_tup + n_dead_tup, 0) * 100, 2) AS pct_muertos,
  last_vacuum,
  last_autovacuum
FROM pg_stat_user_tables
ORDER BY n_live_tup DESC;

-- Cache hit ratio (debe ser > 99%)
SELECT
  SUM(heap_blks_hit) / NULLIF(SUM(heap_blks_hit) + SUM(heap_blks_read), 0) * 100 AS cache_hit_ratio
FROM pg_statio_user_tables;
```

---

## 11. BACKUP Y RESTAURACIÓN

```bash
# === BACKUP ===

# Backup completo en formato personalizado (comprimido, recomendado)
pg_dump -U mi_app_user -h localhost -d mi_base_datos \
  -Fc -f backup_$(date +%Y%m%d_%H%M%S).dump

# Backup en SQL plano (legible)
pg_dump -U mi_app_user -h localhost -d mi_base_datos \
  -Fp -f backup_$(date +%Y%m%d_%H%M%S).sql

# Solo el esquema (sin datos)
pg_dump -U mi_app_user -d mi_base_datos --schema-only -f schema.sql

# Solo los datos (sin esquema)
pg_dump -U mi_app_user -d mi_base_datos --data-only -f datos.sql

# Tabla específica
pg_dump -U mi_app_user -d mi_base_datos -t usuarios -f usuarios.dump

# Backup completo del servidor
pg_dumpall -U postgres -f backup_completo.sql

# Script de backup automático
#!/bin/bash
BACKUP_DIR="/backups/postgres"
FECHA=$(date +%Y%m%d_%H%M%S)
DB="mi_base_datos"
mkdir -p $BACKUP_DIR
pg_dump -U mi_app_user -Fc $DB > "$BACKUP_DIR/${DB}_${FECHA}.dump"
# Eliminar backups de más de 7 días
find $BACKUP_DIR -name "*.dump" -mtime +7 -delete
echo "Backup completado: ${DB}_${FECHA}.dump"

# Agregar al crontab (backup diario a las 2am)
# 0 2 * * * /scripts/backup_postgres.sh

# === RESTAURACIÓN ===

# Restaurar desde formato personalizado
pg_restore -U postgres -h localhost -d mi_base_datos_nueva \
  -Fc backup.dump

# Restaurar desde SQL plano
psql -U postgres -h localhost -d mi_base_datos_nueva < backup.sql

# Restaurar solo una tabla
pg_restore -U postgres -d mi_base_datos -t usuarios backup.dump

# Restaurar con progreso visible
pg_restore -U postgres -d mi_base_datos -v backup.dump
```

---

## 12. ROLES Y SEGURIDAD

```sql
-- === GESTIÓN DE ROLES ===

-- Crear roles
CREATE ROLE solo_lectura;
CREATE ROLE lectura_escritura;
CREATE ROLE administrador;

-- Permisos por rol
-- Solo lectura
GRANT CONNECT ON DATABASE mi_base_datos TO solo_lectura;
GRANT USAGE ON SCHEMA public TO solo_lectura;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO solo_lectura;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO solo_lectura;

-- Lectura/escritura
GRANT CONNECT ON DATABASE mi_base_datos TO lectura_escritura;
GRANT USAGE ON SCHEMA public TO lectura_escritura;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO lectura_escritura;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO lectura_escritura;
ALTER DEFAULT PRIVILEGES IN SCHEMA public
  GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO lectura_escritura;
ALTER DEFAULT PRIVILEGES IN SCHEMA public
  GRANT USAGE, SELECT ON SEQUENCES TO lectura_escritura;

-- Crear usuarios y asignar roles
CREATE USER app_readonly WITH PASSWORD 'password_readonly';
GRANT solo_lectura TO app_readonly;

CREATE USER app_user WITH PASSWORD 'password_app';
GRANT lectura_escritura TO app_user;

-- Revocar permisos
REVOKE INSERT, UPDATE, DELETE ON usuarios FROM app_readonly;

-- Row Level Security (RLS) — usuarios solo ven sus propios datos
ALTER TABLE pedidos ENABLE ROW LEVEL SECURITY;

CREATE POLICY pedidos_usuario_propio ON pedidos
  FOR ALL
  TO app_user
  USING (usuario_id = current_setting('app.usuario_id')::UUID);

-- Activar RLS en la sesión desde Node.js
await cliente.query("SET app.usuario_id = $1", [usuarioId]);
```

---

## 13. CHECKLIST DE BASE DE DATOS EN PRODUCCIÓN

```
DISEÑO
□ UUIDs como primary keys (no secuenciales)
□ TIMESTAMPTZ para fechas (no TIMESTAMP)
□ CITEXT para emails y datos case-insensitive
□ Constraints CHECK en columnas críticas
□ Triggers de updated_at en todas las tablas
□ Soft delete con deleted_at (no DELETE físico en datos importantes)
□ Tabla de auditoría para cambios críticos
□ JSONB para datos flexibles (no columnas para cada cosa)

ÍNDICES
□ Índice en todas las foreign keys
□ Índice en columnas de búsqueda frecuente (email, nombre)
□ Índice GIN para columnas JSONB consultadas frecuentemente
□ Índice GIN para Full Text Search
□ Índice parcial para filtros frecuentes (activo = true)
□ Revisar pg_stat_user_indexes para índices no usados

SEGURIDAD
□ Nunca usar el usuario postgres en la aplicación
□ Usuario de app con mínimos privilegios necesarios
□ Queries siempre parametrizados ($1, $2...)
□ Whitelist de nombres de tablas/columnas dinámicos
□ pg_hba.conf configurado correctamente
□ SSL en conexiones de producción
□ Passwords complejas para todos los usuarios

PERFORMANCE
□ Pool de conexiones configurado (no new Client() por request)
□ VACUUM ANALYZE programado
□ max_connections ajustado según RAM
□ shared_buffers = 25% de la RAM
□ effective_cache_size = 75% de la RAM
□ EXPLAIN ANALYZE en queries lentos
□ Cache hit ratio > 99%

BACKUPS
□ Backup automático diario con pg_dump
□ Backup probado (restaurar y verificar)
□ Retención de al menos 7 días
□ Backup almacenado en ubicación diferente al servidor
□ Point-in-time recovery configurado (WAL archiving)

MONITOREO
□ log_min_duration_statement = 1000 (logear queries lentos)
□ Alertas en pg_stat_activity para queries colgados
□ Monitoreo de tamaño de tablas
□ Dead tuples ratio < 10%
```

---

*Fin del archivo POSTGRESQL_EXPERTO.md — Conocimiento completo de PostgreSQL, diseño de esquemas, SQL avanzado, integración con Node.js, optimización, seguridad y operaciones de producción.*
