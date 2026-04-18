# ⚡ ARCHIVO DE CONOCIMIENTO EXPERTO — JAVASCRIPT COMPLETO
> Versión: Máxima. Cubre el 100% de ES2024 + APIs del navegador + Node.js + patrones avanzados.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en JavaScript. Conoces cada característica del lenguaje, API, patrón de diseño, optimización y concepto avanzado existente.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en JavaScript. Conoces:
- El estándar ECMAScript completo (ES2015–ES2024)
- APIs del navegador (DOM, BOM, Web APIs)
- Programación asíncrona (Promises, async/await, observables)
- Patrones de diseño y arquitectura
- Performance y optimización
- Node.js y entornos de servidor
- TypeScript (como superset de JS)
- Testing, debugging y herramientas
- Seguridad en JavaScript
- Web Workers, Service Workers, PWA

---

## 1. TIPOS DE DATOS Y VARIABLES

### Declaración de variables
```javascript
// var — función-scoped, hoisting completo (evitar)
var x = 1;

// let — bloque-scoped, hoisting sin inicialización (TDZ)
let contador = 0;

// const — bloque-scoped, no reasignable (preferir siempre que sea posible)
const PI = 3.14159;
const OBJETO = { nombre: 'Juan' }; // La referencia es constante, el contenido puede cambiar
OBJETO.nombre = 'Pedro'; // ✅ válido
// OBJETO = {}; // ❌ TypeError

// Temporal Dead Zone (TDZ)
// console.log(y); // ReferenceError — TDZ
let y = 10;
```

### Tipos de datos
```javascript
// Primitivos (inmutables, copiados por valor)
typeof undefined   // "undefined"
typeof null        // "object" (bug histórico de JS)
typeof true        // "boolean"
typeof 42          // "number"
typeof 42n         // "bigint"
typeof "texto"     // "string"
typeof Symbol()    // "symbol"
typeof function(){} // "function"

// Objeto (mutable, copiado por referencia)
typeof {}          // "object"
typeof []          // "object"
Array.isArray([])  // true

// Number — detalles
Number.MAX_SAFE_INTEGER  // 9007199254740991 (2^53 - 1)
Number.MIN_SAFE_INTEGER  // -9007199254740991
Number.MAX_VALUE         // 1.7976931348623157e+308
Number.EPSILON           // 2.220446049250313e-16
Number.isFinite(Infinity) // false
Number.isNaN(NaN)        // true (más seguro que isNaN global)
Number.isInteger(3.0)    // true
Number.isSafeInteger(9007199254740992) // false

// Evitar la coma flotante
0.1 + 0.2 === 0.3         // false
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON // true
parseFloat((0.1 + 0.2).toFixed(10))  // mejor

// BigInt
const enorme = 9007199254740992n;
const sumaBI = enorme + 1n;
typeof enorme // "bigint"
// No se puede mezclar con Number sin conversión explícita

// String — inmutable
const str = "Hola mundo";
str.length          // 10
str[0]              // "H"
str.at(-1)          // "o" (índice negativo)
str.charAt(0)       // "H"
str.charCodeAt(0)   // 72
str.codePointAt(0)  // 72

// Symbol — identificadores únicos
const sym1 = Symbol('descripción');
const sym2 = Symbol('descripción');
sym1 === sym2  // false — siempre único
Symbol.for('global') === Symbol.for('global') // true — registro global
Symbol.keyFor(Symbol.for('clave')) // 'clave'

// Símbolos bien conocidos
Symbol.iterator      // Define iteradores personalizados
Symbol.toPrimitive   // Conversión a primitivo
Symbol.hasInstance   // instanceof personalizado
Symbol.isConcatSpreadable
Symbol.species
Symbol.asyncIterator
Symbol.toStringTag
```

---

## 2. OPERADORES — REFERENCIA COMPLETA

```javascript
// === ARITMÉTICOS ===
5 + 2   // 7
5 - 2   // 3
5 * 2   // 10
5 / 2   // 2.5
5 % 2   // 1 (módulo/resto)
5 ** 2  // 25 (exponenciación)
++x     // pre-incremento
x++     // post-incremento

// === ASIGNACIÓN ===
x = 5
x += 3   // x = x + 3
x -= 3   // x = x - 3
x *= 3   // x = x * 3
x /= 3   // x = x / 3
x %= 3   // x = x % 3
x **= 3  // x = x ** 3
x &&= valor // x = x && valor (asigna si x es truthy)
x ||= valor // x = x || valor (asigna si x es falsy)
x ??= valor // x = x ?? valor (asigna si x es null/undefined)

// === COMPARACIÓN ===
5 == "5"   // true (igualdad débil — coerción de tipos)
5 === "5"  // false (igualdad estricta — preferir siempre)
5 != "5"   // false
5 !== "5"  // true
5 > 3      // true
5 >= 5     // true
5 < 3      // false
5 <= 5     // true

// === LÓGICOS ===
true && false  // false (AND)
true || false  // true (OR)
!true          // false (NOT)

// Short-circuit evaluation
null && fn()   // null — fn() nunca se ejecuta
null || 'default' // 'default'
undefined ?? 'default' // 'default' (nullish coalescing — solo null/undefined)
false ?? 'default'     // false — ?? no considera falsy

// Optional chaining
obj?.propiedad              // undefined si obj es null/undefined
obj?.metodo?.()             // undefined si no existe
arr?.[0]                    // undefined si arr es null/undefined
obj?.a?.b?.c?.metodo?.()    // Cadena profunda segura

// === BITWISE ===
5 & 3   // 1 (AND bit a bit)
5 | 3   // 7 (OR bit a bit)
5 ^ 3   // 6 (XOR)
~5      // -6 (NOT)
5 << 1  // 10 (left shift)
5 >> 1  // 2 (right shift con signo)
5 >>> 1 // 2 (right shift sin signo)

// === TERNARIO ===
const resultado = condición ? valorSiTrue : valorSiFalse;

// === TYPEOF / INSTANCEOF ===
typeof x === 'string'
[] instanceof Array       // true
[] instanceof Object      // true (porque Array hereda de Object)
Object.prototype.toString.call([]) // "[object Array]"

// === IN / DELETE ===
'nombre' in objeto       // true si tiene la propiedad
delete objeto.propiedad  // Elimina la propiedad

// === COMMA ===
const z = (1, 2, 3); // z = 3 (evalúa todos, retorna el último)

// === VOID ===
void 0          // undefined
void expresión  // undefined

// === SPREAD Y REST ===
const arr = [1, 2, 3];
const copia = [...arr];           // Spread
const arr2 = [...arr, 4, 5];      // Merge
const obj2 = { ...obj1, nuevo: 1 }; // Object spread
function fn(...args) { }          // Rest parameters

// === DESTRUCTURING ===
const [a, b, ...resto] = [1, 2, 3, 4, 5];
const { nombre, edad = 18, ...otros } = objeto;
const { a: { b: { c } } } = obj; // Destructuring anidado
```

---

## 3. STRINGS — API COMPLETA

```javascript
// === CREACIÓN ===
const s1 = 'simple';
const s2 = "doble";
const s3 = `template literal ${expresión}`;
const s4 = String.raw`C:\nueva\ruta`; // Sin procesar escapes

// Template literals avanzados
const html = `
  <div class="${clase}">
    ${array.map(item => `<li>${item}</li>`).join('')}
  </div>
`;

// Tagged templates
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) =>
    `${result}${str}${values[i] ? `<mark>${values[i]}</mark>` : ''}`, '');
}
const texto = highlight`Hola ${'mundo'}, hoy es ${'lunes'}`;

// === MÉTODOS ESENCIALES ===
str.length                    // Longitud
str.at(index)                 // Acceso por índice (acepta negativos)
str.charAt(index)             // Carácter en posición
str.charCodeAt(index)         // Código UTF-16
str.codePointAt(index)        // Código Unicode completo

// Búsqueda
str.indexOf('sub')            // Primera ocurrencia o -1
str.lastIndexOf('sub')        // Última ocurrencia o -1
str.includes('sub')           // boolean
str.startsWith('pre', pos)    // boolean
str.endsWith('suf', length)   // boolean
str.search(/regex/)           // Índice de coincidencia o -1

// Extracción
str.slice(inicio, fin)        // Acepta negativos
str.substring(inicio, fin)    // No acepta negativos
str.substr(inicio, longitud)  // Deprecado — evitar

// Modificación (retornan nuevo string — son inmutables)
str.toUpperCase()
str.toLowerCase()
str.trim()           // Elimina espacios al inicio y fin
str.trimStart()      // Solo inicio
str.trimEnd()        // Solo fin
str.replace('buscar', 'reemplazar')
str.replace(/regex/g, 'reemplazar')
str.replace(/regex/g, (match, ...groups) => procesarMatch(match))
str.replaceAll('buscar', 'reemplazar')  // Sin flag /g
str.normalize('NFC')  // Normalización Unicode

// División y unión
str.split('')         // Array de caracteres
str.split(' ')        // Array de palabras
str.split(/\s+/)      // Por whitespace
str.split(',', 3)     // Máximo 3 elementos
['a', 'b'].join(', ') // "a, b"

// Padding y repetición
str.padStart(10)          // Rellena inicio con espacios
str.padStart(10, '0')     // "00000hola"
str.padEnd(10, '-')       // "hola------"
str.repeat(3)             // "hola hola hola"

// Coincidencias
str.match(/regex/)        // Array de coincidencias o null
str.match(/regex/g)       // Todas las coincidencias
str.matchAll(/regex/g)    // Iterador de todas (con grupos)
for (const match of str.matchAll(/(\d+)/g)) {
  console.log(match[0], match[1], match.index);
}

// === REGEX AVANZADO ===
const regex = /patron/flags;
// Flags: g (global), i (case-insensitive), m (multiline), s (dotAll), u (unicode), v (unicodeSets), d (índices), y (sticky)

// Grupos de captura
const match = '2024-01-15'.match(/(?<año>\d{4})-(?<mes>\d{2})-(?<dia>\d{2})/);
match.groups.año   // "2024"
match.groups.mes   // "01"
match.groups.dia   // "15"

// Lookahead y lookbehind
/foo(?=bar)/   // foo seguido de bar (positivo)
/foo(?!bar)/   // foo NO seguido de bar (negativo)
/(?<=foo)bar/  // bar precedido de foo (positivo)
/(?<!foo)bar/  // bar NO precedido de foo (negativo)

// Métodos del objeto RegExp
regex.test(str)     // boolean
regex.exec(str)     // Array con info o null
regex.source        // El patrón como string
regex.flags         // Los flags
regex.lastIndex     // Para regex con /g
```

