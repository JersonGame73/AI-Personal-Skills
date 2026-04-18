# 🟢 ARCHIVO DE CONOCIMIENTO EXPERTO — NODE.JS + EXPRESS COMPLETO
> Versión: Máxima. Cubre Node.js runtime completo + Express framework + mejores prácticas de producción.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en Node.js y Express. Sabes construir APIs REST robustas, seguras y escalables. Conoces el runtime de Node, el sistema de módulos, streams, eventos, y todos los patrones de Express para producción.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en Node.js y Express. Conoces:
- Node.js runtime completo (event loop, streams, buffers, módulos)
- Express.js — routing, middlewares, error handling
- APIs REST — diseño, versioning, documentación
- Seguridad — helmet, rate limiting, validación, sanitización
- Variables de entorno y configuración
- Autenticación — API Keys, JWT, OAuth
- Logging y monitoreo en producción
- Performance y optimización
- Integración con bases de datos (pg, mongoose, prisma)
- WebSockets con Socket.io
- Testing con Jest y Supertest
- Despliegue en producción

---

## 1. INSTALACIÓN Y ESTRUCTURA DE PROYECTO

### Inicializar proyecto
```bash
# Crear proyecto
mkdir mi-api && cd mi-api
npm init -y

# Dependencias principales
npm install express cors helmet morgan dotenv express-rate-limit
npm install express-validator bcryptjs jsonwebtoken uuid

# Dependencias de desarrollo
npm install -D nodemon jest supertest @types/node

# package.json scripts
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "nodemon src/index.js",
    "test": "jest --watchAll",
    "test:ci": "jest --ci --coverage"
  }
}
```

### Estructura de proyecto profesional
```
mi-api/
├── src/
│   ├── index.js              # Punto de entrada
│   ├── app.js                # Configuración de Express
│   ├── config/
│   │   ├── env.js            # Variables de entorno validadas
│   │   ├── database.js       # Conexión a DB
│   │   └── cors.js           # Configuración CORS
│   ├── routes/
│   │   ├── index.js          # Router principal
│   │   ├── usuarios.routes.js
│   │   ├── productos.routes.js
│   │   └── auth.routes.js
│   ├── controllers/
│   │   ├── usuarios.controller.js
│   │   ├── productos.controller.js
│   │   └── auth.controller.js
│   ├── middlewares/
│   │   ├── auth.middleware.js      # Verificar API Key / JWT
│   │   ├── error.middleware.js     # Manejo global de errores
│   │   ├── validate.middleware.js  # Validación de inputs
│   │   └── logger.middleware.js    # Logging personalizado
│   ├── services/
│   │   ├── usuarios.service.js     # Lógica de negocio
│   │   └── email.service.js
│   ├── models/
│   │   └── usuario.model.js        # Modelos de datos
│   └── utils/
│       ├── response.js             # Helpers de respuesta
│       ├── errors.js               # Clases de error
│       └── helpers.js
├── tests/
│   ├── usuarios.test.js
│   └── auth.test.js
├── .env                      # Variables locales (NO subir a git)
├── .env.example              # Plantilla (SÍ subir a git)
├── .gitignore
└── package.json
```

---

## 2. CONFIGURACIÓN BASE — APP.JS

```javascript
// src/app.js
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import morgan from 'morgan';
import rateLimit from 'express-rate-limit';
import { corsOptions } from './config/cors.js';
import { errorHandler } from './middlewares/error.middleware.js';
import { notFoundHandler } from './middlewares/notFound.middleware.js';
import router from './routes/index.js';

const app = express();

// ===== SEGURIDAD =====
// Helmet — cabeceras de seguridad HTTP
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", 'data:', 'https:'],
    },
  },
  crossOriginEmbedderPolicy: false,
}));

// Rate limiting global
const limiterGlobal = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100,                  // máx 100 requests por ventana
  standardHeaders: true,     // Return rate limit info en headers
  legacyHeaders: false,
  message: {
    error: 'Demasiadas peticiones. Intenta en 15 minutos.',
    codigo: 'RATE_LIMIT_EXCEEDED',
  },
  handler: (req, res, next, options) => {
    res.status(429).json(options.message);
  },
});
app.use(limiterGlobal);

// CORS
app.use(cors(corsOptions));

// ===== PARSERS =====
app.use(express.json({ limit: '10mb' }));           // Body JSON
app.use(express.urlencoded({ extended: true, limit: '10mb' })); // Form data
app.use(express.raw({ type: 'application/octet-stream' }));     // Binarios

// ===== LOGGING =====
if (process.env.NODE_ENV === 'development') {
  app.use(morgan('dev')); // Colorido en desarrollo
} else {
  app.use(morgan('combined')); // Formato estándar en producción
}

// ===== RUTAS =====
app.use('/api/v1', router);

// Health check (sin autenticación)
app.get('/health', (req, res) => {
  res.json({
    status: 'ok',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    environment: process.env.NODE_ENV,
    version: process.env.npm_package_version,
  });
});

// ===== MANEJO DE ERRORES (siempre al final) =====
app.use(notFoundHandler);  // 404 — ruta no encontrada
app.use(errorHandler);     // 500 — errores generales

export default app;
```

