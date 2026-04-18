# 🎨 ARCHIVO DE CONOCIMIENTO EXPERTO — TAILWIND CSS COMPLETO
> Versión: Máxima. Cubre Tailwind CSS v3/v4 completo + @midudev/tailwind-animations + patrones profesionales.
> Instrucción para la IA: Al cargar este archivo, te conviertes en un experto absoluto en Tailwind CSS. Sabes cada utilidad, configuración, patrón de componente y técnica avanzada. Combinas Tailwind con las animaciones de Midudev para crear interfaces modernas y elegantes sin escribir CSS manual.

---

## 📌 IDENTIDAD DEL EXPERTO

Eres un experto de nivel mundial en Tailwind CSS. Conoces:
- Tailwind CSS v3 y v4 completo (todas las utilidades)
- Configuración avanzada de tailwind.config.js
- Sistema de diseño con tokens personalizados
- Responsive design mobile-first
- Dark mode, estados y variantes
- JIT (Just-In-Time) compiler
- @midudev/tailwind-animations — librería de animaciones
- Componentes profesionales con Tailwind
- Integración con React, Vue, Astro, Next.js
- Optimización y purga de CSS
- Accesibilidad con Tailwind

---

## 1. INSTALACIÓN Y CONFIGURACIÓN

### Instalación básica
```bash
# Con npm
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Con Vite
npm create vite@latest mi-proyecto
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Con Next.js (ya viene integrado)
npx create-next-app@latest mi-proyecto

# Con Astro
npm create astro@latest
npx astro add tailwind

# Tailwind v4 (nuevo — sin config file por defecto)
npm install tailwindcss@next @tailwindcss/vite
```

### Agregar @midudev/tailwind-animations
```bash
npm install @midudev/tailwind-animations
```

### tailwind.config.js — Configuración completa
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  // Archivos donde Tailwind buscará clases (obligatorio)
  content: [
    './src/**/*.{html,js,ts,jsx,tsx,vue,astro,svelte}',
    './index.html',
    './public/**/*.html',
    // Safelist — clases generadas dinámicamente
  ],

  // Safelist — clases que NUNCA se purguen aunque no aparezcan en el código
  safelist: [
    'bg-red-500',
    'text-3xl',
    { pattern: /bg-(red|green|blue)-(100|500|900)/ }, // Regex
    {
      pattern: /animate-.+/,  // Todas las animaciones de Midudev
      variants: ['hover', 'focus'],
    },
  ],

  // Dark mode
  darkMode: 'class',      // 'class' = manual con clase .dark
  // darkMode: 'media',   // 'media' = según preferencia del sistema
  // darkMode: ['class', '[data-theme="dark"]'], // Selector personalizado

  theme: {
    // === OVERRIDE COMPLETO (reemplaza los defaults) ===
    screens: {
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
      // Personalizados
      'xs': '475px',
      '3xl': '1920px',
      // Mobile-first (min-width por defecto)
      // 'max-sm': { max: '639px' }, // max-width breakpoints
    },

    // === EXTEND (agrega a los defaults sin reemplazarlos) ===
    extend: {
      // Colores personalizados
      colors: {
        // Paleta completa personalizada
        primario: {
          50:  '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
          950: '#172554',
        },
        // Colores semánticos
        marca: '#FF5733',
        'marca-oscuro': '#C0392B',
        fondo: {
          claro: '#FAFAFA',
          oscuro: '#0A0A0A',
        },
        // Con CSS variables (para temas dinámicos)
        base: 'rgb(var(--color-base) / <alpha-value>)',
        acento: 'rgb(var(--color-acento) / <alpha-value>)',
      },

      // Tipografía
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        serif: ['Playfair Display', 'Georgia', 'serif'],
        mono: ['JetBrains Mono', 'Fira Code', 'monospace'],
        display: ['Cal Sans', 'Inter', 'sans-serif'],
      },

      fontSize: {
        'xs':   ['0.75rem',  { lineHeight: '1rem' }],
        'sm':   ['0.875rem', { lineHeight: '1.25rem' }],
        'base': ['1rem',     { lineHeight: '1.5rem' }],
        'lg':   ['1.125rem', { lineHeight: '1.75rem' }],
        'xl':   ['1.25rem',  { lineHeight: '1.75rem' }],
        '2xl':  ['1.5rem',   { lineHeight: '2rem' }],
        '3xl':  ['1.875rem', { lineHeight: '2.25rem' }],
        '4xl':  ['2.25rem',  { lineHeight: '2.5rem' }],
        '5xl':  ['3rem',     { lineHeight: '1.1' }],
        '6xl':  ['3.75rem',  { lineHeight: '1' }],
        '7xl':  ['4.5rem',   { lineHeight: '1' }],
        '8xl':  ['6rem',     { lineHeight: '1' }],
        '9xl':  ['8rem',     { lineHeight: '1' }],
        // Personalizados
        'hero': ['clamp(3rem, 8vw, 7rem)', { lineHeight: '1', letterSpacing: '-0.02em' }],
        'display': ['clamp(2rem, 5vw, 4rem)', { lineHeight: '1.1' }],
      },

      // Espaciado
      spacing: {
        '4.5': '1.125rem',
        '13': '3.25rem',
        '15': '3.75rem',
        '18': '4.5rem',
        '22': '5.5rem',
        '30': '7.5rem',
        '128': '32rem',
        '144': '36rem',
      },

      // Bordes redondeados
      borderRadius: {
        'sm': '0.125rem',
        DEFAULT: '0.25rem',
        'md': '0.375rem',
        'lg': '0.5rem',
        'xl': '0.75rem',
        '2xl': '1rem',
        '3xl': '1.5rem',
        '4xl': '2rem',
        'pill': '9999px',
      },

      // Sombras
      boxShadow: {
        'xs': '0 1px 2px 0 rgb(0 0 0 / 0.05)',
        'sm': '0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)',
        DEFAULT: '0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)',
        'md': '0 4px 6px -1px rgb(0 0 0 / 0.1)',
        'lg': '0 10px 15px -3px rgb(0 0 0 / 0.1)',
        'xl': '0 20px 25px -5px rgb(0 0 0 / 0.1)',
        '2xl': '0 25px 50px -12px rgb(0 0 0 / 0.25)',
        'glow': '0 0 20px rgb(59 130 246 / 0.5)',
        'glow-lg': '0 0 40px rgb(59 130 246 / 0.4)',
        'inner-glow': 'inset 0 0 20px rgb(59 130 246 / 0.2)',
        'card': '0 2px 8px rgb(0 0 0 / 0.08), 0 8px 32px rgb(0 0 0 / 0.06)',
        'none': 'none',
      },

      // Animaciones personalizadas
      keyframes: {
        shimmer: {
          '0%': { backgroundPosition: '-200% 0' },
          '100%': { backgroundPosition: '200% 0' },
        },
        float: {
          '0%, 100%': { transform: 'translateY(0px)' },
          '50%': { transform: 'translateY(-10px)' },
        },
        'pulse-ring': {
          '0%': { transform: 'scale(0.8)', opacity: '1' },
          '100%': { transform: 'scale(2)', opacity: '0' },
        },
        'slide-in-right': {
          '0%': { transform: 'translateX(100%)', opacity: '0' },
          '100%': { transform: 'translateX(0)', opacity: '1' },
        },
        'gradient-x': {
          '0%, 100%': { backgroundPosition: '0% 50%' },
          '50%': { backgroundPosition: '100% 50%' },
        },
        typewriter: {
          '0%': { width: '0%' },
          '100%': { width: '100%' },
        },
      },

      animation: {
        'shimmer': 'shimmer 2s linear infinite',
        'float': 'float 3s ease-in-out infinite',
        'pulse-ring': 'pulse-ring 1.5s cubic-bezier(0.215, 0.61, 0.355, 1) infinite',
        'slide-in-right': 'slide-in-right 0.3s ease-out',
        'gradient-x': 'gradient-x 4s ease infinite',
        'typewriter': 'typewriter 3s steps(40) forwards',
      },

      // Transiciones
      transitionDuration: {
        '0': '0ms',
        '75': '75ms',
        '100': '100ms',
        '150': '150ms',
        '200': '200ms',
        '300': '300ms',
        '500': '500ms',
        '700': '700ms',
        '1000': '1000ms',
      },

      // Z-index
      zIndex: {
        '1': '1',
        '60': '60',
        '70': '70',
        '80': '80',
        '90': '90',
        '100': '100',
      },

      // Aspect ratio
      aspectRatio: {
        '4/3': '4 / 3',
        '3/2': '3 / 2',
        '21/9': '21 / 9',
      },

      // Backdrop blur
      backdropBlur: {
        'xs': '2px',
      },

      // Gradientes (via backgroundImage)
      backgroundImage: {
        'gradient-radial': 'radial-gradient(var(--tw-gradient-stops))',
        'gradient-conic': 'conic-gradient(from 180deg at 50% 50%, var(--tw-gradient-stops))',
        'gradient-mesh': 'radial-gradient(at 40% 20%, hsla(213,100%,74%,1) 0px, transparent 50%), radial-gradient(at 80% 0%, hsla(189,100%,56%,1) 0px, transparent 50%)',
        'shimmer': 'linear-gradient(90deg, transparent 25%, rgba(255,255,255,0.1) 50%, transparent 75%)',
        'noise': "url(\"data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.4'/%3E%3C/svg%3E\")",
      },
    },
  },

  plugins: [
    // Plugin de animaciones de Midudev
    require('@midudev/tailwind-animations'),

    // Plugins oficiales de Tailwind
    require('@tailwindcss/typography'),    // Estilos para prose/markdown
    require('@tailwindcss/forms'),         // Reset de formularios
    require('@tailwindcss/aspect-ratio'), // Aspect ratio (legacy)
    require('@tailwindcss/container-queries'), // Container queries

    // Plugin personalizado
    function({ addUtilities, addComponents, addBase, theme, matchUtilities }) {
      // Utilidades personalizadas
      addUtilities({
        '.text-balance': { 'text-wrap': 'balance' },
        '.text-pretty': { 'text-wrap': 'pretty' },
        '.scrollbar-none': {
          '-ms-overflow-style': 'none',
          'scrollbar-width': 'none',
          '&::-webkit-scrollbar': { display: 'none' },
        },
        '.gpu': { transform: 'translateZ(0)' },
        '.backface-hidden': { 'backface-visibility': 'hidden' },
        '.preserve-3d': { 'transform-style': 'preserve-3d' },
      });

      // Componentes personalizados
      addComponents({
        '.btn': {
          display: 'inline-flex',
          alignItems: 'center',
          justifyContent: 'center',
          padding: `${theme('spacing.2')} ${theme('spacing.4')}`,
          borderRadius: theme('borderRadius.lg'),
          fontWeight: theme('fontWeight.medium'),
          transition: 'all 150ms ease',
          cursor: 'pointer',
        },
        '.card': {
          backgroundColor: theme('colors.white'),
          borderRadius: theme('borderRadius.2xl'),
          padding: theme('spacing.6'),
          boxShadow: theme('boxShadow.card'),
        },
      });

      // Utilidades con valores dinámicos
      matchUtilities(
        {
          'text-shadow': (value) => ({ textShadow: value }),
        },
        {
          values: {
            sm: '0 1px 2px rgba(0,0,0,0.3)',
            DEFAULT: '0 2px 4px rgba(0,0,0,0.3)',
            lg: '0 4px 8px rgba(0,0,0,0.3)',
          },
        }
      );
    },
  ],
};
```

### CSS base (input.css / globals.css)
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Variables CSS globales */
@layer base {
  :root {
    --color-base: 255 255 255;
    --color-acento: 59 130 246;
    --header-height: 4rem;
  }

  .dark {
    --color-base: 10 10 10;
    --color-acento: 99 179 237;
  }

  /* Reset y base */
  * { @apply border-border; }
  body { @apply bg-background text-foreground antialiased; }
  h1, h2, h3, h4, h5, h6 { @apply font-display tracking-tight; }
}

/* Componentes reutilizables */
@layer components {
  .btn-primario {
    @apply inline-flex items-center justify-center
           px-6 py-3 rounded-xl font-semibold
           bg-blue-600 text-white
           hover:bg-blue-700 active:scale-95
           transition-all duration-200
           focus-visible:outline-none focus-visible:ring-2
           focus-visible:ring-blue-500 focus-visible:ring-offset-2
           disabled:opacity-50 disabled:cursor-not-allowed;
  }

  .btn-secundario {
    @apply inline-flex items-center justify-center
           px-6 py-3 rounded-xl font-semibold
           bg-transparent border-2 border-gray-200
           text-gray-700 dark:text-gray-200 dark:border-gray-700
           hover:bg-gray-50 dark:hover:bg-gray-800
           active:scale-95 transition-all duration-200;
  }

  .card {
    @apply bg-white dark:bg-gray-900
           rounded-2xl p-6 border border-gray-100 dark:border-gray-800
           shadow-card hover:shadow-lg
           transition-shadow duration-300;
  }

  .input-base {
    @apply w-full px-4 py-3 rounded-xl
           border border-gray-200 dark:border-gray-700
           bg-white dark:bg-gray-900
           text-gray-900 dark:text-gray-100
           placeholder:text-gray-400
           focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent
           transition-all duration-200;
  }
}

/* Utilidades personalizadas */
@layer utilities {
  .gradient-text {
    @apply bg-gradient-to-r from-blue-600 to-purple-600
           bg-clip-text text-transparent;
  }

  .glass {
    @apply bg-white/10 backdrop-blur-md
           border border-white/20;
  }

  .glass-dark {
    @apply bg-black/20 backdrop-blur-md
           border border-white/10;
  }
}
```

