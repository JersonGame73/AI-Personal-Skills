# 🎨 ARCHIVO DE CONOCIMIENTO EXPERTO — CSS COMPLETO
> Versión: Máxima. Cubre el 100% de CSS3 + CSS moderno (2024-2025) + mejores prácticas.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en CSS. Conoces cada propiedad, valor, selector, función, unidad, API de layout y técnica de optimización existente.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en CSS. Conoces:
- El estándar CSS completo (CSS3 + módulos modernos)
- Flexbox y Grid Layout (nivel experto)
- Variables CSS (Custom Properties)
- Animaciones y transiciones avanzadas
- Responsive Design y Container Queries
- CSS moderno: @layer, :has(), @scope, trigonometry, etc.
- Performance y optimización de CSS
- Arquitecturas CSS: BEM, ITCSS, Utility-first
- Preprocesadores: SASS/SCSS conceptos clave
- Accesibilidad en CSS (prefers-reduced-motion, etc.)

---

## 1. SELECTORES — REFERENCIA COMPLETA

### Selectores básicos
```css
/* Universal */
* { box-sizing: border-box; }

/* Tipo */
p { color: #333; }

/* Clase */
.clase { font-weight: bold; }

/* ID */
#identificador { display: flex; }

/* Atributo */
[href] { text-decoration: none; }                    /* Tiene el atributo */
[type="text"] { border: 1px solid; }                /* Valor exacto */
[class~="activo"] { background: blue; }             /* Valor en lista separada por espacios */
[lang|="es"] { quotes: "«" "»"; }                   /* Igual a o empieza con "es-" */
[href^="https"] { color: green; }                   /* Empieza con */
[href$=".pdf"] { content: " (PDF)"; }               /* Termina con */
[href*="google"] { color: blue; }                   /* Contiene */
[data-estado="activo" i] { opacity: 1; }            /* Case-insensitive */
```

### Selectores de combinación
```css
div p          /* Descendiente */
div > p        /* Hijo directo */
h1 + p         /* Hermano adyacente (inmediatamente después) */
h1 ~ p         /* Hermanos siguientes (todos) */
```

### Pseudo-clases de estado
```css
:hover         /* Al pasar el cursor */
:focus         /* Con foco de teclado/pointer */
:focus-within  /* El elemento O algún descendiente tiene foco */
:focus-visible /* Foco visible (teclado, no click) — para estilos de foco accesibles */
:active        /* En el momento de click */
:visited       /* Link visitado */
:link          /* Link no visitado */
:target        /* Elemento apuntado por el hash de la URL (#id) */
:checked       /* Checkbox/radio marcado */
:indeterminate /* Checkbox en estado indeterminado */
:disabled      /* Elemento deshabilitado */
:enabled       /* Elemento habilitado */
:read-only     /* Solo lectura */
:read-write    /* Editable */
:required      /* Campo requerido */
:optional      /* Campo opcional */
:valid         /* Campo con valor válido */
:invalid       /* Campo con valor inválido */
:in-range      /* Valor dentro del rango (min/max) */
:out-of-range  /* Valor fuera del rango */
:placeholder-shown /* El placeholder está visible */
:autofill      /* El navegador ha rellenado el campo */
:blank         /* Campo vacío */
:default       /* Elemento por defecto en grupo (radio, checkbox, option) */
:any-link      /* Cualquier enlace (:link o :visited) */
:local-link    /* Link al mismo sitio */
:scope         /* El elemento de referencia */
:fullscreen    /* En modo pantalla completa */
:picture-in-picture
:popover-open  /* Popover abierto */
:modal         /* En modo modal (dialog abierto con showModal()) */
:defined       /* Custom element ya definido */
:paused        /* Media en pausa */
:playing       /* Media reproduciéndose */
```

### Pseudo-clases estructurales
```css
:root                    /* Elemento raíz (html) — mayor especificidad que html */
:empty                   /* Sin hijos (incluyendo texto) */

/* Hijos */
:first-child             /* Primer hijo de su padre */
:last-child              /* Último hijo de su padre */
:only-child              /* Único hijo de su padre */
:nth-child(n)            /* Hijo enésimo */
:nth-child(2n)           /* Pares: 2,4,6... */
:nth-child(2n+1)         /* Impares: 1,3,5... — equivale a :nth-child(odd) */
:nth-child(even)         /* Pares */
:nth-child(odd)          /* Impares */
:nth-child(3n+1)         /* 1, 4, 7, 10... */
:nth-last-child(n)       /* Desde el final */
:nth-child(n of .clase)  /* Enésimo con clase específica */

/* Del mismo tipo */
:first-of-type
:last-of-type
:only-of-type
:nth-of-type(n)
:nth-last-of-type(n)
```

### :has() — el "selector padre" revolucionario
```css
/* Contenedor que tiene imagen */
.tarjeta:has(img) { display: grid; grid-template-columns: auto 1fr; }

/* Form con input inválido */
form:has(:invalid) .btn-submit { opacity: 0.5; pointer-events: none; }

/* Li que tiene checkbox marcado */
li:has(input:checked) { background: #e8f5e9; text-decoration: line-through; }

/* Heading seguido de párrafo */
h2:has(+ p) { margin-bottom: 0.5rem; }

/* Nav que NO tiene menú hamburguesa abierto */
nav:not(:has(.menu-abierto)) .overlay { display: none; }

/* Hover en cualquier hijo */
.galeria:has(img:hover) { filter: brightness(0.8); }
.galeria:has(img:hover) img:hover { filter: brightness(1.25); }

/* Input con valor */
.campo:has(input:not(:placeholder-shown)) label {
  transform: translateY(-100%);
  font-size: 0.75rem;
}
```

### :is(), :where(), :not()
```css
/* :is() — agrupa selectores, toma la especificidad del más específico */
:is(h1, h2, h3, h4, h5, h6) { line-height: 1.2; }
:is(article, section, aside) :is(h1, h2, h3) { color: #111; }
:is(.boton, button):is(:hover, :focus) { background: darken(blue, 10%); }

/* :where() — como :is() pero CERO especificidad (ideal para resets) */
:where(h1, h2, h3, h4, h5, h6) { font-weight: bold; } /* Fácil de sobrescribir */
:where(ul, ol) { list-style: none; padding: 0; margin: 0; }

/* :not() — niega uno o más selectores */
p:not(.especial) { color: #555; }
:not(h1):not(h2) { font-size: 1rem; }
input:not([type="submit"]):not([type="reset"]):not([type="button"]) {
  border: 1px solid #ddd;
}
.lista > :not(:last-child) { border-bottom: 1px solid #eee; } /* Separador entre items */
```

### Pseudo-elementos
```css
::before              /* Antes del contenido */
::after               /* Después del contenido */
::first-line          /* Primera línea de texto */
::first-letter        /* Primera letra */
::selection           /* Texto seleccionado por el usuario */
::placeholder         /* Placeholder de input/textarea */
::marker              /* Marcador de lista (•, 1., etc.) */
::backdrop            /* Fondo del dialog/fullscreen/popover */
::spelling-error      /* Error ortográfico */
::grammar-error       /* Error gramatical */
::cue                 /* Subtítulos de video (VTT) */
::file-selector-button /* Botón de input[type="file"] */
::highlight(nombre)   /* Custom highlights (CSS Custom Highlight API) */
::part(nombre)        /* Part en shadow DOM */
::slotted(selector)   /* Elementos asignados a slots */
::scroll-marker       /* Para navegación en scroll */
::column              /* Columnas en multicol */

/* Uso avanzado de ::before y ::after */
.icono-externo::after {
  content: " ↗";
  font-size: 0.75em;
}

.contador {
  counter-increment: mi-contador;
}
.contador::before {
  content: counter(mi-contador) ". ";
}

.cita::before { content: "«"; }
.cita::after  { content: "»"; }
```

---

## 2. LA CASCADA — ESPECIFICIDAD Y CASCADE LAYERS