```javascript
// src/index.js
import 'dotenv/config';
import app from './app.js';
import { conectarDB } from './config/database.js';
import { validarEnv } from './config/env.js';

// Validar variables de entorno al iniciar
validarEnv();

const PORT = process.env.PORT || 3000;
const HOST = process.env.HOST || '0.0.0.0';

async function iniciar() {
  try {
    // Conectar base de datos primero
    await conectarDB();
    console.log('✅ Base de datos conectada');

    // Luego levantar el servidor
    const servidor = app.listen(PORT, HOST, () => {
      console.log(`🚀 Servidor corriendo en http://${HOST}:${PORT}`);
      console.log(`📊 Entorno: ${process.env.NODE_ENV}`);
      console.log(`📖 Docs: http://${HOST}:${PORT}/api/v1/docs`);
    });

    // Graceful shutdown
    process.on('SIGTERM', () => gracefulShutdown(servidor));
    process.on('SIGINT',  () => gracefulShutdown(servidor));

  } catch (error) {
    console.error('❌ Error al iniciar el servidor:', error);
    process.exit(1);
  }
}

async function gracefulShutdown(servidor) {
  console.log('\n🛑 Apagando servidor...');
  servidor.close(async () => {
    console.log('✅ Servidor HTTP cerrado');
    // Cerrar conexiones de DB, etc.
    process.exit(0);
  });
  // Forzar cierre después de 10 segundos
  setTimeout(() => process.exit(1), 10000);
}

iniciar();
```

---

## 3. VARIABLES DE ENTORNO

### .env
```bash
# Servidor
NODE_ENV=development
PORT=3000
HOST=0.0.0.0

# Base de datos
DB_HOST=localhost
DB_PORT=5432
DB_NAME=mi_base_datos
DB_USER=postgres
DB_PASSWORD=mi_password_seguro
DB_SSL=false
DB_POOL_MIN=2
DB_POOL_MAX=10

# Seguridad
API_KEY=mi-api-key-secreta-muy-larga-123456
JWT_SECRET=mi-jwt-secret-muy-largo-y-seguro-456789
JWT_EXPIRES_IN=7d
BCRYPT_ROUNDS=12

# CORS
CORS_ORIGINS=http://localhost:3000,https://script.google.com

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX=100

# Logging
LOG_LEVEL=debug
```

### Validación de variables de entorno
```javascript
// src/config/env.js
const variablesRequeridas = [
  'NODE_ENV',
  'PORT',
  'DB_HOST',
  'DB_NAME',
  'DB_USER',
  'DB_PASSWORD',
  'API_KEY',
  'JWT_SECRET',
];

export function validarEnv() {
  const faltantes = variablesRequeridas.filter(v => !process.env[v]);

  if (faltantes.length > 0) {
    console.error('❌ Variables de entorno faltantes:', faltantes.join(', '));
    console.error('📋 Revisa tu archivo .env basándote en .env.example');
    process.exit(1);
  }

  console.log('✅ Variables de entorno validadas');
}

// Objeto de configuración tipado
export const config = {
  env:  process.env.NODE_ENV || 'development',
  port: parseInt(process.env.PORT) || 3000,
  host: process.env.HOST || '0.0.0.0',

  db: {
    host:     process.env.DB_HOST,
    port:     parseInt(process.env.DB_PORT) || 5432,
    name:     process.env.DB_NAME,
    user:     process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    ssl:      process.env.DB_SSL === 'true',
    pool: {
      min: parseInt(process.env.DB_POOL_MIN) || 2,
      max: parseInt(process.env.DB_POOL_MAX) || 10,
    },
  },

  seguridad: {
    apiKey:        process.env.API_KEY,
    jwtSecret:     process.env.JWT_SECRET,
    jwtExpiresIn:  process.env.JWT_EXPIRES_IN || '7d',
    bcryptRounds:  parseInt(process.env.BCRYPT_ROUNDS) || 12,
  },

  cors: {
    origins: (process.env.CORS_ORIGINS || '').split(',').map(o => o.trim()),
  },

  rateLimit: {
    windowMs: parseInt(process.env.RATE_LIMIT_WINDOW_MS) || 900000,
    max:      parseInt(process.env.RATE_LIMIT_MAX) || 100,
  },
};
```

---

## 4. ROUTING — SISTEMA COMPLETO

### Router principal
```javascript
// src/routes/index.js
import { Router } from 'express';
import usuariosRoutes from './usuarios.routes.js';
import productosRoutes from './productos.routes.js';
import authRoutes from './auth.routes.js';
import { verificarApiKey } from '../middlewares/auth.middleware.js';

const router = Router();

// Rutas públicas (sin autenticación)
router.use('/auth', authRoutes);

// Rutas protegidas (requieren API Key o JWT)
router.use('/usuarios',  verificarApiKey, usuariosRoutes);
router.use('/productos', verificarApiKey, productosRoutes);

// Info de la API
router.get('/', (req, res) => {
  res.json({
    nombre: 'Mi API',
    version: '1.0.0',
    endpoints: {
      auth:      '/api/v1/auth',
      usuarios:  '/api/v1/usuarios',
      productos: '/api/v1/productos',
    },
  });
});