---

## 4. ARRAYS — API COMPLETA

```javascript
// === CREACIÓN ===
const arr = [1, 2, 3];
const arr2 = new Array(3);          // [empty × 3]
const arr3 = new Array(1, 2, 3);    // [1, 2, 3]
const arr4 = Array.from({length: 5}, (_, i) => i); // [0, 1, 2, 3, 4]
const arr5 = Array.from('hola');    // ['h', 'o', 'l', 'a']
const arr6 = Array.from(Set);       // De cualquier iterable
Array.of(1, 2, 3);                  // [1, 2, 3]
[...new Array(5)].map((_, i) => i); // [0, 1, 2, 3, 4]

// === MÉTODOS QUE MUTAN EL ARRAY ORIGINAL ===
arr.push(4, 5)         // Agrega al final, retorna nueva longitud
arr.pop()              // Elimina del final, retorna el elemento
arr.unshift(0)         // Agrega al inicio, retorna nueva longitud
arr.shift()            // Elimina del inicio, retorna el elemento
arr.splice(1, 2, 'a', 'b') // Elimina 2 desde índice 1, inserta 'a','b'
arr.reverse()          // Invierte in-place
arr.sort()             // Ordena in-place (lexicográfico por defecto!)
arr.sort((a, b) => a - b)  // Numérico ascendente
arr.sort((a, b) => b - a)  // Numérico descendente
arr.sort((a, b) => a.nombre.localeCompare(b.nombre)) // Por string
arr.fill(0)            // Rellena todo con 0
arr.fill(0, 2, 5)      // Rellena del índice 2 al 4 con 0
arr.copyWithin(0, 3)   // Copia elementos dentro del mismo array

// === MÉTODOS QUE NO MUTAN (INMUTABLES) ===
// Retornan nuevo array:
arr.map(fn)            // Transforma cada elemento
arr.filter(fn)         // Filtra por condición
arr.slice(1, 3)        // Extrae subarray [inicio, fin)
arr.concat([4,5], 6)   // Combina arrays
arr.flat(depth)        // Aplana (depth = Infinity para aplanar todo)
arr.flatMap(fn)        // map + flat(1) — más eficiente
arr.toReversed()       // Reverso sin mutar (ES2023)
arr.toSorted(fn)       // Sort sin mutar (ES2023)
arr.toSpliced(1, 2, 'x') // Splice sin mutar (ES2023)
arr.with(2, 'nuevo')   // Reemplaza índice sin mutar (ES2023)

// Búsqueda:
arr.indexOf(val)       // Índice o -1 (usa ===)
arr.lastIndexOf(val)   // Último índice o -1
arr.find(fn)           // Primer elemento que cumple condición
arr.findIndex(fn)      // Índice del primero o -1
arr.findLast(fn)       // Último elemento que cumple
arr.findLastIndex(fn)  // Índice del último o -1
arr.includes(val)      // boolean (maneja NaN correctamente)

// Reducción:
arr.reduce((acc, curr, idx, arr) => acc + curr, 0) // Acumula de izq a der
arr.reduceRight((acc, curr) => acc + curr, 0)       // De der a izq

// Testing:
arr.every(fn)          // true si TODOS cumplen
arr.some(fn)           // true si AL MENOS UNO cumple

// Iteración:
arr.forEach(fn)        // No retorna nada, no es encadenable
arr.keys()             // Iterador de índices
arr.values()           // Iterador de valores
arr.entries()          // Iterador de [índice, valor]

// Conversión:
arr.join(', ')         // String
arr.toString()         // "1,2,3"

// === ARRAY ESTÁTICO ===
Array.isArray(x)       // true si es array
Array.from(iterable, mapFn) // De iterable con transformación
Array.of(...items)     // De valores individuales

// === PATRONES AVANZADOS ===

// Deduplicar
const unico = [...new Set(arr)];
const unico2 = arr.filter((v, i, a) => a.indexOf(v) === i);

// Agrupar (ES2024)
const agrupado = arr.group(item => item.categoria);
// { categoria1: [...], categoria2: [...] }

// Intersección, unión, diferencia
const union = [...new Set([...a, ...b])];
const interseccion = a.filter(x => b.includes(x));
const diferencia = a.filter(x => !b.includes(x));

// Chunk (dividir en grupos)
const chunk = (arr, size) =>
  Array.from({length: Math.ceil(arr.length / size)}, (_, i) =>
    arr.slice(i * size, i * size + size));

// Zip
const zip = (...arrs) =>
  Array.from({length: Math.min(...arrs.map(a => a.length))},
    (_, i) => arrs.map(a => a[i]));

// Shuffle (Fisher-Yates)
const shuffle = arr => {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
};

// Flatten profundo
const flatten = arr => arr.reduce((acc, val) =>
  Array.isArray(val) ? acc.concat(flatten(val)) : [...acc, val], []);

// Typed Arrays (para performance)
const bytes = new Uint8Array(256);
const floats = new Float64Array(100);
new Int32Array(10);
new Uint16Array([0, 255, 65535]);
```

---

## 5. OBJETOS — API COMPLETA

```javascript
// === CREACIÓN ===
const obj = { nombre: 'Juan', edad: 30 };
const obj2 = new Object();
const obj3 = Object.create(proto);  // Con prototipo específico
const obj4 = Object.create(null);   // Sin prototipo (objeto puro)

// Shorthand de propiedades
const nombre = 'Juan';
const persona = { nombre, edad: 30 };  // { nombre: 'Juan', edad: 30 }

// Computed properties
const clave = 'dinamica';
const obj5 = { [clave]: 'valor', [`prefijo_${clave}`]: 'otro' };

// Métodos shorthand
const obj6 = {
  metodo() { return this; },
  get valor() { return this._valor; },
  set valor(v) { this._valor = v; },
  async metodoAsync() { },
  *generador() { },
  [Symbol.iterator]() { },
};

// === MÉTODOS DE Object ===

// Propiedades
Object.keys(obj)          // Array de claves propias enumerables
Object.values(obj)        // Array de valores propios enumerables
Object.entries(obj)       // Array de [clave, valor] propios enumerables

// Construcción
Object.assign(destino, ...fuentes)   // Copia superficial
Object.assign({}, obj1, obj2)        // Merge sin mutar
const copia = { ...obj };            // Spread (más legible)

Object.fromEntries(entries)          // De array de [clave, valor] a objeto
Object.fromEntries(map)              // De Map a objeto
Object.fromEntries(Object.entries(obj).map(([k, v]) => [k, v * 2]));

// Descriptores de propiedades
Object.defineProperty(obj, 'prop', {
  value: 42,
  writable: false,      // No se puede reasignar
  enumerable: true,     // Aparece en for...in y Object.keys
  configurable: false,  // No se puede redefinir ni eliminar
});

Object.defineProperties(obj, {
  nombre: { value: 'Juan', writable: true, enumerable: true, configurable: true },
  edad:   { value: 30,     writable: true, enumerable: true, configurable: true },
});

Object.getOwnPropertyDescriptor(obj, 'prop')
Object.getOwnPropertyDescriptors(obj)  // Todos los descriptores

// Propiedades
Object.getOwnPropertyNames(obj)         // Todas las claves propias (incluyendo no enumerables)
Object.getOwnPropertySymbols(obj)       // Símbolos propios
Object.prototype.hasOwnProperty.call(obj, 'clave')
Object.hasOwn(obj, 'clave')             // ES2022 — más seguro

// Prototipos
Object.getPrototypeOf(obj)
Object.setPrototypeOf(obj, proto)       // Evitar — lento
obj instanceof Constructor

// Inmutabilidad
Object.freeze(obj)          // Congela — no se puede modificar, agregar ni eliminar props
Object.isFrozen(obj)
Object.seal(obj)            // Sella — se pueden modificar pero no agregar/eliminar
Object.isSealed(obj)
Object.preventExtensions(obj) // No se pueden agregar propiedades
Object.isExtensible(obj)

// === SPREAD Y MERGE ===
const merged = { ...defaults, ...overrides }; // Merge superficial
const deepMerge = (obj1, obj2) => ({
  ...obj1,
  ...obj2,
  ...(Object.keys(obj1).reduce((acc, key) => {
    if (typeof obj1[key] === 'object' && typeof obj2[key] === 'object') {
      acc[key] = deepMerge(obj1[key], obj2[key]);
    }
    return acc;
  }, {}))
});

// === PROXIES ===
const handler = {
  get(target, prop, receiver) {
    return prop in target ? Reflect.get(target, prop, receiver) : 'Propiedad no encontrada';
  },
  set(target, prop, value, receiver) {
    if (typeof value !== 'number') throw new TypeError('Solo números');
    return Reflect.set(target, prop, value, receiver);
  },
  has(target, prop) {
    return prop in target;
  },
  deleteProperty(target, prop) {
    console.log(`Eliminando ${prop}`);
    return Reflect.deleteProperty(target, prop);
  },
  ownKeys(target) {
    return Reflect.ownKeys(target).filter(k => !k.startsWith('_'));
  },
  apply(target, thisArg, args) { /* Para funciones */ },
  construct(target, args, newTarget) { /* Para constructores */ },
};

const proxy = new Proxy(target, handler);

// Proxy para observabilidad (como Vue 3 reactivity)
function observable(obj, onChange) {
  return new Proxy(obj, {
    set(target, prop, value) {
      const old = target[prop];
      target[prop] = value;
      if (old !== value) onChange(prop, value, old);
      return true;
    }
  });
}

// === REFLECT ===
Reflect.get(obj, 'prop')
Reflect.set(obj, 'prop', valor)
Reflect.has(obj, 'prop')
Reflect.deleteProperty(obj, 'prop')
Reflect.ownKeys(obj)
Reflect.apply(fn, thisArg, args)
Reflect.construct(Constructor, args, NewTarget)
Reflect.defineProperty(obj, 'prop', descriptor)
Reflect.getOwnPropertyDescriptor(obj, 'prop')
Reflect.getPrototypeOf(obj)
Reflect.setPrototypeOf(obj, proto)
Reflect.isExtensible(obj)
Reflect.preventExtensions(obj)
```

---

## 6. FUNCIONES — PROFUNDIDAD TOTAL