---

## 2. TODAS LAS UTILIDADES — REFERENCIA COMPLETA

### Layout
```html
<!-- Display -->
<div class="block">           <!-- display: block -->
<div class="inline-block">    <!-- display: inline-block -->
<div class="inline">          <!-- display: inline -->
<div class="flex">            <!-- display: flex -->
<div class="inline-flex">
<div class="grid">            <!-- display: grid -->
<div class="inline-grid">
<div class="contents">        <!-- display: contents -->
<div class="hidden">          <!-- display: none -->
<div class="table">
<div class="table-cell">
<div class="table-row">

<!-- Position -->
<div class="static">
<div class="relative">
<div class="absolute">
<div class="fixed">
<div class="sticky top-0">    <!-- sticky con top-0 -->

<!-- Inset (top/right/bottom/left) -->
<div class="inset-0">         <!-- top/right/bottom/left: 0 -->
<div class="inset-x-0">       <!-- left + right: 0 -->
<div class="inset-y-0">       <!-- top + bottom: 0 -->
<div class="top-0 right-0 bottom-0 left-0">
<div class="top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"> <!-- Centrado absoluto -->

<!-- Z-index -->
<div class="z-0 z-10 z-20 z-30 z-40 z-50 z-auto">

<!-- Overflow -->
<div class="overflow-hidden overflow-auto overflow-scroll overflow-visible">
<div class="overflow-x-auto overflow-y-hidden">

<!-- Visibility -->
<div class="visible invisible">

<!-- Container -->
<div class="container mx-auto px-4">  <!-- Centrado con padding -->
```

### Flexbox completo
```html
<!-- Dirección -->
<div class="flex flex-row">           <!-- → por defecto -->
<div class="flex flex-row-reverse">   <!-- ← -->
<div class="flex flex-col">           <!-- ↓ -->
<div class="flex flex-col-reverse">   <!-- ↑ -->

<!-- Wrap -->
<div class="flex flex-wrap">
<div class="flex flex-nowrap">        <!-- por defecto -->
<div class="flex flex-wrap-reverse">

<!-- Justify Content (eje principal) -->
<div class="flex justify-start">      <!-- por defecto -->
<div class="flex justify-end">
<div class="flex justify-center">
<div class="flex justify-between">
<div class="flex justify-around">
<div class="flex justify-evenly">
<div class="flex justify-stretch">

<!-- Align Items (eje secundario) -->
<div class="flex items-start">
<div class="flex items-end">
<div class="flex items-center">       <!-- EL MÁS USADO -->
<div class="flex items-baseline">
<div class="flex items-stretch">      <!-- por defecto -->

<!-- Align Content (múltiples líneas) -->
<div class="flex flex-wrap content-start content-center content-between">

<!-- Gap -->
<div class="flex gap-4">             <!-- gap: 1rem -->
<div class="flex gap-x-4 gap-y-2">   <!-- gap horizontal y vertical -->

<!-- Flex items -->
<div class="flex-1">     <!-- flex: 1 1 0% -->
<div class="flex-auto">  <!-- flex: 1 1 auto -->
<div class="flex-none">  <!-- flex: none -->
<div class="flex-initial"> <!-- flex: 0 1 auto -->
<div class="grow">       <!-- flex-grow: 1 -->
<div class="grow-0">     <!-- flex-grow: 0 -->
<div class="shrink">     <!-- flex-shrink: 1 -->
<div class="shrink-0">   <!-- flex-shrink: 0 -->
<div class="basis-1/2">  <!-- flex-basis: 50% -->
<div class="order-first order-last order-none">
<div class="order-1 order-2 order-3">
<div class="self-start self-center self-end self-stretch">

<!-- Patrón: logo izquierda, nav derecha -->
<header class="flex items-center">
  <div class="logo mr-auto">Logo</div>  <!-- mr-auto empuja el resto -->
  <nav class="flex gap-6">...</nav>
</header>
```