export default router;
```

### Rutas con validación completa
```javascript
// src/routes/usuarios.routes.js
import { Router } from 'express';
import { body, param, query } from 'express-validator';
import { validar } from '../middlewares/validate.middleware.js';
import * as usuariosController from '../controllers/usuarios.controller.js';
import { verificarJWT } from '../middlewares/auth.middleware.js';
import rateLimit from 'express-rate-limit';

const router = Router();

// Rate limit específico para creación
const limiterCrear = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hora
  max: 10,
  message: { error: 'Límite de creación alcanzado. Intenta en 1 hora.' },
});

// Validaciones reutilizables
const validarId = [
  param('id')
    .isUUID().withMessage('El ID debe ser un UUID válido'),
];

const validarUsuario = [
  body('nombre')
    .trim()
    .notEmpty().withMessage('El nombre es requerido')
    .isLength({ min: 2, max: 100 }).withMessage('El nombre debe tener entre 2 y 100 caracteres')
    .matches(/^[a-zA-ZáéíóúÁÉÍÓÚñÑ\s]+$/).withMessage('El nombre solo puede contener letras'),

  body('email')
    .trim()
    .notEmpty().withMessage('El email es requerido')
    .isEmail().withMessage('El email no es válido')
    .normalizeEmail(),

  body('edad')
    .optional()
    .isInt({ min: 18, max: 120 }).withMessage('La edad debe ser entre 18 y 120'),

  body('rol')
    .optional()
    .isIn(['admin', 'usuario', 'moderador']).withMessage('Rol inválido'),
];

const validarQuery = [
  query('pagina')
    .optional()
    .isInt({ min: 1 }).withMessage('La página debe ser un número positivo')
    .toInt(),

  query('limite')
    .optional()
    .isInt({ min: 1, max: 100 }).withMessage('El límite debe ser entre 1 y 100')
    .toInt(),

  query('buscar')
    .optional()
    .trim()
    .isLength({ max: 100 }).withMessage('La búsqueda no puede exceder 100 caracteres')
    .escape(), // Sanitizar HTML
];

// ===== ENDPOINTS =====

// GET /api/v1/usuarios — Listar con paginación y búsqueda
router.get('/', validarQuery, validar, usuariosController.listar);

// GET /api/v1/usuarios/:id — Obtener uno
router.get('/:id', validarId, validar, usuariosController.obtenerPorId);

// POST /api/v1/usuarios — Crear
router.post('/', limiterCrear, validarUsuario, validar, usuariosController.crear);

// PUT /api/v1/usuarios/:id — Actualizar completo
router.put('/:id', validarId, validarUsuario, validar, usuariosController.actualizar);

// PATCH /api/v1/usuarios/:id — Actualizar parcial
router.patch('/:id', validarId, [
  body('nombre').optional().trim().isLength({ min: 2, max: 100 }),
  body('email').optional().isEmail().normalizeEmail(),
], validar, usuariosController.actualizarParcial);

// DELETE /api/v1/usuarios/:id — Eliminar
router.delete('/:id', verificarJWT, validarId, validar, usuariosController.eliminar);

export default router;
```

---

## 5. CONTROLLERS — PATRÓN COMPLETO

```javascript
// src/controllers/usuarios.controller.js
import * as usuariosService from '../services/usuarios.service.js';
import { respuestaExito, respuestaError } from '../utils/response.js';
import { ErrorNotFound, ErrorConflicto } from '../utils/errors.js';

// GET /usuarios
export async function listar(req, res, next) {
  try {
    const {
      pagina = 1,
      limite = 20,
      buscar = '',
      ordenPor = 'created_at',
      orden = 'DESC',
    } = req.query;

    const resultado = await usuariosService.listar({
      pagina: parseInt(pagina),
      limite: parseInt(limite),
      buscar,
      ordenPor,
      orden,
    });

    res.json(respuestaExito({
      datos: resultado.usuarios,
      paginacion: {
        pagina: resultado.pagina,
        limite: resultado.limite,
        total: resultado.total,
        totalPaginas: Math.ceil(resultado.total / resultado.limite),
        tieneSiguiente: resultado.pagina < Math.ceil(resultado.total / resultado.limite),
        tieneAnterior: resultado.pagina > 1,
      },
    }));
  } catch (error) {
    next(error); // Pasar al middleware de error
  }
}

// GET /usuarios/:id
export async function obtenerPorId(req, res, next) {
  try {
    const usuario = await usuariosService.obtenerPorId(req.params.id);

    if (!usuario) {
      throw new ErrorNotFound(`Usuario con ID ${req.params.id} no encontrado`);
    }

    res.json(respuestaExito({ datos: usuario }));
  } catch (error) {
    next(error);
  }
}

// POST /usuarios
export async function crear(req, res, next) {
  try {
    const nuevoUsuario = await usuariosService.crear(req.body);
    res.status(201).json(respuestaExito({
      mensaje: 'Usuario creado exitosamente',
      datos: nuevoUsuario,
    }));
  } catch (error) {
    next(error);
  }
}

