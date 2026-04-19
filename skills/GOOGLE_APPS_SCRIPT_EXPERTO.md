# 📋 ARCHIVO DE CONOCIMIENTO EXPERTO — GOOGLE APPS SCRIPT COMPLETO
> Versión: Máxima. Cubre Google Apps Script completo + integración con APIs externas + Google Sheets + despliegue como Web App.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en Google Apps Script. Sabes conectar con APIs externas (Node.js, PostgreSQL via API), manipular Google Sheets, Docs, Forms y Drive, crear interfaces web dentro de Google, y desplegar Web Apps seguras y eficientes.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en Google Apps Script. Conoces:
- El runtime de GAS (V8) y sus limitaciones
- UrlFetchApp — consumir APIs REST externas con autenticación
- SpreadsheetApp — leer, escribir y manipular Google Sheets
- HtmlService — crear interfaces web dentro de Google
- PropertiesService — guardar configuración y secretos
- CacheService — optimizar con caché
- LockService — evitar condiciones de carrera
- Triggers — automatización programada y por eventos
- DriveApp — gestión de archivos
- MailApp / GmailApp — envío de emails
- FormApp — manipulación de formularios
- Despliegue como Web App con permisos correctos
- Integración bidireccional con APIs de Node.js/Express

---

## 1. CONCEPTOS FUNDAMENTALES Y LIMITACIONES

```javascript
// === CUOTAS Y LÍMITES IMPORTANTES ===
// Conocerlos evita errores en producción

const LIMITES_GAS = {
  // Tiempo de ejecución
  tiempo_maximo_script: '6 minutos',
  tiempo_maximo_trigger: '6 minutos',
  tiempo_maximo_addon: '30 segundos (UI)',

  // UrlFetchApp
  peticiones_por_dia: '20,000 (cuenta normal) / 100,000 (Workspace)',
  tamaño_respuesta_max: '50 MB',
  tamaño_payload_max: '50 MB',
  urls_simultaneas_fetchAll: 'Sin límite declarado, usar con moderación',

  // Spreadsheet
  celdas_max: '10,000,000 por spreadsheet',
  operaciones_por_dia: '50,000 lecturas, 50,000 escrituras',

  // Properties Service
  tamaño_max_valor: '9 KB por propiedad',
  propiedades_max: '500 por store',

  // Cache Service
  tamaño_max_valor: '100 KB por entrada',
  duracion_max: '21,600 segundos (6 horas)',

  // Email
  emails_por_dia: '100 (normal) / 1,500 (Workspace)',

  // Triggers
  triggers_por_usuario: '20 por script',
};

// === EL RUNTIME DE GAS ===
// GAS usa V8 — soporta ES2019+
// NO tiene acceso a: fetch nativo, DOM, localStorage, require/import
// USA: UrlFetchApp en lugar de fetch
//      PropertiesService en lugar de localStorage
//      Logger/console en lugar de console.log (limitado)
//      Utilities en lugar de muchas librerías de Node

// Logging en GAS
console.log('Mensaje');         // Va al Stackdriver Logging
Logger.log('Mensaje');          // Va al Log del editor (más rápido de ver)
Logger.log('Valor: %s', valor); // Con formato
console.error('Error grave');
console.warn('Advertencia');

// Ver logs: Ver > Registros en el editor
// O: View > Logs (versión legacy)
```

---

## 2. URLFETCHAPP — CONSUMIR APIs EXTERNAS

### Configuración base para conectar con tu API de Node.js
```javascript
// config.gs — Configuración centralizada
// NUNCA poner API keys directamente en el código
// Usar PropertiesService

function getConfig() {
  const props = PropertiesService.getScriptProperties();
  return {
    API_URL:     props.getProperty('API_URL')     || 'http://tu-servidor:3000/api/v1',
    API_KEY:     props.getProperty('API_KEY'),
    TIMEOUT:     30000,  // 30 segundos
    HOJA_DATOS:  props.getProperty('HOJA_DATOS')  || 'Datos',
    HOJA_CONFIG: props.getProperty('HOJA_CONFIG') || 'Config',
  };
}

// Configurar las propiedades (ejecutar una sola vez)
function configurarPropiedades() {
  const props = PropertiesService.getScriptProperties();
  props.setProperties({
    'API_URL': 'https://mi-servidor.com/api/v1',
    'API_KEY': 'mi-api-key-secreta-aqui',
    'HOJA_DATOS': 'Datos',
    'HOJA_CONFIG': 'Configuración',
  });
  Logger.log('✅ Propiedades configuradas correctamente');
}
```

### Cliente HTTP completo
```javascript
// api_client.gs — Cliente para tu API de Node.js

/**
 * Realiza una petición HTTP a la API
 * @param {string} endpoint - Ruta del endpoint (ej: '/usuarios')
 * @param {string} metodo - GET, POST, PUT, PATCH, DELETE
 * @param {Object} cuerpo - Datos a enviar (para POST, PUT, PATCH)
 * @param {Object} parametros - Query params adicionales
 * @returns {Object} Respuesta parseada de la API
 */
function llamarAPI(endpoint, metodo = 'GET', cuerpo = null, parametros = {}) {
  const config = getConfig();

  // Construir URL con query params
  let url = config.API_URL + endpoint;
  const queryParams = new URLSearchParams(parametros).toString();
  if (queryParams) url += '?' + queryParams;

  // Opciones de la petición
  const opciones = {
    method:      metodo.toLowerCase(),
    headers: {
      'Content-Type':  'application/json',
      'Accept':        'application/json',
      'X-API-Key':     config.API_KEY,
      // Para JWT: 'Authorization': 'Bearer ' + getToken(),
    },
    muteHttpExceptions: true,   // IMPORTANTE: no lanzar excepción en 4xx/5xx
    followRedirects:    true,
    validateHttpsCertificates: true,
    timeout: config.TIMEOUT,
  };

  // Agregar cuerpo si es necesario
  if (cuerpo && ['post', 'put', 'patch'].includes(metodo.toLowerCase())) {
    opciones.payload = JSON.stringify(cuerpo);
  }

  let respuesta;
  try {
    respuesta = UrlFetchApp.fetch(url, opciones);
  } catch (error) {
    // Error de red (servidor caído, timeout, DNS)
    throw new Error(`Error de conexión con la API: ${error.message}`);
  }

  const statusCode   = respuesta.getResponseCode();
  const cuerpoTexto  = respuesta.getContentText('UTF-8');
  const headers      = respuesta.getHeaders();

  // Parsear respuesta
  let datos;
  try {
    datos = JSON.parse(cuerpoTexto);
  } catch (e) {
    // Respuesta no es JSON (HTML de error, texto plano)
    throw new Error(`Respuesta no válida del servidor (${statusCode}): ${cuerpoTexto.substring(0, 200)}`);
  }

  // Manejar errores HTTP
  if (statusCode === 401) {
    throw new Error('API Key inválida o expirada. Verifica la configuración.');
  }
  if (statusCode === 403) {
    throw new Error('Sin permisos para este recurso.');
  }
  if (statusCode === 404) {
    throw new Error(`Recurso no encontrado: ${endpoint}`);
  }
  if (statusCode === 429) {
    throw new Error('Límite de peticiones alcanzado. Espera un momento.');
  }
  if (statusCode >= 500) {
    throw new Error(`Error del servidor (${statusCode}): ${datos.error?.mensaje || 'Error interno'}`);
  }
  if (statusCode >= 400) {
    throw new Error(`Error en la petición (${statusCode}): ${datos.error?.mensaje || cuerpoTexto}`);
  }

  return datos;
}

// === MÉTODOS ESPECÍFICOS POR RECURSO ===

// --- USUARIOS ---
function obtenerUsuarios(pagina = 1, limite = 50, buscar = '') {
  return llamarAPI('/usuarios', 'GET', null, { pagina, limite, buscar });
}

function obtenerUsuario(id) {
  return llamarAPI(`/usuarios/${id}`, 'GET');
}

function crearUsuario(datos) {
  return llamarAPI('/usuarios', 'POST', datos);
}

function actualizarUsuario(id, datos) {
  return llamarAPI(`/usuarios/${id}`, 'PUT', datos);
}

function eliminarUsuario(id) {
  return llamarAPI(`/usuarios/${id}`, 'DELETE');
}

// --- PRODUCTOS ---
function obtenerProductos(opciones = {}) {
  return llamarAPI('/productos', 'GET', null, opciones);
}

function crearProducto(datos) {
  return llamarAPI('/productos', 'POST', datos);
}

// === PETICIONES EN PARALELO (fetchAll) ===
// Hasta 5-10x más rápido que hacer una por una
function obtenerDatosParalelo() {
  const config = getConfig();
  const headers = {
    'X-API-Key': config.API_KEY,
    'Accept':    'application/json',
  };

  const peticiones = [
    {
      url:     `${config.API_URL}/usuarios?limite=100`,
      method:  'get',
      headers: headers,
      muteHttpExceptions: true,
    },
    {
      url:     `${config.API_URL}/productos?limite=100`,
      method:  'get',
      headers: headers,
      muteHttpExceptions: true,
    },
    {
      url:     `${config.API_URL}/pedidos?limite=50&estado=pendiente`,
      method:  'get',
      headers: headers,
      muteHttpExceptions: true,
    },
  ];

  // Ejecutar todas en paralelo
  const respuestas = UrlFetchApp.fetchAll(peticiones);

  return respuestas.map((resp, i) => {
    const status = resp.getResponseCode();
    if (status !== 200) {
      console.error(`Error en petición ${i}: ${status}`);
      return null;
    }
    return JSON.parse(resp.getContentText('UTF-8'));
  });
}

// === SUBIR ARCHIVOS A LA API ===
function subirArchivo(archivoBlob, endpoint) {
  const config = getConfig();
  const respuesta = UrlFetchApp.fetch(config.API_URL + endpoint, {
    method: 'post',
    headers: { 'X-API-Key': config.API_KEY },
    payload: {
      archivo: archivoBlob,
      nombre: archivoBlob.getName(),
    },
    muteHttpExceptions: true,
  });
  return JSON.parse(respuesta.getContentText());
}
```