### Grid completo
```html
<!-- Columnas -->
<div class="grid grid-cols-1">
<div class="grid grid-cols-2">
<div class="grid grid-cols-3">
<div class="grid grid-cols-4">
<div class="grid grid-cols-6">
<div class="grid grid-cols-12">
<div class="grid grid-cols-none">
<div class="grid grid-cols-subgrid">  <!-- hereda columnas del padre -->

<!-- Filas -->
<div class="grid grid-rows-1 grid-rows-2 grid-rows-3 grid-rows-none">
<div class="grid grid-rows-subgrid">

<!-- Gap -->
<div class="grid gap-4">
<div class="grid gap-x-8 gap-y-4">

<!-- Posición de items -->
<div class="col-span-2">     <!-- ocupa 2 columnas -->
<div class="col-span-full">  <!-- ocupa todas las columnas -->
<div class="col-start-1 col-end-3">
<div class="row-span-2">
<div class="row-span-full">

<!-- Auto flow -->
<div class="grid grid-flow-row">
<div class="grid grid-flow-col">
<div class="grid grid-flow-dense">
<div class="grid grid-flow-row-dense">

<!-- Auto columns/rows -->
<div class="grid auto-cols-auto auto-cols-min auto-cols-max auto-cols-fr">
<div class="grid auto-rows-auto auto-rows-min auto-rows-fr">

<!-- Alineación -->
<div class="grid justify-items-start justify-items-center justify-items-end justify-items-stretch">
<div class="grid place-items-center">   <!-- justify + align center -->
<div class="grid place-content-center">

<!-- Items individuales -->
<div class="justify-self-start justify-self-center justify-self-end justify-self-stretch">
<div class="place-self-center">

<!-- Patrón: Grid responsive sin media queries -->
<div class="grid grid-cols-[repeat(auto-fill,minmax(280px,1fr))] gap-6">
  <!-- Esta es la técnica más poderosa de grid responsivo -->
</div>

<!-- Valor arbitrario para columnas complejas -->
<div class="grid grid-cols-[200px_1fr_auto]">
<div class="grid grid-cols-[repeat(3,minmax(0,1fr))]">
```

### Espaciado
```html
<!-- Padding -->
<div class="p-0 p-1 p-2 p-3 p-4 p-5 p-6 p-8 p-10 p-12 p-16 p-20 p-24">
<!-- p-1 = 0.25rem, p-2 = 0.5rem, p-4 = 1rem, p-8 = 2rem -->
<div class="px-4 py-2">         <!-- horizontal | vertical -->
<div class="pt-4 pr-2 pb-4 pl-2"> <!-- top right bottom left -->
<div class="ps-4 pe-4">         <!-- inline-start | inline-end (lógico) -->

<!-- Margin -->
<div class="m-auto mx-auto">    <!-- centrado horizontal -->
<div class="mx-4 my-8">
<div class="mt-4 mr-2 mb-4 ml-2">
<div class="-mt-4">             <!-- margen negativo -->
<div class="ms-auto">           <!-- empujar a la derecha (lógico) -->

<!-- Space between (para hijos) -->
<div class="flex space-x-4">   <!-- margen izquierdo a cada hijo excepto el primero -->
<div class="flex space-y-4">
```

### Tamaños
```html
<!-- Width -->
<div class="w-0 w-px w-0.5 w-1 w-2 w-4 w-8 w-16 w-32 w-64">
<div class="w-1/2 w-1/3 w-2/3 w-1/4 w-3/4 w-1/5 w-2/5">
<div class="w-full">          <!-- 100% -->
<div class="w-screen">        <!-- 100vw -->
<div class="w-svw w-lvw w-dvw"> <!-- small/large/dynamic viewport width -->
<div class="w-min w-max w-fit"> <!-- min/max/fit-content -->
<div class="w-auto">

<!-- Max/Min Width -->
<div class="max-w-xs">    <!-- 20rem -->
<div class="max-w-sm">    <!-- 24rem -->
<div class="max-w-md">    <!-- 28rem -->
<div class="max-w-lg">    <!-- 32rem -->
<div class="max-w-xl">    <!-- 36rem -->
<div class="max-w-2xl">   <!-- 42rem -->
<div class="max-w-3xl">   <!-- 48rem -->
<div class="max-w-4xl">   <!-- 56rem -->
<div class="max-w-5xl">   <!-- 64rem -->
<div class="max-w-6xl">   <!-- 72rem -->
<div class="max-w-7xl">   <!-- 80rem -->
<div class="max-w-full max-w-screen-xl max-w-none">
<div class="min-w-0 min-w-full min-w-min min-w-max">

<!-- Height -->
<div class="h-0 h-px h-1 h-4 h-8 h-16 h-32 h-64 h-auto">
<div class="h-1/2 h-full">
<div class="h-screen">         <!-- 100vh -->
<div class="h-svh h-lvh h-dvh"> <!-- small/large/dynamic — usar dvh en móviles -->
<div class="min-h-screen min-h-0 min-h-full">
<div class="max-h-screen max-h-full max-h-none">

<!-- Size (width y height juntos — v3.4+) -->
<div class="size-4">   <!-- w-4 h-4 -->
<div class="size-full"> <!-- w-full h-full -->

<!-- Aspect Ratio -->
<div class="aspect-square">   <!-- 1/1 -->
<div class="aspect-video">    <!-- 16/9 -->
<div class="aspect-[4/3]">    <!-- valor arbitrario -->
```

---

## 3. TIPOGRAFÍA COMPLETA

```html
<!-- Font Family -->
<p class="font-sans font-serif font-mono">

<!-- Font Size -->
<p class="text-xs text-sm text-base text-lg text-xl text-2xl text-3xl text-4xl text-5xl text-6xl text-7xl text-8xl text-9xl">

<!-- Font Weight -->
<p class="font-thin font-extralight font-light font-normal font-medium font-semibold font-bold font-extrabold font-black">
<!-- 100        200          300        400        500        600          700       800          900 -->

<!-- Font Style -->
<p class="italic not-italic">

<!-- Line Height -->
<p class="leading-none leading-tight leading-snug leading-normal leading-relaxed leading-loose">
<p class="leading-3 leading-4 leading-5 leading-6 leading-7 leading-8 leading-9 leading-10">

<!-- Letter Spacing -->
<p class="tracking-tighter tracking-tight tracking-normal tracking-wide tracking-wider tracking-widest">

<!-- Text Align -->
<p class="text-left text-center text-right text-justify text-start text-end">

<!-- Text Color -->
<p class="text-transparent text-current text-black text-white">
<p class="text-gray-500 text-blue-600 text-red-500 text-green-400">
<p class="text-gray-900 dark:text-gray-100"> <!-- con dark mode -->

<!-- Text Decoration -->
<p class="underline overline line-through no-underline">
<p class="decoration-blue-500 decoration-2 decoration-dashed underline-offset-4 underline">

<!-- Text Transform -->
<p class="uppercase lowercase capitalize normal-case">

<!-- Text Overflow -->
<p class="truncate">          <!-- white-space: nowrap + overflow: hidden + text-overflow: ellipsis -->
<p class="overflow-ellipsis overflow-clip">
<!-- Multiline truncate: -->
<p class="line-clamp-1 line-clamp-2 line-clamp-3 line-clamp-4 line-clamp-5 line-clamp-6 line-clamp-none">

<!-- Whitespace -->
<p class="whitespace-normal whitespace-nowrap whitespace-pre whitespace-pre-line whitespace-pre-wrap whitespace-break-spaces">

<!-- Word Break -->
<p class="break-normal break-words break-all break-keep">

<!-- Text Wrap (v3.4+) -->
<h1 class="text-balance">   <!-- distribución equilibrada de líneas -->
<p class="text-pretty">     <!-- evita huérfanas -->

<!-- Vertical Align -->
<span class="align-baseline align-top align-middle align-bottom align-text-top align-text-bottom">

<!-- Lista de estilos -->
<ul class="list-disc list-decimal list-none">
<ul class="list-inside list-outside">
<li class="marker:text-blue-500">  <!-- estilizar el marker -->

<!-- Prose (con plugin @tailwindcss/typography) -->
<article class="prose prose-lg prose-blue dark:prose-invert max-w-none">
  <!-- Estilos automáticos para markdown/HTML -->
</article>
```