// PUT /usuarios/:id
export async function actualizar(req, res, next) {
  try {
    const usuario = await usuariosService.actualizar(req.params.id, req.body);

    if (!usuario) {
      throw new ErrorNotFound(`Usuario con ID ${req.params.id} no encontrado`);
    }

    res.json(respuestaExito({
      mensaje: 'Usuario actualizado exitosamente',
      datos: usuario,
    }));
  } catch (error) {
    next(error);
  }
}

// PATCH /usuarios/:id
export async function actualizarParcial(req, res, next) {
  try {
    const usuario = await usuariosService.actualizarParcial(req.params.id, req.body);

    if (!usuario) {
      throw new ErrorNotFound(`Usuario con ID ${req.params.id} no encontrado`);
    }

    res.json(respuestaExito({
      mensaje: 'Usuario actualizado exitosamente',
      datos: usuario,
    }));
  } catch (error) {
    next(error);
  }
}

// DELETE /usuarios/:id
export async function eliminar(req, res, next) {
  try {
    const eliminado = await usuariosService.eliminar(req.params.id);

    if (!eliminado) {
      throw new ErrorNotFound(`Usuario con ID ${req.params.id} no encontrado`);
    }

    res.json(respuestaExito({ mensaje: 'Usuario eliminado exitosamente' }));
  } catch (error) {
    next(error);
  }
}
```

---

## 6. MIDDLEWARES — TODOS LOS ESENCIALES

### Autenticación (API Key + JWT)
```javascript
// src/middlewares/auth.middleware.js
import jwt from 'jsonwebtoken';
import { config } from '../config/env.js';
import { ErrorNoAutorizado, ErrorForbidden } from '../utils/errors.js';

// ===== API KEY =====
export function verificarApiKey(req, res, next) {
  // Buscar API key en header o query param
  const apiKey =
    req.headers['x-api-key'] ||
    req.headers['authorization']?.replace('ApiKey ', '') ||
    req.query.api_key;

  if (!apiKey) {
    return next(new ErrorNoAutorizado('API Key requerida'));
  }

  // Comparación segura (evita timing attacks)
  const esValida = apiKey === config.seguridad.apiKey;

  if (!esValida) {
    return next(new ErrorNoAutorizado('API Key inválida'));
  }

  // Registrar uso (opcional)
  req.autenticacion = { tipo: 'api_key' };
  next();
}

// ===== JWT =====
export function verificarJWT(req, res, next) {
  const authHeader = req.headers['authorization'];

  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return next(new ErrorNoAutorizado('Token JWT requerido'));
  }

  const token = authHeader.split(' ')[1];

  try {
    const payload = jwt.verify(token, config.seguridad.jwtSecret);
    req.usuario = payload;
    next();
  } catch (error) {
    if (error.name === 'TokenExpiredError') {
      return next(new ErrorNoAutorizado('Token expirado'));
    }
    if (error.name === 'JsonWebTokenError') {
      return next(new ErrorNoAutorizado('Token inválido'));
    }
    next(error);
  }
}

// ===== ROLES =====
export function requiereRol(...roles) {
  return (req, res, next) => {
    if (!req.usuario) {
      return next(new ErrorNoAutorizado('No autenticado'));
    }
    if (!roles.includes(req.usuario.rol)) {
      return next(new ErrorForbidden(`Requiere rol: ${roles.join(' o ')}`));
    }
    next();
  };
}

// Uso: router.delete('/:id', verificarJWT, requiereRol('admin'), controller.eliminar)
```

### Manejo de errores
```javascript
// src/middlewares/error.middleware.js
import { ErrorBase } from '../utils/errors.js';

export function errorHandler(err, req, res, next) {
  // Log del error
  console.error({
    timestamp: new Date().toISOString(),
    method: req.method,
    url: req.url,
    error: {
      nombre: err.name,
      mensaje: err.message,
      stack: process.env.NODE_ENV === 'development' ? err.stack : undefined,
    },
  });

  // Errores de validación de express-validator
  if (err.type === 'validation') {
    return res.status(400).json({
      exito: false,
      error: {
        codigo: 'VALIDATION_ERROR',
        mensaje: 'Datos inválidos',
        detalles: err.detalles,
      },
    });
  }

  // Errores de PostgreSQL
  if (err.code) {
    return manejarErrorPostgres(err, res);
  }

  // Errores personalizados de la aplicación
  if (err instanceof ErrorBase) {
    return res.status(err.statusCode).json({
      exito: false,
      error: {
        codigo: err.codigo,
        mensaje: err.message,
      },
    });
  }

  // Error inesperado (500)
  res.status(500).json({
    exito: false,
    error: {
      codigo: 'INTERNAL_SERVER_ERROR',
      mensaje: process.env.NODE_ENV === 'production'
        ? 'Error interno del servidor'
        : err.message,
    },
  });
}