### Orden de la cascada (mayor a menor prioridad)
```
1. !important del navegador (accesibilidad)
2. !important del usuario
3. !important del autor (CSS del sitio)
4. Animaciones CSS
5. Estilos normales del autor
6. Estilos normales del usuario
7. Estilos normales del navegador
```

### Cálculo de especificidad
```
(a, b, c) donde:
a = Estilos inline (style="")          → (1, 0, 0, 0)
b = IDs (#id)                          → (0, 1, 0, 0)
c = Clases, atributos, pseudo-clases   → (0, 0, 1, 0)
d = Elementos, pseudo-elementos         → (0, 0, 0, 1)

Ejemplos:
*                   → (0,0,0,0)
h1                  → (0,0,0,1)
.clase              → (0,0,1,0)
#id                 → (0,1,0,0)
h1.clase            → (0,0,1,1)
#id h1.clase        → (0,1,1,1)
style=""            → (1,0,0,0)

:is(), :has(), :not() → La del selector más específico dentro
:where()             → (0,0,0,0) siempre
```

### @layer — Cascade Layers (CSS moderno)
```css
/* Definir el orden de las capas — las últimas ganan */
@layer reset, base, tokens, componentes, utilidades;

/* Capa de reset */
@layer reset {
  *, *::before, *::after { box-sizing: border-box; }
  * { margin: 0; }
  body { line-height: 1.5; -webkit-font-smoothing: antialiased; }
  img, video, svg { display: block; max-width: 100%; }
  input, textarea, select, button { font: inherit; }
  p, h1, h2, h3, h4, h5, h6 { overflow-wrap: break-word; }
}

/* Capa de tokens de diseño */
@layer tokens {
  :root {
    --color-primario: #3B82F6;
    --color-secundario: #6B7280;
    --espacio-base: 1rem;
    --fuente-heading: 'Playfair Display', serif;
    --fuente-cuerpo: 'Inter', sans-serif;
  }
}

/* Capa de componentes */
@layer componentes {
  .btn { padding: 0.5rem 1rem; border-radius: 0.375rem; }
}

/* Capa de utilidades — máxima prioridad sin !important */
@layer utilidades {
  .hidden { display: none; }
  .sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
  }
}

/* Sub-capas */
@layer framework {
  @layer base, componentes;
  @layer base { /* ... */ }
  @layer componentes { /* ... */ }
}

/* Revertir al estilo de la capa anterior */
.override { color: revert-layer; }
```

---

## 3. CUSTOM PROPERTIES (VARIABLES CSS)

```css
:root {
  /* === COLORES === */
  --color-primario-50:  #EFF6FF;
  --color-primario-100: #DBEAFE;
  --color-primario-200: #BFDBFE;
  --color-primario-300: #93C5FD;
  --color-primario-400: #60A5FA;
  --color-primario-500: #3B82F6;
  --color-primario-600: #2563EB;
  --color-primario-700: #1D4ED8;
  --color-primario-800: #1E40AF;
  --color-primario-900: #1E3A8A;

  --color-texto:        #111827;
  --color-texto-suave:  #6B7280;
  --color-fondo:        #FFFFFF;
  --color-borde:        #E5E7EB;
  --color-exito:        #22C55E;
  --color-advertencia:  #F59E0B;
  --color-error:        #EF4444;
  --color-info:         #3B82F6;

  /* === TIPOGRAFÍA === */
  --fuente-heading: 'Playfair Display', Georgia, serif;
  --fuente-cuerpo:  'Inter', system-ui, sans-serif;
  --fuente-mono:    'JetBrains Mono', 'Fira Code', Consolas, monospace;

  --tamaño-xs:   0.75rem;   /* 12px */
  --tamaño-sm:   0.875rem;  /* 14px */
  --tamaño-base: 1rem;      /* 16px */
  --tamaño-lg:   1.125rem;  /* 18px */
  --tamaño-xl:   1.25rem;   /* 20px */
  --tamaño-2xl:  1.5rem;    /* 24px */
  --tamaño-3xl:  1.875rem;  /* 30px */
  --tamaño-4xl:  2.25rem;   /* 36px */
  --tamaño-5xl:  3rem;      /* 48px */
  --tamaño-6xl:  3.75rem;   /* 60px */

  --peso-normal:     400;
  --peso-medio:      500;
  --peso-semibold:   600;
  --peso-bold:       700;
  --peso-extrabold:  800;
  --peso-black:      900;

  --altura-linea-ajustada:  1.1;
  --altura-linea-normal:    1.5;
  --altura-linea-relajada:  1.75;

  /* === ESPACIADO === */
  --espacio-1:  0.25rem;  /* 4px */
  --espacio-2:  0.5rem;   /* 8px */
  --espacio-3:  0.75rem;  /* 12px */
  --espacio-4:  1rem;     /* 16px */
  --espacio-5:  1.25rem;  /* 20px */
  --espacio-6:  1.5rem;   /* 24px */
  --espacio-8:  2rem;     /* 32px */
  --espacio-10: 2.5rem;   /* 40px */
  --espacio-12: 3rem;     /* 48px */
  --espacio-16: 4rem;     /* 64px */
  --espacio-20: 5rem;     /* 80px */
  --espacio-24: 6rem;     /* 96px */

  /* === BORDES === */
  --radio-sm:   0.125rem;  /* 2px */
  --radio-base: 0.25rem;   /* 4px */
  --radio-md:   0.375rem;  /* 6px */
  --radio-lg:   0.5rem;    /* 8px */
  --radio-xl:   0.75rem;   /* 12px */
  --radio-2xl:  1rem;      /* 16px */
  --radio-3xl:  1.5rem;    /* 24px */
  --radio-pill: 9999px;
  --radio-redondo: 50%;

  /* === SOMBRAS === */
  --sombra-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --sombra-sm: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --sombra-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --sombra-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --sombra-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
  --sombra-2xl: 0 25px 50px -12px rgb(0 0 0 / 0.25);
  --sombra-inner: inset 0 2px 4px 0 rgb(0 0 0 / 0.05);

  /* === TRANSICIONES === */
  --transicion-rápida: 100ms ease;
  --transicion-normal: 200ms ease;
  --transicion-lenta:  300ms ease;
  --transicion-muy-lenta: 500ms ease;

  /* === Z-INDEX === */
  --z-debajo:    -1;
  --z-base:       0;
  --z-elevado:   10;
  --z-menu:     100;
  --z-overlay:  200;
  --z-modal:    300;
  --z-notif:    400;
  --z-tooltip:  500;

  /* === BREAKPOINTS (como referencia, no usables en media queries) === */
  --bp-sm:  640px;
  --bp-md:  768px;
  --bp-lg: 1024px;
  --bp-xl: 1280px;

  /* === CONTENIDO MÁXIMO === */
  --ancho-max-sm:   640px;
  --ancho-max-md:   768px;
  --ancho-max-lg:  1024px;
  --ancho-max-xl:  1280px;
  --ancho-max-2xl: 1536px;
}

/* Tema oscuro */
@media (prefers-color-scheme: dark) {
  :root {
    --color-texto:   #F9FAFB;
    --color-texto-suave: #9CA3AF;
    --color-fondo:   #111827;
    --color-borde:   #374151;
  }
}

/* Uso de variables con fallback */
.elemento {
  color: var(--color-primario, #3B82F6);           /* Fallback simple */
  padding: var(--espacio-personalizado, var(--espacio-4, 1rem)); /* Fallback anidado */
}

/* Variables locales (scoped) */
.boton {
  --boton-bg: var(--color-primario-600);
  --boton-color: white;
  --boton-padding-x: var(--espacio-4);
  --boton-padding-y: var(--espacio-2);
  --boton-radio: var(--radio-md);

  background: var(--boton-bg);
  color: var(--boton-color);
  padding: var(--boton-padding-y) var(--boton-padding-x);
  border-radius: var(--boton-radio);
}

.boton--peligro {
  --boton-bg: var(--color-error);
}

/* Variables numéricas para cálculos */
:root {
  --columnas: 12;
  --ancho-columna: calc(100% / var(--columnas));
}

.col-4 {
  width: calc(var(--ancho-columna) * 4);
}
```

---

