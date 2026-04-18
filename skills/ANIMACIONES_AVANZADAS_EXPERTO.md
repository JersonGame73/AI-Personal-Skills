# 🎬 ARCHIVO DE CONOCIMIENTO EXPERTO — ANIMACIONES AVANZADAS COMPLETO
> Versión: Máxima. Cubre GSAP, ScrollTrigger, Three.js, WebGL, Canvas API, física y patrones de integración.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en animaciones web avanzadas. Puedes crear sitios al nivel de Tesla, Apple, Awwwards y Google. Conoces cada API, librería, técnica y patrón visual de alto impacto.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en animaciones web avanzadas. Conoces:
- GSAP (GreenSock) — la librería de animación más poderosa del frontend
- ScrollTrigger — animaciones ligadas al scroll con precisión de pixel
- Three.js — gráficos 3D en el navegador con WebGL
- Canvas API — dibujo 2D, física de partículas, efectos interactivos
- WebGL y GLSL Shaders — programación de GPU para efectos imposibles con CSS
- Lenis / Locomotive Scroll — scroll suave con inercia
- Barba.js — transiciones entre páginas cinematográficas
- Integración de múltiples librerías como lo hacen Tesla, Apple y Netflix
- Performance de animaciones (60fps siempre)
- Técnicas de diseño visual de alto impacto

---

## PARTE 1: GSAP — GUÍA COMPLETA

### Instalación e importación
```html
<!-- CDN (para proyectos rápidos) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/TextPlugin.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/Flip.min.js"></script>

<!-- NPM -->
<!-- npm install gsap -->
```

```javascript
// Importar en módulos
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { TextPlugin } from 'gsap/TextPlugin';
import { Flip } from 'gsap/Flip';
import { SplitText } from 'gsap/SplitText'; // Club GSAP
import { DrawSVGPlugin } from 'gsap/DrawSVGPlugin'; // Club GSAP
import { MorphSVGPlugin } from 'gsap/MorphSVGPlugin'; // Club GSAP

// Registrar plugins (obligatorio)
gsap.registerPlugin(ScrollTrigger, TextPlugin, Flip, SplitText, DrawSVGPlugin);
```

---

### 1.1 GSAP BÁSICO — gsap.to(), gsap.from(), gsap.fromTo()

```javascript
// gsap.to() — anima DESDE el estado actual HACIA los valores definidos
gsap.to('.caja', {
  x: 200,              // translateX (shorthand de GSAP)
  y: -100,             // translateY
  xPercent: 50,        // translateX en %
  yPercent: -50,       // translateY en %
  rotation: 360,       // rotate en grados
  rotationX: 45,       // rotateX (3D)
  rotationY: 180,      // rotateY (3D)
  rotationZ: 45,       // rotateZ
  scale: 1.5,          // scale
  scaleX: 2,
  scaleY: 0.5,
  skewX: 20,
  skewY: 10,
  width: 300,
  height: 200,
  opacity: 0,
  backgroundColor: '#3B82F6',
  color: '#ffffff',
  fontSize: '2rem',
  borderRadius: '50%',
  padding: 20,
  left: '50%',
  top: '100px',
  duration: 1,         // duración en segundos
  delay: 0.5,          // retraso en segundos
  ease: 'power2.out',  // función de easing
  repeat: -1,          // -1 = infinito
  yoyo: true,          // va y vuelve
  repeatDelay: 1,      // pausa entre repeticiones
  stagger: 0.1,        // retraso entre múltiples elementos
  overwrite: 'auto',   // manejo de conflictos
  force3D: true,       // fuerza aceleración GPU
  transformOrigin: 'center center',
  transformPerspective: 800,

  // Callbacks
  onStart: () => console.log('Empieza'),
  onUpdate: (self) => console.log(self.progress()),
  onComplete: () => console.log('Terminó'),
  onRepeat: () => console.log('Repite'),
  onReverseComplete: () => console.log('Reverso completado'),
});

// gsap.from() — anima DESDE los valores definidos HACIA el estado actual
gsap.from('.titulo', {
  y: 100,
  opacity: 0,
  duration: 1,
  ease: 'power3.out',
});

// gsap.fromTo() — control total de inicio y fin
gsap.fromTo('.elemento',
  { x: -200, opacity: 0, scale: 0.5 },   // desde
  { x: 0, opacity: 1, scale: 1, duration: 1, ease: 'back.out(1.7)' } // hasta
);

// gsap.set() — establece valores instantáneamente (sin animación)
gsap.set('.elemento', { x: 0, y: 0, opacity: 1 });

// Múltiples elementos con stagger
gsap.from('.tarjeta', {
  y: 60,
  opacity: 0,
  duration: 0.8,
  ease: 'power2.out',
  stagger: {
    amount: 1,           // tiempo total distribuido entre todos
    from: 'start',       // start | end | center | random | edges
    grid: 'auto',        // para grids: [columnas, filas]
    axis: 'x',           // eje para calcular stagger en grids
    ease: 'power1.inOut',
  },
});
```

---

### 1.2 EASINGS — TODAS LAS FUNCIONES DE TIEMPO

```javascript
// === POWER (suavizado polinomial) ===
'none'           // linear
'power1.in'      // aceleración suave
'power1.out'     // desaceleración suave — el más natural
'power1.inOut'   // suave en ambos extremos
'power2.out'     // desaceleración media — MUY usado
'power3.out'     // desaceleración fuerte
'power4.out'     // desaceleración muy fuerte

// === BACK (rebasa un poco el punto final) ===
'back.in'
'back.out'       // Efecto de salida con rebote — muy natural en UI
'back.inOut'
'back.out(1.7)'  // Parámetro de intensidad del rebote

// === BOUNCE (rebota como una pelota) ===
'bounce.in'
'bounce.out'
'bounce.inOut'

// === ELASTIC (como un resorte) ===
'elastic.in'
'elastic.out'
'elastic.inOut'
'elastic.out(1, 0.3)' // (amplitud, período)

// === EXPO (exponencial — dramático) ===
'expo.in'
'expo.out'
'expo.inOut'

// === CIRC (circular) ===
'circ.in'
'circ.out'
'circ.inOut'

// === SINE (sinusoidal — muy suave) ===
'sine.in'
'sine.out'
'sine.inOut'

// === STEPS (escalonado) ===
'steps(12)'      // 12 pasos discretos

// === CUSTOM CUBIC BEZIER ===
gsap.to('.el', { ease: 'cubic-bezier(0.68, -0.55, 0.265, 1.55)', duration: 1 });

// === CUSTOM EASE con EasePack ===
CustomEase.create('miEase', 'M0,0 C0.14,0 0.242,0.438 0.272,0.561 0.313,0.728 0.354,0.963 0.362,1 0.37,0.985 0.414,0.873 0.455,0.811 0.51,0.726 0.573,0.753 0.621,0.781 0.673,0.812 0.689,0.985 0.696,1 0.694,0.987 0.717,0.96 0.738,0.951 0.756,0.944 0.767,0.95 0.78,0.956 0.805,0.969 0.819,0.984 0.83,1 0.842,0.982 0.871,0.955 0.904,0.967 0.934,0.979 0.954,0.992 1,1');
```

---

### 1.3 TIMELINES — EL CORAZÓN DE GSAP

```javascript
// Timeline = secuencia de animaciones coordinadas
const tl = gsap.timeline({
  defaults: {
    duration: 0.8,
    ease: 'power2.out',
  },
  repeat: 0,
  yoyo: false,
  delay: 0.5,
  paused: false,         // true = no inicia automáticamente
  smoothChildTiming: true,
  autoRemoveChildren: false,
  onStart: () => {},
  onComplete: () => {},
  onUpdate: (self) => {},
});

// Agregar animaciones en secuencia
tl.from('.header', { y: -100, opacity: 0 })
  .from('.hero-titulo', { y: 60, opacity: 0 }, '-=0.4')     // 0.4s antes de que termine el anterior
  .from('.hero-subtitulo', { y: 40, opacity: 0 }, '-=0.3')
  .from('.hero-boton', { scale: 0, opacity: 0, ease: 'back.out(2)' }, '-=0.2')
  .from('.hero-imagen', { x: 200, opacity: 0 }, '<0.2')     // < = al mismo tiempo que el anterior
  .from('.badge', { rotation: -180, scale: 0 }, '+=0.5');   // += pausa antes

// Posiciones en la timeline
tl.to('.a', { x: 100 })                    // al final actual
  .to('.b', { x: 100 }, 0)                // en el segundo 0 absoluto
  .to('.c', { x: 100 }, 1.5)             // en el segundo 1.5 absoluto
  .to('.d', { x: 100 }, '<')             // mismo inicio que el anterior
  .to('.e', { x: 100 }, '<+=0.5')        // 0.5s después del inicio del anterior
  .to('.f', { x: 100 }, '>')             // al terminar el anterior
  .to('.g', { x: 100 }, '>-0.25')        // 0.25s antes de que termine el anterior
  .to('.h', { x: 100 }, '-=0.5')         // 0.5s antes del final actual
  .to('.i', { x: 100 }, 'miLabel')       // en una etiqueta nombrada
  .addLabel('miLabel', 2);               // definir etiqueta en segundo 2

// Control de la timeline
tl.play();
tl.pause();
tl.resume();
tl.reverse();
tl.restart();
tl.seek(2);          // ir al segundo 2
tl.seek('miLabel');  // ir a una etiqueta
tl.progress(0.5);    // ir al 50% de la duración
tl.timeScale(2);     // 2x velocidad
tl.timeScale(0.5);   // 0.5x velocidad (cámara lenta)
tl.kill();           // destruir

// Timeline maestro que controla sub-timelines
const master = gsap.timeline();
master.add(crearAnimacionHeader())
      .add(crearAnimacionHero(), '-=0.5')
      .add(crearAnimacionCards(), '-=0.3');

function crearAnimacionHeader() {
  const tl = gsap.timeline();
  tl.from('.logo', { opacity: 0, x: -30 })
    .from('.nav-links', { opacity: 0, y: -20, stagger: 0.1 }, '<0.2');
  return tl;
}
```