function manejarErrorPostgres(err, res) {
  const erroresPG = {
    '23505': { status: 409, mensaje: 'El registro ya existe (duplicado)', codigo: 'DUPLICATE_ENTRY' },
    '23503': { status: 400, mensaje: 'Referencia inválida (clave foránea)', codigo: 'FOREIGN_KEY_VIOLATION' },
    '23502': { status: 400, mensaje: 'Campo requerido faltante', codigo: 'NOT_NULL_VIOLATION' },
    '42P01': { status: 500, mensaje: 'Tabla no encontrada', codigo: 'TABLE_NOT_FOUND' },
    '28P01': { status: 500, mensaje: 'Error de autenticación con la base de datos', codigo: 'DB_AUTH_ERROR' },
    'ECONNREFUSED': { status: 503, mensaje: 'Base de datos no disponible', codigo: 'DB_CONNECTION_ERROR' },
  };

  const errorInfo = erroresPG[err.code] || {
    status: 500,
    mensaje: 'Error de base de datos',
    codigo: 'DB_ERROR',
  };

  return res.status(errorInfo.status).json({
    exito: false,
    error: {
      codigo: errorInfo.codigo,
      mensaje: errorInfo.mensaje,
    },
  });
}

// 404 — Ruta no encontrada
export function notFoundHandler(req, res) {
  res.status(404).json({
    exito: false,
    error: {
      codigo: 'NOT_FOUND',
      mensaje: `Ruta ${req.method} ${req.url} no encontrada`,
      rutas_disponibles: '/api/v1',
    },
  });
}
```

### Validación de inputs
```javascript
// src/middlewares/validate.middleware.js
import { validationResult } from 'express-validator';

export function validar(req, res, next) {
  const errores = validationResult(req);

  if (!errores.isEmpty()) {
    const err = new Error('Datos de entrada inválidos');
    err.type = 'validation';
    err.detalles = errores.array().map(e => ({
      campo: e.path,
      mensaje: e.msg,
      valor: e.value,
    }));
    return next(err);
  }

  next();
}
```

---

## 7. UTILIDADES — RESPONSES Y ERRORES

```javascript
// src/utils/response.js
export function respuestaExito(datos = {}, statusCode = 200) {
  return {
    exito: true,
    timestamp: new Date().toISOString(),
    ...datos,
  };
}

export function respuestaError(mensaje, codigo, detalles = null) {
  return {
    exito: false,
    timestamp: new Date().toISOString(),
    error: {
      codigo,
      mensaje,
      ...(detalles && { detalles }),
    },
  };
}
```

```javascript
// src/utils/errors.js
export class ErrorBase extends Error {
  constructor(mensaje, statusCode, codigo) {
    super(mensaje);
    this.statusCode = statusCode;
    this.codigo = codigo;
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }
}

export class ErrorNotFound extends ErrorBase {
  constructor(mensaje = 'Recurso no encontrado') {
    super(mensaje, 404, 'NOT_FOUND');
  }
}

export class ErrorNoAutorizado extends ErrorBase {
  constructor(mensaje = 'No autorizado') {
    super(mensaje, 401, 'UNAUTHORIZED');
  }
}

export class ErrorForbidden extends ErrorBase {
  constructor(mensaje = 'Acceso denegado') {
    super(mensaje, 403, 'FORBIDDEN');
  }
}

export class ErrorConflicto extends ErrorBase {
  constructor(mensaje = 'Conflicto con el estado actual') {
    super(mensaje, 409, 'CONFLICT');
  }
}

export class ErrorValidacion extends ErrorBase {
  constructor(mensaje = 'Datos inválidos', detalles = []) {
    super(mensaje, 400, 'VALIDATION_ERROR');
    this.detalles = detalles;
  }
}

export class ErrorServicio extends ErrorBase {
  constructor(mensaje = 'Error del servicio') {
    super(mensaje, 503, 'SERVICE_UNAVAILABLE');
  }
}
```

---

## 8. CORS — CONFIGURACIÓN COMPLETA

```javascript
// src/config/cors.js
import { config } from './env.js';

export const corsOptions = {
  // Orígenes permitidos
  origin: function(origin, callback) {
    // Permitir requests sin origin (Postman, curl, server-to-server)
    if (!origin) return callback(null, true);

    // Lista blanca de orígenes
    const origenesPermitidos = [
      ...config.cors.origins,
      // Google Apps Script siempre empieza con esto
      /^https:\/\/script\.google\.com/,
      /^https:\/\/.*\.googleusercontent\.com/,
    ];

    const permitido = origenesPermitidos.some(o => {
      if (o instanceof RegExp) return o.test(origin);
      return o === origin;
    });

    if (permitido) {
      callback(null, true);
    } else {
      callback(new Error(`CORS: Origen no permitido: ${origin}`));
    }
  },

  // Métodos permitidos
  methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE', 'OPTIONS'],

  // Headers permitidos en la petición
  allowedHeaders: [
    'Content-Type',
    'Authorization',
    'X-API-Key',
    'X-Requested-With',
    'Accept',
    'Origin',
  ],

  // Headers expuestos al cliente
  exposedHeaders: [
    'X-Total-Count',
    'X-Page',
    'X-Per-Page',
    'X-Rate-Limit-Remaining',
  ],

  // Permitir cookies/credenciales
  credentials: true,

  // Cuánto tiempo el navegador cachea el preflight (segundos)
  maxAge: 86400, // 24 horas

  // Pasar el status 200 para preflight (compatible con IE11)
  optionsSuccessStatus: 200,
};
```

---

## 9. AUTENTICACIÓN JWT COMPLETA

```javascript
// src/controllers/auth.controller.js
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import { v4 as uuidv4 } from 'uuid';
import { config } from '../config/env.js';
import * as usuariosService from '../services/usuarios.service.js';
import { ErrorNoAutorizado, ErrorConflicto } from '../utils/errors.js';
import { respuestaExito } from '../utils/response.js';