## 4. FLEXBOX — GUÍA COMPLETA

```css
/* CONTENEDOR FLEX */
.contenedor {
  display: flex;        /* o inline-flex */

  /* Eje principal */
  flex-direction: row;               /* → por defecto */
  flex-direction: row-reverse;       /* ← */
  flex-direction: column;            /* ↓ */
  flex-direction: column-reverse;    /* ↑ */

  /* Desbordamiento */
  flex-wrap: nowrap;    /* Por defecto — sin saltos */
  flex-wrap: wrap;      /* Con saltos */
  flex-wrap: wrap-reverse;

  /* Shorthand */
  flex-flow: row wrap;

  /* Alineación en eje principal */
  justify-content: flex-start;     /* Inicio */
  justify-content: flex-end;       /* Final */
  justify-content: center;         /* Centro */
  justify-content: space-between;  /* Espacio entre items */
  justify-content: space-around;   /* Espacio alrededor */
  justify-content: space-evenly;   /* Espacio uniforme */
  justify-content: stretch;        /* Estira (default en grid) */

  /* Alineación en eje secundario (una línea) */
  align-items: stretch;     /* Por defecto — estira */
  align-items: flex-start;  /* Inicio */
  align-items: flex-end;    /* Final */
  align-items: center;      /* Centro */
  align-items: baseline;    /* Línea base del texto */

  /* Alineación de líneas múltiples (cuando hay wrap) */
  align-content: flex-start;
  align-content: flex-end;
  align-content: center;
  align-content: space-between;
  align-content: space-around;
  align-content: space-evenly;
  align-content: stretch;  /* Por defecto */

  /* Atajo para justify-content + align-items */
  place-content: center space-between;

  /* Gap */
  gap: 1rem;            /* row-gap y column-gap */
  gap: 1rem 2rem;       /* row-gap | column-gap */
  row-gap: 1rem;
  column-gap: 2rem;
}

/* ITEMS FLEX */
.item {
  /* Crecimiento */
  flex-grow: 0;   /* Por defecto — no crece */
  flex-grow: 1;   /* Crece para llenar espacio */
  flex-grow: 2;   /* Crece el doble que flex-grow: 1 */

  /* Encogimiento */
  flex-shrink: 1;  /* Por defecto — se encoge si es necesario */
  flex-shrink: 0;  /* No se encoge nunca */

  /* Tamaño base */
  flex-basis: auto;   /* Por defecto — usa width/height */
  flex-basis: 0;      /* Parte de 0, distribuye todo */
  flex-basis: 200px;
  flex-basis: 30%;
  flex-basis: max-content;
  flex-basis: min-content;

  /* Shorthand flex */
  flex: 0 1 auto;    /* Por defecto: no crece, se encoge, auto basis */
  flex: 1;           /* flex: 1 1 0 — crece y se encoge por igual */
  flex: auto;        /* flex: 1 1 auto */
  flex: none;        /* flex: 0 0 auto — rígido */
  flex: 2 1 200px;   /* grow shrink basis */

  /* Orden visual (sin afectar DOM) */
  order: 0;    /* Por defecto */
  order: -1;   /* Al inicio */
  order: 1;    /* Al final */

  /* Alineación individual (sobreescribe align-items) */
  align-self: auto;
  align-self: flex-start;
  align-self: flex-end;
  align-self: center;
  align-self: stretch;
  align-self: baseline;

  /* justify-self (en flex solo funciona en overflowed axis) */
  margin-left: auto;  /* Truco para empujar items a la derecha */
}

/* PATRONES COMUNES FLEXBOX */

/* Centrado perfecto */
.centrar {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Sticky footer */
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
main { flex: 1; }

/* Holy Grail Layout */
.holy-grail {
  display: flex;
  gap: 1rem;
}
.holy-grail aside { flex: 0 0 200px; }
.holy-grail main  { flex: 1; }

/* Cards de igual altura */
.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}
.card {
  flex: 1 1 300px; /* Mínimo 300px, crece si hay espacio */
  display: flex;
  flex-direction: column;
}
.card__contenido { flex: 1; }
.card__footer    { margin-top: auto; }

/* Navegación con logo a la izquierda y links a la derecha */
nav {
  display: flex;
  align-items: center;
}
nav .logo { margin-right: auto; } /* Empuja el resto a la derecha */
```

---

## 5. CSS GRID — GUÍA COMPLETA

```css
/* CONTENEDOR GRID */
.grid {
  display: grid;  /* o inline-grid */

  /* Columnas explícitas */
  grid-template-columns: 200px 1fr 1fr;
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));  /* Responsive mágico */
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));   /* Auto-fit colapsa columnas vacías */
  grid-template-columns: [inicio] 1fr [mitad] 1fr [fin];        /* Named lines */
  grid-template-columns: minmax(150px, 1fr) 2fr minmax(100px, 300px);
  grid-template-columns: fit-content(300px) 1fr;
  grid-template-columns: subgrid;  /* Hereda columnas del padre */

  /* Filas explícitas */
  grid-template-rows: auto 1fr auto;
  grid-template-rows: repeat(3, 100px);
  grid-template-rows: subgrid;     /* Hereda filas del padre */

  /* Áreas nombradas */
  grid-template-areas:
    "header header header"
    "sidebar main    main"
    "footer footer footer";

  /* Shorthand grid-template */
  grid-template:
    "header" 80px
    "main"   1fr
    "footer" 60px
    / 1fr;  /* columna */

  /* Filas implícitas (las que se crean automáticamente) */
  grid-auto-rows: 100px;
  grid-auto-rows: minmax(100px, auto);
  grid-auto-rows: min-content;

  /* Columnas implícitas */
  grid-auto-columns: 1fr;

  /* Dirección de auto-placement */
  grid-auto-flow: row;            /* Por defecto */
  grid-auto-flow: column;
  grid-auto-flow: row dense;      /* Algoritmo denso — rellena huecos */
  grid-auto-flow: column dense;

  /* Shorthand grid */
  grid: auto-flow 80px / repeat(3, 1fr);

  /* Alineación del grid dentro del contenedor */
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;
  place-content: center; /* Ambos a la vez */

  /* Alineación de items dentro de su celda */
  justify-items: start | end | center | stretch;  /* Horizontal */
  align-items: start | end | center | stretch;    /* Vertical */
  place-items: center;  /* Ambos */

  /* Gap */
  gap: 1rem;
  gap: 1rem 2rem;
  row-gap: 1rem;
  column-gap: 2rem;
}

/* ITEMS GRID */
.item {
  /* Posicionamiento por línea */
  grid-column-start: 1;
  grid-column-end: 3;
  grid-column: 1 / 3;       /* start / end */
  grid-column: 1 / span 2;  /* start / duración */
  grid-column: span 2;      /* Ocupa 2 columnas desde donde esté */
  grid-column: 1 / -1;      /* De inicio a fin */

  grid-row-start: 1;
  grid-row-end: 3;
  grid-row: 1 / 3;
  grid-row: 1 / span 2;

  /* Shorthand */
  grid-area: 2 / 1 / 3 / 4;  /* row-start / col-start / row-end / col-end */

  /* Por nombre de área */
  grid-area: header;
  grid-area: sidebar;
  grid-area: main;
  grid-area: footer;

  /* Por named lines */
  grid-column: inicio / fin;

  /* Alineación individual */
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
  place-self: center;

  /* Orden visual */
  order: 1;
}

/* PATRONES AVANZADOS GRID */

/* Layout de página completo */
.pagina {
  display: grid;
  grid-template-areas:
    "header"
    "nav"
    "main"
    "sidebar"
    "footer";
  grid-template-rows: auto auto 1fr auto auto;
  min-height: 100vh;
}

@media (min-width: 768px) {
  .pagina {
    grid-template-areas:
      "header  header"
      "nav     nav"
      "sidebar main"
      "footer  footer";
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto auto 1fr auto;
  }
}

/* Cards responsive sin media queries */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(100%, 300px), 1fr));
  gap: 1.5rem;
}

/* Masonry-like con subgrid */
.masonry {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: masonry; /* Experimental — Chrome flag */
}

/* Superposición de elementos */
.stack {
  display: grid;
}
.stack > * {
  grid-area: 1 / 1; /* Todos en la misma celda */
}

/* RAM pattern — Repeat Auto Minmax */
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(200px, 100%), 1fr));
  gap: 1rem;
}

/* Tabla avanzada con grid */
.tabla-grid {
  display: grid;
  grid-template-columns: 2fr repeat(4, 1fr);
}
.tabla-grid > * {
  padding: 0.75rem;
  border-bottom: 1px solid #eee;
}

/* subgrid — Alineación entre componentes */
.tarjeta {
  display: grid;
  grid-template-rows: auto 1fr auto;
  row-gap: 1rem;
}

.lista-tarjetas {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
.lista-tarjetas .tarjeta {
  grid-row: span 3;
  display: grid;
  grid-template-rows: subgrid; /* Las tarjetas se alinean entre sí */
}
```