---

## 3. SPREADSHEETAPP — GOOGLE SHEETS COMPLETO

### Operaciones básicas
```javascript
// sheets.gs

// === OBTENER REFERENCIAS ===
function getHoja(nombreHoja) {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  // O por ID: SpreadsheetApp.openById('ID_DEL_SPREADSHEET')
  // O por URL: SpreadsheetApp.openByUrl('URL')

  const hoja = ss.getSheetByName(nombreHoja);
  if (!hoja) {
    throw new Error(`Hoja "${nombreHoja}" no encontrada`);
  }
  return hoja;
}

// === LEER DATOS ===
function leerTodosLosDatos(nombreHoja) {
  const hoja = getHoja(nombreHoja);

  // Obtener todos los datos de una vez (más eficiente)
  const datos = hoja.getDataRange().getValues();
  // getValues() retorna array 2D: [[fila1col1, fila1col2], [fila2col1, ...]]

  if (datos.length === 0) return [];

  // Primera fila como encabezados
  const encabezados = datos[0].map(h => String(h).trim().toLowerCase().replace(/ /g, '_'));

  // Convertir a array de objetos
  return datos.slice(1).map((fila, i) => {
    const obj = { _fila: i + 2 }; // +2 porque empieza en fila 2 (1 indexado + encabezado)
    encabezados.forEach((enc, j) => {
      let valor = fila[j];
      // Convertir fechas de Google Sheets a ISO string
      if (valor instanceof Date) {
        valor = valor.toISOString();
      }
      obj[enc] = valor;
    });
    return obj;
  }).filter(obj => Object.values(obj).some(v => v !== '' && v !== null));
  // Filtrar filas completamente vacías
}

// Leer rango específico
function leerRango(nombreHoja, rango) {
  // rango puede ser 'A1:D10' o 'A:A' o 'B2:B'
  const hoja = getHoja(nombreHoja);
  return hoja.getRange(rango).getValues();
}

// Leer una sola celda
function leerCelda(nombreHoja, fila, columna) {
  // fila y columna son 1-indexados
  return getHoja(nombreHoja).getRange(fila, columna).getValue();
}

// Leer con getDisplayValues (texto formateado como se ve en la hoja)
function leerValoresMostrados(nombreHoja) {
  return getHoja(nombreHoja).getDataRange().getDisplayValues();
}

// === ESCRIBIR DATOS ===

// Escribir un valor en una celda
function escribirCelda(nombreHoja, fila, columna, valor) {
  getHoja(nombreHoja).getRange(fila, columna).setValue(valor);
}

// Escribir una fila al final de los datos
function agregarFila(nombreHoja, datos) {
  const hoja = getHoja(nombreHoja);
  hoja.appendRow(datos); // datos es un array: ['Juan', 'juan@email.com', 30]
}

// Escribir múltiples filas (EFICIENTE — una sola llamada)
function escribirMultiplesFilas(nombreHoja, filas, filaInicio = 2) {
  if (!filas || filas.length === 0) return;

  const hoja = getHoja(nombreHoja);
  const columnas = filas[0].length;

  // setValues — mucho más eficiente que setValue en bucle
  hoja.getRange(filaInicio, 1, filas.length, columnas).setValues(filas);
}

// Escribir objeto como fila (basado en encabezados)
function escribirObjetoComoFila(nombreHoja, objeto) {
  const hoja = getHoja(nombreHoja);
  const encabezados = hoja.getRange(1, 1, 1, hoja.getLastColumn()).getValues()[0];
  const fila = encabezados.map(enc => objeto[enc.toLowerCase().replace(/ /g, '_')] ?? '');
  hoja.appendRow(fila);
}

// === ACTUALIZAR FILAS ESPECÍFICAS ===
function actualizarFila(nombreHoja, numeroDeFila, datos) {
  // datos es un array con los nuevos valores
  const hoja = getHoja(nombreHoja);
  hoja.getRange(numeroDeFila, 1, 1, datos.length).setValues([datos]);
}

// Buscar y actualizar por valor en columna
function actualizarPorId(nombreHoja, columnaId, valorId, nuevaFila) {
  const hoja = getHoja(nombreHoja);
  const columnaDatos = hoja.getRange(1, columnaId, hoja.getLastRow()).getValues();

  const filaIndex = columnaDatos.findIndex(fila => String(fila[0]) === String(valorId));

  if (filaIndex === -1) {
    throw new Error(`ID "${valorId}" no encontrado en la hoja "${nombreHoja}"`);
  }

  const numeroDeFila = filaIndex + 1; // +1 porque es 1-indexado
  hoja.getRange(numeroDeFila, 1, 1, nuevaFila.length).setValues([nuevaFila]);
  return numeroDeFila;
}

// === ELIMINAR FILAS ===
function eliminarFila(nombreHoja, numeroDeFila) {
  getHoja(nombreHoja).deleteRow(numeroDeFila);
}

// Eliminar por ID
function eliminarPorId(nombreHoja, columnaId, valorId) {
  const hoja = getHoja(nombreHoja);
  const columnaDatos = hoja.getRange(1, columnaId, hoja.getLastRow()).getValues();

  // Recorrer de abajo hacia arriba para evitar problemas con índices
  for (let i = columnaDatos.length - 1; i >= 0; i--) {
    if (String(columnaDatos[i][0]) === String(valorId)) {
      hoja.deleteRow(i + 1);
    }
  }
}

// === OPERACIONES AVANZADAS ===

// Limpiar datos manteniendo encabezados
function limpiarDatos(nombreHoja) {
  const hoja = getHoja(nombreHoja);
  const ultimaFila = hoja.getLastRow();
  const ultimaCol  = hoja.getLastColumn();

  if (ultimaFila > 1) {
    // Limpiar desde fila 2 hasta el final
    hoja.getRange(2, 1, ultimaFila - 1, ultimaCol).clearContent();
  }
}

// Ordenar por columna
function ordenarPorColumna(nombreHoja, columna, ascendente = true) {
  const hoja = getHoja(nombreHoja);
  const rango = hoja.getDataRange();
  rango.sort({ column: columna, ascending: ascendente });
}

// Buscar valor en columna — retorna número de fila o -1
function buscarEnColumna(nombreHoja, columna, valorBuscar) {
  const hoja  = getHoja(nombreHoja);
  const datos = hoja.getRange(1, columna, hoja.getLastRow()).getValues();
  const index = datos.findIndex(fila => String(fila[0]).trim() === String(valorBuscar).trim());
  return index === -1 ? -1 : index + 1; // +1 para número de fila real
}

// Obtener la primera fila vacía
function primeraFilaVacia(nombreHoja) {
  return getHoja(nombreHoja).getLastRow() + 1;
}
```