---

## 4. COLORES Y FONDOS

```html
<!-- Colores disponibles de Tailwind -->
<!-- slate, gray, zinc, neutral, stone, red, orange, amber, yellow, lime, green, emerald, teal, cyan, sky, blue, indigo, violet, purple, fuchsia, pink, rose -->
<!-- Escalas: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 -->

<!-- Background Color -->
<div class="bg-white bg-black bg-transparent bg-current">
<div class="bg-blue-500 bg-blue-500/50">  <!-- /50 = 50% opacidad -->
<div class="bg-[#3B82F6]">               <!-- valor arbitrario -->
<div class="dark:bg-gray-900">           <!-- dark mode -->

<!-- Gradientes -->
<div class="bg-gradient-to-r from-blue-500 to-purple-600">
<div class="bg-gradient-to-br from-blue-500 via-purple-500 to-pink-500">
<div class="bg-gradient-to-t from-black/80 to-transparent">
<!-- Direcciones: to-t to-tr to-r to-br to-b to-bl to-l to-tl -->
<!-- from-{color} via-{color} to-{color} -->
<!-- from-0% from-50% — posición del color stop -->

<!-- Background Size y Position -->
<div class="bg-auto bg-cover bg-contain">
<div class="bg-center bg-top bg-bottom bg-left bg-right">
<div class="bg-no-repeat bg-repeat bg-repeat-x bg-repeat-y">

<!-- Background Attachment -->
<div class="bg-fixed bg-local bg-scroll">

<!-- Background Clip -->
<div class="bg-clip-border bg-clip-padding bg-clip-content bg-clip-text">
<!-- Texto con gradiente: -->
<span class="bg-gradient-to-r from-blue-500 to-purple-600 bg-clip-text text-transparent">
  Texto con gradiente
</span>

<!-- Background Origin -->
<div class="bg-origin-border bg-origin-padding bg-origin-content">

<!-- Text Color con opacidad -->
<p class="text-blue-500/75">  <!-- 75% opacidad -->
<p class="text-blue-500/[0.67]"> <!-- valor arbitrario -->
```

---

## 5. BORDES Y EFECTOS VISUALES

```html
<!-- Border Width -->
<div class="border">          <!-- 1px -->
<div class="border-0 border-2 border-4 border-8">
<div class="border-t border-r border-b border-l">  <!-- lados individuales -->
<div class="border-x border-y">                    <!-- horizontal/vertical -->
<div class="border-s border-e">                    <!-- lógico -->

<!-- Border Color -->
<div class="border border-gray-200 dark:border-gray-700">
<div class="border border-blue-500/50">  <!-- con opacidad -->

<!-- Border Style -->
<div class="border-solid border-dashed border-dotted border-double border-hidden border-none">

<!-- Border Radius -->
<div class="rounded-none rounded-sm rounded rounded-md rounded-lg rounded-xl rounded-2xl rounded-3xl rounded-full">
<div class="rounded-t-lg rounded-r-lg rounded-b-lg rounded-l-lg">
<div class="rounded-tl-lg rounded-tr-lg rounded-bl-lg rounded-br-lg">
<div class="rounded-ss-lg rounded-se-lg rounded-es-lg rounded-ee-lg"> <!-- lógico -->

<!-- Outline -->
<div class="outline outline-2 outline-offset-2 outline-blue-500">
<div class="outline-none">  <!-- para quitar focus ring nativo -->
<!-- ¡Nunca usar outline-none sin alternativa accesible! -->
<div class="focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">

<!-- Box Shadow -->
<div class="shadow-none shadow-sm shadow shadow-md shadow-lg shadow-xl shadow-2xl shadow-inner">
<div class="shadow-blue-500/50">  <!-- sombra con color -->
<div class="dark:shadow-blue-500/20">

<!-- Ring (para focus) -->
<div class="ring-0 ring-1 ring-2 ring-4 ring-8 ring-inset">
<div class="ring ring-blue-500 ring-offset-2 ring-offset-white">

<!-- Opacity -->
<div class="opacity-0 opacity-5 opacity-10 opacity-20 opacity-25 opacity-30 opacity-40 opacity-50 opacity-60 opacity-70 opacity-75 opacity-80 opacity-90 opacity-95 opacity-100">

<!-- Mix Blend Mode -->
<div class="mix-blend-normal mix-blend-multiply mix-blend-screen mix-blend-overlay mix-blend-darken mix-blend-lighten mix-blend-color-dodge mix-blend-color-burn mix-blend-hard-light mix-blend-soft-light mix-blend-difference mix-blend-exclusion mix-blend-hue mix-blend-saturation mix-blend-color mix-blend-luminosity">

<!-- Background Blend Mode -->
<div class="bg-blend-multiply bg-blend-screen bg-blend-overlay">

<!-- Filter -->
<div class="blur-none blur-sm blur blur-md blur-lg blur-xl blur-2xl blur-3xl">
<div class="blur-[2px]">  <!-- valor arbitrario -->
<div class="brightness-0 brightness-50 brightness-75 brightness-90 brightness-95 brightness-100 brightness-105 brightness-110 brightness-125 brightness-150 brightness-200">
<div class="contrast-0 contrast-50 contrast-75 contrast-100 contrast-125 contrast-150 contrast-200">
<div class="grayscale grayscale-0">
<div class="hue-rotate-0 hue-rotate-15 hue-rotate-30 hue-rotate-60 hue-rotate-90 hue-rotate-180">
<div class="invert invert-0">
<div class="saturate-0 saturate-50 saturate-100 saturate-150 saturate-200">
<div class="sepia sepia-0">
<div class="drop-shadow-sm drop-shadow drop-shadow-md drop-shadow-lg drop-shadow-xl drop-shadow-2xl drop-shadow-none">

<!-- Backdrop Filter (efecto cristal/glass) -->
<div class="backdrop-blur-sm backdrop-blur backdrop-blur-md backdrop-blur-lg backdrop-blur-xl backdrop-blur-2xl backdrop-blur-3xl">
<div class="backdrop-brightness-75 backdrop-brightness-90">
<div class="backdrop-saturate-150 backdrop-grayscale backdrop-invert">
```

---

## 6. TRANSFORMS Y TRANSICIONES

```html
<!-- Transforms -->
<div class="translate-x-4 translate-y-4">        <!-- 1rem -->
<div class="translate-x-1/2 -translate-y-1/2">  <!-- 50% — para centrado -->
<div class="translate-x-full -translate-x-full"> <!-- 100% -->

<div class="scale-0 scale-50 scale-75 scale-90 scale-95 scale-100 scale-105 scale-110 scale-125 scale-150">
<div class="scale-x-75 scale-y-125">

<div class="rotate-0 rotate-1 rotate-2 rotate-3 rotate-6 rotate-12 rotate-45 rotate-90 rotate-180">
<div class="-rotate-6 -rotate-12 -rotate-45">   <!-- negativo -->

<div class="skew-x-0 skew-x-1 skew-x-2 skew-x-3 skew-x-6 skew-x-12">
<div class="skew-y-0 skew-y-6">

<div class="origin-center origin-top origin-top-right origin-right origin-bottom-right origin-bottom origin-bottom-left origin-left origin-top-left">

<!-- Transition -->
<div class="transition">           <!-- transition: color, background-color, border-color, etc. -->
<div class="transition-all">       <!-- todas las propiedades -->
<div class="transition-colors">    <!-- solo colores -->
<div class="transition-opacity">   <!-- solo opacity -->
<div class="transition-shadow">    <!-- solo sombras -->
<div class="transition-transform"> <!-- solo transforms -->
<div class="transition-none">

<!-- Duration -->
<div class="duration-0 duration-75 duration-100 duration-150 duration-200 duration-300 duration-500 duration-700 duration-1000">

<!-- Easing -->
<div class="ease-linear ease-in ease-out ease-in-out">
<div class="ease-[cubic-bezier(0.68,-0.55,0.265,1.55)]">  <!-- arbitrario -->

<!-- Delay -->
<div class="delay-0 delay-75 delay-100 delay-150 delay-200 delay-300 delay-500 delay-700 delay-1000">

<!-- Will Change (con moderación) -->
<div class="will-change-auto will-change-scroll will-change-contents will-change-transform">
```