---

### 1.4 SCROLLTRIGGER — ANIMACIONES AL SCROLL

```javascript
// Registrar plugin
gsap.registerPlugin(ScrollTrigger);

// === BÁSICO ===
gsap.from('.seccion', {
  y: 100,
  opacity: 0,
  duration: 1,
  scrollTrigger: {
    trigger: '.seccion',       // Elemento que activa el trigger
    start: 'top 80%',          // cuando el top del trigger llega al 80% del viewport
    end: 'bottom 20%',         // cuando el bottom del trigger llega al 20%
    toggleActions: 'play none none none',
    // toggleActions: 'onEnter onLeave onEnterBack onLeaveBack'
    // valores posibles: play | pause | resume | reverse | restart | reset | complete | none
  },
});

// === SCRUB — animación ligada al scroll ===
gsap.to('.imagen-hero', {
  scale: 1.3,
  y: '-20%',
  scrollTrigger: {
    trigger: '.seccion-hero',
    start: 'top top',
    end: 'bottom top',
    scrub: true,        // true = sincronizado con scroll
    scrub: 1,           // 1 = suavizado de 1 segundo (lag suave)
    scrub: 0.5,         // 0.5 = más responsivo
  },
});

// === PIN — anclar un elemento mientras se scrollea ===
gsap.to('.texto-parallax', {
  y: '-50%',
  ease: 'none',
  scrollTrigger: {
    trigger: '.seccion-pin',
    start: 'top top',
    end: '+=1000',       // 1000px de scroll
    pin: true,           // ancla el elemento
    pin: '.contenedor',  // ancla un elemento diferente
    pinSpacing: true,    // true = agrega espacio (default)
    pinSpacing: false,   // false = sin espacio extra
    anticipatePin: 1,    // anticipación en segundos
    scrub: 1,
  },
});

// === CONFIGURACIÓN COMPLETA ===
const st = ScrollTrigger.create({
  trigger: '.mi-seccion',
  start: 'top center',
  end: 'bottom center',
  endTrigger: '.otro-elemento',  // elemento diferente para el end
  scroller: '.contenedor-scroll', // scroller custom (no window)
  horizontal: false,              // false = scroll vertical
  scrub: 1,
  pin: false,
  markers: true,                  // SOLO para desarrollo — muestra marcadores visuales
  id: 'mi-trigger',
  invalidateOnRefresh: true,      // recalcular al resize

  // Callbacks
  onEnter: (self) => {
    console.log('Entró al trigger', self.progress, self.direction, self.isActive);
  },
  onLeave: (self) => {},
  onEnterBack: (self) => {},
  onLeaveBack: (self) => {},
  onToggle: (self) => console.log('Toggle', self.isActive),
  onUpdate: (self) => {
    console.log('Progress:', self.progress.toFixed(3));
    console.log('Velocity:', self.getVelocity());
  },
  onRefresh: (self) => {},
  onScrubComplete: (self) => {},

  // Clases CSS automáticas
  toggleClass: 'activo',
  toggleClass: { targets: '.otro', className: 'activo' },
});

// Controlar desde JS
st.enable();
st.disable();
st.kill();
st.refresh();

// === PATRONES AVANZADOS ===

// Reveal de texto letra por letra
const titulo = document.querySelector('.titulo-hero');
// Con SplitText (plugin Club GSAP):
// const split = new SplitText(titulo, { type: 'chars,words,lines' });
// gsap.from(split.chars, { opacity: 0, y: 50, stagger: 0.02, scrollTrigger: { trigger: titulo } });

// Sin SplitText — manual:
function splitTexto(elemento) {
  const texto = elemento.textContent;
  elemento.innerHTML = texto.split('').map(char =>
    char === ' ' ? '<span class="espacio"> </span>' : `<span class="char">${char}</span>`
  ).join('');
  return elemento.querySelectorAll('.char');
}

const chars = splitTexto(document.querySelector('.titulo'));
gsap.from(chars, {
  opacity: 0,
  y: 80,
  rotationX: -90,
  stagger: 0.03,
  duration: 0.8,
  ease: 'back.out(1.7)',
  scrollTrigger: {
    trigger: '.titulo',
    start: 'top 85%',
  },
});

// Parallax multi-capa (como Tesla)
const capas = [
  { selector: '.fondo', velocidad: 0.3 },
  { selector: '.medio', velocidad: 0.6 },
  { selector: '.frente', velocidad: 0.9 },
];

capas.forEach(({ selector, velocidad }) => {
  gsap.to(selector, {
    yPercent: -30 * velocidad,
    ease: 'none',
    scrollTrigger: {
      trigger: '.seccion-parallax',
      start: 'top bottom',
      end: 'bottom top',
      scrub: true,
    },
  });
});

// Contador animado al llegar al viewport
function animarContador(elemento, inicio, fin, duracion, prefijo = '', sufijo = '') {
  gsap.fromTo(
    { valor: inicio },
    { valor: fin },
    {
      duration: duracion,
      ease: 'power2.out',
      onUpdate: function() {
        elemento.textContent = prefijo + Math.round(this.targets()[0].valor).toLocaleString() + sufijo;
      },
      scrollTrigger: {
        trigger: elemento,
        start: 'top 80%',
        once: true,
      },
    }
  );
}
animarContador(document.querySelector('.contador-ventas'), 0, 48392, 2, '$', '+');

// Horizontal scroll (como portafolios de alto nivel)
const seccionesH = gsap.utils.toArray('.seccion-horizontal');
const contenedorH = document.querySelector('.contenedor-horizontal');

gsap.to(seccionesH, {
  xPercent: -100 * (seccionesH.length - 1),
  ease: 'none',
  scrollTrigger: {
    trigger: '.wrapper-horizontal',
    pin: true,
    scrub: 1,
    snap: 1 / (seccionesH.length - 1), // snap a cada sección
    end: () => `+=${contenedorH.offsetWidth}`,
    invalidateOnRefresh: true,
  },
});

// Batch — para listas largas (performance)
ScrollTrigger.batch('.tarjeta', {
  interval: 0.1,
  batchMax: 3,
  onEnter: (batch) => gsap.from(batch, {
    opacity: 0,
    y: 60,
    stagger: 0.15,
    overwrite: true,
  }),
  onLeave: (batch) => gsap.set(batch, { opacity: 0, y: -60, overwrite: true }),
  onEnterBack: (batch) => gsap.to(batch, { opacity: 1, y: 0, stagger: 0.1, overwrite: true }),
  onLeaveBack: (batch) => gsap.set(batch, { opacity: 0, y: 60, overwrite: true }),
  start: 'top 85%',
  end: 'top 15%',
});

// Refresh global (obligatorio después de cambios de layout)
ScrollTrigger.refresh();

// Cleanup (para SPAs)
ScrollTrigger.getAll().forEach(st => st.kill());
```

---

### 1.5 FLIP PLUGIN — TRANSICIONES MÁGICAS DE LAYOUT

```javascript
// FLIP = First Last Invert Play
// Anima automáticamente la transición entre dos estados de layout

gsap.registerPlugin(Flip);

// Básico
const estado = Flip.getState('.elemento');    // 1. Capturar estado actual
elemento.classList.toggle('expandido');        // 2. Cambiar layout
Flip.from(estado, {                           // 3. Animar desde el estado anterior
  duration: 0.6,
  ease: 'power2.inOut',
  absolute: true,       // usar position absolute durante la animación
  nested: true,         // incluir descendientes
  onComplete: () => {},
});

// Con ScrollTrigger
Flip.from(estado, {
  scrollTrigger: { trigger: '.contenedor', start: 'top center' },
  duration: 0.8,
  stagger: 0.1,
  ease: 'power1.inOut',
});

// Reordenar elementos (ej: filtro de portafolio)
const items = document.querySelectorAll('.item');
const estadoAntes = Flip.getState(items);
// Reordenar el DOM
ordenarItems(items);
Flip.from(estadoAntes, { duration: 0.5, ease: 'power1.inOut', stagger: 0.05 });
```

---

### 1.6 EFECTOS DE TEXTO AVANZADOS

```javascript
// Efecto máquina de escribir
gsap.registerPlugin(TextPlugin);
gsap.to('.display', {
  duration: 3,
  text: {
    value: 'Hola, soy una máquina de escribir...',
    delimiter: '',      // caracter a caracter
    // delimiter: ' ', // palabra a palabra
  },
  ease: 'none',
  repeat: -1,
  yoyo: true,
  repeatDelay: 1,
});

// Scramble Text (efecto Matrix)
// Con ScrambleTextPlugin (Club GSAP):
gsap.to('.titulo', {
  duration: 2,
  scrambleText: {
    text: 'TEXTO FINAL',
    chars: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%',
    revealDelay: 0.5,
    speed: 0.4,
    tweenLength: false,
  },
});

// Reveal de texto con clip (sin plugins extra)
// En CSS: .titulo-wrap { overflow: hidden; }
gsap.from('.titulo-inner', {
  yPercent: 100,
  duration: 1,
  ease: 'power3.out',
  scrollTrigger: { trigger: '.titulo-wrap', start: 'top 80%' },
});

// Texto que se escribe y borra en bucle
const frases = ['Diseñamos experiencias', 'Creamos el futuro', 'Somos diferentes'];
let fraseActual = 0;

function escribirFrase() {
  const tl = gsap.timeline({ onComplete: () => {
    setTimeout(() => {
      fraseActual = (fraseActual + 1) % frases.length;
      escribirFrase();
    }, 1500);
  }});

  tl.to('.cursor', { opacity: 0, repeat: 5, yoyo: true, duration: 0.3 })
    .to('.display', {
      duration: frases[fraseActual].length * 0.05,
      text: frases[fraseActual],
      ease: 'none',
    })
    .to('.cursor', { opacity: 0, duration: 0.3, delay: 1 })
    .to('.display', {
      duration: frases[fraseActual].length * 0.03,
      text: '',
      ease: 'none',
    });
}
escribirFrase();
```

---