---

## 6. TIPOGRAFÍA CSS AVANZADA

```css
/* === FUENTES WEB === */
@font-face {
  font-family: 'MiFuente';
  src:
    url('/fonts/mifuente.woff2') format('woff2'),  /* Prioritario — mejor compresión */
    url('/fonts/mifuente.woff') format('woff');     /* Fallback */
  font-weight: 400;
  font-style: normal;
  font-display: swap;      /* swap: muestra fallback de inmediato, reemplaza cuando cargue */
  /* font-display: optional — solo usa si carga muy rápido */
  /* font-display: block — espera para mostrar */
  /* font-display: fallback — espera poco, si no usa fallback */
  unicode-range: U+0000-00FF, U+0131, U+0152-0153; /* Solo carga para estos caracteres */
}

/* Variable fonts */
@font-face {
  font-family: 'MiFuenteVariable';
  src: url('/fonts/mifuente-variable.woff2') format('woff2-variations');
  font-weight: 100 900;  /* Rango soportado */
  font-style: normal oblique 0deg 15deg;
}

.texto-variable {
  font-family: 'MiFuenteVariable', sans-serif;
  font-weight: 650;        /* Cualquier valor en el rango */
  font-variation-settings:
    'wght' 650,            /* Weight */
    'wdth' 80,             /* Width */
    'ital' 1,              /* Italic */
    'slnt' -5,             /* Slant */
    'GRAD' 100;            /* Grade (custom axis) */
}

/* Sistema de tipografía fluida */
:root {
  /* clamp(mínimo, preferido, máximo) */
  --texto-sm:   clamp(0.75rem, 2vw, 0.875rem);
  --texto-base: clamp(1rem, 2.5vw, 1.125rem);
  --texto-lg:   clamp(1.125rem, 3vw, 1.5rem);
  --texto-xl:   clamp(1.25rem, 4vw, 2rem);
  --texto-2xl:  clamp(1.5rem, 5vw, 3rem);
  --texto-3xl:  clamp(2rem, 7vw, 4.5rem);
  --texto-hero: clamp(2.5rem, 10vw, 7rem);
}

/* === PROPIEDADES DE FUENTE === */
.tipografia {
  /* Familia */
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  font-family: ui-serif, Georgia, Cambria, 'Times New Roman', Times, serif;
  font-family: ui-monospace, 'Cascadia Code', 'Source Code Pro', Menlo, Consolas, monospace;

  /* Tamaño y peso */
  font-size: 1rem;
  font-weight: 400;
  font-style: normal | italic | oblique 15deg;

  /* Altura de línea */
  line-height: 1.5;           /* Sin unidades — relativo al font-size */
  line-height: 1.5rem;        /* Con unidad — fija */
  line-height: normal;        /* ~1.2 según el navegador */

  /* Espaciado */
  letter-spacing: 0.025em;
  letter-spacing: -0.02em;    /* Negativo — bueno para headings grandes */
  word-spacing: 0.05em;

  /* Transformación */
  text-transform: uppercase | lowercase | capitalize | none;

  /* Decoración avanzada */
  text-decoration: underline;
  text-decoration: underline wavy #ff0000 2px;
  text-decoration-line: underline | overline | line-through | none;
  text-decoration-style: solid | dashed | dotted | double | wavy;
  text-decoration-color: currentColor;
  text-decoration-thickness: 2px | from-font | auto;
  text-underline-offset: 0.2em;

  /* Alineación */
  text-align: left | right | center | justify | start | end;
  text-align-last: justify;
  vertical-align: baseline | top | middle | bottom | super | sub | text-top | text-bottom;

  /* Indentación */
  text-indent: 2em;
  text-indent: 2em hanging;       /* Hanging indent */
  text-indent: 2em hanging each-line;

  /* Overflow */
  overflow-wrap: normal | break-word | anywhere;
  word-break: normal | break-all | keep-all | break-word;
  white-space: normal | nowrap | pre | pre-wrap | pre-line | break-spaces;
  white-space-collapse: collapse | discard | preserve | preserve-breaks | preserve-spaces;

  /* Ellipsis */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

  /* Multiline ellipsis */
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;

  /* Texto en columnas */
  text-overflow: clip | ellipsis | "..." | fade;

  /* Sombra de texto */
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
  text-shadow: 1px 1px 0 #000, 2px 2px 0 rgba(0,0,0,0.5); /* Múltiples */

  /* Rendering */
  -webkit-font-smoothing: antialiased;         /* macOS */
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility | optimizeSpeed | auto;

  /* OpenType features */
  font-feature-settings: "kern" 1, "liga" 1, "calt" 1, "ss01" 1;
  font-kerning: auto | normal | none;
  font-variant-ligatures: common-ligatures | no-common-ligatures | discretionary-ligatures;
  font-variant-numeric: lining-nums | oldstyle-nums | proportional-nums | tabular-nums | ordinal | slashed-zero;
  font-variant-caps: small-caps | all-small-caps | petite-caps | unicase;
  font-optical-sizing: auto | none;

  /* Hiphenación automática */
  hyphens: auto | manual | none;
  hyphenate-limit-chars: 6 3 2;  /* total min-antes min-después */

  /* Escritura */
  writing-mode: horizontal-tb | vertical-rl | vertical-lr | sideways-rl | sideways-lr;
  text-orientation: mixed | upright | sideways;
  direction: ltr | rtl;
}

/* Balance de texto en headings */
h1, h2, h3 {
  text-wrap: balance;   /* Distribuye el texto equitativamente en líneas */
}

p {
  text-wrap: pretty;   /* Evita huérfanas al final de párrafos */
}
```

---

## 7. COLORES Y GRADIENTES