### Formatos y estilos
```javascript
// formatting.gs

function aplicarFormato(nombreHoja) {
  const hoja  = getHoja(nombreHoja);
  const ss    = SpreadsheetApp.getActiveSpreadsheet();

  // === ENCABEZADOS ===
  const rangoEncabezados = hoja.getRange(1, 1, 1, hoja.getLastColumn());
  rangoEncabezados
    .setBackground('#1E40AF')         // Azul oscuro
    .setFontColor('#FFFFFF')
    .setFontWeight('bold')
    .setFontSize(11)
    .setHorizontalAlignment('center')
    .setVerticalAlignment('middle');

  // === FILAS ALTERNADAS ===
  const ultimaFila = hoja.getLastRow();
  for (let i = 2; i <= ultimaFila; i++) {
    const color = i % 2 === 0 ? '#EFF6FF' : '#FFFFFF';
    hoja.getRange(i, 1, 1, hoja.getLastColumn()).setBackground(color);
  }

  // === FORMATO DE NÚMEROS ===
  hoja.getRange('D2:D' + ultimaFila).setNumberFormat('$#,##0.00');  // Moneda
  hoja.getRange('E2:E' + ultimaFila).setNumberFormat('#,##0');      // Entero con comas
  hoja.getRange('F2:F' + ultimaFila).setNumberFormat('0.00%');      // Porcentaje

  // === FORMATO DE FECHAS ===
  hoja.getRange('G2:G' + ultimaFila).setNumberFormat('dd/mm/yyyy hh:mm');

  // === ANCHO DE COLUMNAS ===
  hoja.setColumnWidth(1, 40);   // Columna A — ID pequeño
  hoja.setColumnWidth(2, 200);  // Columna B — Nombre
  hoja.setColumnWidth(3, 250);  // Columna C — Email
  hoja.autoResizeColumns(4, 6); // Auto-ajustar columnas D-I

  // === CONGELAR FILAS/COLUMNAS ===
  hoja.setFrozenRows(1);     // Congelar primera fila (encabezados)
  hoja.setFrozenColumns(1);  // Congelar primera columna (ID)

  // === FILTROS ===
  hoja.getDataRange().createFilter();

  // === VALIDACIÓN DE DATOS ===
  const reglaEstado = SpreadsheetApp.newDataValidation()
    .requireValueInList(['activo', 'inactivo', 'pendiente', 'cancelado'], true)
    .setAllowInvalid(false)
    .setHelpText('Selecciona un estado válido')
    .build();
  hoja.getRange('H2:H' + ultimaFila).setDataValidation(reglaEstado);

  // Validación de email
  const reglaEmail = SpreadsheetApp.newDataValidation()
    .requireTextContains('@')
    .setAllowInvalid(true)
    .setHelpText('Ingresa un email válido')
    .build();
  hoja.getRange('C2:C' + ultimaFila).setDataValidation(reglaEmail);

  // === FORMATO CONDICIONAL ===
  const reglas = [];

  // Rojo para "cancelado"
  const reglaRojo = SpreadsheetApp.newConditionalFormatRule()
    .whenTextEqualTo('cancelado')
    .setBackground('#FEE2E2')
    .setFontColor('#991B1B')
    .setRanges([hoja.getRange('H2:H' + ultimaFila)])
    .build();
  reglas.push(reglaRojo);

  // Verde para "activo"
  const reglaVerde = SpreadsheetApp.newConditionalFormatRule()
    .whenTextEqualTo('activo')
    .setBackground('#DCFCE7')
    .setFontColor('#166534')
    .setRanges([hoja.getRange('H2:H' + ultimaFila)])
    .build();
  reglas.push(reglaVerde);

  hoja.setConditionalFormatRules(reglas);

  // === PROTEGER HOJA ===
  // const proteccion = hoja.protect().setDescription('Protección de datos');
  // proteccion.setUnprotectedRanges([hoja.getRange('B2:B100')]); // Solo esta editable
}

// Agregar nota a una celda
function agregarNota(nombreHoja, fila, columna, nota) {
  getHoja(nombreHoja).getRange(fila, columna).setNote(nota);
}

// Resaltar celdas con errores
function resaltarCelda(nombreHoja, fila, columna, color = '#FEF9C3') {
  getHoja(nombreHoja).getRange(fila, columna).setBackground(color);
}
```

---

## 4. SINCRONIZACIÓN BIDIRECCIONAL CON LA API

```javascript
// sync.gs — El corazón de la integración

/**
 * SINCRONIZACIÓN COMPLETA: API → Google Sheets
 * Descarga todos los datos de la API y los pone en la hoja
 */
function sincronizarDesdeAPI() {
  const config = getConfig();
  const hoja   = getHoja(config.HOJA_DATOS);

  try {
    // 1. Mostrar indicador de carga
    SpreadsheetApp.getActiveSpreadsheet().toast('⏳ Sincronizando datos...', 'Cargando', 30);

    // 2. Obtener datos de la API (con paginación si hay muchos)
    const todosLosDatos = obtenerTodosLosDatos('/usuarios');

    if (!todosLosDatos || todosLosDatos.length === 0) {
      SpreadsheetApp.getUi().alert('No hay datos para sincronizar');
      return;
    }

    // 3. Limpiar datos existentes (mantener encabezados)
    limpiarDatos(config.HOJA_DATOS);

    // 4. Definir encabezados y orden de columnas
    const COLUMNAS = ['id', 'nombre', 'email', 'rol', 'activo', 'created_at', 'updated_at'];

    // 5. Escribir encabezados si la hoja está vacía
    if (hoja.getLastRow() === 0) {
      hoja.appendRow(COLUMNAS.map(c => c.toUpperCase().replace(/_/g, ' ')));
    }

    // 6. Preparar filas para escritura batch
    const filas = todosLosDatos.map(item =>
      COLUMNAS.map(col => {
        const valor = item[col];
        if (valor === null || valor === undefined) return '';
        if (typeof valor === 'boolean') return valor ? 'activo' : 'inactivo';
        if (typeof valor === 'object') return JSON.stringify(valor);
        return valor;
      })
    );

    // 7. Escribir todos los datos de una vez (eficiente)
    escribirMultiplesFilas(config.HOJA_DATOS, filas, 2);

    // 8. Aplicar formato
    aplicarFormato(config.HOJA_DATOS);

    // 9. Escribir metadatos de sincronización
    const hojaConfig = getHoja(config.HOJA_CONFIG);
    hojaConfig.getRange('B1').setValue(new Date());
    hojaConfig.getRange('B2').setValue(todosLosDatos.length);
    hojaConfig.getRange('B3').setValue('✅ Exitosa');

    // 10. Notificar éxito
    SpreadsheetApp.getActiveSpreadsheet().toast(
      `✅ ${todosLosDatos.length} registros sincronizados`,
      'Sincronización completa',
      5
    );

    Logger.log(`Sincronización exitosa: ${todosLosDatos.length} registros`);
    return { exito: true, total: todosLosDatos.length };

  } catch (error) {
    Logger.log('❌ Error en sincronización: ' + error.message);

    // Registrar error
    const hojaConfig = getHoja(config.HOJA_CONFIG);
    hojaConfig.getRange('B3').setValue('❌ Error: ' + error.message);

    SpreadsheetApp.getActiveSpreadsheet().toast(
      '❌ Error: ' + error.message,
      'Error de sincronización',
      10
    );

    throw error;
  }
}

// Obtener todos los datos manejando paginación automática
function obtenerTodosLosDatos(endpoint) {
  const todosLosDatos = [];
  let pagina  = 1;
  const limite = 100;
  let hayMas  = true;

  while (hayMas) {
    const respuesta = llamarAPI(endpoint, 'GET', null, { pagina, limite });

    if (!respuesta.exito || !respuesta.datos) break;

    todosLosDatos.push(...respuesta.datos);

    const paginacion = respuesta.paginacion;
    hayMas = paginacion && pagina < paginacion.totalPaginas;
    pagina++;

    // Protección contra bucles infinitos
    if (pagina > 100) break;

    // Pequeña pausa para no saturar la API
    if (hayMas) Utilities.sleep(200);
  }

  return todosLosDatos;
}

/**
 * SINCRONIZACIÓN: Google Sheets → API
 * Envía los cambios de la hoja a la API
 */
function enviarCambiosAAPI() {
  const config = getConfig();

  try {
    const datos = leerTodosLosDatos(config.HOJA_DATOS);

    if (datos.length === 0) {
      SpreadsheetApp.getUi().alert('No hay datos en la hoja para enviar');
      return;
    }

    SpreadsheetApp.getActiveSpreadsheet().toast(
      `⏳ Enviando ${datos.length} registros...`,
      'Enviando',
      60
    );

    let exitosos = 0;
    let errores  = 0;
    const erroresDetalle = [];

    datos.forEach((fila, i) => {
      try {
        if (fila.id) {
          // Actualizar existente
          actualizarUsuario(fila.id, fila);
        } else {
          // Crear nuevo
          const nuevo = crearUsuario(fila);
          // Escribir el ID devuelto por la API en la hoja
          escribirCelda(config.HOJA_DATOS, fila._fila, 1, nuevo.datos.id);
        }
        exitosos++;
      } catch (error) {
        errores++;
        erroresDetalle.push(`Fila ${fila._fila}: ${error.message}`);
        resaltarCelda(config.HOJA_DATOS, fila._fila, 1, '#FEE2E2');
        agregarNota(config.HOJA_DATOS, fila._fila, 1, `Error: ${error.message}`);
      }
    });

    const mensaje = `✅ ${exitosos} exitosos, ❌ ${errores} errores`;
    SpreadsheetApp.getActiveSpreadsheet().toast(mensaje, 'Envío completado', 10);

    if (errores > 0) {
      Logger.log('Errores en envío:\n' + erroresDetalle.join('\n'));
    }

    return { exitosos, errores, erroresDetalle };

  } catch (error) {
    Logger.log('❌ Error enviando a API: ' + error.message);
    throw error;
  }
}
```