## PARTE 2: THREE.JS — GRÁFICOS 3D COMPLETOS

### Instalación
```html
<!-- CDN -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<!-- NPM: npm install three -->
```

```javascript
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader.js';
import { EffectComposer } from 'three/examples/jsm/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/examples/jsm/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass.js';
```

---

### 2.1 ESCENA BASE COMPLETA

```javascript
// === SETUP BÁSICO ===
class Escena3D {
  constructor(canvas) {
    this.canvas = canvas;
    this.width = window.innerWidth;
    this.height = window.innerHeight;

    this.init();
    this.crearObjetos();
    this.agregarEventos();
    this.tick();
  }

  init() {
    // Escena
    this.scene = new THREE.Scene();
    this.scene.background = new THREE.Color('#0a0a0a');
    this.scene.fog = new THREE.Fog('#0a0a0a', 10, 50);
    this.scene.fog = new THREE.FogExp2('#0a0a0a', 0.05); // Niebla exponencial

    // Cámara
    this.camera = new THREE.PerspectiveCamera(
      75,                              // FOV (campo de visión) — 45-90 típico
      this.width / this.height,        // Aspect ratio
      0.1,                             // Near clipping plane
      1000,                            // Far clipping plane
    );
    this.camera.position.set(0, 0, 5); // x, y, z
    this.camera.lookAt(0, 0, 0);       // hacia dónde mira

    // Cámara ortográfica (sin perspectiva — para UIs 2D/3D)
    // this.camera = new THREE.OrthographicCamera(
    //   -this.width/2, this.width/2,    // left, right
    //   this.height/2, -this.height/2,  // top, bottom
    //   0.1, 1000                        // near, far
    // );

    // Renderer
    this.renderer = new THREE.WebGLRenderer({
      canvas: this.canvas,
      antialias: true,               // antialiasing
      alpha: true,                   // fondo transparente
      powerPreference: 'high-performance',
    });
    this.renderer.setSize(this.width, this.height);
    this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2)); // Máx 2x
    this.renderer.shadowMap.enabled = true;
    this.renderer.shadowMap.type = THREE.PCFSoftShadowMap; // Sombras suaves
    this.renderer.outputColorSpace = THREE.SRGBColorSpace;
    this.renderer.toneMapping = THREE.ACESFilmicToneMapping; // Tone mapping cinematográfico
    this.renderer.toneMappingExposure = 1.0;
    this.renderer.physicallyCorrectLights = true;

    // Controles orbitales (mouse para rotar/zoom)
    this.controls = new OrbitControls(this.camera, this.canvas);
    this.controls.enableDamping = true;    // Inercia
    this.controls.dampingFactor = 0.05;
    this.controls.enableZoom = true;
    this.controls.enablePan = false;
    this.controls.minDistance = 2;
    this.controls.maxDistance = 20;
    this.controls.maxPolarAngle = Math.PI / 2; // No pasar el suelo
    this.controls.autoRotate = true;
    this.controls.autoRotateSpeed = 0.5;
    this.controls.target.set(0, 0, 0);    // Punto de rotación

    // Clock para animaciones basadas en tiempo
    this.clock = new THREE.Clock();

    // Mouse
    this.mouse = new THREE.Vector2();
    this.raycaster = new THREE.Raycaster();
  }

  crearObjetos() {
    // Definir en subclases
  }

  agregarEventos() {
    window.addEventListener('resize', () => this.resize());
    window.addEventListener('mousemove', (e) => this.onMouseMove(e));
  }

  resize() {
    this.width = window.innerWidth;
    this.height = window.innerHeight;
    this.camera.aspect = this.width / this.height;
    this.camera.updateProjectionMatrix();
    this.renderer.setSize(this.width, this.height);
    this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  }

  onMouseMove(e) {
    this.mouse.x = (e.clientX / this.width) * 2 - 1;
    this.mouse.y = -(e.clientY / this.height) * 2 + 1;
  }

  tick() {
    const elapsed = this.clock.getElapsedTime();
    const delta = this.clock.getDelta();

    // Actualizar controles
    this.controls.update();

    // Renderizar
    this.renderer.render(this.scene, this.camera);

    // Loop
    requestAnimationFrame(() => this.tick());
  }
}
```

---

### 2.2 GEOMETRÍAS — TODAS LAS FORMAS

```javascript
// === GEOMETRÍAS BÁSICAS ===
new THREE.BoxGeometry(1, 1, 1, 2, 2, 2);           // ancho, alto, prof, segX, segY, segZ
new THREE.SphereGeometry(1, 64, 64);                // radio, segH, segV
new THREE.CylinderGeometry(0.5, 0.5, 2, 32);       // radTop, radBot, altura, seg
new THREE.ConeGeometry(1, 2, 32);                   // radio, altura, seg
new THREE.TorusGeometry(1, 0.3, 16, 100);           // radio, tubo, seg radial, seg tubular
new THREE.TorusKnotGeometry(1, 0.3, 100, 16);       // Nudo toroidal
new THREE.PlaneGeometry(5, 5, 32, 32);              // ancho, alto, segX, segY
new THREE.CircleGeometry(1, 64);                    // radio, seg
new THREE.RingGeometry(0.5, 1, 32);                 // interior, exterior, seg
new THREE.DodecahedronGeometry(1);                  // Dodecaedro
new THREE.IcosahedronGeometry(1, 2);                // Icosaedro (nivel 2 = más suave)
new THREE.OctahedronGeometry(1);                    // Octaedro
new THREE.TetrahedronGeometry(1);                   // Tetraedro
new THREE.CapsuleGeometry(0.5, 1, 4, 8);            // radio, altura, capSeg, radSeg

// === GEOMETRÍA PERSONALIZADA ===
const geometry = new THREE.BufferGeometry();

// Posiciones de vértices (Float32Array)
const vertices = new Float32Array([
   0,  1, 0,  // vértice 0 (x, y, z)
  -1, -1, 0,  // vértice 1
   1, -1, 0,  // vértice 2
]);
geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3));

// Normales (para iluminación correcta)
const normales = new Float32Array([
  0, 0, 1,
  0, 0, 1,
  0, 0, 1,
]);
geometry.setAttribute('normal', new THREE.BufferAttribute(normales, 3));

// UVs (para texturas)
const uvs = new Float32Array([
  0.5, 1,
  0, 0,
  1, 0,
]);
geometry.setAttribute('uv', new THREE.BufferAttribute(uvs, 2));

// Índices (para reutilizar vértices)
geometry.setIndex([0, 1, 2]);

// Calcular normales automáticamente
geometry.computeVertexNormals();
geometry.computeBoundingBox();
geometry.computeBoundingSphere();

// === GEOMETRÍA DE PARTÍCULAS (Sistema de puntos) ===
const cantidadParticulas = 5000;
const posiciones = new Float32Array(cantidadParticulas * 3);
const colores = new Float32Array(cantidadParticulas * 3);

for (let i = 0; i < cantidadParticulas; i++) {
  const i3 = i * 3;

  // Distribución en esfera
  const radio = Math.random() * 5;
  const theta = Math.random() * Math.PI * 2;
  const phi = Math.acos(Math.random() * 2 - 1);

  posiciones[i3]     = radio * Math.sin(phi) * Math.cos(theta);
  posiciones[i3 + 1] = radio * Math.sin(phi) * Math.sin(theta);
  posiciones[i3 + 2] = radio * Math.cos(phi);

  // Color basado en posición
  colores[i3]     = posiciones[i3] / 5 + 0.5;
  colores[i3 + 1] = posiciones[i3 + 1] / 5 + 0.5;
  colores[i3 + 2] = 1;
}

const geoParticulas = new THREE.BufferGeometry();
geoParticulas.setAttribute('position', new THREE.BufferAttribute(posiciones, 3));
geoParticulas.setAttribute('color', new THREE.BufferAttribute(colores, 3));
```

---

### 2.3 MATERIALES — TODOS LOS TIPOS