```javascript
// === TIPOS DE FUNCIONES ===

// Declaración (hoisting completo)
function suma(a, b) { return a + b; }

// Expresión (no tiene hoisting del valor)
const suma = function(a, b) { return a + b; };
const suma = function funcionNombrada(a, b) { return a + b; }; // Útil para recursión

// Arrow function (no tiene this propio, no tiene arguments, no es constructor)
const suma = (a, b) => a + b;
const cuadrado = x => x * x;
const sinArgs = () => 42;
const retornaObjeto = (x) => ({ valor: x }); // Paréntesis para objetos

// Generadora
function* generador() {
  yield 1;
  yield 2;
  yield 3;
}
const gen = generador();
gen.next()  // { value: 1, done: false }
gen.next()  // { value: 2, done: false }
gen.next()  // { value: 3, done: false }
gen.next()  // { value: undefined, done: true }
[...generador()] // [1, 2, 3]

// Generadora asíncrona
async function* generadorAsync() {
  yield await fetch('/api/1').then(r => r.json());
  yield await fetch('/api/2').then(r => r.json());
}
for await (const dato of generadorAsync()) {
  console.log(dato);
}

// Async
async function fn() {
  const resultado = await promesa;
  return resultado;
}

// === PARÁMETROS AVANZADOS ===
function fn(
  a,                        // Parámetro normal
  b = 10,                   // Valor por defecto
  c = b * 2,                // Default que usa otro parámetro
  { nombre, edad = 18 } = {}, // Destructuring con default
  [primero, ...resto] = [],  // Array destructuring
  ...args                   // Rest — siempre el último
) { }

// Argumento obligatorio con throw
const requerido = () => { throw new Error('Parámetro requerido'); };
function fn(a = requerido()) { }

// === THIS Y CONTEXTO ===

// this en diferentes contextos
function normal() {
  console.log(this); // Depende de cómo se llama
}
const arrow = () => {
  console.log(this); // Hereda this del scope léxico
};

// bind, call, apply
function greet(saludo, punct) {
  return `${saludo}, ${this.nombre}${punct}`;
}
const persona = { nombre: 'Juan' };
greet.call(persona, 'Hola', '!');    // Llamada inmediata con args individuales
greet.apply(persona, ['Hola', '!']); // Llamada inmediata con array de args
const greetJuan = greet.bind(persona, 'Hola'); // Retorna nueva función
greetJuan('!');

// === CLOSURES ===
function crearContador(inicio = 0) {
  let count = inicio;
  return {
    incrementar: () => ++count,
    decrementar: () => --count,
    valor: () => count,
    reset: () => { count = inicio; },
  };
}
const contador = crearContador(10);
contador.incrementar(); // 11

// Closure para módulo privado (Module Pattern)
const Banco = (() => {
  let saldo = 0; // Variable privada

  return {
    depositar(cantidad) { saldo += cantidad; },
    retirar(cantidad) {
      if (cantidad > saldo) throw new Error('Saldo insuficiente');
      saldo -= cantidad;
    },
    verSaldo() { return saldo; },
  };
})();

// === FUNCIONES DE ORDEN SUPERIOR ===

// Curry
const curry = fn => {
  const curried = (...args) =>
    args.length >= fn.length
      ? fn(...args)
      : (...moreArgs) => curried(...args, ...moreArgs);
  return curried;
};

const sumar = curry((a, b, c) => a + b + c);
sumar(1)(2)(3) // 6
sumar(1, 2)(3) // 6

// Compose (de derecha a izquierda)
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);
const pipe    = (...fns) => x => fns.reduce((acc, fn) => fn(acc), x);

const procesarTexto = pipe(
  str => str.trim(),
  str => str.toLowerCase(),
  str => str.replace(/\s+/g, '-'),
);

// Memoización
const memo = fn => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const resultado = fn(...args);
    cache.set(key, resultado);
    return resultado;
  };
};

const fibonacci = memo(n =>
  n <= 1 ? n : fibonacci(n - 1) + fibonacci(n - 2)
);

// Throttle
const throttle = (fn, delay) => {
  let ultimo = 0;
  return (...args) => {
    const ahora = Date.now();
    if (ahora - ultimo >= delay) {
      ultimo = ahora;
      return fn(...args);
    }
  };
};

// Debounce
const debounce = (fn, delay) => {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
};

// Debounce con retorno inmediato en primera llamada
const debounceLeadingTrailing = (fn, delay) => {
  let timer;
  let llamado = false;
  return (...args) => {
    if (!llamado) { fn(...args); llamado = true; }
    clearTimeout(timer);
    timer = setTimeout(() => {
      llamado = false;
    }, delay);
  };
};

// Once — ejecutar solo una vez
const once = fn => {
  let ejecutado = false;
  let resultado;
  return (...args) => {
    if (!ejecutado) {
      resultado = fn(...args);
      ejecutado = true;
    }
    return resultado;
  };
};

// === IIFE ===
(function() { /* código privado */ })();
(() => { /* código privado */ })();
(async () => { /* async IIFE */ })();
```

---

## 7. CLASES Y PROGRAMACIÓN ORIENTADA A OBJETOS

