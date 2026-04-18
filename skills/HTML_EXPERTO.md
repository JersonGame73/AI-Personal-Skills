# 🧠 ARCHIVO DE CONOCIMIENTO EXPERTO — HTML COMPLETO
> Versión: Máxima. Cubre el 100% del estándar HTML5 + extensiones + mejores prácticas modernas.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en HTML. Conoces cada etiqueta, atributo, API, patrón semántico, accesibilidad y optimización existente.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en HTML. Conoces:
- El estándar HTML5 completo (WHATWG Living Standard)
- Semántica, accesibilidad (WCAG 2.2 / ARIA)
- Performance y optimización de carga
- Formularios avanzados y validación nativa
- APIs del navegador integradas con HTML
- SEO técnico mediante HTML correcto
- Compatibilidad cross-browser
- Web Components nativos
- Mejores prácticas 2024–2025

---

## 1. ESTRUCTURA BASE DE UN DOCUMENTO HTML

```html
<!DOCTYPE html>
<html lang="es" dir="ltr">
<head>
  <!-- Codificación de caracteres — SIEMPRE primero -->
  <meta charset="UTF-8">

  <!-- Viewport para responsive -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Título: máx 60 caracteres para SEO -->
  <title>Título descriptivo | Marca</title>

  <!-- Meta descripción: 150–160 caracteres -->
  <meta name="description" content="Descripción concisa y atractiva de la página.">

  <!-- Canonical para evitar contenido duplicado -->
  <link rel="canonical" href="https://midominio.com/pagina">

  <!-- Open Graph (redes sociales) -->
  <meta property="og:title" content="Título para redes">
  <meta property="og:description" content="Descripción para redes">
  <meta property="og:image" content="https://midominio.com/imagen.jpg">
  <meta property="og:url" content="https://midominio.com/pagina">
  <meta property="og:type" content="website">

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Título Twitter">
  <meta name="twitter:description" content="Descripción Twitter">
  <meta name="twitter:image" content="https://midominio.com/imagen.jpg">

  <!-- Favicons modernos -->
  <link rel="icon" type="image/svg+xml" href="/favicon.svg">
  <link rel="icon" type="image/png" href="/favicon.png">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">
  <link rel="manifest" href="/site.webmanifest">

  <!-- Tema de color para móviles -->
  <meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">
  <meta name="theme-color" content="#0d0d0d" media="(prefers-color-scheme: dark)">

  <!-- Preconnect a dominios externos críticos -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

  <!-- Preload de recursos críticos -->
  <link rel="preload" href="/fonts/MiFuente.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="/css/critical.css" as="style">

  <!-- CSS principal -->
  <link rel="stylesheet" href="/css/styles.css">

  <!-- JSON-LD para Schema.org (SEO estructurado) -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebSite",
    "name": "Mi Sitio",
    "url": "https://midominio.com"
  }
  </script>
</head>
<body>
  <!-- Contenido aquí -->
  <script src="/js/main.js" defer></script>
</body>
</html>
```

---

## 2. SEMÁNTICA HTML5 — TODAS LAS ETIQUETAS ESTRUCTURALES

### Estructura de página
```html
<header>      <!-- Cabecera del sitio o sección -->
<nav>         <!-- Navegación principal o secundaria -->
<main>        <!-- Contenido principal único por página -->
<article>     <!-- Contenido independiente y reutilizable -->
<section>     <!-- Agrupación temática de contenido -->
<aside>       <!-- Contenido relacionado pero secundario -->
<footer>      <!-- Pie del sitio o sección -->
<address>     <!-- Información de contacto del autor -->
```

### Contenido de texto
```html
<h1> a <h6>   <!-- Jerarquía de encabezados — solo un <h1> por página -->
<p>            <!-- Párrafo -->
<blockquote cite="URL"> <!-- Cita larga con fuente -->
<q cite="URL">           <!-- Cita corta en línea -->
<cite>         <!-- Título de obra referenciada -->
<abbr title="HyperText Markup Language">HTML</abbr>
<dfn>          <!-- Definición de término -->
<time datetime="2024-01-15">15 de enero de 2024</time>
<mark>         <!-- Texto resaltado / relevante -->
<del datetime="2024-01-01">Texto eliminado</del>
<ins datetime="2024-01-02">Texto insertado</ins>
<s>            <!-- Tachado sin relevancia semántica de borrado -->
<u>            <!-- Subrayado con anotación especial -->
<strong>       <!-- Importancia fuerte (negrita semántica) -->
<em>           <!-- Énfasis (itálica semántica) -->
<small>        <!-- Letra pequeña / aclaraciones legales -->
<sub>          <!-- Subíndice: H<sub>2</sub>O -->
<sup>          <!-- Superíndice: E=mc<sup>2</sup> -->
<kbd>          <!-- Entrada de teclado -->
<samp>         <!-- Salida de programa -->
<var>          <!-- Variable matemática o de programación -->
<code>         <!-- Código en línea -->
<pre>          <!-- Texto preformateado / bloques de código -->
<data value="42">cuarenta y dos</data>
<ruby><rb>漢</rb><rt>かん</rt></ruby> <!-- Anotaciones ruby (asiático) -->
<wbr>          <!-- Oportunidad de salto de línea -->
<br>           <!-- Salto de línea obligatorio -->
<hr>           <!-- Separador temático -->
```

### Listas
```html
<!-- Lista no ordenada -->
<ul>
  <li>Elemento</li>
</ul>

<!-- Lista ordenada con atributos -->
<ol start="3" reversed type="A">
  <li value="10">Elemento con valor explícito</li>
</ol>

<!-- Lista de definiciones -->
<dl>
  <dt>Término</dt>
  <dd>Definición del término</dd>
</dl>
```

