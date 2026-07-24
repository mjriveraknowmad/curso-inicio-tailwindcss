# Directivas en Tailwind CSS v3 y diferencias con v4

## ¿Qué son las directivas en Tailwind CSS v3?

Son instrucciones que permiten a Tailwind CSS agregar estilos definidos a nuestros archivos CSS. Se componen de tres capas:

| Directiva | Descripción |
|-----------|-------------|
| `@layer base` | Estilos base para elementos HTML (`p`, `a`, `div`, etc.) |
| `@layer components` | Clases personalizadas para componentes (botones, contenedores, etc.) |
| `@layer utilities` | Utilidades funcionales (márgenes, padding, text-align, etc.) |

Puedes visualizarlo como carpetas: `base` para estilos de elementos, `components` para componentes reutilizables, y `utilities` para utilidades de Tailwind.

---

## El archivo `tailwind.config.js` en v3

Permite configurar y personalizar Tailwind. Sus secciones principales son:

- **`content`**: Rutas de los archivos HTML y JS que Tailwind escanea para generar los estilos.
- **`theme`**: Personalización del tema (tamaños, espaciado, colores, breakpoints).
- **`plugins`**: Extensiones en JavaScript para generar estilos CSS personalizados.

---

## Cambios en v4: enfoque "CSS-first"

### `@layer components` vs `@utility`

`@layer components` sigue funcionando en v4, pero existe una distinción conceptual importante:

| | v3 / v4 | Precedencia |
|---|---|---|
| `@layer components` | Estilos de componentes | Menor — pueden ser sobreescritos por utilidades |
| `@utility` | Utilidades personalizadas | Igual que las utilidades de Tailwind |

```css
/* Componente (agrupa estilos de UI) */
@layer components {
  .nav-dropdown-item {
    @apply block px-4 py-2 hover:bg-slate-700 hover:text-blue-300 transition-colors;
  }
}

/* Utilidad personalizada (v4) */
@utility content-auto {
  content-visibility: auto;
}
```

---

## Configuración: `tailwind.config.js` → `@theme`

En v4 **ya no se usa `tailwind.config.js`**. Toda la configuración vive en el CSS mediante `@theme`.

```js
// v3 — tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: { brand: '#3b82f6' },
      fontFamily: { sans: ['Inter', 'sans-serif'] },
    },
  },
}
```

```css
/* v4 — CSS directo */
@import "tailwindcss";

@theme {
  --color-brand: #3b82f6;
  --font-sans: Inter, sans-serif;
}
```

### Cambios clave en v4

- **Sin `tailwind.config.js`**: colores, fuentes, espaciados y breakpoints se definen en `@theme`.
- **Sin `content`**: el nuevo motor (Rust/Oxide) escanea el proyecto automáticamente.
- **Variables CSS nativas**: `@theme` genera variables CSS reutilizables (`var(--color-brand)`).

> 📖 Más información: [Adding Custom Styles](https://tailwindcss.com/docs/adding-custom-styles) · [Theme](https://tailwindcss.com/docs/theme)

---

## PostCSS y Autoprefixer

PostCSS es un procesador que permite trabajar con CSS y Tailwind usando JavaScript para generar CSS. En v3 era habitual usar PostCSS + autoprefixer juntos.

**En v4 ya no necesitas autoprefixer.** Tailwind usa **Lightning CSS** como motor interno, que incluye:

- Autoprefixing automático (`-webkit-`, `-moz-`, etc.)
- Transpilación de sintaxis CSS moderna
- Optimización y minificación

```js
// v3 — postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},  // ← ya no necesario en v4
  }
}
```

> 📖 Más información: [Using PostCSS](https://tailwindcss.com/docs/installation/using-postcss)

---

## Plugins: de JavaScript a CSS nativo

En v3 necesitabas crear plugins en JavaScript para agregar utilidades, componentes, variantes y estilos base personalizados. En v4 todo se declara directamente en CSS.

### 1. Utilidades con `@utility`

Sustituye a `addUtilities()` / `matchUtilities()` de v3.

**Ejemplo en v3 (JavaScript):**

```js
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    plugin(function({ addUtilities, addComponents, addBase, theme }) {
      
      // Estilos de utilidades
      const newUtilities = {
        '.rotate-90': {
          transform: 'rotate(90)'
        }
      }

      // Estilos para componentes
      const button = {
        '.button': {
          background: theme('colors.blue.500'),
          fontWeight: '600',
          '&:hover': {
            background: '#000'
          }
        }
      }

      // Estilos base
      addBase({
        'h1': { fontSize: theme('spacing.2') },
      })

      addUtilities(newUtilities)
      addComponents(button)
    }),
  ]
}
```

**Ejemplo en v4 (CSS nativo):**

```css
/* Utilidad estática */
@utility content-auto {
  content-visibility: auto;
}

/* Utilidad dinámica (text-stroke-2, text-stroke-red, etc.) */
@utility text-stroke-* {
  -webkit-text-stroke-width: --value(integer)px;
  -webkit-text-stroke-color: --value([*]);
}
```

Las variantes de estado (`hover:`, `md:`) se generan automáticamente.

### 2. Variantes personalizadas con `@custom-variant`

Sustituye a `addVariant()` de v3.

```css
/* v4 */
@custom-variant theme-midnight (&:where([data-theme="midnight"] *));
@custom-variant dark (&:where(.dark, .dark *));
```

### 3. Importar plugins con `@plugin`

Sustituye a la sección `plugins: [...]` de `tailwind.config.js`.

```css
/* v4 */
@import "tailwindcss";
@plugin "@tailwindcss/typography";
@plugin "@tailwindcss/forms";
```

### Tabla resumen: v3 JavaScript → v4 CSS

| v3 (JavaScript) | v4 (CSS nativo) |
|---|---|
| `addUtilities()` / `matchUtilities()` | `@utility` |
| `addVariant()` | `@custom-variant` |
| `addComponents()` | `@layer components { }` |
| `addBase()` | `@layer base { }` |
| `plugins: [require('@tailwindcss/forms')]` | `@plugin "@tailwindcss/forms"` |
| `tailwind.config.js` → `theme.extend` | `@theme { }` |
| `content: [...]` | Detección automática |

### Plugins oficiales

- [@tailwindcss/typography](https://github.com/tailwindlabs/tailwindcss-typography)
- [@tailwindcss/forms](https://github.com/tailwindlabs/tailwindcss-forms) — formatea estilos de formularios
- [@tailwindcss/aspect-ratio](https://github.com/tailwindlabs/tailwindcss-aspect-ratio)

> 📖 Más información: [Custom Styles v4](https://tailwindcss.com/docs/adding-custom-styles#functional-utilities) · [Plugins v3](https://v3.tailwindcss.com/docs/plugins)