---

## 7. RESPONSIVE DESIGN

```html
<!-- Mobile-first (min-width) -->
<!-- Sin prefijo = todos los tamaños (móvil primero) -->
<!-- sm: = ≥640px | md: = ≥768px | lg: = ≥1024px | xl: = ≥1280px | 2xl: = ≥1536px -->

<!-- Ejemplo real completo -->
<div class="
  w-full           <!-- móvil: ancho completo -->
  md:w-1/2         <!-- tablet: mitad -->
  lg:w-1/3         <!-- laptop: un tercio -->
  xl:w-1/4         <!-- desktop: un cuarto -->
">

<!-- Grid responsive -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">

<!-- Ocultar/mostrar por tamaño -->
<div class="hidden md:block">   <!-- oculto en móvil, visible en tablet+ -->
<div class="block md:hidden">   <!-- visible en móvil, oculto en tablet+ -->
<div class="hidden lg:flex">    <!-- solo en laptop+ como flex -->

<!-- Tipografía responsive -->
<h1 class="text-3xl md:text-5xl lg:text-7xl font-bold">

<!-- Padding/margin responsive -->
<section class="px-4 md:px-8 lg:px-16 py-12 md:py-20 lg:py-32">

<!-- Max-width breakpoints -->
<div class="max-sm:text-sm">    <!-- solo en pantallas pequeñas -->
<div class="max-md:hidden">     <!-- oculto solo hasta md -->

<!-- Container queries (con plugin @tailwindcss/container-queries) -->
<div class="@container">
  <div class="@md:grid-cols-2 @lg:grid-cols-3">
  </div>
</div>
```

---

## 8. DARK MODE

```html
<!-- En tailwind.config.js: darkMode: 'class' -->
<!-- Se agrega la clase 'dark' al <html> con JS -->

<!-- Uso básico -->
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">

<!-- Implementación del toggle -->
<script>
  // Detectar preferencia del sistema
  if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
    document.documentElement.classList.add('dark');
  }

  // Toggle manual
  function toggleDark() {
    document.documentElement.classList.toggle('dark');
    localStorage.setItem('tema',
      document.documentElement.classList.contains('dark') ? 'oscuro' : 'claro'
    );
  }

  // Restaurar preferencia guardada
  if (localStorage.getItem('tema') === 'oscuro') {
    document.documentElement.classList.add('dark');
  }
</script>

<!-- Patrones de dark mode comunes -->
<div class="
  bg-white dark:bg-gray-950
  border border-gray-200 dark:border-gray-800
  text-gray-900 dark:text-gray-100
  shadow-sm dark:shadow-gray-900
">

<button class="
  bg-blue-600 hover:bg-blue-700
  dark:bg-blue-500 dark:hover:bg-blue-600
  text-white
">

<input class="
  bg-white dark:bg-gray-900
  border-gray-300 dark:border-gray-600
  text-gray-900 dark:text-gray-100
  placeholder:text-gray-400 dark:placeholder:text-gray-500
">

<!-- Imágenes en dark mode -->
<img class="dark:invert" src="logo-negro.svg" alt="Logo">
<img class="dark:opacity-80 dark:brightness-90" src="foto.jpg" alt="Foto">
```

---

## 9. ESTADOS Y VARIANTES

```html
<!-- Hover -->
<div class="hover:bg-blue-700 hover:scale-105 hover:shadow-lg hover:text-white">

<!-- Focus -->
<input class="focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:border-blue-500">

<!-- Focus Visible (solo teclado) -->
<button class="focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500">

<!-- Focus Within (cuando un hijo tiene foco) -->
<div class="focus-within:ring-2 focus-within:ring-blue-500">

<!-- Active -->
<button class="active:scale-95 active:brightness-90">

<!-- Visited -->
<a class="text-blue-600 visited:text-purple-600">

<!-- Disabled -->
<button class="disabled:opacity-50 disabled:cursor-not-allowed disabled:pointer-events-none" disabled>

<!-- Required / Optional -->
<input class="required:border-red-500 optional:border-gray-300">

<!-- Valid / Invalid -->
<input class="valid:border-green-500 invalid:border-red-500">

<!-- Placeholder -->
<input class="placeholder:text-gray-400 placeholder:italic">

<!-- Selection -->
<p class="selection:bg-blue-500 selection:text-white">

<!-- First / Last / Odd / Even -->
<li class="first:mt-0 last:mb-0">
<li class="odd:bg-gray-50 even:bg-white dark:odd:bg-gray-900 dark:even:bg-gray-950">

<!-- First-of-type / Last-of-type -->
<p class="first-of-type:font-bold last-of-type:mb-0">

<!-- Only / Only-of-type -->
<li class="only:text-center only-of-type:font-bold">

<!-- Empty -->
<div class="empty:hidden">

<!-- Nth-child (arbitrario) -->
<li class="[&:nth-child(3)]:text-blue-500">

<!-- Group hover (hover en padre afecta a hijos) -->
<div class="group hover:bg-blue-500">
  <p class="group-hover:text-white">Texto que cambia</p>
  <svg class="group-hover:fill-white group-hover:scale-110">
</div>

<!-- Group con nombre (múltiples grupos anidados) -->
<div class="group/tarjeta">
  <div class="group/imagen">
    <img class="group-hover/imagen:scale-110">
    <button class="group-hover/tarjeta:opacity-100 opacity-0">
  </div>
</div>

<!-- Peer (hermano previo afecta al siguiente) -->
<input type="checkbox" class="peer" id="toggle">
<label class="peer-checked:text-blue-500 peer-checked:font-bold" for="toggle">

<input type="text" class="peer">
<p class="hidden peer-focus:block peer-invalid:text-red-500">Mensaje de error</p>

<!-- Peer con nombre -->
<input class="peer/email" type="email">
<p class="hidden peer-invalid/email:block text-red-500">Email inválido</p>

<!-- Has (padre que tiene un hijo específico) -->
<div class="has-[:checked]:bg-blue-50 has-[:checked]:border-blue-300 border rounded-lg p-4">
  <input type="checkbox">
  <label>Opción</label>
</div>

<!-- Aria -->
<div class="aria-hidden:invisible">
<button class="aria-expanded:rotate-180">
<li class="aria-selected:bg-blue-500 aria-selected:text-white">
<div class="aria-[sort=ascending]:bg-blue-50">

<!-- Data attributes -->
<div class="data-[activo=true]:bg-blue-500 data-[activo=true]:text-white" data-activo="true">

<!-- RTL / LTR -->
<div class="rtl:text-right ltr:text-left">
<div class="rtl:space-x-reverse">

<!-- Print -->
<div class="print:hidden">         <!-- Oculto al imprimir -->
<div class="hidden print:block">   <!-- Solo visible al imprimir -->

<!-- Motion -->
<div class="motion-safe:animate-spin">   <!-- Solo si NO prefiere reducción -->
<div class="motion-reduce:animate-none"> <!-- Solo si prefiere reducción -->

<!-- Forced colors (Windows High Contrast) -->
<div class="forced-colors:border-2">
```

---

## 10. @MIDUDEV/TAILWIND-ANIMATIONS — GUÍA COMPLETA

### Instalación y setup
```bash
npm install @midudev/tailwind-animations
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@midudev/tailwind-animations'),
  ],
};
```