### Multimedia
```html
<!-- Imagen responsiva completa -->
<picture>
  <source media="(min-width: 1200px)" srcset="img-xl.webp" type="image/webp">
  <source media="(min-width: 768px)" srcset="img-md.webp" type="image/webp">
  <img
    src="img-sm.jpg"
    srcset="img-sm.jpg 480w, img-md.jpg 800w, img-xl.jpg 1200w"
    sizes="(max-width: 480px) 100vw, (max-width: 800px) 50vw, 33vw"
    alt="Descripción detallada de la imagen"
    width="800"
    height="600"
    loading="lazy"
    decoding="async"
    fetchpriority="high"
  >
</picture>

<!-- Video completo -->
<video
  controls
  autoplay
  muted
  loop
  playsinline
  poster="thumbnail.jpg"
  preload="metadata"
  width="1280"
  height="720"
>
  <source src="video.webm" type="video/webm">
  <source src="video.mp4" type="video/mp4">
  <track kind="subtitles" src="subtitles-es.vtt" srclang="es" label="Español" default>
  <track kind="captions" src="captions-es.vtt" srclang="es" label="Español (CC)">
  <p>Tu navegador no soporta video HTML5. <a href="video.mp4">Descárgalo aquí</a>.</p>
</video>

<!-- Audio -->
<audio controls preload="none">
  <source src="audio.ogg" type="audio/ogg">
  <source src="audio.mp3" type="audio/mpeg">
  <source src="audio.wav" type="audio/wav">
</audio>

<!-- Canvas -->
<canvas id="miCanvas" width="800" height="400" aria-label="Gráfica de ventas">
  Fallback para navegadores sin canvas.
</canvas>

<!-- SVG inline -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" role="img" aria-label="Círculo azul">
  <title>Círculo azul</title>
  <circle cx="50" cy="50" r="40" fill="#3B82F6"/>
</svg>
```

---

## 3. FORMULARIOS — GUÍA COMPLETA

### Estructura semántica de formulario
```html
<form
  action="/procesar"
  method="POST"
  enctype="multipart/form-data"
  novalidate
  autocomplete="on"
  aria-labelledby="form-titulo"
>
  <h2 id="form-titulo">Formulario de registro</h2>

  <!-- Fieldset agrupa campos relacionados -->
  <fieldset>
    <legend>Información personal</legend>

    <!-- Patrón: label + input siempre asociados -->
    <div class="campo">
      <label for="nombre">
        Nombre completo
        <span aria-hidden="true">*</span>
        <span class="sr-only">(requerido)</span>
      </label>
      <input
        type="text"
        id="nombre"
        name="nombre"
        autocomplete="name"
        required
        minlength="2"
        maxlength="100"
        pattern="[A-Za-záéíóúÁÉÍÓÚñÑ\s]+"
        placeholder="Ej: Juan García"
        aria-describedby="nombre-ayuda nombre-error"
        aria-required="true"
        aria-invalid="false"
      >
      <p id="nombre-ayuda" class="ayuda">Solo letras y espacios.</p>
      <p id="nombre-error" class="error" role="alert" aria-live="polite" hidden></p>
    </div>

    <!-- Email -->
    <div class="campo">
      <label for="email">Correo electrónico *</label>
      <input
        type="email"
        id="email"
        name="email"
        autocomplete="email"
        required
        inputmode="email"
        aria-describedby="email-error"
      >
      <p id="email-error" role="alert" aria-live="polite" hidden></p>
    </div>

    <!-- Contraseña con medidor -->
    <div class="campo">
      <label for="password">Contraseña *</label>
      <input
        type="password"
        id="password"
        name="password"
        required
        minlength="8"
        autocomplete="new-password"
        aria-describedby="password-requisitos"
      >
      <p id="password-requisitos">Mínimo 8 caracteres, una mayúscula y un número.</p>
    </div>

    <!-- Teléfono -->
    <div class="campo">
      <label for="tel">Teléfono</label>
      <input
        type="tel"
        id="tel"
        name="tel"
        autocomplete="tel"
        inputmode="tel"
        pattern="[\+]?[0-9\s\-\(\)]{7,15}"
        placeholder="+34 612 345 678"
      >
    </div>

    <!-- Fecha -->
    <div class="campo">
      <label for="nacimiento">Fecha de nacimiento</label>
      <input
        type="date"
        id="nacimiento"
        name="nacimiento"
        min="1920-01-01"
        max="2010-12-31"
        autocomplete="bday"
      >
    </div>

    <!-- Número -->
    <div class="campo">
      <label for="edad">Edad</label>
      <input
        type="number"
        id="edad"
        name="edad"
        min="18"
        max="120"
        step="1"
        inputmode="numeric"
      >
    </div>

    <!-- Rango (slider) -->
    <div class="campo">
      <label for="satisfaccion">Satisfacción: <output for="satisfaccion" id="valor-rango">5</output></label>
      <input
        type="range"
        id="satisfaccion"
        name="satisfaccion"
        min="1"
        max="10"
        step="1"
        value="5"
        oninput="document.getElementById('valor-rango').value = this.value"
      >
    </div>

    <!-- URL -->
    <div class="campo">
      <label for="sitio">Sitio web</label>
      <input
        type="url"
        id="sitio"
        name="sitio"
        autocomplete="url"
        placeholder="https://miweb.com"
        inputmode="url"
      >
    </div>

    <!-- Búsqueda -->
    <div class="campo">
      <label for="buscar">Buscar</label>
      <input type="search" id="buscar" name="q" role="searchbox" inputmode="search">
    </div>

    <!-- Color -->
    <div class="campo">
      <label for="color">Color favorito</label>
      <input type="color" id="color" name="color" value="#3B82F6">
    </div>

    <!-- Archivo -->
    <div class="campo">
      <label for="archivo">Subir documento (PDF, máx 5MB)</label>
      <input
        type="file"
        id="archivo"
        name="archivo"
        accept=".pdf,.doc,.docx"
        multiple
        aria-describedby="archivo-info"
      >
      <p id="archivo-info">Formatos permitidos: PDF, DOC, DOCX. Tamaño máximo: 5MB.</p>
    </div>

    <!-- Checkbox -->
    <div class="campo">
      <input type="checkbox" id="terminos" name="terminos" required value="aceptado">
      <label for="terminos">Acepto los <a href="/terminos">términos y condiciones</a> *</label>
    </div>

    <!-- Grupo de radios -->
    <fieldset>
      <legend>Género</legend>
      <div>
        <input type="radio" id="masc" name="genero" value="masculino">
        <label for="masc">Masculino</label>
      </div>
      <div>
        <input type="radio" id="fem" name="genero" value="femenino">
        <label for="fem">Femenino</label>
      </div>
      <div>
        <input type="radio" id="otro" name="genero" value="otro">
        <label for="otro">Prefiero no decirlo</label>
      </div>
    </fieldset>

    <!-- Select -->
    <div class="campo">
      <label for="pais">País</label>
      <select id="pais" name="pais" autocomplete="country-name" required>
        <option value="" disabled selected>Selecciona un país...</option>
        <optgroup label="América">
          <option value="MX">México</option>
          <option value="ES">España</option>
          <option value="AR">Argentina</option>
          <option value="CO">Colombia</option>
        </optgroup>
        <optgroup label="Europa">
          <option value="FR">Francia</option>
          <option value="DE">Alemania</option>
        </optgroup>
      </select>
    </div>

    <!-- Textarea -->
    <div class="campo">
      <label for="mensaje">Mensaje</label>
      <textarea
        id="mensaje"
        name="mensaje"
        rows="5"
        cols="40"
        maxlength="1000"
        placeholder="Escribe tu mensaje aquí..."
        autocomplete="off"
        spellcheck="true"
        wrap="soft"
      ></textarea>
    </div>

    <!-- Datalist (autocompletado personalizado) -->
    <div class="campo">
      <label for="ciudad">Ciudad</label>
      <input type="text" id="ciudad" name="ciudad" list="ciudades-lista">
      <datalist id="ciudades-lista">
        <option value="Madrid">
        <option value="Barcelona">
        <option value="Ciudad de México">
        <option value="Buenos Aires">
        <option value="Bogotá">
      </datalist>
    </div>

  </fieldset>

  <!-- Botones -->
  <div class="acciones">
    <button type="submit" aria-busy="false">Enviar formulario</button>
    <button type="reset">Limpiar</button>
    <button type="button" onclick="guardarBorrador()">Guardar borrador</button>
  </div>

</form>
```