---

## 5. WEB APP — INTERFAZ HTML EN GOOGLE

### Estructura de archivos en GAS
```
Proyecto GAS/
├── Code.gs           # Lógica del servidor (doGet, doPost, funciones)
├── api_client.gs     # Cliente HTTP para tu API
├── sheets.gs         # Operaciones con Sheets
├── sync.gs           # Lógica de sincronización
├── config.gs         # Configuración
├── Index.html        # HTML principal
├── Estilos.html      # CSS (incluido con HtmlService)
└── Scripts.html      # JS del frontend (incluido con HtmlService)
```

### Code.gs — Servidor de la Web App
```javascript
// Code.gs

// Punto de entrada GET — sirve la interfaz web
function doGet(e) {
  // Parámetros de la URL
  const pagina = e.parameter.pagina || 'dashboard';

  const template = HtmlService.createTemplateFromFile('Index');
  template.pagina = pagina;
  template.titulo = 'Mi Dashboard';

  return template.evaluate()
    .setTitle('Mi Dashboard')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
    .addMetaTag('viewport', 'width=device-width, initial-scale=1');
}

// Punto de entrada POST — para formularios
function doPost(e) {
  try {
    const datos = JSON.parse(e.postData.contents);
    const { accion, payload } = datos;

    // Verificar API Key del cliente
    const apiKeyCliente = e.parameter.key || datos.key;
    if (apiKeyCliente !== PropertiesService.getScriptProperties().getProperty('WEB_APP_KEY')) {
      return respuestaJSON({ exito: false, error: 'No autorizado' }, 401);
    }

    // Router de acciones
    const acciones = {
      'sincronizar':     () => sincronizarDesdeAPI(),
      'crear_usuario':   () => crearUsuario(payload),
      'obtener_datos':   () => leerTodosLosDatos(getConfig().HOJA_DATOS),
      'enviar_cambios':  () => enviarCambiosAAPI(),
    };

    const handler = acciones[accion];
    if (!handler) {
      return respuestaJSON({ exito: false, error: `Acción desconocida: ${accion}` }, 400);
    }

    const resultado = handler();
    return respuestaJSON({ exito: true, datos: resultado });

  } catch (error) {
    Logger.log('Error en doPost: ' + error.message);
    return respuestaJSON({ exito: false, error: error.message }, 500);
  }
}

// Helper para respuestas JSON
function respuestaJSON(datos, statusCode = 200) {
  return ContentService
    .createTextOutput(JSON.stringify(datos))
    .setMimeType(ContentService.MimeType.JSON);
}

// Incluir archivos HTML (para modularizar CSS y JS)
function include(filename) {
  return HtmlService.createHtmlOutputFromFile(filename).getContent();
}

// === FUNCIONES LLAMABLES DESDE EL FRONTEND ===
// Estas funciones se llaman con google.script.run desde el HTML

function obtenerDatosParaDashboard() {
  try {
    const [usuarios, productos, pedidos] = obtenerDatosParalelo();
    return {
      exito: true,
      datos: { usuarios, productos, pedidos },
      timestamp: new Date().toISOString(),
    };
  } catch (error) {
    return { exito: false, error: error.message };
  }
}

function guardarFormulario(datos) {
  try {
    // Validar datos del lado del servidor
    if (!datos.nombre || !datos.email) {
      return { exito: false, error: 'Nombre y email son requeridos' };
    }

    // Guardar en la API
    const resultado = crearUsuario(datos);

    // También guardar en Sheets como backup
    agregarFila(getConfig().HOJA_DATOS, [
      resultado.datos.id,
      datos.nombre,
      datos.email,
      datos.rol || 'usuario',
      new Date(),
    ]);

    return { exito: true, datos: resultado.datos };
  } catch (error) {
    return { exito: false, error: error.message };
  }
}

function ejecutarSincronizacion() {
  try {
    return sincronizarDesdeAPI();
  } catch (error) {
    return { exito: false, error: error.message };
  }
}
```

### Index.html — Frontend completo
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard</title>

  <!-- Incluir CSS desde archivo separado -->
  <?!= include('Estilos'); ?>

  <!-- Tailwind desde CDN (funciona en GAS) -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Chart.js para gráficas -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

  <!-- GSAP para animaciones -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