```javascript
// === MATERIALES PRINCIPALES ===

// MeshBasicMaterial — sin iluminación (más rápido)
new THREE.MeshBasicMaterial({
  color: '#3B82F6',
  wireframe: false,
  transparent: true,
  opacity: 0.5,
  side: THREE.FrontSide,    // FrontSide | BackSide | DoubleSide
  map: texture,             // textura de color
  alphaMap: alphaTexture,   // textura de transparencia
  visible: true,
  depthTest: true,
  depthWrite: true,
});

// MeshStandardMaterial — PBR (Physically Based Rendering) — el más usado
new THREE.MeshStandardMaterial({
  color: '#ffffff',
  roughness: 0.5,           // 0 = espejo, 1 = completamente rugoso
  metalness: 0,             // 0 = no metálico, 1 = completamente metálico
  map: colorTexture,        // Albedo / Color
  roughnessMap: roughMap,   // Control de rugosidad por textura
  metalnessMap: metalMap,   // Control de metalness por textura
  normalMap: normalTexture, // Detalle de superficie
  normalScale: new THREE.Vector2(1, 1),
  aoMap: aoTexture,         // Ambient Occlusion
  aoMapIntensity: 1,
  displacementMap: dispMap, // Desplazamiento real de geometría
  displacementScale: 0.1,
  displacementBias: 0,
  emissive: '#ff0000',      // Color que emite (no requiere luz)
  emissiveMap: emissiveTexture,
  emissiveIntensity: 1,
  envMap: envTexture,       // Reflejo del entorno
  envMapIntensity: 1,
  transparent: false,
  opacity: 1,
  alphaTest: 0.5,
  side: THREE.FrontSide,
  wireframe: false,
  flatShading: false,
});

// MeshPhysicalMaterial — extensión de Standard con más opciones
new THREE.MeshPhysicalMaterial({
  // Todo lo de Standard +
  clearcoat: 1,             // Capa de barniz (coches, plástico)
  clearcoatRoughness: 0.1,
  clearcoatMap: texture,
  clearcoatNormalMap: normalMap,
  sheen: 1,                 // Brillo sedoso (tela)
  sheenColor: '#ffffff',
  sheenRoughness: 0.5,
  transmission: 1,          // Transparencia física (vidrio)
  ior: 1.5,                 // Índice de refracción (vidrio=1.5, agua=1.33)
  thickness: 0.5,           // Grosor del vidrio
  attenuationColor: '#ffffff',
  attenuationDistance: 1,
  iridescence: 1,           // Iridiscencia (burbujas de jabón)
  iridescenceIOR: 1.3,
  iridescenceThicknessRange: [100, 400],
  anisotropy: 1,            // Anisotropía (metales cepillados)
  specularIntensity: 1,
  specularColor: '#ffffff',
});

// MeshToonMaterial — cartoon shading
new THREE.MeshToonMaterial({
  color: '#ff6600',
  gradientMap: toonTexture, // Textura de gradiente para celdas
});

// MeshDepthMaterial — para efectos de profundidad
new THREE.MeshDepthMaterial({ depthPacking: THREE.RGBADepthPacking });

// PointsMaterial — para sistemas de partículas
new THREE.PointsMaterial({
  size: 0.02,
  sizeAttenuation: true,    // Tamaño disminuye con la distancia
  color: '#ffffff',
  map: particleTexture,
  alphaMap: particleTexture,
  alphaTest: 0.001,
  transparent: true,
  blending: THREE.AdditiveBlending, // Suma colores (efecto glow)
  vertexColors: true,       // Usar colores por vértice
  depthWrite: false,        // Para blending correcto
});

// ShaderMaterial — control total con GLSL
new THREE.ShaderMaterial({
  vertexShader: `
    varying vec2 vUv;
    varying float vElevation;

    uniform float uTime;

    void main() {
      vUv = uv;
      vec4 modelPosition = modelMatrix * vec4(position, 1.0);
      float elevation = sin(modelPosition.x * 3.0 + uTime) *
                        sin(modelPosition.z * 3.0 + uTime) * 0.3;
      vElevation = elevation;
      modelPosition.y += elevation;
      vec4 viewPosition = viewMatrix * modelPosition;
      vec4 projectedPosition = projectionMatrix * viewPosition;
      gl_Position = projectedPosition;
    }
  `,
  fragmentShader: `
    varying vec2 vUv;
    varying float vElevation;

    uniform vec3 uColorSuperior;
    uniform vec3 uColorInferior;

    void main() {
      float mixStrength = (vElevation + 0.3) / 0.6;
      vec3 color = mix(uColorInferior, uColorSuperior, mixStrength);
      gl_FragColor = vec4(color, 1.0);
    }
  `,
  uniforms: {
    uTime: { value: 0 },
    uColorSuperior: { value: new THREE.Color('#3B82F6') },
    uColorInferior: { value: new THREE.Color('#1E40AF') },
  },
  side: THREE.DoubleSide,
  wireframe: false,
  transparent: false,
});
```

---

### 2.4 LUCES — ILUMINACIÓN COMPLETA

```javascript
// === TIPOS DE LUZ ===

// AmbientLight — iluminación global uniforme
const ambientLight = new THREE.AmbientLight('#ffffff', 0.5); // color, intensidad
scene.add(ambientLight);

// DirectionalLight — como el sol (rayos paralelos)
const dirLight = new THREE.DirectionalLight('#ffffff', 1);
dirLight.position.set(5, 10, 5);
dirLight.castShadow = true;
dirLight.shadow.mapSize.width = 2048;  // resolución de sombra
dirLight.shadow.mapSize.height = 2048;
dirLight.shadow.camera.near = 0.5;
dirLight.shadow.camera.far = 50;
dirLight.shadow.camera.left = -10;
dirLight.shadow.camera.right = 10;
dirLight.shadow.camera.top = 10;
dirLight.shadow.camera.bottom = -10;
dirLight.shadow.bias = -0.001;         // evitar shadow acne
dirLight.shadow.normalBias = 0.02;
scene.add(dirLight);

// Helper para visualizar (solo desarrollo)
const dirHelper = new THREE.DirectionalLightHelper(dirLight, 1);
scene.add(dirHelper);

// PointLight — como una bombilla (omnidireccional)
const pointLight = new THREE.PointLight('#ff6600', 2, 20, 2);
// color, intensidad, distancia, decay
pointLight.position.set(0, 3, 0);
pointLight.castShadow = true;
scene.add(pointLight);

// SpotLight — como un foco
const spotLight = new THREE.SpotLight('#ffffff', 2, 20, Math.PI / 6, 0.25, 1);
// color, intensidad, distancia, ángulo, penumbra, decay
spotLight.position.set(0, 5, 0);
spotLight.target.position.set(0, 0, 0);
spotLight.castShadow = true;
scene.add(spotLight);
scene.add(spotLight.target);

// HemisphereLight — cielo/tierra
const hemiLight = new THREE.HemisphereLight('#87CEEB', '#654321', 0.5);
// color del cielo, color de la tierra, intensidad
scene.add(hemiLight);

// RectAreaLight — como una ventana o pantalla
const rectLight = new THREE.RectAreaLight('#ffffff', 2, 4, 4);
// color, intensidad, ancho, alto
rectLight.position.set(0, 2, 0);
rectLight.lookAt(0, 0, 0);
scene.add(rectLight);
```

---

### 2.5 TEXTURAS Y LOADERS

```javascript
// === TEXTURE LOADER ===
const textureLoader = new THREE.TextureLoader();
const colorTexture = textureLoader.load(
  '/textures/color.jpg',
  (texture) => console.log('Cargada'),
  (progress) => console.log(progress.loaded / progress.total),
  (error) => console.error(error)
);

// Configuración de textura
colorTexture.colorSpace = THREE.SRGBColorSpace; // Para color maps
colorTexture.wrapS = THREE.RepeatWrapping;       // Repetición horizontal
colorTexture.wrapT = THREE.RepeatWrapping;       // Repetición vertical
colorTexture.repeat.set(2, 2);                  // Repetir 2x
colorTexture.offset.set(0.5, 0.5);              // Offset UV
colorTexture.rotation = Math.PI / 4;            // Rotación
colorTexture.center.set(0.5, 0.5);              // Centro de rotación
colorTexture.minFilter = THREE.LinearMipmapLinearFilter; // Para objetos lejanos
colorTexture.magFilter = THREE.NearestFilter;   // Para objetos cercanos (pixel art)
colorTexture.generateMipmaps = true;

// === LOADING MANAGER (para controlar múltiples cargas) ===
const loadingManager = new THREE.LoadingManager(
  () => {
    // onLoad — todo cargado
    gsap.to('.loading-screen', { opacity: 0, duration: 0.5, onComplete: () => {
      document.querySelector('.loading-screen').remove();
    }});
  },
  (url, loaded, total) => {
    // onProgress
    const progreso = loaded / total;
    document.querySelector('.barra-progreso').style.width = `${progreso * 100}%`;
  },
  (url) => console.error('Error cargando:', url) // onError
);

const tLoader = new THREE.TextureLoader(loadingManager);
const gltfLoader = new GLTFLoader(loadingManager);
const cubeLoader = new THREE.CubeTextureLoader(loadingManager);

// === CARGAR MODELO 3D (GLTF/GLB) ===
const dracoLoader = new DRACOLoader();
dracoLoader.setDecoderPath('/draco/');
gltfLoader.setDRACOLoader(dracoLoader);

gltfLoader.load('/models/modelo.glb', (gltf) => {
  const modelo = gltf.scene;
  scene.add(modelo);

  // Configurar sombras en todos los meshes
  modelo.traverse((child) => {
    if (child.isMesh) {
      child.castShadow = true;
      child.receiveShadow = true;
      // Actualizar material
      child.material.envMapIntensity = 1;
    }
  });

  // Animaciones del modelo
  const mixer = new THREE.AnimationMixer(modelo);
  const accion = mixer.clipAction(gltf.animations[0]);
  accion.play();

  // Actualizar en el tick
  // mixer.update(deltaTime);
});

// === ENVIRONMENT MAP (reflejo del entorno) ===
const cubeTexture = cubeLoader.load([
  '/env/px.jpg', '/env/nx.jpg',
  '/env/py.jpg', '/env/ny.jpg',
  '/env/pz.jpg', '/env/nz.jpg',
]);
scene.environment = cubeTexture;
scene.background = cubeTexture;

// Con EXR/HDR:
import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader.js';
const rgbeLoader = new RGBELoader();
rgbeLoader.load('/env/entorno.hdr', (texture) => {
  texture.mapping = THREE.EquirectangularReflectionMapping;
  scene.environment = texture;
  scene.background = texture;
});
```

---

### 2.6 SISTEMA DE PARTÍCULAS AVANZADO