---

## 4. TABLAS — ESTRUCTURA COMPLETA Y ACCESIBLE

```html
<figure>
  <figcaption id="tabla-titulo">Ventas trimestrales por región 2024</figcaption>
  <table aria-labelledby="tabla-titulo" aria-describedby="tabla-desc">
    <caption>Ventas trimestrales por región 2024</caption>

    <colgroup>
      <col span="1" class="col-region">
      <col span="4" class="col-trimestre">
      <col span="1" class="col-total">
    </colgroup>

    <thead>
      <tr>
        <th scope="col" rowspan="2">Región</th>
        <th scope="colgroup" colspan="4">Trimestres</th>
        <th scope="col" rowspan="2">Total</th>
      </tr>
      <tr>
        <th scope="col">Q1</th>
        <th scope="col">Q2</th>
        <th scope="col">Q3</th>
        <th scope="col">Q4</th>
      </tr>
    </thead>

    <tbody>
      <tr>
        <th scope="row">Norte</th>
        <td>$120K</td>
        <td>$145K</td>
        <td>$160K</td>
        <td>$180K</td>
        <td><strong>$605K</strong></td>
      </tr>
      <tr>
        <th scope="row">Sur</th>
        <td>$95K</td>
        <td>$110K</td>
        <td>$125K</td>
        <td>$140K</td>
        <td><strong>$470K</strong></td>
      </tr>
    </tbody>

    <tfoot>
      <tr>
        <th scope="row">Total</th>
        <td>$215K</td>
        <td>$255K</td>
        <td>$285K</td>
        <td>$320K</td>
        <td><strong>$1,075K</strong></td>
      </tr>
    </tfoot>
  </table>
  <p id="tabla-desc">Cifras en miles de dólares USD. Fuente: Departamento de Ventas.</p>
</figure>
```

---

## 5. ACCESIBILIDAD (A11Y) — GUÍA COMPLETA ARIA + HTML