</head>
<body class="bg-gray-50 min-h-screen">

  <!-- Loading overlay -->
  <div id="loading" class="fixed inset-0 bg-white/90 backdrop-blur-sm z-50 flex items-center justify-center">
    <div class="text-center">
      <div class="w-12 h-12 border-4 border-blue-600 border-t-transparent rounded-full animate-spin mx-auto mb-4"></div>
      <p class="text-gray-600 font-medium" id="loading-mensaje">Cargando datos...</p>
    </div>
  </div>

  <!-- Header -->
  <header class="bg-white border-b border-gray-200 sticky top-0 z-40">
    <div class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
      <h1 class="text-xl font-bold text-gray-900">📊 Mi Dashboard</h1>
      <div class="flex items-center gap-3">
        <span id="ultimo-sync" class="text-sm text-gray-500"></span>
        <button onclick="sincronizar()" id="btn-sync"
          class="px-4 py-2 bg-blue-600 text-white rounded-lg text-sm font-medium hover:bg-blue-700 transition-colors flex items-center gap-2">
          <span id="sync-icon">🔄</span>
          Sincronizar
        </button>
      </div>
    </div>
  </header>

  <!-- Contenido principal -->
  <main class="max-w-7xl mx-auto px-4 py-8">

    <!-- Tarjetas de resumen -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8" id="tarjetas">
      <div class="bg-white rounded-2xl p-6 border border-gray-100 shadow-sm">
        <p class="text-sm text-gray-500 mb-1">Total Usuarios</p>
        <p class="text-3xl font-bold text-gray-900" id="total-usuarios">--</p>
        <p class="text-sm text-green-600 mt-2" id="usuarios-activos"></p>
      </div>
      <div class="bg-white rounded-2xl p-6 border border-gray-100 shadow-sm">
        <p class="text-sm text-gray-500 mb-1">Total Productos</p>
        <p class="text-3xl font-bold text-gray-900" id="total-productos">--</p>
      </div>
      <div class="bg-white rounded-2xl p-6 border border-gray-100 shadow-sm">
        <p class="text-sm text-gray-500 mb-1">Pedidos Pendientes</p>
        <p class="text-3xl font-bold text-orange-500" id="pedidos-pendientes">--</p>
      </div>
      <div class="bg-white rounded-2xl p-6 border border-gray-100 shadow-sm">
        <p class="text-sm text-gray-500 mb-1">Ingresos del Mes</p>
        <p class="text-3xl font-bold text-green-600" id="ingresos-mes">--</p>
      </div>
    </div>

    <!-- Gráfica -->
    <div class="bg-white rounded-2xl p-6 border border-gray-100 shadow-sm mb-8">
      <h2 class="text-lg font-semibold text-gray-900 mb-4">Ventas por Mes</h2>
      <canvas id="grafica-ventas" height="100"></canvas>
    </div>

    <!-- Tabla de datos -->
    <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden">
      <div class="p-6 border-b border-gray-100 flex items-center justify-between">
        <h2 class="text-lg font-semibold text-gray-900">Últimos Registros</h2>
        <input type="text" placeholder="Buscar..." id="busqueda"
          oninput="filtrarTabla(this.value)"
          class="px-3 py-2 border border-gray-200 rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm" id="tabla-datos">
          <thead class="bg-gray-50">
            <tr>
              <th class="text-left px-4 py-3 text-gray-600 font-medium">ID</th>
              <th class="text-left px-4 py-3 text-gray-600 font-medium">Nombre</th>
              <th class="text-left px-4 py-3 text-gray-600 font-medium">Email</th>
              <th class="text-left px-4 py-3 text-gray-600 font-medium">Rol</th>
              <th class="text-left px-4 py-3 text-gray-600 font-medium">Estado</th>
              <th class="text-left px-4 py-3 text-gray-600 font-medium">Fecha</th>
            </tr>
          </thead>
          <tbody id="tbody-datos">
            <tr><td colspan="6" class="text-center py-8 text-gray-400">Cargando...</td></tr>
          </tbody>
        </table>
      </div>
    </div>

  </main>

  <!-- Modal de formulario -->
  <div id="modal" class="fixed inset-0 bg-black/50 z-50 hidden items-center justify-center">
    <div class="bg-white rounded-2xl p-6 w-full max-w-md mx-4 shadow-2xl">
      <h3 class="text-lg font-bold text-gray-900 mb-4">Nuevo Usuario</h3>
      <form id="formulario" onsubmit="enviarFormulario(event)" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Nombre</label>
          <input type="text" id="f-nombre" required
            class="w-full px-3 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
          <input type="email" id="f-email" required
            class="w-full px-3 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
        </div>
        <div class="flex gap-3 pt-2">
          <button type="button" onclick="cerrarModal()"
            class="flex-1 py-2 border border-gray-200 rounded-lg text-gray-700 hover:bg-gray-50 transition-colors">
            Cancelar
          </button>
          <button type="submit"
            class="flex-1 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors font-medium">
            Guardar
          </button>
        </div>
      </form>
    </div>
  </div>

  <!-- Incluir scripts -->
  <?!= include('Scripts'); ?>
</body>
</html>
```

### Scripts.html — JavaScript del frontend
```html
<script>
// scripts.html — JavaScript del cliente de la Web App

let datosGlobales = {};
let graficaVentas = null;

// === INICIALIZACIÓN ===
document.addEventListener('DOMContentLoaded', () => {
  cargarDatos();
});

function cargarDatos() {
  mostrarLoading('Conectando con la API...');

  // Llamar función del servidor GAS
  google.script.run
    .withSuccessHandler(onDatosCargados)
    .withFailureHandler(onError)
    .obtenerDatosParaDashboard();
}

function onDatosCargados(respuesta) {
  ocultarLoading();

  if (!respuesta.exito) {
    mostrarError('Error cargando datos: ' + respuesta.error);
    return;
  }

  datosGlobales = respuesta.datos;

  // Actualizar UI con animaciones
  actualizarTarjetas(respuesta.datos);
  actualizarTabla(respuesta.datos.usuarios?.datos || []);
  crearGrafica(respuesta.datos.pedidos?.datos || []);

  // Mostrar timestamp
  document.getElementById('ultimo-sync').textContent =
    'Última sync: ' + new Date(respuesta.timestamp).toLocaleTimeString('es-ES');

  // Animación de entrada
  gsap.from('.bg-white', {
    y: 20,
    opacity: 0,
    stagger: 0.1,
    duration: 0.5,
    ease: 'power2.out',
  });
}

function actualizarTarjetas(datos) {
  const usuarios  = datos.usuarios?.datos  || [];
  const productos = datos.productos?.datos || [];
  const pedidos   = datos.pedidos?.datos   || [];

  const activos  = usuarios.filter(u => u.activo).length;
  const pendientes = pedidos.filter(p => p.estado === 'pendiente').length;
  const ingresos = pedidos
    .filter(p => p.estado === 'entregado')
    .reduce((sum, p) => sum + parseFloat(p.total || 0), 0);

  // Animar números
  animarNumero('total-usuarios', 0, usuarios.length);
  animarNumero('total-productos', 0, productos.length);
  animarNumero('pedidos-pendientes', 0, pendientes);

  document.getElementById('usuarios-activos').textContent = `${activos} activos`;
  document.getElementById('ingresos-mes').textContent =
    '$' + ingresos.toLocaleString('es-MX', { maximumFractionDigits: 2 });
}

function animarNumero(id, desde, hasta) {
  const el = document.getElementById(id);
  const obj = { valor: desde };
  gsap.to(obj, {
    valor: hasta,
    duration: 1.5,
    ease: 'power2.out',
    onUpdate: () => el.textContent = Math.round(obj.valor).toLocaleString(),
  });
}

function actualizarTabla(usuarios) {
  const tbody = document.getElementById('tbody-datos');

  if (!usuarios || usuarios.length === 0) {
    tbody.innerHTML = `<tr><td colspan="6" class="text-center py-8 text-gray-400">Sin datos</td></tr>`;
    return;
  }

  tbody.innerHTML = usuarios.slice(0, 50).map(u => `
    <tr class="border-t border-gray-50 hover:bg-gray-50 transition-colors">
      <td class="px-4 py-3 text-gray-500 font-mono text-xs">${String(u.id || '').substring(0, 8)}...</td>
      <td class="px-4 py-3 font-medium text-gray-900">${escapeHtml(u.nombre || '')}</td>
      <td class="px-4 py-3 text-gray-600">${escapeHtml(u.email || '')}</td>
      <td class="px-4 py-3">
        <span class="px-2 py-1 rounded-full text-xs font-medium
          ${u.rol === 'admin' ? 'bg-purple-100 text-purple-700' : 'bg-gray-100 text-gray-600'}">
          ${u.rol || 'usuario'}
        </span>
      </td>
      <td class="px-4 py-3">
        <span class="px-2 py-1 rounded-full text-xs font-medium
          ${u.activo ? 'bg-green-100 text-green-700' : 'bg-red-100 text-red-700'}">
          ${u.activo ? 'Activo' : 'Inactivo'}
        </span>
      </td>
      <td class="px-4 py-3 text-gray-500 text-xs">
        ${u.created_at ? new Date(u.created_at).toLocaleDateString('es-ES') : '--'}
      </td>
    </tr>
  `).join('');
}