```javascript
class SistemaDeParticulas {
  constructor(scene) {
    this.scene = scene;
    this.count = 8000;
    this.init();
  }

  init() {
    // Geometría
    const geometry = new THREE.BufferGeometry();
    const positions = new Float32Array(this.count * 3);
    const colors = new Float32Array(this.count * 3);
    const scales = new Float32Array(this.count);
    const velocidades = new Float32Array(this.count * 3);

    const colorA = new THREE.Color('#3B82F6');
    const colorB = new THREE.Color('#8B5CF6');

    for (let i = 0; i < this.count; i++) {
      const i3 = i * 3;
      // Distribución aleatoria en cubo
      positions[i3]     = (Math.random() - 0.5) * 20;
      positions[i3 + 1] = (Math.random() - 0.5) * 20;
      positions[i3 + 2] = (Math.random() - 0.5) * 20;

      // Color mezclado
      const mixedColor = colorA.clone().lerp(colorB, Math.random());
      colors[i3]     = mixedColor.r;
      colors[i3 + 1] = mixedColor.g;
      colors[i3 + 2] = mixedColor.b;

      scales[i] = Math.random();

      // Velocidad aleatoria
      velocidades[i3]     = (Math.random() - 0.5) * 0.01;
      velocidades[i3 + 1] = (Math.random() - 0.5) * 0.01;
      velocidades[i3 + 2] = (Math.random() - 0.5) * 0.01;
    }

    geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
    geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));
    geometry.setAttribute('aScale', new THREE.BufferAttribute(scales, 1));

    this.velocidades = velocidades;

    // Material con shader personalizado
    const material = new THREE.ShaderMaterial({
      vertexShader: `
        attribute float aScale;
        varying vec3 vColor;
        uniform float uTime;
        uniform float uSize;

        void main() {
          vColor = color;
          vec4 modelPosition = modelMatrix * vec4(position, 1.0);

          // Movimiento ondulatorio
          modelPosition.y += sin(uTime + modelPosition.x * 100.0) * 0.1;

          vec4 viewPosition = viewMatrix * modelPosition;
          vec4 projectedPosition = projectionMatrix * viewPosition;
          gl_Position = projectedPosition;
          gl_PointSize = uSize * aScale;
          gl_PointSize *= (1.0 / -viewPosition.z);
        }
      `,
      fragmentShader: `
        varying vec3 vColor;

        void main() {
          // Forma circular suave
          float distancia = distance(gl_PointCoord, vec2(0.5));
          if (distancia > 0.5) discard;
          float alpha = 1.0 - (distancia * 2.0);
          gl_FragColor = vec4(vColor, alpha);
        }
      `,
      uniforms: {
        uTime: { value: 0 },
        uSize: { value: 30 * Math.min(window.devicePixelRatio, 2) },
      },
      transparent: true,
      blending: THREE.AdditiveBlending,
      depthWrite: false,
      vertexColors: true,
    });

    this.points = new THREE.Points(geometry, material);
    this.scene.add(this.points);
    this.material = material;
  }

  update(tiempo) {
    this.material.uniforms.uTime.value = tiempo;

    // Actualizar posiciones
    const positions = this.points.geometry.attributes.position.array;
    for (let i = 0; i < this.count; i++) {
      const i3 = i * 3;
      positions[i3]     += this.velocidades[i3];
      positions[i3 + 1] += this.velocidades[i3 + 1];
      positions[i3 + 2] += this.velocidades[i3 + 2];

      // Boundary check — si sale del cubo, vuelve al lado opuesto
      if (Math.abs(positions[i3]) > 10) this.velocidades[i3] *= -1;
      if (Math.abs(positions[i3 + 1]) > 10) this.velocidades[i3 + 1] *= -1;
      if (Math.abs(positions[i3 + 2]) > 10) this.velocidades[i3 + 2] *= -1;
    }
    this.points.geometry.attributes.position.needsUpdate = true;
  }
}
```

---

### 2.7 POST-PROCESADO (efectos cinematográficos)