### Todas las animaciones disponibles
```html
<!-- === FADE === -->
<div class="animate-fade-in">          <!-- aparece con fade -->
<div class="animate-fade-out">         <!-- desaparece con fade -->
<div class="animate-fade-up">          <!-- aparece subiendo -->
<div class="animate-fade-down">        <!-- aparece bajando -->
<div class="animate-fade-left">        <!-- aparece desde la derecha (va a la izquierda) -->
<div class="animate-fade-right">       <!-- aparece desde la izquierda (va a la derecha) -->

<!-- === SLIDE === -->
<div class="animate-slide-in-top">     <!-- entra desde arriba -->
<div class="animate-slide-in-bottom">  <!-- entra desde abajo -->
<div class="animate-slide-in-left">    <!-- entra desde la izquierda -->
<div class="animate-slide-in-right">   <!-- entra desde la derecha -->
<div class="animate-slide-out-top">
<div class="animate-slide-out-bottom">
<div class="animate-slide-out-left">
<div class="animate-slide-out-right">

<!-- === ZOOM / SCALE === -->
<div class="animate-zoom-in">          <!-- aparece escalando desde pequeño -->
<div class="animate-zoom-out">         <!-- desaparece escalando a pequeño -->

<!-- === FLIP === -->
<div class="animate-flip-up">          <!-- voltea hacia arriba -->
<div class="animate-flip-down">        <!-- voltea hacia abajo -->

<!-- === BOUNCE === -->
<div class="animate-bounce-in">        <!-- entra con rebote -->
<div class="animate-bounce-out">       <!-- sale con rebote -->
<div class="animate-bounce-in-top">
<div class="animate-bounce-in-bottom">
<div class="animate-bounce-in-left">
<div class="animate-bounce-in-right">

<!-- === ROTATE === -->
<div class="animate-rotate-x">         <!-- rota en eje X -->
<div class="animate-rotate-y">         <!-- rota en eje Y -->

<!-- === SHAKE === -->
<div class="animate-shake-x">          <!-- vibra horizontalmente -->
<div class="animate-shake-y">          <!-- vibra verticalmente -->

<!-- === SKEW === -->
<div class="animate-skew-x">
<div class="animate-skew-y">

<!-- === BLUR === -->
<div class="animate-blur-in">          <!-- aparece desde blur -->
<div class="animate-blur-out">         <!-- desaparece con blur -->

<!-- === ROLL === -->
<div class="animate-roll-in-left">
<div class="animate-roll-in-right">

<!-- === JUMP === -->
<div class="animate-jump">             <!-- salta -->
<div class="animate-jump-in">
<div class="animate-jump-out">

<!-- === WIGGLE === -->
<div class="animate-wiggle">           <!-- mueve de lado a lado -->
<div class="animate-wiggle-more">      <!-- más pronunciado -->

<!-- === SWING === -->
<div class="animate-swing">            <!-- balanceo como péndulo -->

<!-- === HEARTBEAT === -->
<div class="animate-heartbeat">        <!-- pulso de corazón -->

<!-- === BLINK === -->
<div class="animate-blink">            <!-- parpadeo -->
```

### Controlar duración, delay e iteraciones con Tailwind
```html
<!-- Duración -->
<div class="animate-fade-in duration-300">    <!-- rápido -->
<div class="animate-fade-in duration-500">    <!-- normal -->
<div class="animate-fade-in duration-700">    <!-- lento -->
<div class="animate-fade-in duration-1000">   <!-- muy lento -->
<div class="animate-fade-in [animation-duration:2000ms]"> <!-- arbitrario -->

<!-- Delay -->
<div class="animate-fade-in delay-100">
<div class="animate-fade-in delay-300">
<div class="animate-fade-in delay-500">
<div class="animate-fade-in [animation-delay:1500ms]">  <!-- arbitrario -->

<!-- Iteraciones -->
<div class="animate-heartbeat [animation-iteration-count:infinite]">
<div class="animate-wiggle [animation-iteration-count:3]">

<!-- Fill mode -->
<div class="animate-fade-in [animation-fill-mode:both]">
<div class="animate-fade-in [animation-fill-mode:forwards]">

<!-- Timing function -->
<div class="animate-bounce-in [animation-timing-function:ease-out]">
<div class="animate-slide-in-left ease-in-out">

<!-- Play state (pausar animación) -->
<div class="animate-heartbeat [animation-play-state:paused] hover:[animation-play-state:running]">
```

### Patrones con Midudev animations
```html
<!-- Stagger con CSS (sin JS) -->
<ul>
  <li class="animate-fade-up delay-0">Item 1</li>
  <li class="animate-fade-up delay-100">Item 2</li>
  <li class="animate-fade-up delay-200">Item 3</li>
  <li class="animate-fade-up delay-300">Item 4</li>
</ul>

<!-- Animación al hacer hover -->
<button class="hover:animate-wiggle">¡Haz clic!</button>
<div class="hover:animate-heartbeat">❤️</div>
<div class="group">
  <img class="group-hover:animate-zoom-in" src="foto.jpg">
</div>

<!-- Animación de entrada en cards -->
<div class="grid grid-cols-3 gap-6">
  <div class="card animate-fade-up delay-0">Card 1</div>
  <div class="card animate-fade-up delay-150">Card 2</div>
  <div class="card animate-fade-up delay-300">Card 3</div>
</div>

<!-- Notificación que aparece y desaparece -->
<div class="animate-slide-in-right">
  <div class="bg-green-500 text-white px-4 py-3 rounded-lg">
    ✅ Guardado correctamente
  </div>
</div>

<!-- Loading skeleton con shimmer -->
<div class="animate-pulse bg-gray-200 rounded-lg h-4 w-3/4"></div>

<!-- Icono que llama la atención -->
<button class="animate-bounce-in hover:animate-wiggle">
  🔔 Notificaciones
</button>

<!-- Texto que aparece letra por letra (con JS + Tailwind) -->
<script>
const texto = 'Bienvenido';
const contenedor = document.querySelector('.typewriter');
texto.split('').forEach((letra, i) => {
  const span = document.createElement('span');
  span.textContent = letra;
  span.className = `inline-block animate-fade-in [animation-fill-mode:both]`;
  span.style.animationDelay = `${i * 60}ms`;
  contenedor.appendChild(span);
});
</script>
```

---

## 11. COMPONENTES PROFESIONALES COMPLETOS

### Hero Section moderno
```html
<section class="relative min-h-screen flex items-center justify-center overflow-hidden bg-gray-950">
  <!-- Fondo con gradiente mesh -->
  <div class="absolute inset-0 bg-gradient-to-br from-blue-950 via-gray-950 to-purple-950"></div>

  <!-- Círculos decorativos -->
  <div class="absolute top-1/4 left-1/4 w-96 h-96 bg-blue-500/10 rounded-full blur-3xl animate-pulse"></div>
  <div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-purple-500/10 rounded-full blur-3xl animate-pulse delay-1000"></div>

  <!-- Contenido -->
  <div class="relative z-10 text-center px-4 max-w-5xl mx-auto">
    <!-- Badge -->
    <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-blue-500/10 border border-blue-500/20 text-blue-400 text-sm font-medium mb-8 animate-fade-down">
      <span class="w-2 h-2 rounded-full bg-blue-400 animate-pulse"></span>
      Disponible para proyectos
    </div>

    <!-- Título principal -->
    <h1 class="text-5xl md:text-7xl lg:text-8xl font-black text-white leading-none tracking-tight mb-6 animate-fade-up delay-100">
      Creamos
      <span class="bg-gradient-to-r from-blue-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">
        experiencias
      </span>
      digitales
    </h1>

    <!-- Subtítulo -->
    <p class="text-xl md:text-2xl text-gray-400 max-w-2xl mx-auto mb-10 leading-relaxed animate-fade-up delay-200">
      Diseño y desarrollo web de alto impacto. Desde landing pages hasta aplicaciones complejas.
    </p>

    <!-- CTAs -->
    <div class="flex flex-col sm:flex-row items-center justify-center gap-4 animate-fade-up delay-300">
      <a href="#contacto" class="
        inline-flex items-center gap-2 px-8 py-4 rounded-2xl
        bg-blue-600 hover:bg-blue-500
        text-white font-semibold text-lg
        transition-all duration-200 hover:scale-105 active:scale-95
        shadow-lg shadow-blue-500/25 hover:shadow-blue-500/40
      ">
        Empezar proyecto
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"/>
        </svg>
      </a>
      <a href="#trabajo" class="
        inline-flex items-center gap-2 px-8 py-4 rounded-2xl
        bg-white/5 hover:bg-white/10 border border-white/10 hover:border-white/20
        text-white font-semibold text-lg
        transition-all duration-200 hover:scale-105 active:scale-95
        backdrop-blur-sm
      ">
        Ver trabajo
      </a>
    </div>
  </div>

  <!-- Scroll indicator -->
  <div class="absolute bottom-8 left-1/2 -translate-x-1/2 animate-bounce">
    <div class="w-6 h-10 rounded-full border-2 border-white/20 flex justify-center pt-2">
      <div class="w-1.5 h-3 rounded-full bg-white/40 animate-bounce"></div>
    </div>
  </div>
</section>
```

