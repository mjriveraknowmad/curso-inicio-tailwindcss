# Curso Tailwind CSS - Inicio

Proyecto educativo siguiendo el [Curso Definitivo de Tailwind CSS](https://www.udemy.com/course/tailwind-css-el-curso-definitivo) en Udemy.

## Descripción

Este proyecto es un ejemplo introductorio para aprender los fundamentos de **Tailwind CSS** utilizando la instalación oficial con **Tailwind CLI**.

> **Nota**: El curso en Udemy utiliza **Tailwind CSS v3**, sin embargo, este proyecto está configurado con **Tailwind CSS v4**.

## Instalación

La forma más simple y rápida para comenzar con Tailwind CSS desde cero es utilizar la herramienta **Tailwind CLI**. También está disponible como [ejecutable independiente](https://github.com/tailwindlabs/tailwindcss/releases/latest) si deseas utilizarlo sin instalar Node.js.

### Paso 1: Instalar Tailwind CSS

Instala `tailwindcss` y `@tailwindcss/cli` mediante npm:

```bash
npm install tailwindcss @tailwindcss/cli
```

### Paso 2: Importar Tailwind en tu CSS

Agrega la importación `@import "tailwindcss";` a tu archivo CSS principal:

**src/input.css**
```css
@import "tailwindcss";
```

### Paso 3: Iniciar el proceso de compilación

Ejecuta la herramienta CLI para escanear tus archivos fuente en busca de clases y compilar tu CSS:

```bash
npx @tailwindcss/cli -i ./src/input.css -o ./src/output.css --watch
```

La bandera `--watch` mantiene el proceso en ejecución, recompilando automáticamente cuando realices cambios.

O ejecuta el script de este proyecto
```bash
npm run tailwind:build
```

### Paso 4: Usar Tailwind en tu HTML

Agrega el archivo CSS compilado a la etiqueta `<head>` y comienza a usar las clases de utilidad de Tailwind:

**src/index.html**
```html
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="./output.css" rel="stylesheet">
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    ¡Hola mundo!
  </h1>
</body>
</html>
```

## Estructura del Proyecto

```
├── src/
│   ├── index.html      # Archivo HTML principal
│   ├── input.css       # Archivo CSS de entrada (con importación de Tailwind)
│   └── output.css      # Archivo CSS compilado (generado automáticamente)
├── package.json        # Dependencias del proyecto
└── README.md          # Este archivo
```

## Uso

1. **Iniciar el compilador**: Ejecuta el comando del Paso 3 para que Tailwind compile tu CSS en tiempo real
2. **Desarrollar**: Abre `src/index.html` en tu navegador y comienza a agregar clases de Tailwind
3. **Ver cambios**: Los estilos se actualizarán automáticamente gracias al modo `--watch`

## Documentación

Los pasos seguidos, los puedes encontrar en [tailwind cli](https://tailwindcss.com/docs/installation/tailwind-cli). 
Para más información sobre Tailwind CSS, consulta la [documentación oficial](https://tailwindcss.com/docs)

## Curso

Este proyecto es parte del aprendizaje del curso "Tailwind CSS: El Curso Definitivo" en Udemy, donde aprenderemos:
- Conceptos fundamentales de Tailwind CSS
- Instalación y configuración
- Clases de utilidad
- Personalización y configuración avanzada
- Mejores prácticas

---

**Creado con ❤️ para aprender Tailwind CSS**