```javascript
import { EffectComposer } from 'three/examples/jsm/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/examples/jsm/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass.js';
import { FilmPass } from 'three/examples/jsm/postprocessing/FilmPass.js';
import { GlitchPass } from 'three/examples/jsm/postprocessing/GlitchPass.js';
import { ShaderPass } from 'three/examples/jsm/postprocessing/ShaderPass.js';
import { SMAAPass } from 'three/examples/jsm/postprocessing/SMAAPass.js';
import { BokehPass } from 'three/examples/jsm/postprocessing/BokehPass.js';
import { SSAOPass } from 'three/examples/jsm/postprocessing/SSAOPass.js';

// Setup del compositor
const composer = new EffectComposer(renderer);

// 1. Render pass — renderizar la escena
const renderPass = new RenderPass(scene, camera);
composer.addPass(renderPass);

// 2. Bloom — efecto de brillo/glow
const bloomPass = new UnrealBloomPass(
  new THREE.Vector2(window.innerWidth, window.innerHeight),
  1.5,   // intensidad
  0.4,   // radio
  0.85   // umbral de brillo
);
composer.addPass(bloomPass);

// 3. Film grain — ruido de película
const filmPass = new FilmPass(
  0.35,  // intensidad de ruido
  0.025, // intensidad de líneas de barrido
  648,   // count de líneas
  false  // blanco y negro
);
composer.addPass(filmPass);

// 4. Glitch — efecto de corrupción
const glitchPass = new GlitchPass();
glitchPass.goWild = false; // solo glitch de vez en cuando
composer.addPass(glitchPass);

// 5. SMAA — antialiasing de calidad
const smaaPass = new SMAAPass(window.innerWidth, window.innerHeight);
composer.addPass(smaaPass);

// 6. Bokeh — desenfoque de profundidad (Depth of Field)
const bokehPass = new BokehPass(scene, camera, {
  focus: 5.0,      // distancia de foco
  aperture: 0.025,
  maxblur: 0.01,
});
composer.addPass(bokehPass);

// Pass personalizado (tinte de color)
const colorCorrectionShader = {
  uniforms: {
    tDiffuse: { value: null },
    uTint: { value: new THREE.Color('#ff9900') },
    uTintStrength: { value: 0.15 },
  },
  vertexShader: `
    varying vec2 vUv;
    void main() {
      vUv = uv;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
  `,
  fragmentShader: `
    uniform sampler2D tDiffuse;
    uniform vec3 uTint;
    uniform float uTintStrength;
    varying vec2 vUv;
    void main() {
      vec4 color = texture2D(tDiffuse, vUv);
      color.rgb = mix(color.rgb, uTint, uTintStrength);
      gl_FragColor = color;
    }
  `,
};

const tintPass = new ShaderPass(colorCorrectionShader);
composer.addPass(tintPass);

// En el render loop, usar composer en lugar de renderer
// composer.render() en lugar de renderer.render(scene, camera)
```

---

## PARTE 3: CANVAS API — FÍSICA Y EFECTOS 2D

### 3.1 SETUP Y CONCEPTOS BASE

```javascript
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d', {
  alpha: false,           // Sin transparencia = más rápido
  desynchronized: true,   // Reduce latencia en algunos browsers
});

// Tamaño con pixel ratio
function redimensionarCanvas() {
  canvas.width = window.innerWidth * devicePixelRatio;
  canvas.height = window.innerHeight * devicePixelRatio;
  canvas.style.width = window.innerWidth + 'px';
  canvas.style.height = window.innerHeight + 'px';
  ctx.scale(devicePixelRatio, devicePixelRatio);
}
redimensionarCanvas();
window.addEventListener('resize', redimensionarCanvas);

// Offscreen Canvas (mejor performance — no bloquea el main thread)
const offscreen = new OffscreenCanvas(800, 600);
const offCtx = offscreen.getContext('2d');
```

---

### 3.2 SISTEMA DE PARTÍCULAS COMPLETO CON FÍSICA

```javascript
class SistemaParticulas2D {
  constructor(canvas) {
    this.canvas = canvas;
    this.ctx = canvas.getContext('2d');
    this.particulas = [];
    this.mouse = { x: -999, y: -999, radio: 100 };
    this.animId = null;

    this.init();
    this.agregarEventos();
    this.animar();
  }

  init() {
    const cantidad = Math.floor((this.canvas.width * this.canvas.height) / 9000);
    for (let i = 0; i < cantidad; i++) {
      this.particulas.push(new Particula(this.canvas));
    }
  }

  agregarEventos() {
    this.canvas.addEventListener('mousemove', (e) => {
      const rect = this.canvas.getBoundingClientRect();
      this.mouse.x = (e.clientX - rect.left) * devicePixelRatio;
      this.mouse.y = (e.clientY - rect.top) * devicePixelRatio;
    });

    this.canvas.addEventListener('mouseleave', () => {
      this.mouse.x = -999;
      this.mouse.y = -999;
    });

    this.canvas.addEventListener('click', (e) => {
      // Explotar partículas al hacer clic
      for (let i = 0; i < 30; i++) {
        this.particulas.push(new Particula(this.canvas, {
          x: e.clientX * devicePixelRatio,
          y: e.clientY * devicePixelRatio,
          explotar: true,
        }));
      }
    });
  }

  dibujarConexiones() {
    // Dibujar líneas entre partículas cercanas
    for (let i = 0; i < this.particulas.length; i++) {
      for (let j = i + 1; j < this.particulas.length; j++) {
        const dx = this.particulas[i].x - this.particulas[j].x;
        const dy = this.particulas[i].y - this.particulas[j].y;
        const distancia = Math.sqrt(dx * dx + dy * dy);
        const distanciaMax = 120;

        if (distancia < distanciaMax) {
          const opacidad = (1 - distancia / distanciaMax) * 0.4;
          this.ctx.beginPath();
          this.ctx.strokeStyle = `rgba(99, 179, 237, ${opacidad})`;
          this.ctx.lineWidth = 0.5;
          this.ctx.moveTo(this.particulas[i].x, this.particulas[i].y);
          this.ctx.lineTo(this.particulas[j].x, this.particulas[j].y);
          this.ctx.stroke();
        }
      }
    }
  }

  animar() {
    // Limpiar con fade (trail effect)
    this.ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
    this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);

    // O limpiar completamente
    // this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

    this.dibujarConexiones();

    this.particulas = this.particulas.filter(p => p.viva);
    this.particulas.forEach(p => {
      p.actualizar(this.mouse);
      p.dibujar(this.ctx);
    });

    this.animId = requestAnimationFrame(() => this.animar());
  }

  destruir() {
    cancelAnimationFrame(this.animId);
  }
}

class Particula {
  constructor(canvas, opciones = {}) {
    this.canvas = canvas;
    this.viva = true;

    this.x = opciones.x ?? Math.random() * canvas.width;
    this.y = opciones.y ?? Math.random() * canvas.height;

    this.baseX = this.x;
    this.baseY = this.y;

    this.radio = Math.random() * 3 + 1;
    this.masa = this.radio;

    if (opciones.explotar) {
      const angulo = Math.random() * Math.PI * 2;
      const velocidad = Math.random() * 8 + 2;
      this.vx = Math.cos(angulo) * velocidad;
      this.vy = Math.sin(angulo) * velocidad;
      this.vida = 1;
      this.decayVida = Math.random() * 0.02 + 0.01;
    } else {
      this.vx = (Math.random() - 0.5) * 1;
      this.vy = (Math.random() - 0.5) * 1;
      this.vida = 1;
    }

    // Color en HSL para fácil animación
    this.hue = Math.random() * 60 + 200; // Azul-púrpura
    this.saturacion = 70;
    this.luminosidad = 60;
    this.opacidad = Math.random() * 0.5 + 0.3;
  }

  actualizar(mouse) {
    // Física básica
    const dx = mouse.x - this.x;
    const dy = mouse.y - this.y;
    const distancia = Math.sqrt(dx * dx + dy * dy);

    if (distancia < mouse.radio) {
      // Repulsión del mouse
      const fuerza = (mouse.radio - distancia) / mouse.radio;
      const angulo = Math.atan2(dy, dx);
      this.vx -= Math.cos(angulo) * fuerza * 3;
      this.vy -= Math.sin(angulo) * fuerza * 3;
    } else {
      // Volver a posición original (spring)
      const dxBase = this.baseX - this.x;
      const dyBase = this.baseY - this.y;
      this.vx += dxBase * 0.03;
      this.vy += dyBase * 0.03;
    }

    // Fricción
    this.vx *= 0.92;
    this.vy *= 0.92;

    // Gravedad (opcional)
    // this.vy += 0.1;

    // Mover
    this.x += this.vx;
    this.y += this.vy;

    // Rebote en bordes
    if (this.x < 0 || this.x > this.canvas.width) this.vx *= -1;
    if (this.y < 0 || this.y > this.canvas.height) this.vy *= -1;

    // Vida (para partículas de explosión)
    if (this.decayVida) {
      this.vida -= this.decayVida;
      if (this.vida <= 0) this.viva = false;
    }

    // Animar hue
    this.hue += 0.5;
  }

  dibujar(ctx) {
    ctx.save();
    ctx.globalAlpha = this.opacidad * this.vida;

    // Glow
    ctx.shadowBlur = 10;
    ctx.shadowColor = `hsl(${this.hue}, ${this.saturacion}%, ${this.luminosidad}%)`;

    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radio * this.vida, 0, Math.PI * 2);
    ctx.fillStyle = `hsl(${this.hue}, ${this.saturacion}%, ${this.luminosidad}%)`;
    ctx.fill();

    ctx.restore();
  }
}
```

---

### 3.3 EFECTOS CANVAS AVANZADOS

```javascript
// === EFECTO ONDA LÍQUIDA ===
class OndaLiquida {
  constructor(ctx, canvas) {
    this.ctx = ctx;
    this.canvas = canvas;
    this.puntos = [];
    this.amplitud = 50;
    this.frecuencia = 0.02;
    this.velocidad = 0.05;
    this.offset = 0;
    this.crearPuntos();
  }

  crearPuntos() {
    for (let x = 0; x <= this.canvas.width; x += 20) {
      this.puntos.push({ x, y: this.canvas.height / 2 });
    }
  }

  dibujar(tiempo) {
    this.offset = tiempo * this.velocidad;
    this.ctx.beginPath();
    this.ctx.moveTo(0, this.canvas.height);

    this.puntos.forEach((punto, i) => {
      const y = this.canvas.height / 2 +
        Math.sin(punto.x * this.frecuencia + this.offset) * this.amplitud +
        Math.sin(punto.x * this.frecuencia * 2 + this.offset * 1.3) * (this.amplitud * 0.5);
      this.ctx.lineTo(punto.x, y);
    });

    this.ctx.lineTo(this.canvas.width, this.canvas.height);
    this.ctx.closePath();

    const gradiente = this.ctx.createLinearGradient(0, this.canvas.height / 2 - this.amplitud, 0, this.canvas.height);
    gradiente.addColorStop(0, 'rgba(59, 130, 246, 0.8)');
    gradiente.addColorStop(1, 'rgba(29, 78, 216, 0.2)');
    this.ctx.fillStyle = gradiente;
    this.ctx.fill();
  }
}

// === MORPHING DE FORMAS ===
function lerp(a, b, t) { return a + (b - a) * t; }

class MorphShape {
  constructor(ctx, formaA, formaB) {
    this.ctx = ctx;
    this.formaA = formaA;
    this.formaB = formaB;
    this.progreso = 0;
    this.direccion = 1;
  }

  dibujar() {
    const puntos = this.formaA.map((pA, i) => {
      const pB = this.formaB[i % this.formaB.length];
      return {
        x: lerp(pA.x, pB.x, this.progreso),
        y: lerp(pA.y, pB.y, this.progreso),
      };
    });

    this.ctx.beginPath();
    this.ctx.moveTo(puntos[0].x, puntos[0].y);
    puntos.forEach(p => this.ctx.lineTo(p.x, p.y));
    this.ctx.closePath();
    this.ctx.fill();

    // Animar
    this.progreso += 0.005 * this.direccion;
    if (this.progreso >= 1 || this.progreso <= 0) this.direccion *= -1;
  }
}

// === PLASMA / PERLIN NOISE EFFECT ===
class EfectoPlasma {
  constructor(ctx, canvas) {
    this.ctx = ctx;
    this.canvas = canvas;
    this.imageData = ctx.createImageData(canvas.width, canvas.height);
    this.tiempo = 0;
  }

  animar() {
    const data = this.imageData.data;
    const w = this.canvas.width;
    const h = this.canvas.height;

    for (let y = 0; y < h; y++) {
      for (let x = 0; x < w; x++) {
        const i = (y * w + x) * 4;
        const t = this.tiempo;

        const v1 = Math.sin(x / 10 + t);
        const v2 = Math.sin(10 * (x * Math.sin(t / 2) + y * Math.cos(t / 3)) + t);
        const cx = x + 50 * Math.sin(t / 5);
        const cy = y + 50 * Math.cos(t / 3);
        const v3 = Math.sin(Math.sqrt(100 * (cx * cx + cy * cy) + 1) + t);
        const v = v1 + v2 + v3;

        data[i]     = Math.floor(128 + 128 * Math.sin(Math.PI * v / 2));         // R
        data[i + 1] = Math.floor(128 + 128 * Math.sin(Math.PI * v / 2 + 2.094)); // G
        data[i + 2] = Math.floor(128 + 128 * Math.sin(Math.PI * v / 2 + 4.188)); // B
        data[i + 3] = 255; // A
      }
    }

    this.ctx.putImageData(this.imageData, 0, 0);
    this.tiempo += 0.05;
  }
}

// === TEXTO EN CANVAS CON FÍSICA ===
class TextoExplotado {
  constructor(ctx, canvas, texto, x, y, estilo) {
    this.ctx = ctx;
    this.canvas = canvas;
    this.particulas = [];
    this.crearDeTexto(texto, x, y, estilo);
  }

  crearDeTexto(texto, x, y, estilo = '40px Arial') {
    // Renderizar texto en canvas temporal
    const tempCanvas = document.createElement('canvas');
    const tempCtx = tempCanvas.getContext('2d');
    tempCanvas.width = this.canvas.width;
    tempCanvas.height = this.canvas.height;
    tempCtx.font = estilo;
    tempCtx.fillStyle = 'white';
    tempCtx.textAlign = 'center';
    tempCtx.fillText(texto, x, y);

    // Obtener píxeles del texto
    const data = tempCtx.getImageData(0, 0, tempCanvas.width, tempCanvas.height).data;

    // Crear partícula por cada N píxeles del texto
    const paso = 6;
    for (let py = 0; py < tempCanvas.height; py += paso) {
      for (let px = 0; px < tempCanvas.width; px += paso) {
        const i = (py * tempCanvas.width + px) * 4;
        if (data[i + 3] > 128) { // Si el píxel tiene alpha
          this.particulas.push({
            x: px, y: py,
            baseX: px, baseY: py,
            vx: 0, vy: 0,
            color: `rgba(${data[i]},${data[i+1]},${data[i+2]},1)`,
            radio: 2,
          });
        }
      }
    }
  }

  actualizar(mouse) {
    this.particulas.forEach(p => {
      const dx = mouse.x - p.x;
      const dy = mouse.y - p.y;
      const dist = Math.sqrt(dx * dx + dy * dy);

      if (dist < 80) {
        const fuerza = (80 - dist) / 80;
        const angulo = Math.atan2(dy, dx);
        p.vx -= Math.cos(angulo) * fuerza * 5;
        p.vy -= Math.sin(angulo) * fuerza * 5;
      }

      // Spring hacia posición original
      p.vx += (p.baseX - p.x) * 0.05;
      p.vy += (p.baseY - p.y) * 0.05;
      p.vx *= 0.85;
      p.vy *= 0.85;
      p.x += p.vx;
      p.y += p.vy;
    });
  }

  dibujar() {
    this.particulas.forEach(p => {
      this.ctx.fillStyle = p.color;
      this.ctx.fillRect(p.x, p.y, p.radio, p.radio);
    });
  }
}
```

---

## PARTE 4: SCROLL SUAVE CON LENIS

```javascript
// npm install @studio-freight/lenis
import Lenis from '@studio-freight/lenis';

const lenis = new Lenis({
  duration: 1.2,            // Duración del scroll suave
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)), // Exponential ease
  direction: 'vertical',    // vertical | horizontal
  gestureDirection: 'vertical',
  smooth: true,
  mouseMultiplier: 1,        // Velocidad con mouse
  smoothTouch: false,        // Smooth en touch (cuidado con performance)
  touchMultiplier: 2,
  infinite: false,           // Loop infinito
});

// Integrar con GSAP ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);

gsap.ticker.add((time) => {
  lenis.raf(time * 1000);
});
gsap.ticker.lagSmoothing(0);

// Controlar programáticamente
lenis.scrollTo('#seccion', { offset: -80, duration: 1.5, easing: (t) => t });
lenis.scrollTo(0); // Volver al inicio
lenis.stop();
lenis.start();
lenis.destroy();
```

---

## PARTE 5: TRANSICIONES DE PÁGINA CON BARBA.JS

```javascript
// npm install @barba/core
import barba from '@barba/core';
import gsap from 'gsap';

barba.init({
  transitions: [
    {
      name: 'transicion-fade',
      // Reglas de cuando aplica
      from: { namespace: ['home'] },
      to: { namespace: ['about', 'work'] },

      async leave(data) {
        // Animar salida de la página actual
        await gsap.to(data.current.container, {
          opacity: 0,
          y: -30,
          duration: 0.5,
          ease: 'power2.in',
        });
      },

      async enter(data) {
        // Animar entrada de la nueva página
        gsap.from(data.next.container, {
          opacity: 0,
          y: 30,
          duration: 0.5,
          ease: 'power2.out',
        });
      },
    },

    // Transición con overlay
    {
      name: 'transicion-overlay',

      async leave(data) {
        const overlay = document.querySelector('.overlay-transicion');
        await gsap.to(overlay, {
          scaleX: 1,
          duration: 0.6,
          ease: 'power2.inOut',
          transformOrigin: 'left center',
        });
      },

      async enter(data) {
        const overlay = document.querySelector('.overlay-transicion');
        await gsap.to(overlay, {
          scaleX: 0,
          duration: 0.6,
          ease: 'power2.inOut',
          transformOrigin: 'right center',
        });
      },
    },
  ],

  views: [
    {
      namespace: 'home',
      beforeEnter() {
        // Inicializar animaciones específicas del home
        iniciarAnimacionesHome();
      },
      afterLeave() {
        // Limpiar
        ScrollTrigger.getAll().forEach(st => st.kill());
      },
    },
  ],

  // Hooks globales
  hooks: {
    before(data) {
      document.documentElement.classList.add('is-transitioning');
    },
    after(data) {
      document.documentElement.classList.remove('is-transitioning');
      // Re-inicializar ScrollTrigger con el nuevo contenido
      ScrollTrigger.refresh();
    },
  },
});
```

---

## PARTE 6: PATRONES DE INTEGRACIÓN — NIVEL TESLA/APPLE

### 6.1 Hero Section al nivel de Tesla

```javascript
// HTML:
// <section class="hero">
//   <canvas id="canvas-hero"></canvas>
//   <video class="hero-video" autoplay muted loop playsinline>
//   <div class="hero-contenido">
//     <h1 class="hero-titulo">Model S</h1>
//     <p class="hero-sub">0-60 mph en 1.99 segundos</p>
//     <div class="hero-botones">...</div>
//   </div>
// </section>

function iniciarHeroTesla() {
  const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });

  // El video/imagen ya está visible, animamos el contenido encima
  tl.from('.hero-titulo', {
    y: 60,
    opacity: 0,
    duration: 1.2,
    delay: 0.5,
  })
  .from('.hero-sub', {
    y: 40,
    opacity: 0,
    duration: 1,
  }, '-=0.7')
  .from('.hero-botones', {
    y: 30,
    opacity: 0,
    duration: 0.8,
  }, '-=0.6')
  .from('.hero-scroll-hint', {
    opacity: 0,
    duration: 0.5,
  }, '-=0.3');
}

// Parallax del video al hacer scroll
gsap.to('.hero-video', {
  yPercent: 30,
  ease: 'none',
  scrollTrigger: {
    trigger: '.hero',
    start: 'top top',
    end: 'bottom top',
    scrub: true,
  },
});

// Fade del contenido al salir del hero
gsap.to('.hero-contenido', {
  y: -100,
  opacity: 0,
  ease: 'none',
  scrollTrigger: {
    trigger: '.hero',
    start: 'top top',
    end: '30% top',
    scrub: true,
  },
});
```

### 6.2 Sección de características con scroll

```javascript
// Sección que hace pin mientras muestran diferentes características
const caracteristicas = gsap.utils.toArray('.caracteristica');

gsap.to('.caracteristica-activa', {
  scrollTrigger: {
    trigger: '.seccion-caracteristicas',
    start: 'top top',
    end: `+=${caracteristicas.length * 100}vh`,
    pin: true,
    scrub: 1,
  },
});

caracteristicas.forEach((car, i) => {
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: '.seccion-caracteristicas',
      start: `${(i / caracteristicas.length) * 100}% top`,
      end: `${((i + 1) / caracteristicas.length) * 100}% top`,
      scrub: 1,
      toggleActions: 'play none none reverse',
    },
  });

  tl.from(car.querySelector('.car-titulo'), { y: 40, opacity: 0 })
    .from(car.querySelector('.car-desc'), { y: 30, opacity: 0 }, '-=0.3')
    .from(car.querySelector('.car-imagen'), { scale: 0.8, opacity: 0 }, '<');
});
```

### 6.3 Three.js + GSAP + ScrollTrigger integrado

```javascript
class ExperienciaCompleta {
  constructor() {
    this.escena = new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera(75, innerWidth/innerHeight, 0.1, 100);
    this.camera.position.z = 5;
    this.renderer = new THREE.WebGLRenderer({ canvas: document.querySelector('#canvas'), alpha: true });
    this.renderer.setSize(innerWidth, innerHeight);
    this.renderer.setPixelRatio(Math.min(devicePixelRatio, 2));

    this.crearGeometria();
    this.vincularConGSAP();
    this.tick();
  }

  crearGeometria() {
    const geo = new THREE.IcosahedronGeometry(2, 20);
    const mat = new THREE.MeshStandardMaterial({
      color: '#3B82F6',
      roughness: 0.2,
      metalness: 0.8,
      wireframe: false,
    });
    this.malla = new THREE.Mesh(geo, mat);

    // Guardar posiciones originales para ondular
    this.posicionesOriginales = geo.attributes.position.array.slice();

    this.escena.add(this.malla);
    this.escena.add(new THREE.AmbientLight('#ffffff', 0.5));
    const dirLight = new THREE.DirectionalLight('#ffffff', 1);
    dirLight.position.set(5, 5, 5);
    this.escena.add(dirLight);
  }

  vincularConGSAP() {
    // Objeto proxy para animar propiedades de Three.js con GSAP
    const proxy = {
      rotacionY: 0,
      posicionZ: 5,
      escala: 1,
    };

    // Animación de entrada
    gsap.from(proxy, {
      posicionZ: 15,
      escala: 0,
      duration: 2,
      ease: 'power3.out',
      onUpdate: () => {
        this.camera.position.z = proxy.posicionZ;
        this.malla.scale.setScalar(proxy.escala);
      },
    });

    // Vincular rotación con scroll
    gsap.to(proxy, {
      rotacionY: Math.PI * 2,
      ease: 'none',
      scrollTrigger: {
        trigger: 'body',
        start: 'top top',
        end: 'bottom bottom',
        scrub: 1,
        onUpdate: () => {
          this.malla.rotation.y = proxy.rotacionY;
        },
      },
    });

    // Reaccionar al mouse
    window.addEventListener('mousemove', (e) => {
      const x = (e.clientX / innerWidth - 0.5) * 2;
      const y = -(e.clientY / innerHeight - 0.5) * 2;

      gsap.to(this.malla.rotation, {
        x: y * 0.3,
        y: x * 0.3,
        duration: 1,
        ease: 'power2.out',
      });
    });
  }

  tick() {
    const tiempo = performance.now() * 0.001;

    // Ondular la geometría (como morphing)
    const positions = this.malla.geometry.attributes.position;
    for (let i = 0; i < positions.count; i++) {
      const i3 = i * 3;
      const ox = this.posicionesOriginales[i3];
      const oy = this.posicionesOriginales[i3 + 1];
      const oz = this.posicionesOriginales[i3 + 2];

      // Offset senoidal basado en posición y tiempo
      const offset = Math.sin(ox * 2 + tiempo) *
                     Math.sin(oy * 3 + tiempo * 0.7) *
                     Math.sin(oz * 1.5 + tiempo * 0.5) * 0.15;

      // Normalizar y aplicar offset
      const longitud = Math.sqrt(ox*ox + oy*oy + oz*oz);
      positions.setXYZ(
        i,
        ox + (ox / longitud) * offset,
        oy + (oy / longitud) * offset,
        oz + (oz / longitud) * offset,
      );
    }
    positions.needsUpdate = true;
    this.malla.geometry.computeVertexNormals();

    this.renderer.render(this.escena, this.camera);
    requestAnimationFrame(() => this.tick());
  }
}
```

---

## PARTE 7: PERFORMANCE Y OPTIMIZACIÓN

```javascript
// === REGLAS DE ORO PARA 60FPS ===

// 1. Solo animar transform y opacity — sin layout thrashing
// ✅ Usar siempre:
gsap.to('.el', { x: 100, y: 50, opacity: 0, scale: 1.2 });
// ❌ Evitar:
gsap.to('.el', { left: 100, top: 50, width: 200, height: 100 }); // Causa reflow

// 2. will-change para promover a GPU (usar con moderación)
elemento.style.willChange = 'transform';
// Remover cuando ya no se anima:
elemento.addEventListener('animationend', () => elemento.style.willChange = 'auto');

// 3. Limpiar ScrollTriggers al destruir componentes
function destruirAnimaciones() {
  ScrollTrigger.getAll().forEach(st => st.kill());
  gsap.globalTimeline.clear();
}

// 4. Usar gsap.context() para limpieza automática (como en React/Vue)
const ctx = gsap.context(() => {
  gsap.from('.elemento', { opacity: 0, duration: 1 });
  ScrollTrigger.create({ /* ... */ });
}, scopeElement); // Limitar al scope del componente

// Para limpiar todo:
ctx.revert(); // Revierte todos los cambios y mata todos los triggers

// 5. Throttle de mousemove para Three.js
let mouseMoveTimeout;
window.addEventListener('mousemove', (e) => {
  if (mouseMoveTimeout) return;
  mouseMoveTimeout = requestAnimationFrame(() => {
    actualizarMouse(e);
    mouseMoveTimeout = null;
  });
});

// 6. Detector de dispositivos lentos
const conexionLenta = navigator.connection?.effectiveType === '2g' ||
                      navigator.connection?.effectiveType === 'slow-2g';
const dispositivoLento = navigator.hardwareConcurrency <= 2;
const prefiereMenosMovimiento = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (prefiereMenosMovimiento || conexionLenta || dispositivoLento) {
  // Desactivar animaciones o reducirlas
  gsap.globalTimeline.timeScale(100); // Acelerar todo a casi instantáneo
  // O simplemente no inicializar Three.js / Canvas
}

// 7. Lazy init de Three.js con IntersectionObserver
const observer = new IntersectionObserver((entries) => {
  if (entries[0].isIntersecting) {
    import('./escena3d.js').then(({ init }) => init());
    observer.disconnect();
  }
}, { threshold: 0.1 });
observer.observe(document.querySelector('#canvas-3d'));

// 8. Pooling de objetos Three.js (reutilizar geometrías y materiales)
const geometriaCompartida = new THREE.SphereGeometry(1, 32, 32);
const materialCompartido = new THREE.MeshStandardMaterial({ color: '#3B82F6' });

// Todas las partículas comparten la MISMA geometría y material — mucho más eficiente
particulas.forEach(p => {
  const mesh = new THREE.Mesh(geometriaCompartida, materialCompartido);
  mesh.position.set(p.x, p.y, p.z);
  escena.add(mesh);
});

// 9. InstancedMesh para miles de objetos iguales
const count = 10000;
const instanced = new THREE.InstancedMesh(geometriaCompartida, materialCompartido, count);
const matrix = new THREE.Matrix4();
const color = new THREE.Color();

for (let i = 0; i < count; i++) {
  matrix.setPosition(
    (Math.random() - 0.5) * 20,
    (Math.random() - 0.5) * 20,
    (Math.random() - 0.5) * 20,
  );
  instanced.setMatrixAt(i, matrix);
  instanced.setColorAt(i, color.setHSL(Math.random(), 0.7, 0.6));
}
instanced.instanceMatrix.needsUpdate = true;
instanced.instanceColor.needsUpdate = true;
escena.add(instanced);
```

---

## PARTE 8: PLANTILLA COMPLETA — SITIO AL NIVEL AWWWARDS

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sitio Premium</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    html { overflow-x: hidden; }
    body { background: #050510; color: #fff; font-family: system-ui; }
    canvas { position: fixed; top: 0; left: 0; z-index: 0; pointer-events: none; }
    .contenido { position: relative; z-index: 1; }
    .seccion { min-height: 100vh; display: flex; align-items: center; justify-content: center; padding: 2rem; }
    .hero-titulo { font-size: clamp(3rem, 10vw, 8rem); font-weight: 900; line-height: 1; overflow: hidden; }
    .hero-titulo span { display: block; }
    .overflow-hidden { overflow: hidden; }
    .loading { position: fixed; inset: 0; background: #050510; z-index: 9999; display: flex; align-items: center; justify-content: center; }
    .barra-progreso-wrap { width: 200px; height: 2px; background: #333; }
    .barra-progreso { height: 100%; background: #3B82F6; width: 0%; transition: width 0.3s; }
  </style>
</head>
<body>

<div class="loading" id="loading">
  <div>
    <div class="barra-progreso-wrap"><div class="barra-progreso" id="barra"></div></div>
  </div>
</div>

<canvas id="canvas-bg"></canvas>

<div class="contenido">
  <section class="seccion hero">
    <div class="hero-titulo">
      <div class="overflow-hidden"><span class="linea-1">Diseño</span></div>
      <div class="overflow-hidden"><span class="linea-2">que</span></div>
      <div class="overflow-hidden"><span class="linea-3">impacta.</span></div>
    </div>
  </section>
  <section class="seccion s2">Segunda sección</section>
  <section class="seccion s3">Tercera sección</section>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
gsap.registerPlugin(ScrollTrigger);

// Loading screen
const manager = new THREE.LoadingManager(
  () => {
    gsap.to('#loading', { opacity: 0, duration: 0.8, onComplete: () => document.getElementById('loading').remove() });
    iniciarAnimaciones();
  },
  (url, loaded, total) => {
    document.getElementById('barra').style.width = `${(loaded/total)*100}%`;
  }
);

// Three.js en el fondo
const escena = new THREE.Scene();
const camara = new THREE.PerspectiveCamera(75, innerWidth/innerHeight, 0.1, 100);
camara.position.z = 3;
const renderer = new THREE.WebGLRenderer({ canvas: document.getElementById('canvas-bg'), alpha: true });
renderer.setSize(innerWidth, innerHeight);
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));

// Geometría animada
const geo = new THREE.IcosahedronGeometry(1.5, 5);
const mat = new THREE.MeshStandardMaterial({ color: '#1D4ED8', roughness: 0.3, metalness: 0.9, wireframe: true });
const malla = new THREE.Mesh(geo, mat);
escena.add(malla);
escena.add(new THREE.AmbientLight('#ffffff', 0.5));
const luz = new THREE.DirectionalLight('#6366F1', 2);
luz.position.set(5, 5, 5);
escena.add(luz);

window.addEventListener('resize', () => {
  camara.aspect = innerWidth/innerHeight;
  camara.updateProjectionMatrix();
  renderer.setSize(innerWidth, innerHeight);
});

let reloj = { ultimo: 0 };
function tick(tiempo) {
  const t = tiempo * 0.001;
  malla.rotation.x = t * 0.2;
  malla.rotation.y = t * 0.3;
  renderer.render(escena, camara);
  requestAnimationFrame(tick);
}
requestAnimationFrame(tick);

// Finalizar carga (simular recursos)
setTimeout(() => manager.onLoad(), 1000);

function iniciarAnimaciones() {
  const tl = gsap.timeline({ defaults: { ease: 'power3.out', duration: 1 } });
  tl.from('.linea-1', { y: '110%', delay: 0.2 })
    .from('.linea-2', { y: '110%' }, '-=0.7')
    .from('.linea-3', { y: '110%' }, '-=0.7');

  gsap.to(malla.rotation, {
    y: Math.PI * 2,
    ease: 'none',
    scrollTrigger: { trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 1 },
  });
}
</script>
</body>
</html>
```

---

## PARTE 9: REFERENCIA RÁPIDA — EFECTOS POR NOMBRE

```javascript
// ¿Qué librería usar para cada efecto?

const efectos = {
  // GSAP
  'elementos que entran con scroll':     'gsap.from() + ScrollTrigger',
  'parallax':                            'gsap.to() scrub + ScrollTrigger',
  'texto que se escribe':                'TextPlugin de GSAP',
  'sección anclada mientras scrolleas':  'ScrollTrigger pin:true',
  'scroll horizontal':                   'ScrollTrigger + xPercent',
  'contador animado':                    'gsap.to() en objeto plano',
  'morphing de layout':                  'Flip plugin de GSAP',
  'transición entre páginas':            'Barba.js + GSAP',
  'timeline de animación compleja':      'gsap.timeline()',
  'hover con física (rebote, elástico)': 'gsap.to() + ease back/elastic',

  // Three.js
  'objeto 3D rotando':                   'Three.js Mesh + requestAnimationFrame',
  'modelo de coche/producto 3D':         'Three.js + GLTFLoader',
  'fondo de partículas 3D':              'Three.js PointsMaterial',
  'efecto de onda/blob 3D':              'Three.js ShaderMaterial + GLSL',
  'efecto de glow/bloom':                'Three.js UnrealBloomPass',
  'tierra/planeta 3D':                   'Three.js SphereGeometry + texturas',
  'vidrio/cristal':                      'Three.js MeshPhysicalMaterial transmission',
  'miles de objetos iguales':            'Three.js InstancedMesh',

  // Canvas 2D
  'red de partículas con líneas':        'Canvas API + física manual',
  'texto que explota en partículas':     'Canvas getImageData + física',
  'ola líquida animada':                 'Canvas API + Math.sin()',
  'efecto plasma/psicodélico':           'Canvas putImageData + loops',
  'raycasting (click en objetos 3D)':    'Three.js Raycaster',
  'confeti':                             'Canvas API + física de caída',

  // CSS + GSAP (sin librerías 3D)
  'scroll suave con inercia':            'Lenis',
  'tarjetas que entran en stagger':      'ScrollTrigger.batch()',
  'clip reveal (cortina)':               'GSAP clipPath animation',
  'número que cuenta':                   'GSAP + objeto plano',
  'cursor personalizado':                'JS mousemove + GSAP lagging',
};
```

---

## CHECKLIST DE SITIO DE ALTO NIVEL

```
ANTES DE EMPEZAR
□ ¿Tiene sentido el 3D/Canvas o es innecesario?
□ ¿Hay fallback para dispositivos lentos?
□ ¿Se respeta prefers-reduced-motion?
□ ¿Funciona en móvil sin lag?

SETUP
□ Loading screen mientras cargan assets 3D
□ Three.js con pixelRatio limitado a Math.min(dpr, 2)
□ resize handler en renderer, cámara y canvas
□ ScrollTrigger.refresh() en puntos de cambio de layout

PERFORMANCE
□ Solo animar transform/opacity con GSAP (no width/height/left/top)
□ InstancedMesh para objetos repetidos (>100)
□ will-change removido después de la animación
□ requestAnimationFrame único por escena
□ Dispose de geometrías y materiales al destruir
□ ScrollTrigger.kill() al navegar en SPAs
□ gsap.context() para cleanup automático

CALIDAD VISUAL
□ PostprocesadoBbloom para efectos de glow
□ Antialiasing activado (antialias: true o SMAAPass)
□ ACES Filmic tone mapping en Three.js
□ Sombras con PCFSoftShadowMap
□ Environment map para reflections realistas
□ Scroll suave con Lenis integrado con ScrollTrigger

ACCESIBILIDAD
□ Canvas con role="img" y aria-label
□ Animaciones desactivables con prefers-reduced-motion
□ Contenido real disponible aunque no cargue Three.js
□ No depender de animaciones para comunicar información
```

---

*Fin del archivo ANIMACIONES_AVANZADAS_EXPERTO.md — Conocimiento completo de GSAP, ScrollTrigger, Three.js, Canvas API, física de partículas, postprocesado y patrones de integración al nivel de Tesla, Apple y Awwwards.*