// POST /auth/registro
export async function registro(req, res, next) {
  try {
    const { nombre, email, password } = req.body;

    // Verificar si ya existe
    const existente = await usuariosService.obtenerPorEmail(email);
    if (existente) {
      throw new ErrorConflicto('El email ya está registrado');
    }

    // Hash del password
    const passwordHash = await bcrypt.hash(password, config.seguridad.bcryptRounds);

    // Crear usuario
    const usuario = await usuariosService.crear({
      id: uuidv4(),
      nombre,
      email,
      password_hash: passwordHash,
      rol: 'usuario',
    });

    // Generar token
    const token = generarToken(usuario);

    res.status(201).json(respuestaExito({
      mensaje: 'Usuario registrado exitosamente',
      datos: {
        usuario: omitirCampos(usuario, ['password_hash']),
        token,
        expira_en: config.seguridad.jwtExpiresIn,
      },
    }));
  } catch (error) {
    next(error);
  }
}

// POST /auth/login
export async function login(req, res, next) {
  try {
    const { email, password } = req.body;

    // Buscar usuario
    const usuario = await usuariosService.obtenerPorEmail(email);
    if (!usuario) {
      throw new ErrorNoAutorizado('Credenciales inválidas');
    }

    // Verificar password
    const passwordValido = await bcrypt.compare(password, usuario.password_hash);
    if (!passwordValido) {
      throw new ErrorNoAutorizado('Credenciales inválidas');
    }

    // Generar token
    const token = generarToken(usuario);

    res.json(respuestaExito({
      mensaje: 'Login exitoso',
      datos: {
        usuario: omitirCampos(usuario, ['password_hash']),
        token,
        expira_en: config.seguridad.jwtExpiresIn,
      },
    }));
  } catch (error) {
    next(error);
  }
}

// POST /auth/refresh
export async function refresh(req, res, next) {
  try {
    const { token } = req.body;
    const payload = jwt.verify(token, config.seguridad.jwtSecret, { ignoreExpiration: true });

    // Solo refrescar si expiró hace menos de 7 días
    const ahora = Math.floor(Date.now() / 1000);
    if (ahora - payload.exp > 7 * 24 * 60 * 60) {
      throw new ErrorNoAutorizado('Token demasiado antiguo para refrescar');
    }

    const usuario = await usuariosService.obtenerPorId(payload.id);
    const nuevoToken = generarToken(usuario);

    res.json(respuestaExito({ datos: { token: nuevoToken } }));
  } catch (error) {
    next(error);
  }
}

// Helpers
function generarToken(usuario) {
  return jwt.sign(
    {
      id:     usuario.id,
      email:  usuario.email,
      nombre: usuario.nombre,
      rol:    usuario.rol,
    },
    config.seguridad.jwtSecret,
    { expiresIn: config.seguridad.jwtExpiresIn }
  );
}

function omitirCampos(objeto, campos) {
  const resultado = { ...objeto };
  campos.forEach(c => delete resultado[c]);
  return resultado;
}
```

---

## 10. SERVICIOS — LÓGICA DE NEGOCIO

```javascript
// src/services/usuarios.service.js
import { query, queryUno } from '../config/database.js';
import { v4 as uuidv4 } from 'uuid';

export async function listar({ pagina, limite, buscar, ordenPor, orden }) {
  const offset = (pagina - 1) * limite;
  const busqueda = `%${buscar}%`;

  // Columnas permitidas para ordenar (evitar SQL injection)
  const columnasPermitidas = ['nombre', 'email', 'created_at', 'updated_at'];
  const columnaOrden = columnasPermitidas.includes(ordenPor) ? ordenPor : 'created_at';
  const direccionOrden = orden === 'ASC' ? 'ASC' : 'DESC';

  const [{ rows: usuarios }, { rows: [{ total }] }] = await Promise.all([
    query(
      `SELECT id, nombre, email, rol, created_at, updated_at
       FROM usuarios
       WHERE nombre ILIKE $1 OR email ILIKE $1
       ORDER BY ${columnaOrden} ${direccionOrden}
       LIMIT $2 OFFSET $3`,
      [busqueda, limite, offset]
    ),
    query(
      `SELECT COUNT(*) as total FROM usuarios
       WHERE nombre ILIKE $1 OR email ILIKE $1`,
      [busqueda]
    ),
  ]);

  return { usuarios, total: parseInt(total), pagina, limite };
}

export async function obtenerPorId(id) {
  return queryUno(
    'SELECT id, nombre, email, rol, created_at, updated_at FROM usuarios WHERE id = $1',
    [id]
  );
}