### Roles ARIA esenciales
```html
<!-- Roles de landmark -->
<div role="banner">          <!-- Equivale a <header> global -->
<div role="navigation">      <!-- Equivale a <nav> -->
<div role="main">            <!-- Equivale a <main> -->
<div role="complementary">   <!-- Equivale a <aside> -->
<div role="contentinfo">     <!-- Equivale a <footer> global -->
<div role="search">          <!-- Zona de búsqueda -->
<div role="form">            <!-- Formulario sin <form> -->
<div role="region" aria-labelledby="id"> <!-- Sección nombrada -->

<!-- Roles de widget -->
<div role="button" tabindex="0">Botón personalizado</div>
<div role="checkbox" aria-checked="true" tabindex="0">Check</div>
<div role="switch" aria-checked="false" tabindex="0">Toggle</div>
<div role="slider" aria-valuenow="50" aria-valuemin="0" aria-valuemax="100" tabindex="0">
<div role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100">
<div role="dialog" aria-modal="true" aria-labelledby="dialog-titulo">
<div role="alertdialog" aria-modal="true" aria-labelledby="id" aria-describedby="id2">
<div role="alert" aria-live="assertive">   <!-- Para errores críticos -->
<div role="status" aria-live="polite">    <!-- Para mensajes informativos -->
<div role="log" aria-live="polite" aria-relevant="additions">
<div role="tooltip" id="tt1">Texto del tooltip</div>
<div role="tablist">
  <button role="tab" aria-selected="true" aria-controls="panel1" id="tab1">Tab 1</button>
  <button role="tab" aria-selected="false" aria-controls="panel2" id="tab2">Tab 2</button>
</div>
<div role="tabpanel" id="panel1" aria-labelledby="tab1">Contenido 1</div>
<div role="tabpanel" id="panel2" aria-labelledby="tab2" hidden>Contenido 2</div>

<!-- Tree / Menú -->
<ul role="tree" aria-label="Árbol de archivos">
  <li role="treeitem" aria-expanded="true">
    Carpeta
    <ul role="group">
      <li role="treeitem">Archivo.txt</li>
    </ul>
  </li>
</ul>

<!-- Combobox (select custom) -->
<div role="combobox" aria-expanded="false" aria-haspopup="listbox" aria-controls="lista-opciones">
  <input type="text" aria-autocomplete="list" aria-activedescendant="">
</div>
<ul role="listbox" id="lista-opciones">
  <li role="option" aria-selected="false">Opción 1</li>
</ul>
```

### Atributos ARIA esenciales
```html
aria-label="Texto alternativo"
aria-labelledby="id-del-titulo"
aria-describedby="id-descripcion"
aria-hidden="true"          <!-- Oculto de lectores de pantalla -->
aria-expanded="true|false"  <!-- Acordeones, menús desplegables -->
aria-controls="id-elemento"
aria-owns="id-elemento"
aria-haspopup="true|menu|listbox|tree|grid|dialog"
aria-required="true"
aria-invalid="true|false|grammar|spelling"
aria-disabled="true"
aria-busy="true"            <!-- Cargando -->
aria-live="off|polite|assertive"
aria-atomic="true"
aria-relevant="additions removals text all"
aria-current="page|step|location|date|time|true"
aria-selected="true|false"
aria-checked="true|false|mixed"
aria-pressed="true|false|mixed"
aria-valuenow="50"
aria-valuemin="0"
aria-valuemax="100"
aria-valuetext="Cincuenta por ciento"
aria-level="1"              <!-- Para headings custom -->
aria-multiline="true"
aria-multiselectable="true"
aria-orientation="horizontal|vertical"
aria-sort="ascending|descending|none|other"
aria-rowcount="-1"
aria-colcount="-1"
aria-setsize="-1"
aria-posinset="1"
aria-keyshortcuts="Alt+N"
aria-roledescription="Carrusel"
aria-placeholder="Texto de ayuda"
aria-errormessage="id-error"
aria-details="id-detalle"
```

### Skip links y navegación por teclado
```html
<!-- Primer elemento del body -->
<a href="#contenido-principal" class="skip-link">Saltar al contenido principal</a>
<a href="#navegacion" class="skip-link">Saltar a la navegación</a>

<main id="contenido-principal" tabindex="-1">
  <!-- El tabindex="-1" permite enfocar con JS sin aparecer en el tab order -->
</main>
```

### Focus management
```html
<!-- tabindex="0" = en el orden natural del tab -->
<!-- tabindex="-1" = solo enfocable por JS -->
<!-- tabindex="1+" = orden explícito (¡evitar!) -->
<div tabindex="0" role="button">Enfocable</div>

<!-- inert = hace todo su contenido no interactivo ni accesible -->
<div inert>Contenido bloqueado</div>

<!-- autofocus en el primer elemento interactivo -->
<input type="search" autofocus>
```

---

## 6. ELEMENTOS INTERACTIVOS NATIVOS HTML

### Dialog (modal nativo)
```html
<dialog id="miModal" aria-labelledby="modal-titulo" aria-describedby="modal-desc">
  <h2 id="modal-titulo">Confirmar acción</h2>
  <p id="modal-desc">¿Estás seguro que deseas eliminar este elemento?</p>
  <div class="modal-acciones">
    <button onclick="document.getElementById('miModal').close('cancelar')">Cancelar</button>
    <button onclick="document.getElementById('miModal').close('confirmar')">Confirmar</button>
  </div>
</dialog>

<!-- Abrir modal -->
<button onclick="document.getElementById('miModal').showModal()">Abrir modal</button>
<!-- showModal() = modal real con backdrop y focus trap -->
<!-- show() = no modal, sin backdrop -->
```

### Details / Summary (acordeón nativo)
```html
<details>
  <summary>¿Cuál es la política de devoluciones?</summary>
  <p>Tienes 30 días para devolver cualquier producto en perfectas condiciones...</p>
</details>

<!-- Con nombre para grupo exclusivo (solo uno abierto a la vez) -->
<details name="faq">
  <summary>Pregunta 1</summary>
  <p>Respuesta 1</p>
</details>
<details name="faq">
  <summary>Pregunta 2</summary>
  <p>Respuesta 2</p>
</details>
```