### Navbar completo con glassmorphism
```html
<header class="
  fixed top-0 inset-x-0 z-50
  bg-white/80 dark:bg-gray-950/80
  backdrop-blur-xl
  border-b border-gray-200/50 dark:border-gray-800/50
  transition-all duration-300
" id="navbar">
  <nav class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
    <!-- Logo -->
    <a href="/" class="flex items-center gap-2 font-bold text-xl text-gray-900 dark:text-white hover:opacity-80 transition-opacity">
      <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center text-white text-sm font-black">
        M
      </div>
      MiMarca
    </a>

    <!-- Links desktop -->
    <ul class="hidden md:flex items-center gap-1">
      <li><a href="#inicio" class="px-4 py-2 rounded-xl text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-all duration-150">Inicio</a></li>
      <li><a href="#servicios" class="px-4 py-2 rounded-xl text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-all duration-150">Servicios</a></li>
      <li><a href="#trabajo" class="px-4 py-2 rounded-xl text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-all duration-150">Trabajo</a></li>
      <li><a href="#blog" class="px-4 py-2 rounded-xl text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-all duration-150">Blog</a></li>
    </ul>

    <!-- Acciones -->
    <div class="flex items-center gap-3">
      <!-- Toggle dark mode -->
      <button onclick="toggleDark()" class="w-10 h-10 rounded-xl flex items-center justify-center text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors" aria-label="Cambiar tema">
        <svg class="w-5 h-5 dark:hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"/></svg>
        <svg class="w-5 h-5 hidden dark:block" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"/></svg>
      </button>

      <a href="#contacto" class="hidden md:inline-flex items-center gap-2 px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-sm transition-all duration-150 hover:scale-105 active:scale-95 shadow-sm">
        Contactar
      </a>

      <!-- Hamburguesa móvil -->
      <button class="md:hidden w-10 h-10 rounded-xl flex items-center justify-center hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors" aria-label="Menú" aria-expanded="false" id="menu-btn">
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
        </svg>
      </button>
    </div>
  </nav>

  <!-- Menú móvil -->
  <div class="md:hidden hidden border-t border-gray-200 dark:border-gray-800" id="menu-movil">
    <ul class="px-4 py-4 flex flex-col gap-1">
      <li><a href="#inicio" class="block px-4 py-3 rounded-xl text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-colors">Inicio</a></li>
      <li><a href="#servicios" class="block px-4 py-3 rounded-xl text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-colors">Servicios</a></li>
      <li><a href="#trabajo" class="block px-4 py-3 rounded-xl text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 font-medium transition-colors">Trabajo</a></li>
    </ul>
  </div>
</header>
```

### Cards modernas
```html
<!-- Card básica con hover -->
<div class="group bg-white dark:bg-gray-900 rounded-2xl border border-gray-100 dark:border-gray-800 p-6 hover:border-blue-200 dark:hover:border-blue-800 hover:shadow-xl hover:shadow-blue-500/5 transition-all duration-300 cursor-pointer">
  <div class="w-12 h-12 rounded-xl bg-blue-50 dark:bg-blue-950 flex items-center justify-center mb-4 group-hover:scale-110 group-hover:bg-blue-100 dark:group-hover:bg-blue-900 transition-all duration-300">
    <svg class="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
    </svg>
  </div>
  <h3 class="font-bold text-gray-900 dark:text-white text-lg mb-2 group-hover:text-blue-600 dark:group-hover:text-blue-400 transition-colors">
    Rendimiento
  </h3>
  <p class="text-gray-500 dark:text-gray-400 text-sm leading-relaxed">
    Optimizado para 100 en Lighthouse. Carga rápida en cualquier dispositivo.
  </p>
</div>

<!-- Card de producto con imagen -->
<div class="group bg-white dark:bg-gray-900 rounded-3xl overflow-hidden border border-gray-100 dark:border-gray-800 hover:shadow-2xl transition-all duration-500 hover:-translate-y-1">
  <!-- Imagen -->
  <div class="relative aspect-video overflow-hidden bg-gray-100 dark:bg-gray-800">
    <img
      src="producto.jpg"
      alt="Producto"
      class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-700"
    >
    <!-- Badge -->
    <div class="absolute top-3 left-3">
      <span class="px-3 py-1 rounded-full bg-green-500 text-white text-xs font-bold">NUEVO</span>
    </div>
    <!-- Overlay con botones -->
    <div class="absolute inset-0 bg-black/40 opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-center justify-center gap-3">
      <button class="px-4 py-2 rounded-xl bg-white text-gray-900 font-semibold text-sm hover:bg-gray-100 transition-colors animate-fade-up">
        Ver más
      </button>
      <button class="w-10 h-10 rounded-xl bg-white/20 backdrop-blur-sm border border-white/30 text-white flex items-center justify-center hover:bg-white/30 transition-colors animate-fade-up delay-75">
        ♡
      </button>
    </div>
  </div>
  <!-- Contenido -->
  <div class="p-5">
    <div class="flex items-start justify-between gap-4 mb-2">
      <h3 class="font-bold text-gray-900 dark:text-white">Nombre del producto</h3>
      <span class="text-lg font-black text-blue-600">$99</span>
    </div>
    <p class="text-gray-500 dark:text-gray-400 text-sm mb-4">Descripción breve del producto.</p>
    <button class="w-full py-3 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-sm transition-all duration-150 active:scale-95">
      Agregar al carrito
    </button>
  </div>
</div>
```

### Formulario completo
```html
<form class="bg-white dark:bg-gray-900 rounded-3xl border border-gray-100 dark:border-gray-800 p-8 shadow-xl max-w-lg mx-auto">
  <h2 class="text-2xl font-bold text-gray-900 dark:text-white mb-2">Contáctanos</h2>
  <p class="text-gray-500 dark:text-gray-400 mb-8">Te respondemos en menos de 24 horas.</p>

  <div class="space-y-5">
    <!-- Nombre -->
    <div>
      <label for="nombre" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
        Nombre completo <span class="text-red-500">*</span>
      </label>
      <input
        type="text" id="nombre" name="nombre" required
        placeholder="Juan García"
        class="w-full px-4 py-3 rounded-xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder:text-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all duration-200 invalid:border-red-300 invalid:focus:ring-red-500"
      >
    </div>

    <!-- Email -->
    <div>
      <label for="email" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
        Correo electrónico <span class="text-red-500">*</span>
      </label>
      <div class="relative">
        <input
          type="email" id="email" name="email" required
          placeholder="juan@ejemplo.com"
          class="w-full pl-11 pr-4 py-3 rounded-xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder:text-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all duration-200"
        >
        <svg class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
        </svg>
      </div>
    </div>

    <!-- Select -->
    <div>
      <label for="servicio" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Servicio</label>
      <select id="servicio" name="servicio" class="w-full px-4 py-3 rounded-xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all duration-200 cursor-pointer appearance-none bg-[url('data:image/svg+xml;charset=utf-8,%3Csvg xmlns=%22http://www.w3.org/2000/svg%22 fill=%22none%22 viewBox=%220 0 20 20%22%3E%3Cpath stroke=%22%236B7280%22 stroke-linecap=%22round%22 stroke-linejoin=%22round%22 stroke-width=%221.5%22 d=%22M6 8l4 4 4-4%22/%3E%3C/svg%3E')] bg-[length:20px] bg-[right_12px_center] bg-no-repeat pr-10">
        <option value="">Selecciona un servicio...</option>
        <option value="web">Desarrollo Web</option>
        <option value="app">Aplicación Móvil</option>
        <option value="diseno">Diseño UI/UX</option>
      </select>
    </div>

    <!-- Textarea -->
    <div>
      <label for="mensaje" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
        Mensaje <span class="text-red-500">*</span>
      </label>
      <textarea
        id="mensaje" name="mensaje" required rows="4"
        placeholder="Cuéntanos sobre tu proyecto..."
        class="w-full px-4 py-3 rounded-xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder:text-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all duration-200 resize-none"
      ></textarea>
    </div>

    <!-- Checkbox -->
    <label class="flex items-start gap-3 cursor-pointer group">
      <input type="checkbox" required class="mt-0.5 w-5 h-5 rounded border-gray-300 dark:border-gray-600 text-blue-600 focus:ring-blue-500 focus:ring-offset-0 cursor-pointer flex-shrink-0">
      <span class="text-sm text-gray-600 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-gray-200 transition-colors">
        Acepto la <a href="/privacidad" class="text-blue-600 hover:underline">política de privacidad</a>
      </span>
    </label>

    <!-- Submit -->
    <button type="submit" class="w-full flex items-center justify-center gap-2 py-4 px-6 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-base transition-all duration-200 hover:shadow-lg hover:shadow-blue-500/30 active:scale-[0.98] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed">
      <span>Enviar mensaje</span>
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"/>
      </svg>
    </button>
  </div>
</form>
```