```css
/* === FORMATOS DE COLOR === */
.colores {
  /* Named colors */
  color: red;
  color: transparent;
  color: currentColor;  /* Hereda el color actual del texto */

  /* Hex */
  color: #3B82F6;
  color: #3B82F6CC;   /* Con alpha (CC = 80% opacidad) */
  color: #38F;         /* Shorthand */
  color: #38FC;        /* Shorthand con alpha */

  /* RGB */
  color: rgb(59 130 246);          /* Sintaxis moderna sin comas */
  color: rgb(59 130 246 / 0.8);    /* Con alpha */
  color: rgb(59, 130, 246);        /* Sintaxis legada */
  color: rgba(59, 130, 246, 0.8);  /* Legada con alpha */

  /* HSL — más intuitivo para diseñadores */
  color: hsl(213deg 94% 60%);
  color: hsl(213deg 94% 60% / 0.8);
  color: hsl(213, 94%, 60%);       /* Legado */

  /* HWB — Hue Whiteness Blackness */
  color: hwb(213deg 23% 4%);

  /* LAB — Perceptualmente uniforme */
  color: lab(50 -20 30);
  color: lab(50 -20 30 / 0.8);

  /* OKLCH — Mejor espacio de color moderno */
  color: oklch(60% 0.2 240deg);
  color: oklch(60% 0.2 240deg / 0.8);

  /* LCH */
  color: lch(60 40 240);

  /* OKLAB */
  color: oklab(0.6 -0.1 -0.1);

  /* P3 (pantallas con gama amplia) */
  color: color(display-p3 0.5 0.8 1.0);
  color: color(srgb 0.2 0.5 1.0 / 0.8);

  /* Relative colors (CSS Color Level 5) */
  color: hsl(from var(--color-base) h s calc(l - 20%));
  color: oklch(from var(--color-primario) calc(l - 0.1) c h);
}

/* === GRADIENTES === */
.gradientes {
  /* Linear */
  background: linear-gradient(to right, #667eea, #764ba2);
  background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
  background: linear-gradient(to bottom, transparent, rgba(0,0,0,0.7));
  background: linear-gradient(red, yellow 30%, green 60%, blue);  /* Múltiples paradas */
  background: linear-gradient(in oklch, blue, red); /* En espacio de color específico */

  /* Radial */
  background: radial-gradient(circle, #667eea, #764ba2);
  background: radial-gradient(circle at center top, #fff, transparent);
  background: radial-gradient(ellipse 60% 40% at 50% 50%, #fff, transparent);
  background: radial-gradient(circle at 70% 30%, #fdbb2d 0%, #22c1c3 50%, #fdbb2d 100%);

  /* Conic */
  background: conic-gradient(from 0deg, red, yellow, green, blue, red);
  background: conic-gradient(from 45deg at center, #f6d365, #fda085, #f6d365);

  /* Repetición */
  background: repeating-linear-gradient(45deg, #ccc 0px, #ccc 10px, transparent 10px, transparent 20px);
  background: repeating-radial-gradient(circle at center, #fff 0, #fff 5px, #f0f0f0 5px, #f0f0f0 10px);
  background: repeating-conic-gradient(from 0deg, #f0f0f0 0deg, #f0f0f0 10deg, white 10deg, white 20deg);

  /* Múltiples backgrounds */
  background:
    url('patron.svg') repeat,
    linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

/* === FONDOS AVANZADOS === */
.fondos {
  background-color: #f0f4f8;
  background-image: url('imagen.jpg');
  background-size: cover | contain | auto | 100% 100% | 50px 50px;
  background-position: center | top left | 50% 50% | 20px 30px;
  background-repeat: no-repeat | repeat | repeat-x | repeat-y | space | round;
  background-attachment: scroll | fixed | local;
  background-origin: content-box | padding-box | border-box;
  background-clip: content-box | padding-box | border-box | text;

  /* Texto con gradiente/imagen */
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;

  /* Shorthand */
  background: #f0f4f8 url('imagen.jpg') no-repeat center / cover;
}

/* mix-blend-mode */
.overlay {
  mix-blend-mode: multiply | screen | overlay | darken | lighten |
                  color-dodge | color-burn | hard-light | soft-light |
                  difference | exclusion | hue | saturation | color | luminosity;
}

/* Filtros */
.filtros {
  filter: blur(4px);
  filter: brightness(0.8);
  filter: contrast(1.2);
  filter: grayscale(100%);
  filter: hue-rotate(90deg);
  filter: invert(100%);
  filter: opacity(0.5);
  filter: saturate(2);
  filter: sepia(100%);
  filter: drop-shadow(4px 4px 8px rgba(0,0,0,0.5)); /* Respeta transparencia */
  filter: blur(2px) brightness(0.8) contrast(1.1) saturate(0.8); /* Múltiples */

  /* Filtros de fondo (frosted glass) */
  backdrop-filter: blur(10px) brightness(0.9);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: blur(10px);
}
```

---

## 8. ANIMACIONES Y TRANSICIONES

```css
/* === TRANSICIONES === */
.elemento {
  /* Propiedad | duración | función de tiempo | retraso */
  transition: color 200ms ease, background 200ms ease;
  transition: all 300ms ease-in-out;
  transition: none; /* Deshabilitar */

  /* Funciones de tiempo */
  transition-timing-function: ease;          /* Por defecto */
  transition-timing-function: linear;
  transition-timing-function: ease-in;
  transition-timing-function: ease-out;
  transition-timing-function: ease-in-out;
  transition-timing-function: step-start;
  transition-timing-function: step-end;
  transition-timing-function: steps(4, end);
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.265, 1.55);  /* Bounce */
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);            /* Material ease */

  /* Propiedades que son animables con buena performance */
  /* Solo estas no causan reflow ni repaint: */
  /* transform, opacity, filter, backdrop-filter, clip-path */
}

/* === KEYFRAME ANIMATIONS === */
@keyframes aparecer {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes latido {
  0%   { transform: scale(1); }
  50%  { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@keyframes rotar {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-8px); }
  20%, 40%, 60%, 80% { transform: translateX(8px); }
}

@keyframes ola {
  0%   { transform: rotate(0deg); }
  10%  { transform: rotate(14deg); }
  20%  { transform: rotate(-8deg); }
  30%  { transform: rotate(14deg); }
  40%  { transform: rotate(-4deg); }
  50%  { transform: rotate(10deg); }
  60%  { transform: rotate(0deg); }
  100% { transform: rotate(0deg); }
}

@keyframes brillar {
  0%   { box-shadow: 0 0 5px  rgba(59,130,246,0.5); }
  50%  { box-shadow: 0 0 20px rgba(59,130,246,0.8); }
  100% { box-shadow: 0 0 5px  rgba(59,130,246,0.5); }
}

@keyframes typing {
  from { width: 0; }
  to   { width: 100%; }
}

@keyframes blink-cursor {
  from, to { border-color: transparent; }
  50%      { border-color: currentColor; }
}

/* === PROPIEDADES DE ANIMACIÓN === */
.animado {
  animation-name: aparecer;
  animation-duration: 0.5s;
  animation-timing-function: ease-out;
  animation-delay: 0.2s;
  animation-iteration-count: 1 | infinite | 3;
  animation-direction: normal | reverse | alternate | alternate-reverse;
  animation-fill-mode: none | forwards | backwards | both;
  animation-play-state: running | paused;
  animation-timeline: auto | scroll() | view();  /* CSS Animation Level 2 */

  /* Shorthand */
  animation: aparecer 0.5s ease-out 0.2s 1 normal both;

  /* Múltiples animaciones */
  animation:
    aparecer 0.5s ease-out both,
    latido 2s ease-in-out infinite 0.5s;
}

/* === SCROLL-DRIVEN ANIMATIONS (CSS moderno) === */

/* Animación ligada al scroll de la página */
@keyframes progreso-scroll {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}

.barra-progreso {
  transform-origin: left;
  animation: progreso-scroll linear;
  animation-timeline: scroll(root);
  animation-fill-mode: both;
}

/* Animación al entrar en viewport */
@keyframes revelar {
  from {
    opacity: 0;
    transform: translateY(50px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.revelar-scroll {
  animation: revelar linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 40%;
}

/* Parallax con scroll */
.parallax {
  animation: parallax-efecto linear;
  animation-timeline: scroll(root);
}

@keyframes parallax-efecto {
  from { transform: translateY(0); }
  to   { transform: translateY(-50%); }
}

/* === VIEW TRANSITIONS API === */
/* En CSS */
::view-transition-old(root) {
  animation: 200ms ease-out both slide-out;
}
::view-transition-new(root) {
  animation: 200ms ease-in both slide-in;
}

.elemento-especial {
  view-transition-name: tarjeta-hero;
}

/* === TRANSFORMS === */
.transformar {
  /* 2D */
  transform: translateX(100px);
  transform: translateY(-50%);
  transform: translate(100px, -50%);
  transform: scaleX(1.5);
  transform: scaleY(0.8);
  transform: scale(1.2);
  transform: rotate(45deg);
  transform: skewX(10deg);
  transform: skewY(-5deg);
  transform: matrix(a, b, c, d, tx, ty);

  /* 3D */
  transform: translateZ(100px);
  transform: translate3d(100px, 50px, 0);
  transform: scale3d(1.2, 1.2, 1);
  transform: rotateX(45deg);
  transform: rotateY(45deg);
  transform: rotateZ(45deg);
  transform: rotate3d(1, 1, 0, 45deg);
  transform: perspective(1000px) rotateY(30deg);
  transform: matrix3d(...);

  /* Propiedades de perspectiva */
  perspective: 1000px;       /* En el contenedor padre */
  perspective-origin: 50% 50%;
  transform-style: flat | preserve-3d;
  backface-visibility: visible | hidden;

  /* Punto de origen */
  transform-origin: 50% 50%;       /* Por defecto — centro */
  transform-origin: top left;
  transform-origin: 0 0;
  transform-origin: center bottom;
  transform-origin: 100% 100%;

  /* Individual (CSS moderno — mejor performance) */
  translate: 100px 50px;
  rotate: 45deg;
  scale: 1.2;
}
```