function crearGrafica(pedidos) {
  const ctx = document.getElementById('grafica-ventas').getContext('2d');

  // Agrupar por mes
  const porMes = {};
  pedidos.filter(p => p.estado === 'entregado').forEach(p => {
    const mes = new Date(p.created_at).toLocaleDateString('es-ES', { month: 'short', year: '2-digit' });
    porMes[mes] = (porMes[mes] || 0) + parseFloat(p.total || 0);
  });

  const labels = Object.keys(porMes);
  const valores = Object.values(porMes);

  if (graficaVentas) graficaVentas.destroy();

  graficaVentas = new Chart(ctx, {
    type: 'line',
    data: {
      labels,
      datasets: [{
        label: 'Ventas ($)',
        data: valores,
        borderColor: '#3B82F6',
        backgroundColor: 'rgba(59, 130, 246, 0.1)',
        borderWidth: 2,
        fill: true,
        tension: 0.4,
        pointBackgroundColor: '#3B82F6',
        pointRadius: 4,
      }],
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: false },
        tooltip: {
          callbacks: {
            label: ctx => '$' + ctx.parsed.y.toLocaleString('es-MX'),
          },
        },
      },
      scales: {
        y: {
          beginAtZero: true,
          ticks: { callback: v => '$' + v.toLocaleString() },
        },
      },
    },
  });
}

// === SINCRONIZACIÓN ===
function sincronizar() {
  const btn = document.getElementById('btn-sync');
  btn.disabled = true;
  btn.innerHTML = '<span class="animate-spin inline-block">🔄</span> Sincronizando...';

  google.script.run
    .withSuccessHandler((resultado) => {
      btn.disabled = false;
      btn.innerHTML = '<span>🔄</span> Sincronizar';
      mostrarToast(`✅ ${resultado.total} registros sincronizados`, 'success');
      setTimeout(cargarDatos, 1000);
    })
    .withFailureHandler((error) => {
      btn.disabled = false;
      btn.innerHTML = '<span>🔄</span> Sincronizar';
      mostrarToast('❌ Error: ' + error.message, 'error');
    })
    .ejecutarSincronizacion();
}

// === FORMULARIO ===
function enviarFormulario(e) {
  e.preventDefault();
  const datos = {
    nombre: document.getElementById('f-nombre').value,
    email:  document.getElementById('f-email').value,
  };

  google.script.run
    .withSuccessHandler((resultado) => {
      if (resultado.exito) {
        cerrarModal();
        mostrarToast('✅ Usuario creado exitosamente', 'success');
        cargarDatos();
      } else {
        mostrarToast('❌ ' + resultado.error, 'error');
      }
    })
    .withFailureHandler(onError)
    .guardarFormulario(datos);
}

// === BÚSQUEDA EN TABLA ===
function filtrarTabla(busqueda) {
  const filas = document.querySelectorAll('#tbody-datos tr');
  const termino = busqueda.toLowerCase();

  filas.forEach(fila => {
    const texto = fila.textContent.toLowerCase();
    fila.style.display = texto.includes(termino) ? '' : 'none';
  });
}

// === MODAL ===
function abrirModal() {
  document.getElementById('modal').classList.remove('hidden');
  document.getElementById('modal').classList.add('flex');
}

function cerrarModal() {
  document.getElementById('modal').classList.add('hidden');
  document.getElementById('modal').classList.remove('flex');
  document.getElementById('formulario').reset();
}

// === UI HELPERS ===
function mostrarLoading(mensaje = 'Cargando...') {
  document.getElementById('loading').style.display = 'flex';
  document.getElementById('loading-mensaje').textContent = mensaje;
}

function ocultarLoading() {
  document.getElementById('loading').style.display = 'none';
}

function mostrarToast(mensaje, tipo = 'info') {
  const colores = {
    success: 'bg-green-500',
    error:   'bg-red-500',
    info:    'bg-blue-500',
  };

  const toast = document.createElement('div');
  toast.className = `fixed bottom-4 right-4 ${colores[tipo]} text-white px-6 py-3 rounded-xl shadow-lg z-50 text-sm font-medium`;
  toast.textContent = mensaje;
  document.body.appendChild(toast);

  gsap.from(toast, { y: 20, opacity: 0, duration: 0.3 });
  setTimeout(() => {
    gsap.to(toast, { y: 20, opacity: 0, duration: 0.3, onComplete: () => toast.remove() });
  }, 4000);
}

function mostrarError(mensaje) {
  mostrarToast(mensaje, 'error');
  console.error(mensaje);
}

function onError(error) {
  ocultarLoading();
  mostrarError('Error del servidor: ' + (error.message || error));
}

// Prevenir XSS al insertar HTML
function escapeHtml(texto) {
  const div = document.createElement('div');
  div.appendChild(document.createTextNode(String(texto)));
  return div.innerHTML;
}
</script>
```

---

## 6. TRIGGERS — AUTOMATIZACIÓN

```javascript
// triggers.gs

// === TRIGGER DE TIEMPO — ejecutar automáticamente ===

// Sincronizar cada hora
function crearTriggerSincronizacion() {
  // Eliminar triggers existentes del mismo tipo
  ScriptApp.getProjectTriggers().forEach(trigger => {
    if (trigger.getHandlerFunction() === 'sincronizarDesdeAPI') {
      ScriptApp.deleteTrigger(trigger);
    }
  });

  // Crear nuevo trigger cada hora
  ScriptApp.newTrigger('sincronizarDesdeAPI')
    .timeBased()
    .everyHours(1)
    .create();

  Logger.log('✅ Trigger de sincronización creado (cada 1 hora)');
}

// Reporte diario a las 8am
function crearTriggerReporteDiario() {
  ScriptApp.newTrigger('enviarReporteDiario')
    .timeBased()
    .everyDays(1)
    .atHour(8)
    .create();
}

// Reporte semanal los lunes
function crearTriggerReporteSemanal() {
  ScriptApp.newTrigger('enviarReporteSemanal')
    .timeBased()
    .onWeekDay(ScriptApp.WeekDay.MONDAY)
    .atHour(9)
    .create();
}

// Trigger mensual (primer día del mes)
function crearTriggerMensual() {
  ScriptApp.newTrigger('procesarCierreMensual')
    .timeBased()
    .onMonthDay(1)
    .atHour(0)
    .create();
}

// === TRIGGER DE EVENTOS EN SPREADSHEET ===

// Al editar una celda específica
function onEdit(e) {
  const hoja  = e.source.getActiveSheet();
  const rango = e.range;

  // Solo actuar si editaron la hoja de datos
  if (hoja.getName() !== getConfig().HOJA_DATOS) return;

  const fila    = rango.getRow();
  const columna = rango.getColumn();

  // Si editaron columna de estado (columna 5)
  if (columna === 5 && fila > 1) {
    const nuevoEstado = rango.getValue();
    const id = hoja.getRange(fila, 1).getValue();

    if (id && nuevoEstado) {
      try {
        actualizarUsuario(id, { estado: nuevoEstado });
        hoja.getRange(fila, columna).setBackground('#DCFCE7'); // Verde = sincronizado
        SpreadsheetApp.getActive().toast('✅ Estado actualizado en la API', '', 3);
      } catch (error) {
        hoja.getRange(fila, columna).setBackground('#FEE2E2'); // Rojo = error
        SpreadsheetApp.getActive().toast('❌ Error: ' + error.message, '', 5);
      }
    }
  }
}