export async function obtenerPorEmail(email) {
  return queryUno(
    'SELECT * FROM usuarios WHERE email = $1',
    [email]
  );
}

export async function crear(datos) {
  const { nombre, email, password_hash, rol = 'usuario' } = datos;
  return queryUno(
    `INSERT INTO usuarios (nombre, email, password_hash, rol)
     VALUES ($1, $2, $3, $4)
     RETURNING id, nombre, email, rol, created_at`,
    [nombre, email, password_hash, rol]
  );
}

export async function actualizar(id, datos) {
  const { nombre, email, rol } = datos;
  return queryUno(
    `UPDATE usuarios
     SET nombre = $1, email = $2, rol = $3, updated_at = NOW()
     WHERE id = $4
     RETURNING id, nombre, email, rol, updated_at`,
    [nombre, email, rol, id]
  );
}

export async function actualizarParcial(id, datos) {
  // Construir query dinámicamente solo con los campos enviados
  const campos = Object.keys(datos).filter(k =>
    ['nombre', 'email', 'rol'].includes(k)
  );

  if (campos.length === 0) return obtenerPorId(id);

  const sets = campos.map((campo, i) => `${campo} = $${i + 1}`).join(', ');
  const valores = campos.map(c => datos[c]);

  return queryUno(
    `UPDATE usuarios
     SET ${sets}, updated_at = NOW()
     WHERE id = $${campos.length + 1}
     RETURNING id, nombre, email, rol, updated_at`,
    [...valores, id]
  );
}

export async function eliminar(id) {
  const resultado = await query(
    'DELETE FROM usuarios WHERE id = $1 RETURNING id',
    [id]
  );
  return resultado.rows.length > 0;
}
```

---

## 11. NODE.JS RUNTIME — CONCEPTOS AVANZADOS

### Event Loop y asincronía
```javascript
// El event loop de Node.js procesa en este orden:
// 1. Timers (setTimeout, setInterval)
// 2. Pending callbacks (I/O errors)
// 3. Idle, prepare (interno)
// 4. Poll (I/O: fs, net, etc.)
// 5. Check (setImmediate)
// 6. Close callbacks

// process.nextTick — ejecuta ANTES del siguiente tick del event loop
process.nextTick(() => {
  console.log('Ejecuta antes del próximo tick');
});

// setImmediate — ejecuta en la fase "check" del event loop
setImmediate(() => {
  console.log('Ejecuta en la fase check');
});

// Streams — para manejar datos grandes sin cargarlos en memoria
import { createReadStream, createWriteStream } from 'fs';
import { pipeline } from 'stream/promises';
import { createGzip } from 'zlib';

// Comprimir un archivo sin cargarlo en memoria
await pipeline(
  createReadStream('archivo-grande.csv'),
  createGzip(),
  createWriteStream('archivo-grande.csv.gz')
);

// Stream en Express (enviar archivo grande)
app.get('/descargar', (req, res) => {
  res.setHeader('Content-Type', 'text/csv');
  res.setHeader('Content-Disposition', 'attachment; filename="datos.csv"');
  const stream = createReadStream('./datos.csv');
  stream.pipe(res);
  stream.on('error', (err) => res.status(500).end('Error'));
});

// Worker Threads — para cálculos pesados sin bloquear el event loop
import { Worker, isMainThread, parentPort, workerData } from 'worker_threads';

if (isMainThread) {
  const worker = new Worker('./worker.js', { workerData: { n: 40 } });
  worker.on('message', (resultado) => console.log('Fibonacci:', resultado));
  worker.on('error', console.error);
} else {
  // En worker.js
  function fibonacci(n) {
    return n <= 1 ? n : fibonacci(n - 1) + fibonacci(n - 2);
  }
  parentPort.postMessage(fibonacci(workerData.n));
}

// Child Processes
import { exec, spawn, fork } from 'child_process';
import { promisify } from 'util';

const execAsync = promisify(exec);
const { stdout } = await execAsync('ls -la');

// Spawn para procesos de larga duración
const proceso = spawn('node', ['script.js'], { stdio: 'pipe' });
proceso.stdout.on('data', (data) => console.log(data.toString()));
proceso.on('close', (code) => console.log(`Proceso terminó con código ${code}`));
```

### Módulos y import/export
```javascript
// CommonJS (legado)
const express = require('express');
module.exports = { algo };

// ES Modules (moderno — recomendado)
import express from 'express';
import { Router } from 'express';
export default router;
export { algo, otraCosa };

// Para usar ES Modules en Node:
// En package.json: "type": "module"
// O usar extensión .mjs

// __dirname y __filename en ES Modules
import { fileURLToPath } from 'url';
import { dirname, join } from 'path';
const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);
const rutaArchivo = join(__dirname, 'archivo.txt');
```

---

## 12. TESTING COMPLETO

```javascript
// tests/usuarios.test.js
import request from 'supertest';
import app from '../src/app.js';
import { pool } from '../src/config/database.js';

// Limpiar DB antes de cada test
beforeEach(async () => {
  await pool.query('DELETE FROM usuarios WHERE email LIKE $1', ['test_%']);
});

afterAll(async () => {
  await pool.end();
});