---

## 9. RESPONSIVE DESIGN MODERNO

```css
/* === MEDIA QUERIES === */

/* Breakpoints (mobile-first) */
/* Base: móvil (< 640px) */

@media (min-width: 640px)  { /* sm: tablets pequeños */ }
@media (min-width: 768px)  { /* md: tablets */ }
@media (min-width: 1024px) { /* lg: laptops */ }
@media (min-width: 1280px) { /* xl: desktops */ }
@media (min-width: 1536px) { /* 2xl: pantallas grandes */ }

/* Desktop-first */
@media (max-width: 1279px) { /* Hasta xl */ }
@media (max-width: 1023px) { /* Hasta lg */ }
@media (max-width: 767px)  { /* Hasta md */ }
@media (max-width: 639px)  { /* Solo móvil */ }

/* Rangos */
@media (min-width: 768px) and (max-width: 1023px) { /* Solo tablet */ }
@media (768px <= width <= 1023px) { /* Sintaxis moderna */ }

/* Orientación */
@media (orientation: landscape) { }
@media (orientation: portrait)  { }

/* Resolución (para retina) */
@media (min-resolution: 2dppx)   { }  /* 2x */
@media (min-resolution: 192dpi)  { }
@media (-webkit-min-device-pixel-ratio: 2) { }  /* Safari */

/* Preferencias del usuario */
@media (prefers-color-scheme: dark)         { }
@media (prefers-color-scheme: light)        { }
@media (prefers-reduced-motion: reduce)     { }  /* Menos animaciones */
@media (prefers-reduced-motion: no-preference) { }
@media (prefers-contrast: high)             { }  /* Alto contraste */
@media (prefers-contrast: low)              { }
@media (prefers-reduced-data: reduce)       { }  /* Ahorro de datos */
@media (prefers-reduced-transparency: reduce) { }
@media (forced-colors: active)              { }  /* Windows High Contrast Mode */

/* Capacidades del dispositivo */
@media (hover: hover) { }      /* Dispositivo con hover (mouse) */
@media (hover: none)  { }      /* Táctil */
@media (pointer: fine)   { }   /* Mouse preciso */
@media (pointer: coarse) { }   /* Táctil (área grande) */
@media (pointer: none)   { }   /* Sin puntero */
@media (any-hover: hover) { }  /* Al menos un input con hover */

/* Display */
@media (display-mode: standalone) { } /* PWA instalada */
@media (display-mode: fullscreen) { }

/* Tipo de medio */
@media print { }
@media screen { }
@media speech { }

/* Reducción de movimiento — CRÍTICO para accesibilidad */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* === CONTAINER QUERIES === */

/* Definir el contenedor */
.componente-padre {
  container-type: inline-size;  /* Responsive al ancho inline */
  /* container-type: size;       -- Al ancho y alto */
  /* container-type: normal;     -- Solo estilos, no layout queries */
  container-name: tarjeta;      /* Nombre opcional */
}

/* Consultar al contenedor (no al viewport) */
@container (min-width: 400px) {
  .componente {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}

@container tarjeta (min-width: 600px) {
  .tarjeta__titulo { font-size: 1.5rem; }
}

/* Container style queries */
@container style(--variante: destacado) {
  .tarjeta { border: 2px solid gold; }
}

/* === UNIDADES MODERNAS === */

/* Viewport */
.elem {
  width: 100vw;   /* viewport width */
  height: 100vh;  /* viewport height */
  width: 100svw;  /* small viewport width (excluye barras móviles) */
  height: 100svh; /* small viewport height */
  width: 100lvw;  /* large viewport width */
  height: 100lvh; /* large viewport height */
  width: 100dvw;  /* dynamic viewport width (cambia según barras) */
  height: 100dvh; /* dynamic viewport height — ¡úsalo en lugar de vh en móviles! */
  height: 100cqh; /* container query height */
  width: 100cqw;  /* container query width */
  font-size: 5cqi;  /* container query inline */
  font-size: 5cqb;  /* container query block */
}

/* Funciones matemáticas */
.calc {
  width: calc(100% - 2rem);
  width: calc(100vw - var(--sidebar-ancho, 0px));
  font-size: clamp(1rem, 2.5vw, 2rem);             /* min, preferido, max */
  padding: min(2rem, 5vw);                          /* El menor */
  margin: max(1rem, 3vh);                           /* El mayor */
  width: round(50.3px, 1px);                        /* Redondeo */

  /* Trigonometría CSS */
  transform: rotate(calc(sin(var(--angulo)) * 45deg));
  transform: translateX(calc(cos(45deg) * 100px));
  width: calc(tan(30deg) * 100px);
  transform: rotateZ(calc(atan2(1, 1) * 1rad));
}
```

---

## 10. LAYOUT AVANZADO

```css
/* === BOX MODEL === */
.caja {
  /* Display */
  display: block | inline | inline-block | flex | inline-flex | grid | inline-grid
           | contents | none | list-item | table | table-cell | table-row;

  /* Tamaño */
  width: 300px | 50% | auto | max-content | min-content | fit-content(200px);
  height: 200px | 50vh | auto | min-content | max-content;
  min-width: 200px;
  max-width: 1200px;
  min-height: 100px;
  max-height: none;

  /* Box sizing */
  box-sizing: content-box;  /* Default: padding/border se añaden al width */
  box-sizing: border-box;   /* padding/border incluidos en el width */

  /* Márgenes */
  margin: 1rem;
  margin: 1rem 2rem;         /* vertical | horizontal */
  margin: 1rem 2rem 3rem;    /* top | horizontal | bottom */
  margin: 1rem 2rem 3rem 4rem; /* top | right | bottom | left */
  margin-inline: auto;       /* Centrado horizontal lógico */
  margin-block: 1rem;        /* Vertical lógico */
  margin-inline-start: 1rem;
  margin-inline-end: 1rem;
  margin-block-start: 1rem;
  margin-block-end: 1rem;

  /* Padding */
  padding: 1rem;
  padding-inline: 2rem;
  padding-block: 1rem;
  padding-inline-start: 2rem;

  /* Borde */
  border: 1px solid #ddd;
  border-width: 2px;
  border-style: solid | dashed | dotted | double | groove | ridge | inset | outset | none | hidden;
  border-color: #ddd;
  border-radius: 8px;
  border-radius: 8px 4px 8px 4px;         /* top-left, top-right, bottom-right, bottom-left */
  border-radius: 50% / 30%;               /* horizontal / vertical */
  border-top-left-radius: 50% 30%;
  border-image: linear-gradient(135deg, #667eea, #764ba2) 1;
  border-start-start-radius: 8px;         /* Lógico */

  /* Outline */
  outline: 2px solid #3B82F6;
  outline-offset: 4px;   /* Separa el outline del borde */
  outline: none;         /* ¡Nunca sin alternativa de foco! */

  /* Overflow */
  overflow: visible | hidden | scroll | auto | clip;
  overflow-x: hidden;
  overflow-y: auto;
  overflow-clip-margin: 10px;

  /* Posicionamiento */
  position: static | relative | absolute | fixed | sticky;
  top: 0; right: 0; bottom: 0; left: 0;
  inset: 0;             /* top + right + bottom + left = 0 */
  inset: 10px 20px;     /* vertical | horizontal */
  inset-block: 10px;    /* top + bottom */
  inset-inline: 20px;   /* left + right */
  z-index: 10;

  /* Float (legado, preferir flex/grid) */
  float: left | right | none;
  clear: left | right | both | none | inline-start | inline-end;

  /* Columnas */
  columns: 3;
  columns: 200px;        /* Auto-fill columns of 200px */
  column-count: 3;
  column-width: 200px;
  column-gap: 2rem;
  column-rule: 1px solid #ddd;
  column-span: none | all;  /* En items: all = ocupa todas las columnas */
  break-inside: avoid;      /* Evitar salto de columna dentro del elemento */
  break-before: column;
  break-after: column;
}

/* === ASPECT RATIO === */
.proporcional {
  aspect-ratio: 16 / 9;
  aspect-ratio: 1;       /* Cuadrado */
  aspect-ratio: 4 / 3;
  aspect-ratio: auto;    /* Para imágenes con dimensiones intrínsecas */
}

/* Patrón video responsive sin JS */
.video-responsive {
  aspect-ratio: 16 / 9;
  width: 100%;
}

/* === OBJECT FIT para imágenes y videos === */
img.cover {
  width: 100%;
  height: 300px;
  object-fit: cover;             /* Cubre el área (puede recortar) */
  object-position: center top;   /* Dónde anclar */
}

img.contain  { object-fit: contain; }   /* Cabe completa */
img.fill     { object-fit: fill; }      /* Estira (distorsiona) */
img.scale-down { object-fit: scale-down; } /* El menor entre none y contain */
img.none     { object-fit: none; }      /* Tamaño original */
```