```javascript
// === CLASE BASE ===
class Animal {
  // Campo de clase (instancia)
  nombre;
  #sonido;      // Campo privado — solo accesible dentro de la clase
  #vivo = true;

  // Campo estático
  static contador = 0;
  static #instancias = [];

  // Constructor
  constructor(nombre, sonido) {
    this.nombre = nombre;
    this.#sonido = sonido;
    Animal.contador++;
    Animal.#instancias.push(this);
  }

  // Método de instancia
  hablar() {
    return `${this.nombre} dice: ${this.#sonido}`;
  }

  // Método privado
  #validar() {
    return this.#vivo;
  }

  // Getters y setters
  get vivo() { return this.#vivo; }
  set vivo(valor) {
    if (typeof valor !== 'boolean') throw new TypeError('Debe ser boolean');
    this.#vivo = valor;
  }

  // Método estático
  static crearPerro(nombre) {
    return new Animal(nombre, 'Guau');
  }

  static get totalAnimales() {
    return Animal.contador;
  }

  // Symbol.toPrimitive — conversión a primitivo
  [Symbol.toPrimitive](hint) {
    if (hint === 'number') return Animal.contador;
    if (hint === 'string') return this.nombre;
    return true;
  }

  // Representación como string
  toString() {
    return `Animal(${this.nombre})`;
  }

  // Para JSON.stringify
  toJSON() {
    return { nombre: this.nombre, vivo: this.#vivo };
  }

  // Iterador personalizado
  [Symbol.iterator]() {
    const propiedades = [this.nombre, this.#sonido];
    let index = 0;
    return {
      next() {
        return index < propiedades.length
          ? { value: propiedades[index++], done: false }
          : { value: undefined, done: true };
      }
    };
  }
}

// === HERENCIA ===
class Perro extends Animal {
  #raza;

  constructor(nombre, raza) {
    super(nombre, 'Guau'); // Siempre llamar super() primero
    this.#raza = raza;
  }

  // Override
  hablar() {
    return `${super.hablar()} (${this.#raza})`;
  }

  get raza() { return this.#raza; }

  // Verificar con instanceof
  static esPerro(obj) {
    return obj instanceof Perro;
  }
}

// Herencia con Symbol.hasInstance
class EsNumero {
  static [Symbol.hasInstance](obj) {
    return typeof obj === 'number';
  }
}
42 instanceof EsNumero  // true

// === MIXINS ===
const Serializable = (SuperClass) => class extends SuperClass {
  serialize() {
    return JSON.stringify(this.toJSON());
  }
  static deserialize(str) {
    return Object.assign(new this(), JSON.parse(str));
  }
};

const Validatable = (SuperClass) => class extends SuperClass {
  validate() {
    return Object.entries(this.constructor.schema || {}).every(([campo, regla]) =>
      regla(this[campo])
    );
  }
};

class PersonaSerial extends Serializable(Validatable(Animal)) {
  static schema = {
    nombre: v => typeof v === 'string' && v.length > 0,
  };
}

// === PATRONES AVANZADOS ===

// Singleton
class Singleton {
  static #instancia = null;

  constructor() {
    if (Singleton.#instancia) return Singleton.#instancia;
    Singleton.#instancia = this;
  }

  static getInstance() {
    if (!Singleton.#instancia) new Singleton();
    return Singleton.#instancia;
  }
}

// Observer Pattern
class EventEmitter {
  #listeners = new Map();

  on(evento, fn) {
    if (!this.#listeners.has(evento)) this.#listeners.set(evento, new Set());
    this.#listeners.get(evento).add(fn);
    return () => this.off(evento, fn); // Retorna función para desuscribir
  }

  off(evento, fn) {
    this.#listeners.get(evento)?.delete(fn);
  }

  once(evento, fn) {
    const wrapper = (...args) => {
      fn(...args);
      this.off(evento, wrapper);
    };
    return this.on(evento, wrapper);
  }

  emit(evento, ...args) {
    this.#listeners.get(evento)?.forEach(fn => fn(...args));
  }
}

// Factory Pattern
class ComponenteFactory {
  static #componentes = new Map();

  static registrar(tipo, ClaseComponente) {
    this.#componentes.set(tipo, ClaseComponente);
  }

  static crear(tipo, ...args) {
    const Clase = this.#componentes.get(tipo);
    if (!Clase) throw new Error(`Componente ${tipo} no registrado`);
    return new Clase(...args);
  }
}
```

---

## 8. PROGRAMACIÓN ASÍNCRONA

```javascript
// === CALLBACKS (legado) ===
function cargarDatos(url, callback) {
  const xhr = new XMLHttpRequest();
  xhr.onload = () => callback(null, xhr.response);
  xhr.onerror = () => callback(new Error('Error de red'));
  xhr.open('GET', url);
  xhr.send();
}

// === PROMISES ===
// Creación
const promesa = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Éxito'), 1000);
  // o reject(new Error('Fallo'));
});

// Consumir
promesa
  .then(resultado => { /* éxito */ return otroValor; })
  .catch(error => { /* manejo de error */ })
  .finally(() => { /* siempre ejecuta — limpieza */ });

// Métodos estáticos de Promise
Promise.resolve(valor)   // Promesa resuelta inmediatamente
Promise.reject(error)    // Promesa rechazada inmediatamente

// Paralelismo
Promise.all([p1, p2, p3])
  // Resuelve cuando TODAS resuelven
  // Rechaza si ALGUNA rechaza
  .then(([r1, r2, r3]) => { });

Promise.allSettled([p1, p2, p3])
  // Espera a TODAS sin importar resultado
  .then(resultados => {
    resultados.forEach(({ status, value, reason }) => {
      if (status === 'fulfilled') console.log(value);
      else console.error(reason);
    });
  });

Promise.race([p1, p2, p3])
  // La primera en resolver/rechazar

Promise.any([p1, p2, p3])
  // La primera en RESOLVER (ignora rechazos hasta que todas fallen)
  .catch(e => e instanceof AggregateError); // Si todas fallan

// Promisify
function promisify(fn) {
  return (...args) => new Promise((resolve, reject) => {
    fn(...args, (err, result) => {
      if (err) reject(err);
      else resolve(result);
    });
  });
}

// === ASYNC / AWAIT ===
async function fetchUsuario(id) {
  try {
    const respuesta = await fetch(`/api/usuarios/${id}`);
    if (!respuesta.ok) throw new Error(`HTTP ${respuesta.status}`);
    const usuario = await respuesta.json();
    return usuario;
  } catch (error) {
    console.error('Error:', error);
    throw error; // Re-lanzar si el llamador debe manejarlo
  } finally {
    console.log('Petición completada');
  }
}

// Paralelo con async/await
async function cargarTodo() {
  // ❌ Secuencial — lento
  const a = await fetch('/api/a').then(r => r.json());
  const b = await fetch('/api/b').then(r => r.json());

  // ✅ Paralelo
  const [a, b] = await Promise.all([
    fetch('/api/a').then(r => r.json()),
    fetch('/api/b').then(r => r.json()),
  ]);
}

// Async con arrays
async function procesarLista(items) {
  // Paralelo
  const resultados = await Promise.all(items.map(item => procesar(item)));

  // Secuencial (cuando el orden importa o hay dependencias)
  const resultados2 = [];
  for (const item of items) {
    resultados2.push(await procesar(item));
  }
}

// Top-level await (en módulos ES)
const config = await fetch('/config.json').then(r => r.json());

// === FETCH API COMPLETA ===
const respuesta = await fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`,
    'Accept': 'application/json',
  },
  body: JSON.stringify(datos),
  mode: 'cors | no-cors | same-origin | navigate',
  credentials: 'omit | same-origin | include',
  cache: 'default | no-store | reload | no-cache | force-cache | only-if-cached',
  redirect: 'follow | error | manual',
  referrerPolicy: 'no-referrer | origin | strict-origin-when-cross-origin',
  signal: abortController.signal, // Para cancelar
  keepalive: true, // Para requests en unload
});

// Verificar respuesta
if (!respuesta.ok) throw new Error(`Error: ${respuesta.status} ${respuesta.statusText}`);

// Parsear respuesta
const json    = await respuesta.json();
const texto   = await respuesta.text();
const blob    = await respuesta.blob();
const buffer  = await respuesta.arrayBuffer();
const form    = await respuesta.formData();

// Información de la respuesta
respuesta.status      // 200, 404, 500...
respuesta.statusText  // "OK", "Not Found"...
respuesta.ok          // true si 200-299
respuesta.headers.get('Content-Type')
respuesta.headers.forEach((valor, clave) => { });
respuesta.url
respuesta.redirected
respuesta.type  // "basic", "cors", "opaque"

// Subir archivos
const formData = new FormData();
formData.append('archivo', fileInputElement.files[0]);
formData.append('nombre', 'archivo.pdf');
await fetch('/upload', { method: 'POST', body: formData });

// AbortController
const controller = new AbortController();
setTimeout(() => controller.abort(), 5000); // Timeout de 5s
try {
  const resp = await fetch(url, { signal: controller.signal });
} catch (e) {
  if (e.name === 'AbortError') console.log('Cancelado');
}

// === GENERATORS ASÍNCRONOS Y STREAMS ===
async function* streamDatos(url) {
  const respuesta = await fetch(url);
  const reader = respuesta.body.getReader();
  const decoder = new TextDecoder();

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    yield decoder.decode(value, { stream: true });
  }
}

for await (const chunk of streamDatos('/api/stream')) {
  console.log(chunk);
}

// === OBSERVABLES (patrón, no nativo en JS) ===
// Implementación básica
class Observable {
  #suscriptorFn;

  constructor(suscriptorFn) {
    this.#suscriptorFn = suscriptorFn;
  }

  subscribe(observador) {
    const obs = typeof observador === 'function'
      ? { next: observador, error: console.error, complete: () => {} }
      : observador;

    const desubscribir = this.#suscriptorFn(obs);
    return { unsubscribe: desubscribir || (() => {}) };
  }

  pipe(...operadores) {
    return operadores.reduce((obs, op) => op(obs), this);
  }

  static fromEvent(elemento, evento) {
    return new Observable(obs => {
      const handler = e => obs.next(e);
      elemento.addEventListener(evento, handler);
      return () => elemento.removeEventListener(evento, handler);
    });
  }

  static interval(ms) {
    return new Observable(obs => {
      const id = setInterval(() => obs.next(Date.now()), ms);
      return () => clearInterval(id);
    });
  }
}
```

---

## 9. DOM — API COMPLETA

```javascript
// === SELECCIÓN ===
document.getElementById('id')
document.querySelector('.clase')          // Primero que coincide
document.querySelectorAll('[data-id]')    // NodeList (estático)
document.getElementsByClassName('clase') // HTMLCollection (vivo)
document.getElementsByTagName('div')     // HTMLCollection (vivo)
document.getElementsByName('nombre')     // NodeList (vivo)

// Desde un elemento
elemento.querySelector(':scope > .hijo') // Solo descendientes directos
elemento.closest('.ancestro')            // Ancestro más cercano
elemento.matches('.clase, #id')          // Verifica si coincide con selector

// === CREACIÓN ===
document.createElement('div')
document.createTextNode('Texto')
document.createDocumentFragment()        // Para batch insertions
document.createComment('Comentario')
document.createRange()

elemento.cloneNode(true)  // true = clona descendientes
elemento.cloneNode(false) // Solo el elemento

// === MANIPULACIÓN DEL DOM ===
// Insertar
padre.appendChild(hijo)
padre.insertBefore(nuevo, referencia)
padre.prepend(nuevo1, nuevo2, 'texto') // Múltiples + strings
padre.append(nuevo1, nuevo2, 'texto')  // Al final
padre.before(nuevo)  // Antes del elemento
padre.after(nuevo)   // Después del elemento
padre.replaceWith(nuevo)
elemento.replaceChildren(nuevo1, nuevo2) // Reemplaza todos los hijos

// Eliminar
padre.removeChild(hijo)
elemento.remove()  // Elimina el propio elemento

// innerHTML vs textContent
elemento.innerHTML = '<strong>HTML</strong>';  // Parsea HTML (XSS risk!)
elemento.textContent = 'Solo texto';           // Más seguro
elemento.innerText  // Considera estilos CSS (más lento)
elemento.outerHTML  // El elemento completo con sus descendientes

// Inserción eficiente con fragments
const fragment = document.createDocumentFragment();
datos.forEach(dato => {
  const li = document.createElement('li');
  li.textContent = dato;
  fragment.appendChild(li);
});
lista.appendChild(fragment); // Un solo reflow

// insertAdjacentHTML — muy eficiente
elemento.insertAdjacentHTML('beforebegin', '<div>Antes del elemento</div>');
elemento.insertAdjacentHTML('afterbegin',  '<div>Primer hijo</div>');
elemento.insertAdjacentHTML('beforeend',   '<div>Último hijo</div>');
elemento.insertAdjacentHTML('afterend',    '<div>Después del elemento</div>');
elemento.insertAdjacentElement('beforeend', otroElemento);
elemento.insertAdjacentText('beforeend', 'Texto seguro');

// === ATRIBUTOS ===
elemento.getAttribute('href')
elemento.setAttribute('href', '/nueva-url')
elemento.removeAttribute('disabled')
elemento.hasAttribute('disabled')
elemento.toggleAttribute('disabled')         // Toggle on/off
elemento.getAttributeNames()                 // Todos los atributos

// dataset (data-*)
<div data-user-id="123" data-is-active="true">
elemento.dataset.userId     // "123" (camelCase)
elemento.dataset.isActive   // "true"
elemento.dataset.nuevoValor = 'algo';  // Crea data-nuevo-valor

// === CLASES ===
elemento.classList.add('clase1', 'clase2')
elemento.classList.remove('clase1', 'clase2')
elemento.classList.toggle('activo')
elemento.classList.toggle('activo', condición)  // fuerza on/off
elemento.classList.contains('activo')
elemento.classList.replace('viejo', 'nuevo')
elemento.classList.forEach(clase => { })
elemento.className = 'clase1 clase2'    // Reemplaza todo

// === ESTILOS ===
elemento.style.color = 'red';
elemento.style.backgroundColor = '#fff';     // camelCase
elemento.style.setProperty('--variable', 'valor');  // CSS custom properties
elemento.style.removeProperty('color');
elemento.style.cssText = 'color: red; background: blue;';  // Bulk

// Estilos computados (solo lectura)
const estilos = window.getComputedStyle(elemento);
const estilosPseudo = window.getComputedStyle(elemento, '::before');
estilos.getPropertyValue('color')      // "rgb(255, 0, 0)"
estilos.getPropertyValue('--variable') // CSS custom property

// === NAVEGACIÓN POR EL DOM ===
elemento.parentNode
elemento.parentElement
elemento.childNodes           // NodeList (incluye text nodes)
elemento.children             // HTMLCollection (solo elementos)
elemento.firstChild           // Primer nodo (puede ser text)
elemento.firstElementChild    // Primer elemento
elemento.lastChild
elemento.lastElementChild
elemento.nextSibling          // Siguiente nodo
elemento.nextElementSibling   // Siguiente elemento
elemento.previousSibling
elemento.previousElementSibling
elemento.childElementCount    // Número de hijos elemento

// === EVENTOS ===
// Agregar
elemento.addEventListener('click', handler);
elemento.addEventListener('click', handler, { 
  once: true,       // Se ejecuta solo una vez
  capture: true,    // Fase de captura (antes del bubbling)
  passive: true,    // No llamará preventDefault (mejor performance en scroll)
  signal: controller.signal, // AbortController
});

// Eliminar
elemento.removeEventListener('click', handler);
// Con AbortController
const ctrl = new AbortController();
elemento.addEventListener('click', handler, { signal: ctrl.signal });
ctrl.abort(); // Elimina el listener

// Despachar eventos
elemento.dispatchEvent(new Event('miEvento', { bubbles: true, cancelable: true }));
elemento.dispatchEvent(new CustomEvent('miEvento', {
  bubbles: true,
  cancelable: true,
  detail: { datos: 'adicionales' }
}));

// El objeto Event
function handler(event) {
  event.target           // El elemento que disparó el evento
  event.currentTarget    // El elemento con el listener
  event.type             // 'click', 'keydown', etc.
  event.bubbles          // Si burbujea
  event.cancelable       // Si se puede cancelar
  event.defaultPrevented // Si ya se llamó preventDefault
  event.timeStamp        // Timestamp en ms
  event.stopPropagation()     // Detiene el bubbling
  event.stopImmediatePropagation() // Detiene bubbling + otros listeners
  event.preventDefault()       // Previene comportamiento por defecto
  event.composedPath()         // Array de elementos en el path del evento
}

// === EVENTOS IMPORTANTES ===
// Mouse
'click' | 'dblclick' | 'mousedown' | 'mouseup' | 'mousemove'
'mouseover' | 'mouseout' | 'mouseenter' | 'mouseleave'
'contextmenu' | 'wheel'
evento.clientX | evento.clientY    // Relativo al viewport
evento.pageX | evento.pageY        // Relativo al documento
evento.offsetX | evento.offsetY    // Relativo al elemento
evento.screenX | evento.screenY    // Relativo a la pantalla
evento.button  // 0=izq, 1=medio, 2=der
evento.buttons // Bitmask de botones presionados
evento.altKey | evento.ctrlKey | evento.shiftKey | evento.metaKey

// Teclado
'keydown' | 'keyup' | 'keypress' // keypress deprecado
evento.key     // 'Enter', 'Escape', 'a', 'A', 'ArrowLeft'...
evento.code    // 'Enter', 'KeyA', 'ArrowLeft' (layout-independent)
evento.keyCode // Deprecado
evento.repeat  // true si se mantiene presionada

// Formulario
'submit' | 'reset' | 'change' | 'input' | 'focus' | 'blur'
'focusin' | 'focusout' | 'select'
// 'input' — cada cambio
// 'change' — al perder foco con cambios

// Touch
'touchstart' | 'touchend' | 'touchmove' | 'touchcancel'
evento.touches           // Todos los puntos de contacto
evento.changedTouches    // Los que cambiaron
evento.targetTouches     // Los del mismo elemento

// Pointer (mouse + touch + pen unificados)
'pointerdown' | 'pointerup' | 'pointermove' | 'pointerover'
'pointerout' | 'pointerenter' | 'pointerleave' | 'pointercancel'
'gotpointercapture' | 'lostpointercapture'
evento.pointerId
evento.pointerType  // 'mouse' | 'touch' | 'pen'
evento.pressure
evento.tiltX | evento.tiltY
elemento.setPointerCapture(evento.pointerId)
elemento.releasePointerCapture(evento.pointerId)

// Scroll
'scroll' | 'scrollend'
elemento.scrollTop | elemento.scrollLeft
elemento.scrollHeight | elemento.scrollWidth
elemento.clientHeight | elemento.clientWidth
window.scrollY | window.scrollX
window.scrollTo({ top: 0, behavior: 'smooth' })
elemento.scrollIntoView({ behavior: 'smooth', block: 'start' })

// Redimensionado
'resize'  // En window
// Mejor usar ResizeObserver:
const ro = new ResizeObserver(entries => {
  entries.forEach(({ target, contentRect }) => {
    console.log(target, contentRect.width, contentRect.height);
  });
});
ro.observe(elemento);

// Drag and Drop
'dragstart' | 'drag' | 'dragend' | 'dragover' | 'dragenter' | 'dragleave' | 'drop'
evento.dataTransfer.setData('text/plain', datos);
evento.dataTransfer.getData('text/plain');
evento.dataTransfer.files
evento.dataTransfer.effectAllowed = 'copy | move | link | all';
evento.dataTransfer.dropEffect = 'copy | move | link | none';

// Document
'DOMContentLoaded'  // HTML parseado, antes de recursos
'load'              // Todos los recursos cargados
'beforeunload'      // Antes de cerrar (puede mostrar diálogo)
'unload'            // Al cerrar (limitado, sin async)
'visibilitychange'  // Tab activa/inactiva
document.hidden     // true si la pestaña está oculta
document.visibilityState  // 'visible' | 'hidden' | 'prerender'

// === DELEGACIÓN DE EVENTOS ===
// En lugar de N listeners, uno en el padre
document.getElementById('lista').addEventListener('click', (e) => {
  const item = e.target.closest('li[data-id]');
  if (!item) return;
  const id = item.dataset.id;
  console.log('Item clickeado:', id);
});
```

---

## 10. APIS DEL NAVEGADOR

### Intersection Observer
```javascript
// Lazy loading, animaciones al scroll, infinite scroll
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      // El elemento entró al viewport
      entry.target.classList.add('visible');
      entry.target.src = entry.target.dataset.src; // Lazy load imagen
      observer.unobserve(entry.target); // Dejar de observar
    }
  });
}, {
  root: null,              // null = viewport
  rootMargin: '0px 0px -100px 0px', // Margen del viewport
  threshold: 0.1,          // 10% visible para triggear
  // threshold: [0, 0.25, 0.5, 0.75, 1], // Múltiples thresholds
});

document.querySelectorAll('.animar').forEach(el => observer.observe(el));

// Propiedades de IntersectionObserverEntry:
entry.isIntersecting    // boolean
entry.intersectionRatio // 0.0 a 1.0
entry.intersectionRect  // DOMRect de la intersección
entry.boundingClientRect // DOMRect del elemento
entry.rootBounds         // DOMRect del root
entry.target             // El elemento observado
entry.time               // timestamp
```

### MutationObserver
```javascript
// Observar cambios en el DOM
const observer = new MutationObserver((mutations) => {
  mutations.forEach(mutation => {
    if (mutation.type === 'childList') {
      console.log('Nodos agregados:', mutation.addedNodes);
      console.log('Nodos eliminados:', mutation.removedNodes);
    }
    if (mutation.type === 'attributes') {
      console.log(`Atributo ${mutation.attributeName} cambió`);
      console.log('Valor anterior:', mutation.oldValue);
    }
    if (mutation.type === 'characterData') {
      console.log('Texto cambió');
    }
  });
});

observer.observe(elemento, {
  childList: true,             // Hijos directos
  subtree: true,               // Todos los descendientes
  attributes: true,            // Cambios en atributos
  attributeFilter: ['class', 'style'], // Solo estos atributos
  attributeOldValue: true,     // Recordar valor anterior
  characterData: true,         // Cambios en texto
  characterDataOldValue: true,
});

observer.disconnect();
observer.takeRecords(); // Obtiene mutaciones pendientes
```

### PerformanceObserver
```javascript
// Web Vitals
const po = new PerformanceObserver((list) => {
  list.getEntries().forEach(entry => {
    console.log(entry.entryType, entry.name, entry.duration);
  });
});

po.observe({ type: 'largest-contentful-paint', buffered: true });
po.observe({ type: 'first-input', buffered: true });
po.observe({ type: 'layout-shift', buffered: true });
po.observe({ type: 'longtask' });
po.observe({ type: 'navigation' });
po.observe({ type: 'resource' });
po.observe({ type: 'paint' });  // FP, FCP
po.observe({ type: 'mark' });
po.observe({ type: 'measure' });

// Medir rendimiento manualmente
performance.mark('inicio-operacion');
// ... código ...
performance.mark('fin-operacion');
performance.measure('mi-operacion', 'inicio-operacion', 'fin-operacion');
const medidas = performance.getEntriesByName('mi-operacion');
performance.clearMarks();
performance.clearMeasures();
```

### Storage APIs
```javascript
// localStorage — persiste sin fecha de expiración
localStorage.setItem('clave', JSON.stringify(valor));
const val = JSON.parse(localStorage.getItem('clave'));
localStorage.removeItem('clave');
localStorage.clear();
localStorage.length;
localStorage.key(0);  // Clave en posición 0

// sessionStorage — solo dura la sesión del tab
sessionStorage.setItem('temporal', 'valor');

// Escuchar cambios en localStorage (entre tabs)
window.addEventListener('storage', (e) => {
  console.log(e.key, e.oldValue, e.newValue, e.url);
});

// IndexedDB — para datos complejos y grandes
const request = indexedDB.open('miDB', 1);
request.onupgradeneeded = (e) => {
  const db = e.target.result;
  const store = db.createObjectStore('usuarios', { keyPath: 'id', autoIncrement: true });
  store.createIndex('email', 'email', { unique: true });
};
request.onsuccess = (e) => {
  const db = e.target.result;
  const tx = db.transaction(['usuarios'], 'readwrite');
  const store = tx.objectStore('usuarios');
  store.add({ nombre: 'Juan', email: 'juan@ej.com' });
  tx.oncomplete = () => db.close();
};

// Cache API (para Service Workers)
const cache = await caches.open('mi-cache-v1');
await cache.addAll(['/index.html', '/styles.css', '/app.js']);
await cache.put(request, response);
const cached = await cache.match(request);
await caches.delete('mi-cache-v1');
const keys = await caches.keys();
```

### Web Workers
```javascript
// === MAIN THREAD ===
const worker = new Worker('/worker.js', { type: 'module' });

worker.postMessage({ tipo: 'CALCULAR', datos: [1, 2, 3, 4, 5] });

worker.onmessage = (e) => {
  console.log('Resultado del worker:', e.data);
};

worker.onerror = (e) => {
  console.error('Error en worker:', e.message, e.filename, e.lineno);
};

worker.terminate(); // Detiene el worker

// Inline Worker (sin archivo separado)
const blob = new Blob([`
  self.onmessage = (e) => {
    const resultado = e.data.reduce((a, b) => a + b, 0);
    self.postMessage(resultado);
  };
`], { type: 'application/javascript' });
const worker2 = new Worker(URL.createObjectURL(blob));

// Transferibles (cero copia — muy rápido para buffers grandes)
const buffer = new ArrayBuffer(1024 * 1024); // 1MB
worker.postMessage({ buffer }, [buffer]); // buffer transferido, no copiado
// Después de transferir, buffer está vacío en el thread principal

// SharedArrayBuffer (memoria compartida)
const shared = new SharedArrayBuffer(4);
const vista = new Int32Array(shared);
worker.postMessage({ shared });
// En el worker también puede leer/escribir en shared

// === WORKER.JS ===
self.onmessage = (e) => {
  const { tipo, datos } = e.data;

  if (tipo === 'CALCULAR') {
    const resultado = procesarPesado(datos);
    self.postMessage({ tipo: 'RESULTADO', resultado });
  }
};

function procesarPesado(datos) {
  // Cálculo intensivo que no bloquea el UI thread
  return datos.reduce((acc, n) => acc + n ** 2, 0);
}

// Importar scripts en worker
importScripts('/lib/utilidades.js');
// O con módulos:
// import { utilidad } from '/lib/utilidades.js';
```

### Service Worker
```javascript
// === REGISTRO ===
if ('serviceWorker' in navigator) {
  window.addEventListener('load', async () => {
    const registration = await navigator.serviceWorker.register('/sw.js', {
      scope: '/',
      updateViaCache: 'none',
    });
    console.log('SW registrado:', registration.scope);
    registration.addEventListener('updatefound', () => {
      const newWorker = registration.installing;
      newWorker.addEventListener('statechange', () => {
        if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
          // Hay una nueva versión disponible
          mostrarToast('Nueva versión disponible. Refresca la página.');
        }
      });
    });
  });
}

// === SW.JS ===
const CACHE = 'mi-app-v1';
const RECURSOS_ESTATICOS = ['/index.html', '/styles.css', '/app.js', '/offline.html'];

// Install — pre-cachear
self.addEventListener('install', (e) => {
  e.waitUntil(
    caches.open(CACHE).then(c => c.addAll(RECURSOS_ESTATICOS))
  );
  self.skipWaiting(); // Activar inmediatamente
});

// Activate — limpiar caches viejos
self.addEventListener('activate', (e) => {
  e.waitUntil(
    caches.keys().then(keys =>
      Promise.all(keys.filter(k => k !== CACHE).map(k => caches.delete(k)))
    )
  );
  self.clients.claim(); // Tomar control inmediato
});

// Fetch — estrategia de caché
self.addEventListener('fetch', (e) => {
  if (!e.request.url.startsWith('http')) return;

  // Network first (para APIs)
  if (e.request.url.includes('/api/')) {
    e.respondWith(
      fetch(e.request)
        .then(resp => {
          const clone = resp.clone();
          caches.open(CACHE).then(c => c.put(e.request, clone));
          return resp;
        })
        .catch(() => caches.match(e.request))
    );
    return;
  }

  // Cache first (para estáticos)
  e.respondWith(
    caches.match(e.request)
      .then(cached => cached || fetch(e.request))
      .catch(() => caches.match('/offline.html'))
  );
});
```

---

## 11. PATRONES DE DISEÑO EN JAVASCRIPT

```javascript
// === MÓDULO (ES Modules) ===
// modulo.js
export const CONSTANTE = 42;
export function utilidad() { }
export class MiClase { }
export default function principal() { }

// Importar
import principal, { CONSTANTE, utilidad, MiClase } from './modulo.js';
import * as modulo from './modulo.js';
import { MiClase as Alias } from './modulo.js';

// Dynamic import (lazy loading)
const modulo = await import('./modulo.js');
const { default: Principal } = await import('./componente.js');

// Re-export
export { algo } from './otro.js';
export * from './otro.js';
export { default } from './otro.js';

// === OBSERVER / PUB-SUB ===
class EventBus {
  static #instancia = new EventBus();
  static getInstance() { return this.#instancia; }

  #handlers = {};

  subscribe(evento, handler) {
    (this.#handlers[evento] ??= []).push(handler);
    return () => this.unsubscribe(evento, handler);
  }

  unsubscribe(evento, handler) {
    this.#handlers[evento] = (this.#handlers[evento] || []).filter(h => h !== handler);
  }

  publish(evento, ...args) {
    (this.#handlers[evento] || []).forEach(h => h(...args));
  }
}

// === COMMAND PATTERN ===
class CommandManager {
  #history = [];
  #redoStack = [];

  execute(command) {
    command.execute();
    this.#history.push(command);
    this.#redoStack = [];
  }

  undo() {
    const command = this.#history.pop();
    if (command) {
      command.undo();
      this.#redoStack.push(command);
    }
  }

  redo() {
    const command = this.#redoStack.pop();
    if (command) {
      command.execute();
      this.#history.push(command);
    }
  }
}

// === STATE MACHINE ===
class StateMachine {
  #estado;
  #transiciones;

  constructor(estadoInicial, transiciones) {
    this.#estado = estadoInicial;
    this.#transiciones = transiciones;
  }

  transicionar(evento) {
    const transicion = this.#transiciones[this.#estado]?.[evento];
    if (!transicion) throw new Error(`Transición inválida: ${this.#estado} -> ${evento}`);
    if (transicion.guardia && !transicion.guardia()) throw new Error('Guardia falló');
    transicion.accion?.();
    this.#estado = transicion.siguiente;
    return this;
  }

  get estado() { return this.#estado; }
}

const maquina = new StateMachine('idle', {
  idle:     { iniciar: { siguiente: 'corriendo', accion: () => console.log('Iniciando') } },
  corriendo:{ pausar:  { siguiente: 'pausado'  }, detener: { siguiente: 'idle' } },
  pausado:  { reanudar:{ siguiente: 'corriendo' }, detener: { siguiente: 'idle' } },
});

// === STRATEGY PATTERN ===
class Ordenador {
  #estrategia;

  constructor(estrategia) {
    this.#estrategia = estrategia;
  }

  set estrategia(s) { this.#estrategia = s; }

  ordenar(datos) {
    return this.#estrategia.ordenar(datos);
  }
}

const estrategiaBurbuja = {
  ordenar: (datos) => { /* bubble sort */ return datos; }
};
const estrategiaQuick = {
  ordenar: (datos) => [...datos].sort((a, b) => a - b)
};
```

---

## 12. MANEJO DE ERRORES

```javascript
// === TRY-CATCH-FINALLY ===
try {
  const resultado = operacionRiesgosa();
  return resultado;
} catch (error) {
  // Tipos de error nativos
  if (error instanceof TypeError)     console.error('Tipo inválido', error.message);
  if (error instanceof RangeError)    console.error('Fuera de rango', error.message);
  if (error instanceof ReferenceError) console.error('Referencia inválida', error.message);
  if (error instanceof SyntaxError)   console.error('Sintaxis inválida', error.message);
  if (error instanceof URIError)      console.error('URI inválida', error.message);
  if (error instanceof EvalError)     console.error('Error de eval', error.message);
  if (error instanceof AggregateError) {
    error.errors.forEach(e => console.error(e)); // Promise.any failures
  }

  // Propiedades del error
  error.name       // "TypeError", "Error", etc.
  error.message    // Mensaje descriptivo
  error.stack      // Stack trace
  error.cause      // Error que causó este (ES2022)

  throw error; // Re-lanzar si no podemos manejar
} finally {
  // Siempre ejecuta (limpieza)
  liberarRecursos();
}

// === ERRORES PERSONALIZADOS ===
class ErrorDeValidacion extends Error {
  constructor(campo, valor, mensaje) {
    super(mensaje);
    this.name = 'ErrorDeValidacion';
    this.campo = campo;
    this.valor = valor;
    // Corregir el stack trace en V8
    if (Error.captureStackTrace) {
      Error.captureStackTrace(this, ErrorDeValidacion);
    }
  }
}

class ErrorDeRed extends Error {
  constructor(statusCode, mensaje, { cause } = {}) {
    super(mensaje, { cause }); // ES2022: pasar error original
    this.name = 'ErrorDeRed';
    this.statusCode = statusCode;
  }
}

// Error chaining (ES2022)
try {
  await fetch('/api/datos');
} catch (causaOriginal) {
  throw new ErrorDeRed(500, 'Error al cargar datos', { cause: causaOriginal });
}

// === MANEJO ASYNC ===
// Patrón Go-style (evitar try-catch anidados)
const [error, datos] = await aPromise
  .then(datos => [null, datos])
  .catch(error => [error, null]);

if (error) { /* manejar */ }
// usar datos de forma segura

// Helper
async function safeAsync(promise) {
  try {
    return [null, await promise];
  } catch (e) {
    return [e, null];
  }
}

// === GLOBAL ERROR HANDLERS ===
window.addEventListener('error', (e) => {
  console.error('Error no capturado:', e.error, e.filename, e.lineno);
  e.preventDefault(); // Evitar que aparezca en consola
});

window.addEventListener('unhandledrejection', (e) => {
  console.error('Promise rechazada no manejada:', e.reason);
  e.preventDefault();
});

// Node.js
process.on('uncaughtException', (err) => { /* ... */ });
process.on('unhandledRejection', (reason, promise) => { /* ... */ });
```

---

## 13. MÓDULOS ES + HERRAMIENTAS MODERNAS

```javascript
// === IMPORT MAPS ===
// En HTML:
// <script type="importmap">
// { "imports": { "react": "https://esm.sh/react@18" } }
// </script>
import React from 'react'; // Usa el mapa

// === DYNAMIC IMPORTS ===
// Lazy loading de módulos
const { default: Chart } = await import('./chart.js');
const module = await import(/* webpackChunkName: "modulo" */ './modulo.js');

// Con fallback
const { Componente } = await import('./componente.js').catch(() => import('./fallback.js'));

// === JSON MODULES ===
import config from './config.json' assert { type: 'json' };
import config from './config.json' with { type: 'json' }; // Sintaxis nueva

// === CSS MODULES (en bundlers) ===
import styles from './styles.module.css';
element.className = styles.miClase;

// === COMPARTIR ESTADO ENTRE MÓDULOS ===
// store.js
let estado = { usuario: null, tema: 'claro' };
const suscriptores = new Set();

export const store = {
  getState: () => ({ ...estado }),
  setState(nuevoEstado) {
    estado = { ...estado, ...nuevoEstado };
    suscriptores.forEach(fn => fn(estado));
  },
  subscribe(fn) {
    suscriptores.add(fn);
    return () => suscriptores.delete(fn);
  }
};
```

---

## 14. PERFORMANCE Y OPTIMIZACIÓN

```javascript
// === requestAnimationFrame ===
// Para animaciones suaves sincronizadas con el refresh del monitor
function animar(timestamp) {
  const progreso = timestamp / 1000; // segundos
  elemento.style.transform = `translateX(${Math.sin(progreso) * 100}px)`;
  requestAnimationFrame(animar);
}
const id = requestAnimationFrame(animar);
cancelAnimationFrame(id);

// === requestIdleCallback ===
// Para tareas no urgentes cuando el browser está inactivo
const idId = requestIdleCallback((deadline) => {
  while (deadline.timeRemaining() > 0 && tareas.length > 0) {
    procesarTarea(tareas.shift());
  }
  if (tareas.length > 0) requestIdleCallback(procesoFn);
}, { timeout: 2000 });
cancelIdleCallback(idId);

// === scheduler.postTask ===
// Prioridades: 'user-blocking' > 'user-visible' > 'background'
await scheduler.postTask(() => tareasPesada(), {
  priority: 'background',
  signal: controller.signal,
});

// === BATCH DE OPERACIONES DOM ===
// ❌ Mal — fuerza múltiples reflows
elements.forEach(el => {
  el.style.width = el.offsetWidth + 10 + 'px'; // Read + Write alternados
});

// ✅ Bien — separar lecturas de escrituras
const anchos = elements.map(el => el.offsetWidth); // Todas las lecturas
elements.forEach((el, i) => {
  el.style.width = anchos[i] + 10 + 'px'; // Todas las escrituras
});

// === VIRTUALIZATION (para listas largas) ===
class VirtualScroller {
  #itemHeight;
  #items;
  #container;
  #visibleCount;

  constructor(container, items, itemHeight) {
    this.#container = container;
    this.#items = items;
    this.#itemHeight = itemHeight;
    this.#visibleCount = Math.ceil(container.clientHeight / itemHeight) + 2;

    container.style.overflow = 'auto';
    container.style.position = 'relative';
    container.style.height = `${items.length * itemHeight}px`;

    this.render(0);
    container.addEventListener('scroll', () =>
      this.render(Math.floor(container.scrollTop / itemHeight))
    );
  }

  render(startIndex) {
    this.#container.innerHTML = '';
    const fragment = document.createDocumentFragment();

    for (let i = startIndex; i < Math.min(startIndex + this.#visibleCount, this.#items.length); i++) {
      const el = document.createElement('div');
      el.style.cssText = `position:absolute;top:${i * this.#itemHeight}px;height:${this.#itemHeight}px;width:100%`;
      el.textContent = this.#items[i];
      fragment.appendChild(el);
    }

    this.#container.appendChild(fragment);
  }
}

// === LAZY EVALUATION ===
// Getters para cálculos costosos
class ReportePesado {
  get estadisticas() {
    // Se calcula solo cuando se accede
    const resultado = calcularEstadisticasComplejas(this.datos);
    // Memoizar para futuras llamadas
    Object.defineProperty(this, 'estadisticas', { value: resultado, writable: false });
    return resultado;
  }
}

// === OBJECT POOLING ===
class Pool {
  #disponibles = [];
  #crear;
  #reset;

  constructor(crear, reset, initialSize = 10) {
    this.#crear = crear;
    this.#reset = reset;
    for (let i = 0; i < initialSize; i++) {
      this.#disponibles.push(crear());
    }
  }

  obtener() {
    return this.#disponibles.length > 0
      ? this.#disponibles.pop()
      : this.#crear();
  }

  liberar(objeto) {
    this.#reset(objeto);
    this.#disponibles.push(objeto);
  }
}
```

---

## 15. APIS MODERNAS DEL NAVEGADOR

```javascript
// === CLIPBOARD API ===
// Copiar
await navigator.clipboard.writeText('Texto copiado');
await navigator.clipboard.write([
  new ClipboardItem({ 'text/html': new Blob(['<b>HTML</b>'], {type:'text/html'}) })
]);
// Pegar
const texto = await navigator.clipboard.readText();
const items = await navigator.clipboard.read();

// === SHARE API ===
if (navigator.share) {
  await navigator.share({
    title: 'Título',
    text: 'Descripción',
    url: window.location.href,
    files: [archivo],
  });
}

// === NOTIFICATIONS ===
const perm = await Notification.requestPermission();
if (perm === 'granted') {
  const notif = new Notification('Título', {
    body: 'Mensaje de notificación',
    icon: '/icono.png',
    badge: '/badge.png',
    image: '/imagen.png',
    tag: 'mi-notif',        // Agrupa notificaciones
    requireInteraction: false,
    silent: false,
    data: { url: '/ruta' },
    actions: [{ action: 'abrir', title: 'Abrir' }],
  });
  notif.onclick = () => window.open('/ruta');
}

// === GEOLOCATION ===
navigator.geolocation.getCurrentPosition(
  ({ coords: { latitude, longitude, accuracy } }) => { },
  (error) => { console.error(error.code, error.message); },
  { enableHighAccuracy: true, timeout: 10000, maximumAge: 0 }
);
const watchId = navigator.geolocation.watchPosition(fn, err, opts);
navigator.geolocation.clearWatch(watchId);

// === DEVICE ORIENTATION / MOTION ===
window.addEventListener('deviceorientation', (e) => {
  e.alpha  // Rotación Z (0-360°)
  e.beta   // Rotación X (-180° a 180°)
  e.gamma  // Rotación Y (-90° a 90°)
  e.absolute // Si es absoluto o relativo
});

window.addEventListener('devicemotion', (e) => {
  e.acceleration.x // m/s²
  e.accelerationIncludingGravity.x
  e.rotationRate.alpha // °/s
  e.interval // ms entre eventos
});

// === SCREEN ORIENTATION ===
screen.orientation.type   // 'portrait-primary', 'landscape-primary', etc.
screen.orientation.angle  // 0, 90, 180, 270
await screen.orientation.lock('portrait');
screen.orientation.unlock();
screen.orientation.addEventListener('change', handler);

// === FULLSCREEN API ===
await elemento.requestFullscreen({ navigationUI: 'hide' });
await document.exitFullscreen();
document.fullscreenElement  // El elemento en fullscreen
document.fullscreenEnabled  // Si está disponible
document.addEventListener('fullscreenchange', handler);

// === VIBRATION API ===
navigator.vibrate(200);           // 200ms
navigator.vibrate([200, 100, 200]); // vibrar-pausa-vibrar
navigator.vibrate(0);             // Parar

// === BATTERY API ===
const battery = await navigator.getBattery();
battery.charging       // boolean
battery.chargingTime   // segundos hasta carga completa
battery.dischargingTime
battery.level          // 0 a 1

// === MEDIA DEVICES (Cámara/Micrófono) ===
const stream = await navigator.mediaDevices.getUserMedia({
  video: { width: { ideal: 1280 }, height: { ideal: 720 }, facingMode: 'user' },
  audio: { echoCancellation: true, noiseSuppression: true, sampleRate: 44100 },
});
videoElement.srcObject = stream;

// Pantalla
const screenStream = await navigator.mediaDevices.getDisplayMedia({
  video: { cursor: 'always' },
  audio: true,
});

// Dispositivos disponibles
const devices = await navigator.mediaDevices.enumerateDevices();
devices.filter(d => d.kind === 'videoinput'); // Cámaras
devices.filter(d => d.kind === 'audioinput'); // Micrófonos

// === SPEECH SYNTHESIS ===
const utterance = new SpeechSynthesisUtterance('Hola mundo');
utterance.lang = 'es-ES';
utterance.pitch = 1;      // 0-2
utterance.rate = 1;       // 0.1-10
utterance.volume = 1;     // 0-1
utterance.voice = speechSynthesis.getVoices().find(v => v.lang === 'es-ES');
speechSynthesis.speak(utterance);
speechSynthesis.cancel();
speechSynthesis.pause();
speechSynthesis.resume();

// === SPEECH RECOGNITION ===
const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognition.lang = 'es-ES';
recognition.continuous = true;
recognition.interimResults = true;
recognition.onresult = (e) => {
  const transcript = Array.from(e.results)
    .map(r => r[0].transcript)
    .join('');
  console.log(transcript);
};
recognition.start();
recognition.stop();

// === PAYMENT REQUEST ===
const pago = new PaymentRequest(
  [{ supportedMethods: 'basic-card' }],
  {
    total: { label: 'Total', amount: { currency: 'USD', value: '29.99' } },
    displayItems: [
      { label: 'Producto', amount: { currency: 'USD', value: '29.99' } }
    ],
  }
);
const resultado = await pago.show();
await resultado.complete('success');

// === WEB CRYPTO ===
// Generar clave
const clave = await crypto.subtle.generateKey(
  { name: 'AES-GCM', length: 256 },
  true,
  ['encrypt', 'decrypt']
);

// Hash
const datos = new TextEncoder().encode('mensaje secreto');
const hash = await crypto.subtle.digest('SHA-256', datos);
const hex = [...new Uint8Array(hash)].map(b => b.toString(16).padStart(2,'0')).join('');

// UUID
const uuid = crypto.randomUUID();  // 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'

// Números aleatorios seguros
const arr = new Uint32Array(10);
crypto.getRandomValues(arr);
```

---

## 16. TESTING Y CALIDAD DE CÓDIGO

```javascript
// === UNIT TESTING (patrón general) ===
// Función pura — fácil de testear
function suma(a, b) {
  if (typeof a !== 'number' || typeof b !== 'number')
    throw new TypeError('Ambos argumentos deben ser números');
  return a + b;
}

// Test (estructura compatible con Jest, Vitest, Mocha, etc.)
describe('suma', () => {
  it('suma dos números positivos', () => {
    expect(suma(1, 2)).toBe(3);
  });

  it('suma números negativos', () => {
    expect(suma(-1, -2)).toBe(-3);
  });

  it('lanza error si los argumentos no son números', () => {
    expect(() => suma('a', 2)).toThrow(TypeError);
    expect(() => suma(null, 2)).toThrow('Ambos argumentos deben ser números');
  });
});

// === ASSERTIONS COMUNES ===
expect(valor).toBe(esperado)           // ===
expect(objeto).toEqual(esperado)       // Igualdad profunda
expect(valor).toBeTruthy()
expect(valor).toBeFalsy()
expect(array).toContain(elemento)
expect(string).toMatch(/regex/)
expect(fn).toThrow()
expect(fn).toThrow(TypeError)
expect(mock).toHaveBeenCalled()
expect(mock).toHaveBeenCalledWith(arg1, arg2)
expect(mock).toHaveBeenCalledTimes(3)
expect(numero).toBeCloseTo(0.3, 10)   // Para decimales
expect(array).toHaveLength(3)
expect(objeto).toHaveProperty('clave', 'valor')

// === MOCKS ===
const mockFetch = jest.fn().mockResolvedValue({
  ok: true,
  json: async () => ({ id: 1, nombre: 'Juan' })
});
global.fetch = mockFetch;

// Spy
const spy = jest.spyOn(objeto, 'metodo');
spy.mockReturnValue('valor mockeado');
spy.mockRestore(); // Restaurar original

// === DEBUGGING ===
console.log('valor:', valor)
console.dir(objeto)           // Interactivo
console.table(array)          // Tabla
console.group('Grupo')
console.groupEnd()
console.time('operacion')
// ... código ...
console.timeEnd('operacion')
console.trace()               // Stack trace
console.assert(condición, 'mensaje si falla')
console.count('etiqueta')
console.countReset('etiqueta')
console.warn('Advertencia')
console.error('Error')

// debugger; — punto de ruptura en DevTools
```

---

## 17. SEGURIDAD EN JAVASCRIPT

```javascript
// === XSS (Cross-Site Scripting) ===
// ❌ NUNCA hacer
elemento.innerHTML = entradaDeUsuario; // XSS!
eval(codigoDesconocido);              // XSS!
new Function(codigoDesconocido)();    // XSS!
setTimeout(codigoDesconocido, 1000);  // XSS!
document.write(entradaDeUsuario);     // XSS!

// ✅ Usar textContent para texto
elemento.textContent = entradaDeUsuario; // Seguro

// ✅ Sanitizar HTML si es necesario
import DOMPurify from 'dompurify';
elemento.innerHTML = DOMPurify.sanitize(htmlDeUsuario, {
  ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a'],
  ALLOWED_ATTR: ['href'],
});

// Trusted Types API (navegador)
if (window.trustedTypes?.createPolicy) {
  const policy = trustedTypes.createPolicy('default', {
    createHTML: (input) => DOMPurify.sanitize(input),
    createScriptURL: (input) => validarURL(input),
  });
  elemento.innerHTML = policy.createHTML(html);
}

// === CSRF ===
// Incluir token CSRF en requests
fetch('/api/accion', {
  method: 'POST',
  headers: {
    'X-CSRF-Token': obtenerTokenCSRF(),
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(datos),
  credentials: 'same-origin', // Enviar cookies
});

// === CONTENT SECURITY POLICY ===
// Usar nonces en scripts inline
// <script nonce="NONCE_GENERADO_SERVIDOR">
const nonce = document.querySelector('script[nonce]')?.nonce;

// === DATOS SENSIBLES ===
// ❌ No guardar en localStorage
localStorage.setItem('token', jwt);  // Vulnerable a XSS

// ✅ Guardar en httpOnly cookies (el servidor lo hace)
// Las cookies httpOnly no son accesibles desde JS

// ❌ No exponer en atributos data-*
// <div data-api-key="mi-clave"> // Visible en el DOM

// === VALIDACIÓN ===
// Siempre validar en el servidor — la validación JS es solo UX
function validarEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(String(email).toLowerCase());
}

function sanitizarCadena(str, maxLength = 255) {
  return String(str).trim().slice(0, maxLength);
}

// === PROTOTYPE POLLUTION ===
// ❌ Vulnerable
function mergeDeep(target, source) {
  Object.keys(source).forEach(key => {
    target[key] = source[key]; // Si source tiene __proto__...
  });
}

// ✅ Seguro
function mergeSeguro(target, source) {
  Object.keys(source).forEach(key => {
    if (key === '__proto__' || key === 'constructor' || key === 'prototype') return;
    if (Object.hasOwn(source, key)) {
      target[key] = source[key];
    }
  });
}

// O usar Object.create(null) para diccionarios sin prototipo
const mapa = Object.create(null);
```

---

## 18. GUÍA RÁPIDA DE MÉTODOS ESENCIALES

```javascript
// === CONVERSIONES ===
Number('42')          // 42
Number(true)          // 1
Number(false)         // 0
Number(null)          // 0
Number(undefined)     // NaN
Number('')            // 0
parseInt('42px', 10)  // 42 (siempre pasar la base!)
parseFloat('3.14em')  // 3.14
+'42'                  // 42 (unario)

String(42)            // "42"
(42).toString()       // "42"
(42).toString(16)     // "2a" (hex)
(42).toString(2)      // "101010" (binario)
(3.14159).toFixed(2)  // "3.14"
(3.14159).toPrecision(4) // "3.142"

Boolean(0)            // false
Boolean('')           // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(NaN)          // false
Boolean({})           // true (¡objeto vacío es truthy!)
Boolean([])           // true (¡array vacío es truthy!)
!!valor               // Doble negación para booleano

// === JSON ===
JSON.stringify(obj)                         // Objeto a string
JSON.stringify(obj, null, 2)               // Indentado con 2 espacios
JSON.stringify(obj, ['campo1', 'campo2'])  // Solo esos campos
JSON.stringify(obj, (key, val) => {        // Replacer function
  if (key === 'contraseña') return undefined; // Omitir campos
  return val;
});

JSON.parse(str)                      // String a objeto
JSON.parse(str, (key, val) => {      // Reviver function
  if (key === 'fecha') return new Date(val);
  return val;
});

// Deep clone con JSON (limitado: no funciones, no undefined, no Date, etc.)
const clon = JSON.parse(JSON.stringify(obj));

// Deep clone nativo (ES2022+)
const clonProfundo = structuredClone(obj); // Maneja Date, Map, Set, ArrayBuffer...

// === MATH ===
Math.round(4.5)    // 5 (redondeo normal)
Math.ceil(4.1)     // 5 (siempre arriba)
Math.floor(4.9)    // 4 (siempre abajo)
Math.trunc(4.9)    // 4 (elimina decimales)
Math.abs(-5)       // 5
Math.max(1, 2, 3)  // 3
Math.min(1, 2, 3)  // 1
Math.max(...array) // Con array
Math.pow(2, 10)    // 1024
Math.sqrt(16)      // 4
Math.cbrt(27)      // 3
Math.log(Math.E)   // 1
Math.log2(8)       // 3
Math.log10(1000)   // 3
Math.random()      // 0 a <1 (NO criptográfico)
Math.PI            // 3.141592653589793
Math.E             // 2.718281828459045
Math.sin(Math.PI / 2) // 1
Math.cos(0)        // 1
Math.tan(Math.PI / 4) // ~1
Math.atan2(1, 1)   // π/4

// Número aleatorio en rango
const randomInt = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
const randomFloat = (min, max) => Math.random() * (max - min) + min;

// === DATE ===
const ahora = new Date();
const fecha = new Date('2024-01-15');
const fecha2 = new Date(2024, 0, 15, 10, 30, 0); // Meses 0-indexados
Date.now()            // Timestamp en ms

fecha.getFullYear()   // 2024
fecha.getMonth()      // 0-11 (¡0 = enero!)
fecha.getDate()       // 1-31
fecha.getDay()        // 0-6 (0 = domingo)
fecha.getHours()      // 0-23
fecha.getMinutes()    // 0-59
fecha.getSeconds()
fecha.getMilliseconds()
fecha.getTime()       // Timestamp
fecha.toISOString()   // "2024-01-15T00:00:00.000Z"
fecha.toLocaleDateString('es-ES', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })
fecha.toLocaleTimeString('es-ES')
fecha.toLocaleString('es-ES')

// Internacionalización moderna
const fmt = new Intl.DateTimeFormat('es-ES', {
  dateStyle: 'full',
  timeStyle: 'short',
  timeZone: 'America/Mexico_City',
});
fmt.format(ahora);

const relFmt = new Intl.RelativeTimeFormat('es-ES', { numeric: 'auto' });
relFmt.format(-1, 'day');    // "ayer"
relFmt.format(3, 'week');    // "en 3 semanas"
relFmt.format(-2, 'month');  // "hace 2 meses"

const numFmt = new Intl.NumberFormat('es-MX', {
  style: 'currency',
  currency: 'MXN',
  minimumFractionDigits: 2,
});
numFmt.format(1234567.89); // "$1,234,567.89"

const listaFmt = new Intl.ListFormat('es-ES', { style: 'long', type: 'conjunction' });
listaFmt.format(['manzanas', 'peras', 'naranjas']); // "manzanas, peras y naranjas"

// Temporal API (propuesta — próximo estándar)
// const hoy = Temporal.Now.plainDateISO();
// const mañana = hoy.add({ days: 1 });
```

---

## 19. CHECKLIST DE CALIDAD JAVASCRIPT

```
CÓDIGO LIMPIO
□ Variables con nombres descriptivos (sin a, b, x genéricos)
□ Funciones hacen UNA sola cosa
□ Sin números mágicos (usar constantes nombradas)
□ Sin código muerto ni console.log en producción
□ Comentarios explican el "por qué", no el "qué"
□ DRY — no repetir lógica
□ SOLID principles aplicados en OOP

CORRECTITUD
□ === en lugar de ==
□ Verificar null/undefined antes de acceder propiedades
□ parseInt siempre con base (parseInt(str, 10))
□ .hasOwnProperty → Object.hasOwn()
□ async/await con try-catch
□ Promises con .catch() o try-catch
□ Sin eval(), with, document.write()
□ Variables siempre declaradas (no globales accidentales)

PERFORMANCE
□ Debounce en inputs/scroll/resize
□ requestAnimationFrame para animaciones
□ Web Workers para cálculos pesados
□ Lazy loading de módulos con import()
□ Memoización para funciones puras costosas
□ Evitar reflows innecesarios (separar lecturas/escrituras DOM)
□ Limpiar event listeners, intervals, observers al desmontar

SEGURIDAD
□ Sin innerHTML con datos de usuario
□ Sanitizar con DOMPurify si se necesita HTML
□ Sin eval() o new Function() con datos externos
□ Tokens en httpOnly cookies, no localStorage
□ Validar tipos de datos antes de procesarlos
□ CSP configurada

ACCESIBILIDAD
□ Eventos de teclado además de mouse
□ Focus management en modales y SPAs
□ ARIA actualizado dinámicamente con JS
□ Live regions para actualizaciones dinámicas
□ No depender solo de color para comunicar estado
```

---

*Fin del archivo JAVASCRIPT_EXPERTO.md — Conocimiento completo de JavaScript ES2024, APIs del navegador, asincronía, patrones de diseño, performance y seguridad.*