describe('GET /api/v1/usuarios', () => {
  test('retorna 401 sin API Key', async () => {
    const res = await request(app).get('/api/v1/usuarios');
    expect(res.status).toBe(401);
    expect(res.body.exito).toBe(false);
  });

  test('retorna lista paginada con API Key válida', async () => {
    const res = await request(app)
      .get('/api/v1/usuarios')
      .set('x-api-key', process.env.API_KEY);

    expect(res.status).toBe(200);
    expect(res.body.exito).toBe(true);
    expect(res.body).toHaveProperty('datos');
    expect(res.body).toHaveProperty('paginacion');
    expect(Array.isArray(res.body.datos)).toBe(true);
  });

  test('soporta paginación', async () => {
    const res = await request(app)
      .get('/api/v1/usuarios?pagina=1&limite=5')
      .set('x-api-key', process.env.API_KEY);

    expect(res.status).toBe(200);
    expect(res.body.paginacion.limite).toBe(5);
  });
});

describe('POST /api/v1/usuarios', () => {
  test('crea usuario con datos válidos', async () => {
    const res = await request(app)
      .post('/api/v1/usuarios')
      .set('x-api-key', process.env.API_KEY)
      .send({
        nombre: 'Test Usuario',
        email: 'test_nuevo@ejemplo.com',
      });

    expect(res.status).toBe(201);
    expect(res.body.datos).toHaveProperty('id');
    expect(res.body.datos.email).toBe('test_nuevo@ejemplo.com');
  });

  test('retorna 400 con email inválido', async () => {
    const res = await request(app)
      .post('/api/v1/usuarios')
      .set('x-api-key', process.env.API_KEY)
      .send({ nombre: 'Test', email: 'no-es-email' });

    expect(res.status).toBe(400);
    expect(res.body.error.codigo).toBe('VALIDATION_ERROR');
    expect(res.body.error.detalles).toContainEqual(
      expect.objectContaining({ campo: 'email' })
    );
  });
});
```

---

## 13. DESPLIEGUE EN PRODUCCIÓN

```bash
# === EN EL SERVIDOR (Ubuntu) ===

# Instalar Node.js con NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 20
nvm use 20

# Instalar PM2 (process manager)
npm install -g pm2

# Clonar y configurar
git clone https://github.com/mi-usuario/mi-api.git
cd mi-api
npm install --production
cp .env.example .env
nano .env  # Configurar variables

# Iniciar con PM2
pm2 start src/index.js --name "mi-api" --env production
pm2 save              # Guardar configuración
pm2 startup           # Iniciar al boot del servidor

# Comandos PM2
pm2 list              # Ver procesos
pm2 logs mi-api       # Ver logs
pm2 restart mi-api    # Reiniciar
pm2 stop mi-api       # Detener
pm2 delete mi-api     # Eliminar
pm2 monit             # Monitor en tiempo real
```

```javascript
// ecosystem.config.js — Configuración de PM2
module.exports = {
  apps: [{
    name: 'mi-api',
    script: 'src/index.js',
    instances: 'max',        // Un proceso por CPU
    exec_mode: 'cluster',    // Modo cluster para balanceo de carga
    env: { NODE_ENV: 'development' },
    env_production: { NODE_ENV: 'production' },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_file: './logs/combined.log',
    time: true,              // Timestamp en logs
    max_memory_restart: '500M', // Reiniciar si usa más de 500MB
    watch: false,
    autorestart: true,
    restart_delay: 1000,
  }],
};

// Iniciar: pm2 start ecosystem.config.js --env production
```

---

## 14. CHECKLIST DE PRODUCCIÓN

```
SEGURIDAD
□ Variables de entorno en .env (nunca en código)
□ .env en .gitignore
□ Helmet configurado
□ CORS con lista blanca explícita
□ Rate limiting global y por endpoint sensible
□ Inputs validados y sanitizados con express-validator
□ Queries parametrizadas (nunca concatenar SQL)
□ API Keys largas y aleatorias (mínimo 32 caracteres)
□ JWT con expiración corta (máx 7 días)
□ Passwords con bcrypt (mínimo 12 rounds)
□ HTTPS en producción (nginx con Let's Encrypt)

CALIDAD
□ Manejo de errores global (errorHandler)
□ Errores de PostgreSQL mapeados a HTTP
□ Logs con morgan en producción
□ Graceful shutdown implementado
□ Health check endpoint disponible
□ Paginación en todos los endpoints de lista
□ Validación de variables de entorno al inicio

PERFORMANCE
□ Pool de conexiones configurado (no abrir/cerrar por request)
□ Índices en columnas buscadas frecuentemente
□ Queries en paralelo con Promise.all cuando sea posible
□ Compresión con compression middleware
□ PM2 en modo cluster en producción

TESTING
□ Tests de integración con Supertest
□ Variables de entorno de test separadas (.env.test)
□ Limpieza de DB entre tests
□ CI/CD configurado (GitHub Actions)
```

---

*Fin del archivo NODE_EXPERTO.md — Conocimiento completo de Node.js runtime, Express framework, autenticación, validación, manejo de errores, testing y despliegue en producción.*