---

## 11. SCROLL Y OVERSCROLL

```css
/* === SCROLL BEHAVIOR === */
html {
  scroll-behavior: smooth;     /* Scroll suave global */
  scroll-behavior: auto;       /* Instantáneo */
}

/* Respetar preferencia de movimiento */
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
}

/* === SCROLL SNAP === */
.carrusel {
  display: flex;
  overflow-x: scroll;
  scroll-snap-type: x mandatory;      /* Eje | comportamiento */
  scroll-snap-type: y proximity;
  scroll-snap-type: both mandatory;
  overscroll-behavior-x: contain;     /* Evita que el scroll se propague */
  -webkit-overflow-scrolling: touch;  /* Scroll suave en iOS (legado) */
  scrollbar-width: none;              /* Ocultar scrollbar */
}

.carrusel::-webkit-scrollbar { display: none; }

.item-carrusel {
  flex: 0 0 100%;
  scroll-snap-align: start | center | end;
  scroll-snap-stop: normal | always;   /* always = no saltar items */
  scroll-margin: 1rem;    /* Margen al hacer snap */
}

/* === SCROLL PADDING (para fixed headers) === */
html {
  scroll-padding-top: 80px;   /* Altura del header fijo */
}

/* Por elemento */
.seccion {
  scroll-margin-top: 100px;
}

/* === SCROLLBAR ESTILIZADO === */
/* Estándar (Firefox, Edge) */
.contenedor {
  scrollbar-width: thin | auto | none;
  scrollbar-color: #3B82F6 #E5E7EB;  /* thumb | track */
  scrollbar-gutter: stable;          /* Reserva espacio para scrollbar (evita layout shift) */
}

/* WebKit (Chrome, Safari) */
.contenedor::-webkit-scrollbar { width: 8px; height: 8px; }
.contenedor::-webkit-scrollbar-track { background: #f1f1f1; border-radius: 4px; }
.contenedor::-webkit-scrollbar-thumb { background: #3B82F6; border-radius: 4px; }
.contenedor::-webkit-scrollbar-thumb:hover { background: #2563EB; }
.contenedor::-webkit-scrollbar-corner { background: transparent; }

/* === OVERSCROLL === */
.modal {
  overscroll-behavior: contain;  /* Evita que el scroll del modal mueva la página */
  overscroll-behavior-y: none;   /* Elimina el efecto rebote */
}
```

---

## 12. SHAPES Y CLIP PATH

```css
/* === CLIP PATH === */
.formas {
  clip-path: circle(50%);
  clip-path: circle(50% at 50% 50%);
  clip-path: ellipse(50% 40% at center);
  clip-path: inset(10px);                        /* Rectángulo con margen */
  clip-path: inset(10px round 8px);             /* Con bordes redondeados */
  clip-path: inset(10px 20px 30px 40px);
  clip-path: polygon(0 0, 100% 0, 100% 80%, 80% 100%, 0 100%);  /* Polígono */
  clip-path: path('M 0 0 L 100 0 L 100 80 Q 80 100 0 100 Z');  /* SVG path */

  /* Hover con transición */
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
  transition: clip-path 0.4s ease;
}
.formas:hover {
  clip-path: polygon(10% 10%, 90% 10%, 90% 90%, 10% 90%);
}

/* === SHAPE OUTSIDE (texto alrededor) === */
.float-imagen {
  float: left;
  shape-outside: circle(50%);
  shape-outside: polygon(0 0, 100% 0, 50% 100%);
  shape-outside: url('imagen.png');               /* Usa la transparencia */
  shape-margin: 1rem;
  shape-image-threshold: 0.5;
}

/* === MASK === */
.mascara {
  mask-image: url('mascara.png');
  mask-image: linear-gradient(to right, transparent, black);
  mask-size: cover;
  mask-position: center;
  mask-repeat: no-repeat;
  mask-composite: intersect | subtract | add | exclude;
  -webkit-mask-image: linear-gradient(to right, transparent, black);
}
```

---

## 13. ACCESIBILIDAD EN CSS

```css
/* === FOCO ACCESIBLE === */

/* Nunca deshabilitar el foco sin alternativa */
:focus {
  outline: 2px solid #3B82F6;
  outline-offset: 2px;
}

/* Foco visible solo cuando se navega con teclado */
:focus:not(:focus-visible) {
  outline: none;
}

:focus-visible {
  outline: 2px solid #3B82F6;
  outline-offset: 2px;
  border-radius: 2px;
}

/* === OCULTAR VISUALMENTE PERO NO DE LECTORES === */
.sr-only,
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* Versión con clip-path (más moderna) */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip-path: inset(50%);
  white-space: nowrap;
}

/* Mostrar al enfocar (skip links) */
.sr-only:focus,
.sr-only:active {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
  white-space: normal;
}

/* === MOTION === */
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}

/* === CONTRASTE Y COLOR === */
@media (forced-colors: active) {
  /* Windows High Contrast Mode */
  .boton {
    border: 2px solid ButtonText;
    forced-color-adjust: auto; /* o none para mantener colores propios */
  }
}

@media (prefers-contrast: more) {
  :root {
    --color-borde: #000;
    --color-texto-suave: #333;
  }
}

/* === PRINT === */
@media print {
  *,
  *::before,
  *::after {
    background: transparent !important;
    color: black !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }

  a[href]::after { content: " (" attr(href) ")"; }
  abbr[title]::after { content: " (" attr(title) ")"; }

  pre, blockquote { border: 1px solid #999; page-break-inside: avoid; }
  thead { display: table-header-group; }
  tr, img { page-break-inside: avoid; }
  img { max-width: 100% !important; }
  h2, h3 { page-break-after: avoid; }
  .no-print { display: none !important; }
}
```

---

## 14. CSS AVANZADO — CARACTERÍSTICAS MODERNAS

