# Fundamentos de la Web (HTML5, CSS3 y Formularios)

- **Duración Total**: 120 Minutos
- **Público**: Nuevos programadores sin experiencia previa en desarrollo web.
- **Objetivo**: Comprender la estructura (HTML), la presentación (CSS) y la captura de datos (Formularios). Al finalizar, los estudiantes podrán construir y estilizar una página web estática con un formulario de contacto.

## [Módulo 1](1-fundamentos.md): Conceptos Fundamentales de la Web

- **Actividad**: Discusión / Presentación.
- **Puntos Clave**:
  - **La Analogía de la Casa**:
    - **HTML**: Estructura (paredes, habitaciones).
    - **CSS**: Presentación (pintura, muebles).
    - **JavaScript**: Funcionalidad (electricidad, interruptores).
- **Herramientas**: Editor de código y Herramientas de Desarrollador del navegador (F12).

## [Módulo 2](2-html.md): HTML5 - El Esqueleto

- **Objetivo**: Entender la sintaxis de HTML y su estructura semántica.
- **Material Teórico**:
  - Qué es un elemento (tag), atributo y anidamiento.
  - Estructura básica (`<!DOCTYPE>`, `<html>`, `<head>`, `<body>`).
  - **HTML Semántico**: `<header>`, `<main>`, `<footer>`, `<section>`, `<h1>`, `<p>`, `<ul>`, `<li>`, `<a>`, `<img>`.
- **Práctica 1: _Mi Página de Perfil_**:
  - Crear `index.html`.
  - Escribir la estructura básica.
  - Construir el layout semántico: `<header>` (con `<h1>`), `<main>` (con `<img>`, `<p>`, `<ul>`) y `<footer>` (con `<a>`).

## [Módulo 3](3-css.md): CSS3 - Dando Estilo

- **Objetivo**: Entender cómo CSS se conecta a HTML, los selectores básicos y el modelo de caja.
- **Material Teórico**:
  - Sintaxis CSS (`selector { propiedad: valor; }`).
  - Formas de vincular CSS (externo con `<link>` es la mejor práctica).
  - **Selectores**: Por Etiqueta, por **Clase** (el más importante) y por ID.
  - **El Modelo de Caja**: Explicar **Margin**, **Border**, **Padding** y **Content**.
- **Práctica 2: Estilizando _Mi Página de Perfil_**:
  - Crear `style.css` y vincularlo.
  - Estilizar `body` (fuente, fondo).
  - Usar una **clase** `.container` para centrar el `<main>` (con `width` y `margin: auto`) y darle `padding`.
  - Usar una clase `.profile-pic` para hacer la imagen redonda (`border-radius: 50%`).
  - Quitar el subrayado del enlace del footer.

## [Módulo 4](4-formulario.md): Introducción a Formularios HTML

- **Objetivo**: Aprender a crear formularios básicos para la captura de datos del usuario.
- **Material Teórico**:
  - La etiqueta contenedora `<form>`. Explicar brevemente los atributos `action` (a dónde van los datos) y `method` (cómo se envían - GET/POST).
  - La etiqueta `<input>`: La más importante.
  - El atributo `type`: Mostrar `text`, `email`, `password`.
  - La etiqueta `<label>`: Fundamental para la accesibilidad. Explicar cómo el atributo `for` se conecta con el `id` del `input`.
  - Otras etiquetas útiles: `<textarea>` (para mensajes largos) y `<button type="submit">` (para enviar el formulario).
- **Práctica 3: _Formulario de Contacto_**:
  - Dentro del `<main>` de `index.html`, añadir una nueva `<section>`.
  - Añadir un `<h2>Contáctame</h2>`.
  - Crear un `<form>` básico dentro de la sección.
  - Añadir los siguientes campos, cada uno con su `<label>` e `id` correspondiente:
    - Campo para "Nombre" (`<input type="text">`).
    - Campo para "Email" (`<input type="email">`).
    - Campo para "Mensaje" (`<textarea>`).
    - Un botón `<button type="submit">Enviar</button>`.
  - **Nota**: Explicar a los estudiantes que al presionar "Enviar" la página se recargará. Esto es el comportamiento por defecto del navegador. La _magia_ de procesar esos datos sin recargar la página (o enviarlos a un servidor) es el trabajo de JavaScript.

## [Módulo 5](5-conclusiones.md): Conclusión y Siguientes Pasos

- **Actividad**: Discusión y cierre.
- **Puntos Clave**:
  - **Revisión**: Hoy creamos un sitio estático completo.
  - **Los Tres Pilares**:
    - **Estructura (HTML)**: Define el contenido y ahora también cómo recibir contenido (formularios).
    - **Presentación (CSS)**: Define la apariencia.
    - **Interactividad (JavaScript)**: Es el "siguiente paso" lógico.
  - **El Puente**: "Construimos un formulario, pero no hace nada. El siguiente paso en su aprendizaje (con JavaScript) es tomar control de ese formulario: validarlo antes de enviarlo, mostrar mensajes de error, o enviarlo a un servidor sin recargar la página. Esas técnicas son la base de todas las aplicaciones web modernas."