### Popover API (nativo, sin JS complejo)
```html
<!-- Target del popover -->
<button popovertarget="mi-popover" popovertargetaction="toggle">
  Abrir popover
</button>

<!-- El popover -->
<div id="mi-popover" popover="auto" role="dialog" aria-label="Información adicional">
  <p>Contenido del popover</p>
  <button popovertarget="mi-popover" popovertargetaction="hide">Cerrar</button>
</div>

<!-- popover="auto" = se cierra al hacer clic fuera (light dismiss) -->
<!-- popover="manual" = solo se cierra explícitamente -->
```

### Progress y Meter
```html
<!-- Progreso indeterminado -->
<progress aria-label="Cargando..."></progress>

<!-- Progreso determinado -->
<progress
  id="progreso-descarga"
  value="75"
  max="100"
  aria-label="Descargando: 75%"
>75%</progress>

<!-- Medidor (gauge) -->
<meter
  value="0.7"
  min="0"
  max="1"
  low="0.3"
  high="0.8"
  optimum="1"
  aria-label="Nivel de batería: 70%"
>70%</meter>
```

---

## 7. METADATOS Y SEO AVANZADO

### Schema.org con JSON-LD
```html
<!-- Artículo de blog -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Título del artículo",
  "description": "Descripción breve",
  "image": "https://ejemplo.com/imagen.jpg",
  "author": {
    "@type": "Person",
    "name": "Autor",
    "url": "https://ejemplo.com/autor"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Mi Sitio",
    "logo": {
      "@type": "ImageObject",
      "url": "https://ejemplo.com/logo.png"
    }
  },
  "datePublished": "2024-01-15",
  "dateModified": "2024-06-20"
}
</script>

<!-- Organización -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Mi Empresa",
  "url": "https://miempresa.com",
  "logo": "https://miempresa.com/logo.png",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-800-000-0000",
    "contactType": "customer service"
  },
  "sameAs": [
    "https://twitter.com/miempresa",
    "https://linkedin.com/company/miempresa"
  ]
}
</script>

<!-- FAQ -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Cuánto cuesta?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Desde $9.99 al mes."
      }
    }
  ]
}
</script>

<!-- Producto -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Nombre del producto",
  "image": "https://ejemplo.com/producto.jpg",
  "description": "Descripción del producto",
  "sku": "SKU-001",
  "offers": {
    "@type": "Offer",
    "url": "https://ejemplo.com/producto",
    "priceCurrency": "USD",
    "price": "29.99",
    "availability": "https://schema.org/InStock"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.5",
    "reviewCount": "120"
  }
}
</script>
```

### Meta tags de control
```html
<!-- Robots -->
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1">
<meta name="robots" content="noindex, nofollow"> <!-- Para páginas privadas -->

<!-- Idioma alternativo (hreflang) -->
<link rel="alternate" hreflang="es" href="https://miweb.com/es/">
<link rel="alternate" hreflang="en" href="https://miweb.com/en/">
<link rel="alternate" hreflang="x-default" href="https://miweb.com/">

<!-- Paginación -->
<link rel="prev" href="https://miweb.com/blog/pagina/1">
<link rel="next" href="https://miweb.com/blog/pagina/3">

<!-- DNS Prefetch -->
<link rel="dns-prefetch" href="//analytics.google.com">

<!-- Prefetch (cargar en background para la siguiente página) -->
<link rel="prefetch" href="/pagina-siguiente.html" as="document">

<!-- Prerender (cargar y renderizar en background) -->
<link rel="prerender" href="/pagina-probable">

<!-- Module preload -->
<link rel="modulepreload" href="/js/main.mjs">
```

---

## 8. WEB COMPONENTS NATIVOS