```css
/* === @scope === */
@scope (.tarjeta) {
  /* Solo afecta elementos dentro de .tarjeta */
  .titulo { font-size: 1.5rem; }
  img { border-radius: 8px; }
}

@scope (.tarjeta) to (.contenido) {
  /* Desde .tarjeta hasta (pero sin incluir) .contenido */
  p { color: #666; }
}

/* === @starting-style === */
/* Para animar elementos que acaban de aparecer en el DOM o salen del display:none */
.popup {
  transition: opacity 0.3s, transform 0.3s;
  opacity: 1;
  transform: translateY(0);
}

@starting-style {
  .popup {
    opacity: 0;
    transform: translateY(-10px);
  }
}

/* === color-scheme === */
:root {
  color-scheme: light dark;     /* Soporta ambos */
  color-scheme: light;
  color-scheme: dark;
}

/* === light-dark() función === */
.elemento {
  color: light-dark(#333, #f0f0f0);          /* Claro | Oscuro */
  background: light-dark(white, #1a1a2e);
}

/* === Counters avanzados === */
ol {
  counter-reset: lista-items;
}
li {
  counter-increment: lista-items;
}
li::before {
  content: counter(lista-items, decimal-leading-zero) ". ";
}

/* Contadores anidados */
ol ol {
  counter-reset: sub-lista;
}
ol ol li::before {
  content: counter(lista-items) "." counter(sub-lista);
  counter-increment: sub-lista;
}

/* === Custom Properties Avanzadas === */
/* Registro de tipos para animación de variables */
@property --progreso {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 0%;
}

.barra {
  --progreso: 0%;
  background: linear-gradient(to right, blue var(--progreso), gray var(--progreso));
  transition: --progreso 1s ease;
}
.barra.activo { --progreso: 75%; }

/* === :nth-child avanzado === */
/* Seleccionar del 3 al 7 */
li:nth-child(n+3):nth-child(-n+7) { background: yellow; }

/* Últimos 3 */
li:nth-last-child(-n+3) { font-weight: bold; }

/* Impar de los que tienen clase .especial */
:nth-child(odd of .especial) { background: #f0f0f0; }

/* === Funciones de color avanzadas === */
.color-relativo {
  /* Ajustar luminosidad de un color base */
  --color-hover: oklch(from var(--color-base) calc(l + 0.1) c h);

  /* Crear paleta automáticamente */
  --color-100: oklch(from var(--color-primario) 0.95 calc(c * 0.2) h);
  --color-500: var(--color-primario);
  --color-900: oklch(from var(--color-primario) 0.25 calc(c * 0.8) h);
}
```

---

## 15. ARQUITECTURA CSS — METODOLOGÍAS

### BEM (Block Element Modifier)
```css
/* Bloque */
.tarjeta { }

/* Elemento */
.tarjeta__imagen { }
.tarjeta__titulo { }
.tarjeta__descripcion { }
.tarjeta__acciones { }

/* Modificador */
.tarjeta--destacada { }
.tarjeta--horizontal { }
.tarjeta__boton--primario { }
.tarjeta__boton--secundario { }

/* Con estado */
.tarjeta.is-cargando { }
.tarjeta.is-seleccionada { }
.tarjeta--activa { }
```

### ITCSS (Inverted Triangle CSS)
```css
/* 1. Settings — Variables y config (sin CSS) */
/* 2. Tools — Mixins y funciones (sin CSS) */
/* 3. Generic — Normalize, box-sizing */
/* 4. Elements — Estilos de elementos HTML */
/* 5. Objects — Patrones de layout sin estilos visuales */
/* 6. Components — UI componentes */
/* 7. Utilities — Clases de utilidad (alta especificidad) */
```

### Utility-first (estilo Tailwind)
```css
/* Clases atómicas de utilidad */
.flex        { display: flex; }
.grid        { display: grid; }
.hidden      { display: none; }
.block       { display: block; }

.items-center { align-items: center; }
.justify-center { justify-content: center; }

.p-4         { padding: 1rem; }
.px-4        { padding-inline: 1rem; }
.py-2        { padding-block: 0.5rem; }
.m-auto      { margin: auto; }
.mt-4        { margin-top: 1rem; }

.text-sm     { font-size: 0.875rem; }
.text-lg     { font-size: 1.125rem; }
.font-bold   { font-weight: 700; }
.text-center { text-align: center; }

.rounded     { border-radius: 0.25rem; }
.rounded-lg  { border-radius: 0.5rem; }
.rounded-full { border-radius: 9999px; }

.shadow      { box-shadow: 0 1px 3px 0 rgb(0 0 0/0.1); }
.opacity-50  { opacity: 0.5; }

.w-full      { width: 100%; }
.h-screen    { height: 100vh; }
.max-w-4xl   { max-width: 56rem; }

.truncate    { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.sr-only     { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0,0,0,0); }
```

---

## 16. COMPONENTES CSS AVANZADOS

### Toggle switch (solo CSS)
```css
.toggle {
  appearance: none;
  -webkit-appearance: none;
  width: 3rem;
  height: 1.5rem;
  background: #ddd;
  border-radius: 9999px;
  cursor: pointer;
  position: relative;
  transition: background 200ms;
}

.toggle::after {
  content: '';
  position: absolute;
  width: 1.25rem;
  height: 1.25rem;
  background: white;
  border-radius: 50%;
  top: 0.125rem;
  left: 0.125rem;
  transition: transform 200ms;
  box-shadow: 0 1px 3px rgba(0,0,0,0.2);
}

.toggle:checked { background: #3B82F6; }
.toggle:checked::after { transform: translateX(1.5rem); }
.toggle:focus-visible { outline: 2px solid #3B82F6; outline-offset: 2px; }
```

### Custom checkbox
```css
.checkbox-custom {
  appearance: none;
  -webkit-appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  position: relative;
  transition: all 200ms;
  flex-shrink: 0;
}

.checkbox-custom:checked {
  background: #3B82F6;
  border-color: #3B82F6;
}

.checkbox-custom:checked::after {
  content: '';
  position: absolute;
  left: 3px;
  top: 0px;
  width: 6px;
  height: 11px;
  border: 2px solid white;
  border-top: none;
  border-left: none;
  transform: rotate(45deg);
}

.checkbox-custom:indeterminate {
  background: #3B82F6;
  border-color: #3B82F6;
}
.checkbox-custom:indeterminate::after {
  content: '';
  position: absolute;
  left: 2px;
  top: 50%;
  width: 12px;
  height: 2px;
  background: white;
  transform: translateY(-50%);
}

.checkbox-custom:focus-visible { outline: 2px solid #3B82F6; outline-offset: 2px; }
```

### Skeleton loading
```css
@keyframes skeleton {
  from { background-position: -200px 0; }
  to   { background-position: calc(200px + 100%) 0; }
}

.skeleton {
  background: linear-gradient(90deg,
    #f0f0f0 25%,
    #e0e0e0 50%,
    #f0f0f0 75%
  );
  background-size: 200px 100%;
  animation: skeleton 1.5s infinite;
  border-radius: 4px;
}

.skeleton-texto  { height: 1rem; margin-bottom: 0.5rem; }
.skeleton-titulo { height: 1.5rem; width: 60%; margin-bottom: 1rem; }
.skeleton-imagen { aspect-ratio: 16/9; }
```

---

## 17. CHECKLIST Y BUENAS PRÁCTICAS CSS

```
ORGANIZACIÓN
□ Variables CSS definidas en :root
□ Uso de @layer para gestionar cascada
□ Mobile-first approach
□ Metodología consistente (BEM, etc.)

PERFORMANCE
□ Sin @import en CSS (usar link en HTML o bundler)
□ Propiedades animadas: solo transform y opacity
□ will-change solo cuando sea necesario (y eliminarlo después)
□ contain: layout style paint para componentes aislados
□ content-visibility: auto para elementos fuera del viewport
□ Fuentes con font-display: swap

ACCESIBILIDAD
□ Nunca outline: none sin alternativa
□ :focus-visible implementado
□ prefers-reduced-motion respetado
□ Suficiente contraste de color (WCAG AA: 4.5:1)
□ No comunicar información solo por color
□ .sr-only para texto solo para lectores de pantalla

RESPONSIVE
□ Mobile-first (min-width)
□ No usar px fijos para media queries
□ Container queries para componentes complejos
□ Imágenes con max-width: 100%
□ Texto fluido con clamp()
□ dvh en lugar de vh para móviles

MANTENIBILIDAD
□ Sin !important (salvo utilities y resets)
□ Selectores de baja especificidad
□ Sin ID selectors en CSS
□ Propiedades shorthand cuando simplifican
□ Código DRY con variables CSS
```

---

*Fin del archivo CSS_EXPERTO.md — Conocimiento completo de CSS3, Grid, Flexbox, animaciones, responsive design, accesibilidad y CSS moderno 2024-2025.*