### Toast / Notificación
```html
<!-- Contenedor de toasts -->
<div id="toasts" class="fixed bottom-4 right-4 z-50 flex flex-col gap-2 pointer-events-none">
  <!-- Toast de éxito -->
  <div class="flex items-center gap-3 px-4 py-3 rounded-xl bg-green-50 dark:bg-green-950 border border-green-200 dark:border-green-800 shadow-lg max-w-sm pointer-events-auto animate-slide-in-right">
    <div class="w-8 h-8 rounded-full bg-green-100 dark:bg-green-900 flex items-center justify-center flex-shrink-0">
      <svg class="w-4 h-4 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
      </svg>
    </div>
    <div class="flex-1 min-w-0">
      <p class="text-sm font-semibold text-green-900 dark:text-green-100">¡Guardado!</p>
      <p class="text-xs text-green-700 dark:text-green-300">Los cambios se guardaron correctamente.</p>
    </div>
    <button class="text-green-500 hover:text-green-700 p-1 rounded-lg hover:bg-green-100 dark:hover:bg-green-900 transition-colors">
      <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
      </svg>
    </button>
  </div>
</div>
```

### Badge / Chip
```html
<!-- Badges -->
<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-blue-100 text-blue-700 dark:bg-blue-950 dark:text-blue-300">
  Nuevo
</span>
<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-green-100 text-green-700 dark:bg-green-950 dark:text-green-300">
  <span class="w-1.5 h-1.5 rounded-full bg-green-500 animate-pulse"></span>
  Activo
</span>
<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-red-100 text-red-700">
  Error
</span>
<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-yellow-100 text-yellow-700">
  Pendiente
</span>
<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-gray-100 text-gray-600 dark:bg-gray-800 dark:text-gray-400">
  Archivado
</span>
```

---

## 12. VALORES ARBITRARIOS — ESCAPE HATCH

```html
<!-- Cuando Tailwind no tiene la clase exacta, usamos [] -->
<div class="w-[537px]">               <!-- width: 537px -->
<div class="mt-[117px]">              <!-- margin-top: 117px -->
<div class="bg-[#1da1f2]">            <!-- color hex exacto -->
<div class="bg-[rgb(29,161,242)]">    <!-- rgb exacto -->
<div class="text-[22px]">             <!-- font-size exacto -->
<div class="grid-cols-[1fr_auto_2fr]">  <!-- columnas complejas (espacio = _) -->
<div class="content-['Hola']">        <!-- content CSS -->
<div class="h-[calc(100vh-4rem)]">    <!-- calc() -->
<div class="top-[var(--header-height)]"> <!-- CSS variable -->
<div class="bg-[url('/imagen.png')]"> <!-- URL en background -->
<div class="[mask-type:luminance]">   <!-- propiedad CSS arbitraria -->
<div class="[--mi-variable:red]">     <!-- definir CSS variable -->

<!-- Modificadores arbitrarios -->
<div class="[@media(min-width:900px)]:text-lg">       <!-- media query personalizada -->
<div class="[@supports(display:grid)]:grid">           <!-- @supports -->
<div class="[&>*]:text-blue-500">                     <!-- selector complejo -->
<div class="[&_p]:text-gray-600">                     <!-- descendiente -->
<div class="[&:nth-child(3)]:font-bold">              <!-- nth-child -->
<div class="group-[.sidebar-abierta]:w-full">         <!-- group con clase -->
```

---

## 13. TAILWIND v4 — LO NUEVO

```css
/* Tailwind v4 usa CSS nativo — NO tailwind.config.js por defecto */
/* En tu archivo CSS principal: */

@import "tailwindcss";

/* Definir tokens directamente en CSS */
@theme {
  --color-primario: oklch(0.6 0.2 240);
  --color-acento: oklch(0.7 0.15 180);
  --font-display: "Cal Sans", sans-serif;
  --breakpoint-xs: 475px;
  --spacing-18: 4.5rem;
  --radius-4xl: 2rem;
}

/* Variante personalizada */
@variant dark (&:where(.dark, .dark *));
@variant hocus (&:hover, &:focus);

/* Utilidad personalizada */
@utility scrollbar-hidden {
  scrollbar-width: none;
  &::-webkit-scrollbar { display: none; }
}

/* Importar plugins */
@plugin "@tailwindcss/typography";
@plugin "@midudev/tailwind-animations";
```

```html
<!-- En v4, las clases son las mismas pero con más poderes -->
<div class="bg-primario text-white">    <!-- usa el token definido -->
<div class="hocus:underline">           <!-- variante personalizada -->
<div class="scrollbar-hidden">          <!-- utilidad personalizada -->
<div class="xs:text-sm">               <!-- breakpoint personalizado -->
<div class="spacing-18">               <!-- spacing personalizado -->
```

---

## 14. CHECKLIST DE CALIDAD — TAILWIND

```
SETUP
□ content[] configurado correctamente (evitar purge incorrecto)
□ darkMode: 'class' con toggle funcional
□ Colores de marca definidos en theme.extend.colors
□ Fuentes personalizadas en theme.extend.fontFamily
□ @midudev/tailwind-animations instalado y registrado

RESPONSIVE
□ Mobile-first siempre (sin prefijo = móvil)
□ Clases en orden: base → sm: → md: → lg: → xl:
□ Probado en 320px, 768px, 1024px, 1440px
□ Texto fluido con text-balance en headings

ACCESIBILIDAD
□ focus-visible en todos los elementos interactivos
□ Contraste suficiente (text-gray-900 sobre white mínimo)
□ Nunca solo outline-none sin ring alternativo
□ aria-label en iconos sin texto
□ motion-reduce en animaciones

DARK MODE
□ Todas las clases de color tienen variante dark:
□ bg-white dark:bg-gray-950 (no gray-900 — muy claro)
□ text-gray-900 dark:text-gray-100
□ border-gray-200 dark:border-gray-800
□ Imágenes con dark:opacity-80 si es necesario

RENDIMIENTO
□ No usar @apply innecesariamente (pierde tree-shaking)
□ Purge funcionando (bundle < 10KB en producción típicamente)
□ will-change-transform solo donde sea necesario
□ Animaciones con motion-safe: para respetar preferencias
□ Imágenes con aspect-ratio para evitar CLS

ANIMACIONES (Midudev)
□ Usar animation-fill-mode: both en entradas
□ Stagger con delay-0, delay-100, delay-200...
□ Combinar con motion-safe: para accesibilidad
□ No abusar — máximo 2-3 animaciones simultáneas visibles
```

---

## 15. CUÁNDO USAR TAILWIND VS CSS PURO

```
USAR TAILWIND CUANDO:
✅ Proyectos con componentes reutilizables
✅ Equipos con múltiples desarrolladores
✅ Velocidad de desarrollo es prioridad
✅ Design system basado en tokens
✅ Next.js, Nuxt, Astro, SvelteKit
✅ Prototipos rápidos

USAR CSS PURO CUANDO:
✅ Animaciones muy complejas (usar CSS_EXPERTO.md + ANIMACIONES_AVANZADAS_EXPERTO.md)
✅ Lógica de estilo muy dinámica (variables calculadas en JS)
✅ Estilos que Tailwind no puede expresar limpiamente
✅ Sitios muy pequeños sin bundler

COMBINAR AMBOS (lo más común):
✅ Tailwind para layout, tipografía y colores
✅ CSS puro / GSAP para animaciones complejas
✅ Three.js para efectos 3D
✅ @midudev/tailwind-animations para entradas simples
```

---

*Fin del archivo TAILWIND_EXPERTO.md — Conocimiento completo de Tailwind CSS v3/v4, @midudev/tailwind-animations, componentes profesionales, dark mode, responsive design y patrones de integración modernos.*