// Al abrir el spreadsheet
function onOpen() {
  // Crear menú personalizado
  const ui = SpreadsheetApp.getUi();
  ui.createMenu('🚀 Mi API')
    .addItem('🔄 Sincronizar desde API', 'sincronizarDesdeAPI')
    .addItem('📤 Enviar cambios a API', 'enviarCambiosAAPI')
    .addSeparator()
    .addItem('⚙️ Configurar', 'mostrarConfiguracion')
    .addItem('📊 Abrir Dashboard', 'abrirDashboard')
    .addSeparator()
    .addItem('🔧 Crear triggers automáticos', 'crearTodosLosTriggers')
    .addToUi();
}

// Crear todos los triggers de una vez
function crearTodosLosTriggers() {
  crearTriggerSincronizacion();
  crearTriggerReporteDiario();

  SpreadsheetApp.getUi().alert(
    '✅ Triggers creados:\n' +
    '• Sincronización: cada hora\n' +
    '• Reporte: diario a las 8am'
  );
}

// Ver triggers activos
function verTriggers() {
  const triggers = ScriptApp.getProjectTriggers();
  const info = triggers.map(t =>
    `• ${t.getHandlerFunction()} — ${t.getEventType()}`
  ).join('\n');

  SpreadsheetApp.getUi().alert(
    `Triggers activos (${triggers.length}):\n${info}`
  );
}

// Eliminar TODOS los triggers (cuidado)
function eliminarTodosTriggers() {
  ScriptApp.getProjectTriggers().forEach(t => ScriptApp.deleteTrigger(t));
  Logger.log('✅ Todos los triggers eliminados');
}
```

---

## 7. PROPERTIES Y CACHE SERVICE

```javascript
// storage.gs

// === PROPERTIES SERVICE — datos persistentes ===
// ScriptProperties  — compartidas entre todos los usuarios del script
// UserProperties    — específicas del usuario actual
// DocumentProperties — específicas del documento actual

// Guardar configuración
function guardarConfiguracion(config) {
  const props = PropertiesService.getScriptProperties();
  props.setProperties({
    'API_URL':   config.apiUrl,
    'API_KEY':   config.apiKey,
    'INTERVALO': config.intervalo.toString(),
  });
}

// Leer configuración
function leerConfiguracion() {
  const props = PropertiesService.getScriptProperties();
  return {
    apiUrl:   props.getProperty('API_URL'),
    apiKey:   props.getProperty('API_KEY'),
    intervalo: parseInt(props.getProperty('INTERVALO') || '60'),
  };
}

// Guardar token JWT
function guardarToken(token, expiracion) {
  PropertiesService.getUserProperties().setProperties({
    'JWT_TOKEN':    token,
    'JWT_EXPIRA':   expiracion.toString(),
  });
}

// Obtener token si no ha expirado
function obtenerToken() {
  const props  = PropertiesService.getUserProperties();
  const token  = props.getProperty('JWT_TOKEN');
  const expira = parseInt(props.getProperty('JWT_EXPIRA') || '0');

  if (!token || Date.now() > expira - 60000) { // 1 minuto de margen
    return null; // Token expirado
  }
  return token;
}

// === CACHE SERVICE — datos temporales ===
// Evita llamar a la API repetidamente para los mismos datos

function obtenerConCache(clave, duracion, generarDatos) {
  const cache = CacheService.getScriptCache();
  const cacheKey = `cache_${clave}`;

  // Intentar desde cache
  const cachedStr = cache.get(cacheKey);
  if (cachedStr) {
    try {
      const cached = JSON.parse(cachedStr);
      Logger.log(`Cache HIT: ${clave}`);
      return cached;
    } catch (e) {
      // Cache corrupto, regenerar
    }
  }

  // Generar datos frescos
  Logger.log(`Cache MISS: ${clave} — consultando API`);
  const datos = generarDatos();

  // Guardar en cache
  try {
    const serializado = JSON.stringify(datos);
    if (serializado.length < 100000) { // Máx 100KB
      cache.put(cacheKey, serializado, duracion);
    }
  } catch (e) {
    Logger.log('Error guardando en cache: ' + e.message);
  }

  return datos;
}

// Uso del cache
function obtenerUsuariosConCache() {
  return obtenerConCache(
    'usuarios_lista',
    300, // 5 minutos
    () => obtenerTodosLosDatos('/usuarios')
  );
}

// Invalidar cache
function invalidarCache(clave) {
  CacheService.getScriptCache().remove(`cache_${clave}`);
}

// Invalidar todo el cache
function invalidarTodoElCache() {
  // El cache expira solo, pero podemos usar claves con versión
  const props = PropertiesService.getScriptProperties();
  const version = parseInt(props.getProperty('CACHE_VERSION') || '0') + 1;
  props.setProperty('CACHE_VERSION', version.toString());
}

// === LOCK SERVICE — evitar condiciones de carrera ===
function ejecutarConLock(funcion) {
  const lock = LockService.getScriptLock();

  try {
    // Esperar hasta 30 segundos para obtener el lock
    lock.waitLock(30000);
    return funcion();
  } catch (e) {
    if (e.message.includes('Could not obtain lock')) {
      throw new Error('Otro proceso está ejecutándose. Intenta en un momento.');
    }
    throw e;
  } finally {
    lock.releaseLock(); // SIEMPRE liberar
  }
}

// Uso: sincronización que no debe ejecutarse en paralelo
function sincronizarSeguro() {
  return ejecutarConLock(() => sincronizarDesdeAPI());
}
```

---

## 8. DESPLIEGUE COMO WEB APP

```javascript
// === PASOS PARA DESPLEGAR ===

/*
1. En el editor de Apps Script:
   Desplegar > Nueva implementación

2. Configuración:
   Tipo: Aplicación web
   Descripción: v1.0 - Dashboard
   Ejecutar como: Yo (mi cuenta)  ← La API se llama con TUS permisos
   Quién puede acceder: 
     - "Solo yo" — para uso personal
     - "Cualquier usuario de Google" — para tu equipo
     - "Cualquiera" — acceso público (sin autenticación de Google)

3. URL del Web App (ejemplo):
   https://script.google.com/macros/s/AKfycb.../exec

4. Para actualizar, hacer nueva implementación o
   Desplegar > Administrar implementaciones > Editar
*/

// === VERIFICACIÓN DE ACCESO EN doGet ===
function doGet(e) {
  // Verificar que sea un usuario autorizado (si acceso = "Cualquiera")
  const usuarioActual = Session.getActiveUser().getEmail();
  const usuariosPermitidos = PropertiesService.getScriptProperties()
    .getProperty('USUARIOS_PERMITIDOS')?.split(',') || [];

  if (usuariosPermitidos.length > 0 && !usuariosPermitidos.includes(usuarioActual)) {
    return HtmlService.createHtmlOutput('<h1>Acceso denegado</h1>');
  }

  return HtmlService.createTemplateFromFile('Index')
    .evaluate()
    .setTitle('Dashboard')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}

// === LLAMAR A LA WEB APP DESDE FUERA ===
// Desde tu API de Node.js u otro sistema
async function llamarWebApp() {
  const WEB_APP_URL = 'https://script.google.com/macros/s/AKfycb.../exec';

  // GET — abrir la interfaz
  const response = await fetch(WEB_APP_URL);

  // POST — enviar datos (si configuraste doPost)
  const responsePost = await fetch(WEB_APP_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      accion: 'actualizar_dato',
      payload: { id: '123', valor: 'nuevo' },
    }),
    redirect: 'follow', // IMPORTANTE: GAS hace redirect al endpoint real
  });
}
```

---

## 9. EMAILS Y NOTIFICACIONES

```javascript
// notifications.gs

// Enviar email simple
function enviarEmailSimple(destinatario, asunto, cuerpo) {
  MailApp.sendEmail(destinatario, asunto, cuerpo);
}

// Email con HTML y adjuntos
function enviarEmailCompleto(opciones) {
  MailApp.sendEmail({
    to:       opciones.para,
    cc:       opciones.cc || '',
    bcc:      opciones.cco || '',
    subject:  opciones.asunto,
    htmlBody: opciones.htmlCuerpo,
    body:     opciones.textoCuerpo, // Fallback texto plano
    name:     'Mi Sistema de Reportes',
    replyTo:  'noreply@miempresa.com',
    attachments: opciones.adjuntos || [],
    noReply:  false,
  });
}