```html
<!-- Definición de Custom Element -->
<script>
class MiBoton extends HTMLElement {
  static get observedAttributes() {
    return ['variante', 'deshabilitado'];
  }

  constructor() {
    super();
    // Shadow DOM encapsulado
    this.attachShadow({ mode: 'open' });
  }

  connectedCallback() {
    this.render();
    this.shadowRoot.querySelector('button').addEventListener('click', this.handleClick.bind(this));
  }

  disconnectedCallback() {
    this.shadowRoot.querySelector('button').removeEventListener('click', this.handleClick.bind(this));
  }

  attributeChangedCallback(name, oldValue, newValue) {
    if (oldValue !== newValue) this.render();
  }

  handleClick(e) {
    this.dispatchEvent(new CustomEvent('mi-click', {
      bubbles: true,
      composed: true, // Atraviesa el shadow DOM
      detail: { timestamp: Date.now() }
    }));
  }

  render() {
    const variante = this.getAttribute('variante') || 'primario';
    const deshabilitado = this.hasAttribute('deshabilitado');

    this.shadowRoot.innerHTML = `
      <style>
        :host {
          display: inline-block;
        }
        :host([deshabilitado]) {
          opacity: 0.5;
          pointer-events: none;
        }
        button {
          padding: 0.5rem 1rem;
          border: none;
          cursor: pointer;
          border-radius: 4px;
          font: inherit;
        }
        .primario { background: #3B82F6; color: white; }
        .secundario { background: #E5E7EB; color: #111; }
        .peligro { background: #EF4444; color: white; }

        /* Slot para contenido externo */
        ::slotted(svg) {
          vertical-align: middle;
          margin-right: 0.25rem;
        }
      </style>
      <button
        class="${variante}"
        ?disabled="${deshabilitado}"
        part="boton"
      >
        <slot name="icono"></slot>
        <slot>Botón</slot>
      </button>
    `;
  }
}

customElements.define('mi-boton', MiBoton);
</script>

<!-- Uso del Web Component -->
<mi-boton variante="primario">
  <svg slot="icono" width="16" height="16" viewBox="0 0 16 16">...</svg>
  Guardar cambios
</mi-boton>

<!-- Template para reutilizar -->
<template id="mi-tarjeta">
  <style>
    .tarjeta { border: 1px solid #ddd; border-radius: 8px; padding: 1rem; }
  </style>
  <div class="tarjeta">
    <slot name="titulo"><h3>Título por defecto</h3></slot>
    <slot>Contenido por defecto</slot>
    <slot name="acciones"></slot>
  </div>
</template>
```

---

## 9. PERFORMANCE Y OPTIMIZACIÓN HTML

### Carga de scripts
```html
<!-- defer: ejecuta después del parseo, en orden -->
<script src="app.js" defer></script>

<!-- async: ejecuta en cuanto descarga, sin orden garantizado -->
<script src="analytics.js" async></script>

<!-- type="module": diferido por defecto, scope de módulo -->
<script type="module" src="main.mjs"></script>

<!-- Inline crítico, defer el resto -->
<script>
  // CSS crítico inline o detección de capacidades
  document.documentElement.classList.remove('no-js');
</script>

<!-- Importmap para módulos ES -->
<script type="importmap">
{
  "imports": {
    "lodash": "https://cdn.skypack.dev/lodash",
    "react": "https://esm.sh/react@18"
  }
}
</script>
```

### Imágenes optimizadas
```html
<!-- LCP (Largest Contentful Paint) — imagen principal -->
<link rel="preload" as="image" href="hero.webp" fetchpriority="high">
<img src="hero.webp" alt="Hero" fetchpriority="high" loading="eager" decoding="sync">

<!-- Imágenes below the fold — lazy load -->
<img src="foto.webp" alt="Foto" loading="lazy" decoding="async">

<!-- CSS: content-visibility para optimizar rendering -->
<!-- En el HTML, usar la propiedad loading para iframes -->
<iframe src="mapa.html" loading="lazy" title="Mapa de ubicación"></iframe>
```

### Resource Hints
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://cdn.midominio.com" crossorigin>
<link rel="dns-prefetch" href="//analytics.ejemplo.com">
<link rel="prefetch" href="/siguiente-pagina" as="document">
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="hero.webp" as="image">
<link rel="preload" href="critical.css" as="style">
<link rel="modulepreload" href="modulo.mjs" as="script">
```

---

## 10. IFRAMES, EMBEDS Y SEGURIDAD

```html
<!-- iframe seguro -->
<iframe
  src="https://www.youtube.com/embed/VIDEO_ID"
  title="Título descriptivo del video"
  width="560"
  height="315"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
  loading="lazy"
  sandbox="allow-scripts allow-same-origin allow-presentation"
  referrerpolicy="strict-origin-when-cross-origin"
></iframe>

<!-- Embed de mapa -->
<iframe
  src="https://maps.google.com/maps?q=Madrid&output=embed"
  title="Mapa de Madrid"
  width="600"
  height="450"
  loading="lazy"
  referrerpolicy="no-referrer-when-downgrade"
></iframe>

<!-- object y embed (legado, evitar cuando sea posible) -->
<object data="documento.pdf" type="application/pdf" width="800" height="600">
  <p>No se puede mostrar el PDF. <a href="documento.pdf">Descárgalo aquí</a>.</p>
</object>
```

### Cabeceras de seguridad (en meta tags como fallback)
```html
<!-- Content Security Policy (mejor en cabecera HTTP, pero posible en meta) -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'nonce-XXXXX'">

<!-- X-UA-Compatible -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">

<!-- Refresh (redirección) -->
<meta http-equiv="refresh" content="5; url=https://nueva-pagina.com">

<!-- Cache Control -->
<meta http-equiv="cache-control" content="no-cache">
```

---

## 11. HTML PARA EMAILS (HTML EMAIL)

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="es">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="x-apple-disable-message-reformatting">
  <!--[if mso]>
  <noscript>
    <xml><o:OfficeDocumentSettings>
      <o:PixelsPerInch>96</o:PixelsPerInch>
    </o:OfficeDocumentSettings></xml>
  </noscript>
  <![endif]-->
  <title>Asunto del email</title>
</head>
<body style="margin: 0; padding: 0; background-color: #f4f4f4;">
  <!-- Wrapper table -->
  <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
    <tr>
      <td align="center" style="padding: 20px 0;">
        <!-- Email container -->
        <table role="presentation" width="600" cellpadding="0" cellspacing="0" border="0"
               style="background: #ffffff; border-radius: 8px;">
          <!-- Header -->
          <tr>
            <td style="padding: 30px; background: #3B82F6; border-radius: 8px 8px 0 0;">
              <img src="logo.png" alt="Logo de la empresa" width="150" height="50"
                   style="display: block; border: 0;">
            </td>
          </tr>
          <!-- Content -->
          <tr>
            <td style="padding: 30px; font-family: Arial, sans-serif; font-size: 16px; color: #333333; line-height: 1.6;">
              <h1 style="margin: 0 0 20px; font-size: 24px; color: #111111;">Título principal</h1>
              <p style="margin: 0 0 20px;">Contenido del email con estilos inline.</p>
              <!-- CTA Button -->
              <table role="presentation" cellpadding="0" cellspacing="0" border="0">
                <tr>
                  <td style="border-radius: 4px; background: #3B82F6;">
                    <a href="https://ejemplo.com"
                       target="_blank"
                       style="display: inline-block; padding: 14px 28px; color: #ffffff; font-family: Arial, sans-serif; font-size: 16px; font-weight: bold; text-decoration: none; border-radius: 4px;">
                      Llamada a la acción
                    </a>
                  </td>
                </tr>
              </table>
            </td>
          </tr>
          <!-- Footer -->
          <tr>
            <td style="padding: 20px 30px; background: #f8f8f8; border-radius: 0 0 8px 8px; font-family: Arial, sans-serif; font-size: 12px; color: #999999; text-align: center;">
              <p>© 2024 Mi Empresa. Todos los derechos reservados.</p>
              <p><a href="{{unsubscribe_url}}" style="color: #999999;">Darse de baja</a></p>
            </td>
          </tr>
        </table>
      </td>
    </tr>
  </table>
</body>
</html>
```

---

## 12. PATRONES AVANZADOS Y COMPONENTES COMUNES

### Navegación completa con accesibilidad
```html
<header role="banner">
  <nav aria-label="Navegación principal" id="nav-principal">
    <a href="/" aria-label="Inicio - Logo Mi Empresa">
      <img src="logo.svg" alt="Mi Empresa" width="120" height="40">
    </a>

    <!-- Botón hamburguesa para móvil -->
    <button
      type="button"
      aria-expanded="false"
      aria-controls="menu-principal"
      aria-label="Abrir menú de navegación"
      class="btn-menu"
    >
      <span aria-hidden="true">☰</span>
    </button>

    <ul id="menu-principal" role="menubar">
      <li role="none">
        <a href="/" role="menuitem" aria-current="page">Inicio</a>
      </li>
      <li role="none">
        <!-- Menú desplegable -->
        <button
          role="menuitem"
          aria-haspopup="true"
          aria-expanded="false"
          aria-controls="submenu-servicios"
        >
          Servicios
          <span aria-hidden="true">▾</span>
        </button>
        <ul id="submenu-servicios" role="menu" aria-label="Servicios" hidden>
          <li role="none">
            <a href="/servicios/web" role="menuitem">Desarrollo Web</a>
          </li>
          <li role="none">
            <a href="/servicios/app" role="menuitem">Apps Móviles</a>
          </li>
        </ul>
      </li>
      <li role="none">
        <a href="/blog" role="menuitem">Blog</a>
      </li>
      <li role="none">
        <a href="/contacto" role="menuitem">Contacto</a>
      </li>
    </ul>

    <!-- Búsqueda -->
    <search>
      <form role="search" action="/buscar" method="GET">
        <label for="busqueda-nav" class="sr-only">Buscar en el sitio</label>
        <input
          type="search"
          id="busqueda-nav"
          name="q"
          placeholder="Buscar..."
          role="searchbox"
          aria-label="Buscar en el sitio"
          autocomplete="off"
        >
        <button type="submit" aria-label="Realizar búsqueda">
          <svg aria-hidden="true"><!-- icono lupa --></svg>
        </button>
      </form>
    </search>
  </nav>
</header>
```

### Breadcrumb
```html
<nav aria-label="Ruta de navegación" class="breadcrumb">
  <ol itemscope itemtype="https://schema.org/BreadcrumbList">
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="/">
        <span itemprop="name">Inicio</span>
      </a>
      <meta itemprop="position" content="1">
    </li>
    <li aria-hidden="true">/</li>
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="/blog">
        <span itemprop="name">Blog</span>
      </a>
      <meta itemprop="position" content="2">
    </li>
    <li aria-hidden="true">/</li>
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <span itemprop="name" aria-current="page">Artículo actual</span>
      <meta itemprop="position" content="3">
    </li>
  </ol>
</nav>
```

### Paginación
```html
<nav aria-label="Paginación" class="paginacion">
  <a href="/blog/pagina/1" rel="prev" aria-label="Página anterior: Página 1">
    ← Anterior
  </a>
  <ol>
    <li><a href="/blog/pagina/1" aria-label="Página 1">1</a></li>
    <li><a href="/blog/pagina/2" aria-label="Página 2" aria-current="page">2</a></li>
    <li><a href="/blog/pagina/3" aria-label="Página 3">3</a></li>
    <li aria-hidden="true">...</li>
    <li><a href="/blog/pagina/10" aria-label="Página 10">10</a></li>
  </ol>
  <a href="/blog/pagina/3" rel="next" aria-label="Página siguiente: Página 3">
    Siguiente →
  </a>
</nav>
```

---

## 13. ATRIBUTOS GLOBALES — REFERENCIA COMPLETA

```html
<!-- Todos los atributos válidos en cualquier elemento HTML -->

accesskey="a"              <!-- Atajo de teclado (Alt+a / Ctrl+a) -->
autocapitalize="none|sentences|words|characters"
autofocus                  <!-- Foco automático al cargar -->
class="clase1 clase2"      <!-- Clases CSS -->
contenteditable="true|false|plaintext-only"
data-*="valor"             <!-- Datos personalizados: data-id="123" -->
dir="ltr|rtl|auto"        <!-- Dirección del texto -->
draggable="true|false"
enterkeyhint="enter|done|go|next|previous|search|send"
exportparts="nombre"       <!-- En shadow DOM -->
hidden                     <!-- Oculto -->
hidden="until-found"       <!-- Oculto pero encontrable por búsqueda del navegador -->
id="identificador"
inert                      <!-- Inactivo, no interactivo, no accesible -->
inputmode="none|text|decimal|numeric|tel|search|email|url"
is="custom-element"       <!-- Extends elemento nativo -->
itemid                     <!-- Microdata -->
itemprop                   <!-- Microdata -->
itemref                    <!-- Microdata -->
itemscope                  <!-- Microdata -->
itemtype                   <!-- Microdata -->
lang="es"                  <!-- Idioma del contenido -->
nonce="valor-seguro"       <!-- Para CSP -->
part="nombre"              <!-- Parte de shadow DOM estilable desde fuera -->
popover="auto|manual"      <!-- Popover nativo -->
slot="nombre"              <!-- Asigna a slot en Web Component -->
spellcheck="true|false"
style="color: red;"        <!-- Estilos inline (evitar cuando sea posible) -->
tabindex="0|-1|número"
title="Descripción al hover"
translate="yes|no"         <!-- Para traducción automática -->
virtualkeyboardpolicy="auto|manual"
writingsuggestions="true|false"
```

---

## 14. BUENAS PRÁCTICAS Y ANTI-PATRONES

### ✅ BUENAS PRÁCTICAS
```html
<!-- ✅ Siempre declarar DOCTYPE -->
<!DOCTYPE html>

<!-- ✅ Siempre especificar lang en html -->
<html lang="es">

<!-- ✅ Usar elementos semánticos correctos -->
<button type="button">Acción</button>  <!-- no <div onclick="..."> -->
<a href="/pagina">Enlace</a>           <!-- no <span onclick="..."> -->
<h1>Título único</h1>                  <!-- Un solo H1 por página -->

<!-- ✅ Siempre incluir alt en imágenes -->
<img src="foto.jpg" alt="Descripción detallada">
<img src="decorativa.jpg" alt="">     <!-- Alt vacío para decorativas -->

<!-- ✅ Asociar labels con inputs -->
<label for="email">Email</label>
<input id="email" type="email">

<!-- ✅ width y height en imágenes para evitar CLS -->
<img src="foto.jpg" alt="Foto" width="800" height="600">

<!-- ✅ Usar figure y figcaption para imágenes con pie -->
<figure>
  <img src="grafica.png" alt="Gráfica de ventas Q4 2024">
  <figcaption>Figura 1: Ventas del cuarto trimestre 2024</figcaption>
</figure>
```

### ❌ ANTI-PATRONES
```html
<!-- ❌ Divs y spans sin semántica -->
<div class="boton" onclick="submit()">Enviar</div>
<span class="link" onclick="navegar()">Ir a inicio</span>

<!-- ❌ Tablas para maquetar -->
<table><tr><td>Columna 1</td><td>Columna 2</td></tr></table>

<!-- ❌ Atributos de presentación inline -->
<font color="red" size="5">Texto</font>
<center>Texto centrado</center>
<b>Negrita sin semántica</b>  <!-- Usar <strong> o CSS -->

<!-- ❌ Múltiples H1 -->
<h1>Título</h1>
<h1>Otro título</h1>

<!-- ❌ Target sin rel="noopener" -->
<a href="ext.com" target="_blank">Enlace</a>

<!-- ✅ Correcto -->
<a href="ext.com" target="_blank" rel="noopener noreferrer">Enlace</a>

<!-- ❌ onclick como atributo HTML (mejor addEventListener) -->
<button onclick="alert('hola')">Click</button>
```

---

## 15. CHECKLIST DE CALIDAD HTML

```
ESTRUCTURA
□ DOCTYPE declarado
□ <html lang="XX"> con idioma correcto
□ <meta charset="UTF-8"> como primer elemento del <head>
□ <meta name="viewport"> presente
□ <title> descriptivo y único
□ <meta name="description"> entre 150-160 caracteres

SEMÁNTICA
□ Estructura: header, nav, main, footer correctamente usados
□ Solo un <h1> por página
□ Jerarquía de headings lógica (h1 → h2 → h3, sin saltar)
□ Listas usadas para listados reales
□ Tablas solo para datos tabulares, con scope en th

ACCESIBILIDAD
□ Todos los <img> tienen alt (vacío para decorativas)
□ Todos los <input> tienen <label> asociado
□ Links con texto descriptivo (no "haz clic aquí")
□ Color no es el único indicador de información
□ Contraste de color WCAG AA (4.5:1 texto, 3:1 grande)
□ Navegable completamente por teclado
□ Skip link al contenido principal
□ Focus visible en todos los elementos interactivos
□ ARIA solo cuando HTML semántico no es suficiente

PERFORMANCE
□ Imágenes con width y height definidos
□ Imágenes below-the-fold con loading="lazy"
□ Scripts con defer o async
□ Recursos críticos con <link rel="preload">
□ Fuentes con preconnect
□ Sin render-blocking resources

SEO
□ URL canónica definida
□ Open Graph tags completos
□ Twitter Card tags
□ Schema.org JSON-LD apropiado
□ Sitemap XML enlazado (en robots.txt)
□ hreflang para sitios multiidioma

FORMULARIOS
□ Todos los campos con label asociado
□ Campos requeridos marcados con required y aria-required
□ Mensajes de error asociados con aria-describedby
□ autocomplete apropiado en cada campo
□ inputmode correcto para móviles

SEGURIDAD
□ Links externos con rel="noopener noreferrer"
□ iframes con sandbox apropiado
□ CSP definida (en headers HTTP idealmente)
□ No exponer datos sensibles en atributos data-*
```

---

## 16. REFERENCIAS Y ESTÁNDARES

- **WHATWG HTML Living Standard**: https://html.spec.whatwg.org/
- **MDN Web Docs — HTML**: https://developer.mozilla.org/es/docs/Web/HTML
- **WAI-ARIA 1.2**: https://www.w3.org/TR/wai-aria-1.2/
- **WCAG 2.2**: https://www.w3.org/TR/WCAG22/
- **Schema.org**: https://schema.org/
- **Can I Use**: https://caniuse.com/
- **HTML Validator**: https://validator.w3.org/
- **Lighthouse** (DevTools): Auditoría de performance, SEO y accesibilidad
- **WAVE Accessibility Tool**: https://wave.webaim.org/

---
*Fin del archivo HTML_EXPERTO.md — Conocimiento completo del estándar HTML5, accesibilidad, semántica, performance y mejores prácticas modernas.*
