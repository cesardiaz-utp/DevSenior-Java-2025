# Módulo 1: Conceptos Fundamentales de la Web

¡Bienvenido al apasionante mundo del desarrollo web! Este primer módulo es, sin duda, el más importante de tu carrera. Es la base sobre la que se construye todo lo demás, desde un blog personal hasta aplicaciones empresariales complejas que se ejecutan en tu navegador.

No te preocupes si los conceptos parecen abstractos o abrumadores al principio. Es normal. El desarrollo web se aprende conectando ideas, y todo cobrará sentido en cuanto empecemos a escribir código y veas cómo estos conceptos interactúan.

El objetivo de este módulo es construir un mapa mental sólido, entender el "qué" (qué es HTML) y el "por qué" (por qué se separa de CSS) antes de saltar al "cómo" (la sintaxis específica). Dominar estos fundamentos te permitirá aprender cualquier tecnología futura (frameworks, librerías, etc.) con mucha más facilidad, porque entenderás el lenguaje nativo del navegador.

## 1. La Trinidad de la Web: HTML, CSS y JavaScript

![Analogía Casa](https://www.keentodesign.com.au/cdn-cgi/imagedelivery/eOylWWvDYZyJkbAUtQZpuQ/www.keentodesign.com.au/html.png/w=768)

La analogía más famosa (y efectiva) para entender cómo funciona una página web es pensar en ella como la construcción de una casa. Es crucial entender que no son una sola cosa, sino tres lenguajes distintos diseñados para trabajar juntos. Cada uno tiene una responsabilidad clara y definida.

Toda página web, desde la más simple hasta la más compleja como Facebook o Google, se construye sobre estos tres pilares. Cuando un usuario visita tu sitio web, su navegador (el "cliente") recibe archivos de estos tres tipos (por ejemplo, `pagina.html`, `estilos.css` y `logica.js`) y los ensambla para crear la experiencia interactiva que ve en pantalla.

![Analogía Persona](https://d2dkqamqz2k831.cloudfront.net/posts/338-1733217432633.jpg)

### a. HTML (HyperText Markup Language) - El Esqueleto

El HTML es la **estructura** y el **significado** (la semántica) de la casa. Define todos los elementos que componen la casa y su jerarquía:

- ¿Hay un cimiento o pie de página? (`<footer>`)
- ¿Hay paredes estructurales o secciones? (`<section>`)
- ¿Hay habitaciones o contenedores genéricos? (`<div>`)
- ¿Hay una puerta o un botón para interactuar? (`<button>`)
- ¿Hay ventanas para ver contenido externo? (`<img>`)
- ¿Hay texto en las paredes, como párrafos? (`<p>`)
- ¿Hay un título principal en la entrada? (`<h1>`)

![HTML Semántico](https://www.aulaclic.es/articulos/graficos/estructura-html5.jpg)

**Puntos Clave**:

- **No es un lenguaje de programación**. Es un **lenguaje de marcado** (como su nombre indica). Esto significa que no puedes "calcular" cosas, usar lógica como condicionales (`if/else`) o crear bucles. Simplemente "marcas" un trozo de texto y le dices al navegador: "Navegador, trata esto como un título" (`<h1>...</h1>`) o "esto es una imagen" (`<img>`).
- **Usa elementos (o "tags")**. Son la unidad básica. La mayoría vienen en pares: una etiqueta de apertura (`<p>`) y una de cierre (</p>). El contenido va en medio. Algunos elementos, como `<img>` (imagen) o `<br>` (salto de línea), son "auto-cerrados" porque no tienen contenido interno.
- **Introduce los Atributos**: Los elementos pueden tener "**atributos**", que son información extra que se coloca en la etiqueta de apertura. Por ejemplo, en `<img src="foto.jpg">`, `src` (source) es el atributo que le dice al navegador _dónde_ encontrar el archivo de la imagen. En `<a href="google.com">`, `href` es el atributo que define el destino del enlace.
- El HTML es el **sustantivo** de tu página web. Es el "qué" de tu contenido.

### b. CSS (Cascading Style Sheets) - La Decoración

Si el HTML es la estructura de la casa, el CSS es la **presentación** o el estilo. Es todo lo que la hace visualmente atractiva y le da personalidad:

- ¿De qué color es la pared? (`color`, `background-color`)
- ¿Qué tan grandes son las ventanas? (`width`, `height`)
- ¿La tipografía es moderna o clásica? (`font-family`)
- ¿Los muebles están en el centro de la habitación o a la izquierda? (`text-align`)
- ¿Hay espacio entre los muebles? (`margin`)
- ¿El texto dentro de un botón está centrado y tiene espacio para respirar? (`padding`)

**Puntos Clave**:

- **CSS es un lenguaje de hojas de estilo**. Su único trabajo es aplicar reglas visuales al HTML. No puede cambiar la estructura (no puede convertir un `<p>` en un `<h1>`), pero puede hacer que un `<p>` se vea como un `<h1>`.
- **El poder de "Cascading" (Cascada)**: La "C" de CSS es el concepto más importante. Significa que los estilos "caen en cascada". Puedes definir un estilo general (ej. toda la letra del `<body>` es gris) y luego uno más específico (ej. la letra dentro del `<h1>` es negra). Las reglas más específicas (o las últimas en ser declaradas) sobrescriben a las más generales. Esto te da un control inmenso y granular.
- **Funciona mediante selectores**. Para aplicar un estilo, primero debes "seleccionar" a qué elemento(s) HTML aplicarlo. Puedes seleccionar por tipo de etiqueta (`p`), por una "clase" reutilizable (`.noticia-destacada`), o por un "ID" único (`#menu-principal`).
- El CSS es el **adjetivo** de tu página web. Es el "cómo se ve".

### c. JavaScript (JS) - La Funcionalidad

Finalmente, JavaScript es lo que le da **interactividad**, lógica y vida a la casa. Es la electricidad, la plomería y la domótica:

- Cuando presionas el interruptor (`<button>`), la luz se enciende (el CSS cambia o se añade una clase).
- Cuando abres el grifo, sale agua (se muestran nuevos datos cargados desde un servidor).
- Las puertas del garaje se abren automáticamente (una animación se dispara).
- ¿Qué pasa cuando el usuario envía un formulario? (JS puede validar que el email tenga un formato correcto antes de enviarlo).
- ¿La página necesita cargar nuevos datos sin recargarse? (Como un chat en vivo, un feed de noticias o los "me gusta" de una publicación).

**Puntos Clave**:

- **Este SÍ es un lenguaje de programación**. Con él puedes manejar lógica, cálculos, eventos del usuario (clics, movimientos del mouse, escritura en el teclado) y tomar decisiones.
- **Manipula el DOM**: Este es su trabajo principal en la web. JavaScript puede crear nuevos elementos HTML (`<p>`) desde cero, borrar otros, cambiar atributos (`src` de una `<img>`) y modificar los estilos CSS en respuesta a las acciones del usuario.
- **Es lo que permite crear aplicaciones complejas**. Cualquier cosa que parezca una "aplicación" dentro de tu navegador (Google Docs, un mapa interactivo, un videojuego) es posible gracias a JavaScript.
- El JavaScript es el **verbo** de tu página web. Es el "qué hace".

### Concepto Fundamental: Separación de Responsabilidades

- **¿Podríamos poner la pintura (CSS) directamente mezclada con los ladrillos (HTML) usando el atributo `style`?** Sí, pero sería un desastre de mantener.
- **¿Podríamos poner la lógica (JS) directamente dentro del HTML usando etiquetas `<script>` en medio del contenido?** También, pero sería igualmente caótico.

El principio más importante que aprenderás es mantener estos tres lenguajes en archivos separados:

- `index.html` (para la estructura)
- `style.css` (para los estilos)
- `app.js` (para la interacción)

Esto no es solo una "buena idea", es el estándar profesional absoluto. Facilita enormemente:

- **Mantenimiento**: Si quieres cambiar el color principal de tu marca, sabes que debes ir a un solo archivo (`.css`), en lugar de cazarlo dentro de 10 archivos `.html` diferentes.
- **Colaboración**: Un diseñador puede trabajar en el `.css` mientras un programador de backend trabaja en la lógica del `.js` sin pisarse el trabajo.
- **Rendimiento**: El navegador puede descargar y guardar en la "memoria caché" tu archivo `style.css` una sola vez. Luego, cuando el usuario visite otras 5 páginas de tu sitio, reutilizará ese archivo ya descargado, haciendo que la navegación sea mucho más rápida.

## 2. ¿Qué es el DOM? (Document Object Model)

Este es el concepto más técnico del módulo, pero es crucial para entender cómo funciona JavaScript.

- **La Analogía**: Si el archivo `index.html` es el plano de construcción de la casa.
- **El Navegador (Chrome, Firefox)**: Es el equipo de construcción (el lector de planos).
- **El DOM**: Es la **casa real y construida** en la memoria del navegador.

Cuando el navegador lee tu archivo HTML (el plano), usa esa información para construir una representación "viva" y estructurada de tu página en la memoria del computador. Esta representación se llama **DOM (Modelo de Objetos del Documento)**.

El DOM está organizado como un **árbol de nodos** (o un árbol genealógico). El `<html>` es el ancestro principal (la raíz), que tiene "hijos" como `<head>` y `<body>`. A su vez, `<body>` puede tener "hijos" como `<h1>` y `<p>`.

El DOM es "vivo". No es una foto estática de tu HTML; es una estructura de datos dinámica que se puede cambiar en cualquier momento.

**¿Por qué es tan importante el DOM?**

Porque JavaScript **no lee tu archivo HTML**. JavaScript interactúa con el **DOM**.

Cuando quieres cambiar el texto de un título, no editas el archivo `index.html` (eso es imposible después de que la página cargó). Le das una instrucción a JavaScript como: "Oye, ve al DOM, busca el elemento con el ID 'titulo-principal' y cambia su propiedad `textContent` (su texto interno) por '¡Hola Mundo!'". El navegador, que está constantemente observando el DOM, ve ese cambio y actualiza instantáneamente lo que el usuario ve en la pantalla.

**En resumen**:

- HTML es el texto estático que escribes en tu editor de código.
- El DOM es la versión "viva", interactiva y modificable de tu HTML que existe en el navegador.
- JavaScript es la herramienta que usamos para _modificar_ el DOM sobre la marcha.

## 3. Herramientas Esenciales del Desarrollador

No necesitas mucho software para empezar, pero el que usas es muy importante para tu productividad y para aprender.

### a. Navegador Web (Chrome o Firefox)

Tu navegador no es solo para ver páginas web, es tu principal herramienta de depuración. Ambos, Chrome y Firefox, vienen con un conjunto de "Herramientas de Desarrollador" (Developer Tools).

**Cómo abrirlas**: Simplemente haz clic derecho en cualquier página web y selecciona "**Inspeccionar**" (Inspect), o presiona la tecla **F12**.

Las pestañas más importantes para ti al inicio serán:

- **Inspector (o Elements)**: Te muestra el **DOM en vivo**. Puedes ver el HTML y el CSS que componen cualquier sitio web (¡incluyendo el de tu competencia!). Puedes incluso editar el CSS en vivo para probar cambios sin tener que guardar y recargar. Es tu mejor amigo para depurar por qué "algo no se ve bien". Te mostrará gráficamente el Modelo de Caja (margen, borde, padding) de cada elemento y te dirá exactamente qué reglas CSS se están aplicando y de dónde vienen (¡hola, cascada!).
- **Consola (Console)**: Aquí es donde verás los errores de JavaScript. Si tu código no funciona o algo no pasa cuando haces clic, la consola es el primer lugar donde debes mirar. También es un lugar donde puedes escribir y probar pequeños fragmentos de JavaScript directamente.
- **Network (Red)**: Más adelante, usarás esta pestaña para ver si tus imágenes, archivos CSS, fuentes y datos se están cargando correctamente, y para detectar si algo está tardando demasiado en cargar.

### b. Editor de Código (Recomendado: Visual Studio Code)

Podrías escribir HTML en el Bloc de Notas, pero sería muy doloroso. Un editor de código es un editor de texto "con superpoderes" para programadores.

Te ofrece:

- **Resaltado de sintaxis**: Pone el código de diferentes colores (ej. etiquetas HTML en azul, atributos en verde) para que sea fácil de leer y detectar errores.
- **Autocompletado (IntelliSense)**: Empiezas a escribir `<p` y te sugiere `<p></p>`. Te muestra qué atributos CSS son válidos, ahorrándote errores de tipeo.
- **Manejo de archivos**: Te permite ver toda la estructura de carpetas de tu proyecto en un panel lateral.
- **Terminal integrada**: Puedes ejecutar comandos (como instalar herramientas) sin salir del editor.
- **Extensiones**: Puedes añadirle plugins para todo tipo de funcionalidades. Dos extensiones que recomendamos instalar de inmediato son:
  - **Live Server**: Te permite abrir tu archivo `index.html` en un servidor local que se recarga automáticamente en tu navegador cada vez que guardas un cambio. ¡Ahorra muchísimo tiempo!
  - **Prettier**: Un formateador de código que ordena y limpia automáticamente tu código para que sea legible y siga los estándares cada vez que guardas.

**Nuestra recomendación estándar de la industria es Visual Studio Code (VS Code)**. Es gratuito, potente y usado por la gran mayoría de desarrolladores en el mundo.

## 4. Recursos Adicionales (Para seguir estudiando)

Este curso te dará las bases, pero la curiosidad y la práctica constante te harán un gran desarrollador. Te recomendamos marcar estos sitios como favoritos:

### 1. [MDN Web Docs (Mozilla Developer Network)](https://developer.mozilla.org/es/docs/Web)

Es la "Biblia" del desarrollo web. Es la documentación oficial y más completa, mantenida por Mozilla (creadores de Firefox) y colaboradores de Google, Microsoft y la comunidad. Es técnica, precisa y siempre actualizada. Úsala como una enciclopedia o un diccionario de referencia.

### 2. [freeCodeCamp](https://www.freecodecamp.org/learn/)

Ofrece un currículo interactivo y gratuito de desarrollo web completo. Aprendes leyendo un poco y escribiendo código directamente en el navegador. Ideal para practicar y solidificar lo aprendido después de clase.

### 3. [W3Schools](https://www.w3schools.com/)

Es excelente para referencias rápidas. No es tan "oficial" o profundo como MDN, pero a menudo es más fácil de entender para principiantes y tiene ejemplos de código sencillos que puedes probar directamente en el navegador.

### 4. Juegos de CSS (¡Para practicar!)

- [Flexbox Froggy](https://flexboxfroggy.com/#es)
- [Grid Garden](https://cssgridgarden.com/#es)

Son la forma más divertida y efectiva de aprender conceptos de layout de CSS como Flexbox y Grid (que veremos más adelante). Aprenderás jugando.

### 5. [Visual Studio Code](https://code.visualstudio.com/)

Para instalar la herramienta principal de trabajo que usaremos en el curso.

---

¡Estás listo para empezar! En el próximo módulo, dejaremos la teoría y escribiremos nuestra primera página web. Recuerda, cada desarrollador senior de hoy fue un principiante absoluto ayer.

¡Vamos a construir!