// Reporte automático por email
function enviarReporteDiario() {
  try {
    const datos = llamarAPI('/reportes/diario', 'GET');
    const hoja  = SpreadsheetApp.getActiveSpreadsheet();

    // Crear HTML del reporte
    const html = `
      <!DOCTYPE html>
      <html>
      <body style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
        <div style="background: #1E40AF; color: white; padding: 20px; border-radius: 8px 8px 0 0;">
          <h1 style="margin:0; font-size: 20px;">📊 Reporte Diario</h1>
          <p style="margin:5px 0 0; opacity:0.8;">${new Date().toLocaleDateString('es-ES', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</p>
        </div>
        <div style="background: #f8fafc; padding: 20px; border: 1px solid #e2e8f0;">
          <div style="display:grid; gap:16px;">
            <div style="background:white; padding:16px; border-radius:8px; border:1px solid #e2e8f0;">
              <p style="color:#6b7280; margin:0 0 4px; font-size:14px;">Nuevos usuarios hoy</p>
              <p style="font-size:28px; font-weight:bold; color:#111827; margin:0;">${datos.usuarios_nuevos || 0}</p>
            </div>
            <div style="background:white; padding:16px; border-radius:8px; border:1px solid #e2e8f0;">
              <p style="color:#6b7280; margin:0 0 4px; font-size:14px;">Pedidos del día</p>
              <p style="font-size:28px; font-weight:bold; color:#111827; margin:0;">${datos.pedidos_hoy || 0}</p>
            </div>
            <div style="background:white; padding:16px; border-radius:8px; border:1px solid #e2e8f0;">
              <p style="color:#6b7280; margin:0 0 4px; font-size:14px;">Ingresos del día</p>
              <p style="font-size:28px; font-weight:bold; color:#16a34a; margin:0;">$${(datos.ingresos_hoy || 0).toLocaleString('es-MX')}</p>
            </div>
          </div>
          <p style="text-align:center; color:#9ca3af; font-size:12px; margin-top:20px;">
            Generado automáticamente por Mi Sistema.<br>
            <a href="${hoja.getUrl()}" style="color:#3b82f6;">Ver Spreadsheet</a>
          </p>
        </div>
      </body>
      </html>
    `;

    // Adjuntar la hoja como Excel
    const excel = hoja.getAs('application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
    excel.setName(`Reporte_${Utilities.formatDate(new Date(), 'America/Mexico_City', 'yyyy-MM-dd')}.xlsx`);

    enviarEmailCompleto({
      para:      PropertiesService.getScriptProperties().getProperty('EMAIL_REPORTE'),
      asunto:    `📊 Reporte Diario — ${new Date().toLocaleDateString('es-ES')}`,
      htmlCuerpo: html,
      textoCuerpo: `Reporte del día:\n- Usuarios nuevos: ${datos.usuarios_nuevos}\n- Pedidos: ${datos.pedidos_hoy}\n- Ingresos: $${datos.ingresos_hoy}`,
      adjuntos:  [excel],
    });

    Logger.log('✅ Reporte diario enviado');
  } catch (error) {
    Logger.log('❌ Error enviando reporte: ' + error.message);
  }
}
```

---

## 10. UTILIDADES

```javascript
// utils.gs

// Formatear fecha para mostrar
function formatearFecha(fecha, zona = 'America/Mexico_City') {
  return Utilities.formatDate(
    fecha instanceof Date ? fecha : new Date(fecha),
    zona,
    'dd/MM/yyyy HH:mm'
  );
}

// Generar UUID simple
function generarId() {
  return Utilities.getUuid();
}

// Codificar/decodificar Base64
function codificarBase64(texto) {
  return Utilities.base64Encode(texto);
}

function decodificarBase64(b64) {
  return Utilities.base64Decode(b64, Utilities.Charset.UTF_8);
}

// Hash SHA256
function hashSHA256(texto) {
  return Utilities.computeDigest(Utilities.DigestAlgorithm.SHA_256, texto)
    .map(b => ('0' + (b < 0 ? b + 256 : b).toString(16)).slice(-2))
    .join('');
}

// Comprimir datos grandes para cache
function comprimir(datos) {
  const blob = Utilities.newBlob(JSON.stringify(datos));
  const comprimido = Utilities.gzip(blob);
  return Utilities.base64Encode(comprimido.getBytes());
}

function descomprimir(datosComprimidos) {
  const bytes = Utilities.base64Decode(datosComprimidos);
  const blob = Utilities.newBlob(bytes, 'application/x-gzip');
  const descomprimido = Utilities.ungzip(blob);
  return JSON.parse(descomprimido.getDataAsString());
}

// Pausa (para no saturar APIs)
function esperar(ms) {
  Utilities.sleep(ms);
}

// Convertir array de objetos a CSV
function aCSV(datos, encabezados) {
  const filas = [encabezados];
  datos.forEach(item => {
    filas.push(encabezados.map(enc => {
      const valor = item[enc] ?? '';
      // Escapar comillas en CSV
      return typeof valor === 'string' && valor.includes(',')
        ? `"${valor.replace(/"/g, '""')}"`
        : valor;
    }));
  });
  return filas.map(f => f.join(',')).join('\n');
}

// Mostrar sidebar
function mostrarSidebar() {
  const html = HtmlService.createHtmlOutput('<p>Contenido del sidebar</p>')
    .setTitle('Mi Sidebar')
    .setWidth(300);
  SpreadsheetApp.getUi().showSidebar(html);
}

// Mostrar modal
function mostrarModal(titulo, html) {
  const output = HtmlService.createHtmlOutput(html)
    .setWidth(500)
    .setHeight(400);
  SpreadsheetApp.getUi().showModalDialog(output, titulo);
}

// Mostrar diálogo de confirmación
function confirmar(mensaje) {
  const ui = SpreadsheetApp.getUi();
  const resultado = ui.alert(mensaje, ui.ButtonSet.YES_NO);
  return resultado === ui.Button.YES;
}
```

---

## 11. CHECKLIST DE DESPLIEGUE

```
SEGURIDAD
□ API Keys en PropertiesService (nunca en código)
□ Web App Key para validar requests al doPost
□ Verificar origen de peticiones en doPost
□ CORS manejado por GAS automáticamente
□ No exponer datos sensibles en URLs (usar POST)
□ Revisar permisos del Web App (¿quién puede acceder?)

PERFORMANCE
□ Usar fetchAll para peticiones paralelas
□ Implementar CacheService para datos que no cambian seguido
□ Leer/escribir en batch (getValues/setValues, no celda por celda)
□ Limitar datos a lo necesario (no cargar 10,000 filas si no hace falta)
□ Usar LockService en funciones que no deben ejecutarse en paralelo

CONFIABILIDAD
□ muteHttpExceptions: true en todas las llamadas a UrlFetchApp
□ Manejo de errores en todos los llamados a la API
□ Retry logic para errores temporales (503, timeout)
□ Logs claros con Logger.log para debugging
□ Triggers con manejo de errores (no romper silenciosamente)

WEB APP
□ doGet retorna HtmlOutput válido siempre
□ doPost retorna ContentService.createTextOutput (no HtmlOutput)
□ include() para modularizar CSS y JS
□ google.script.run con withSuccessHandler Y withFailureHandler
□ Validar datos del lado del servidor (no solo en el cliente)
□ Probar en modo incógnito (simula acceso externo)

MANTENIMIENTO
□ Versiones documentadas en las implementaciones
□ .env equivalente con PropertiesService.getScriptProperties()
□ Script de setup inicial (configurarPropiedades, crearTodosLosTriggers)
□ Función para ver el estado actual del sistema
□ Email de alertas si falla la sincronización automática
```

---

*Fin del archivo GOOGLE_APPS_SCRIPT_EXPERTO.md — Conocimiento completo de GAS: UrlFetchApp para consumir APIs externas, SpreadsheetApp, HtmlService para Web Apps, triggers automáticos, PropertiesService, CacheService, emails y despliegue seguro.*
