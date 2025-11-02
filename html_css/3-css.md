# Módulo 3: CSS3 - Dando Estilo a la Web

En el Módulo 2, construimos el plano de nuestra casa (`index.html`). Ahora, en el Módulo 3, nos convertimos en diseñadores de interiores y paisajistas. Vamos a tomar esa estructura "fea" pero sólida y la haremos lucir increíble usando **CSS (Cascading Style Sheets)**.

CSS es el lenguaje que usamos para describir la presentación de un documento HTML. Controla los colores, las fuentes, los espacios, el tamaño, el posicionamiento y, en resumen, todo el aspecto visual de tu página.

## 1. ¿Cómo se Conecta CSS con HTML?

Tenemos tres formas de aplicar reglas CSS a nuestro HTML. De peor a mejor:

### a. En Línea (Inline) - Evítalo

Se aplica el estilo directamente a una etiqueta HTML usando el atributo style.

```html
<!-- MALA PRÁCTICA -->
<h1 style="color: blue; font-size: 30px;">Mi Título</h1>
```

**Es una pesadilla de mantener**. Si quieres cambiar el color de 10 títulos, tienes que editar 10 archivos. Mezcla estructura y presentación (¡rompe la regla de separación de responsabilidades!). Además, los estilos en línea tienen una "especificidad" muy alta, lo que los hace difíciles de sobrescribir con un CSS externo, causando frustración.

### b. Interno (Internal)

Se colocan las reglas CSS dentro de una etiqueta `<style>` en el `<head>` del archivo HTML.

```html
<!-- MEJOR, PERO NO IDEAL -->
<head>
  <title>Mi Página</title>
  <style>
    h1 {
        color: blue;
        font-size: 30px;
    }
  </style>
</head>
```

Sigue estando en el mismo archivo que el HTML. Si tu sitio tiene 5 páginas, tienes que copiar y pegar este bloque en las 5. Si cambias un estilo, debes cambiarlo 5 veces. No permite que el navegador guarde en caché el CSS, por lo que la página carga más lento.

### c. Externo (External) - La Mejor Práctica

Creamos un archivo completamente separado con la extensión `.css` (ej. `style.css`). Luego, lo "vinculamos" desde el `<head>` de nuestro archivo HTML usando la etiqueta `<link>`.

**En `style.css` (Archivo nuevo)**:

```css
/* Esto es un comentario en CSS */
h1 {
    color: blue;
    font-size: 30px;
}
```

**En index.html (Dentro del `<head>`)**:

```html
<head>
    <title>Mi Página</title>
    <!-- Se vincula el archivo CSS -->
    <!-- 
      rel="stylesheet" le dice al navegador QUÉ ES este archivo (una hoja de estilos).
      href="style.css" le dice DÓNDE ESTÁ (la ruta al archivo).
    -->
    <link rel="stylesheet" href="style.css">
</head>
```

**¡Esta es la forma profesional!**

- **Separación Total**: Tu HTML solo se preocupa de la estructura y tu CSS solo del estilo.
- **Mantenimiento Fácil**: Puedes tener _mil_ páginas HTML vinculadas al mismo archivo `style.css`. Si quieres cambiar el color de todos los títulos del sitio, ¡solo editas una línea en un solo archivo!
- **Rendimiento (Caché)**: El navegador descarga `style.css` _una vez_ y lo guarda en su "caché". Cuando el usuario visita otra página de tu sitio, no necesita volver a descargarlo, haciendo que el sitio cargue mucho más rápido.

## 2. La Sintaxis y la "Cascada"

### a. La Sintaxis Básica

Una regla CSS se compone de dos partes principales: un **selector** y un **bloque de declaración**.

```css
/* Selector -> h1 */
h1 {
    /* Bloque de declaración */
    propiedad: valor;
    color: green;
    font-size: 24px;
}
```

- **Selector (`h1`)**: Apunta al elemento (o elementos) HTML que quieres estilizar.
- **Bloque de Declaración (`{...}`)**: Contiene una o más declaraciones.
- **Declaración (`color: green;`)**: Es el par `propiedad: valor;`.
  - **Propiedad**: El atributo que quieres cambiar (ej. `color`, `font-size`, `background-color`).
  - **Valor**: El nuevo ajuste que quieres aplicar (ej. `green`, `24px`, `#FFFFFF`).
- **¡No olvides el punto y coma!** Cada declaración debe terminar con `;`. Es un error muy común olvidarlo.

### b. ¿Qué significa "Cascada"?

El nombre **Cascading Style Sheets** (Hojas de Estilo en Cascada) es la clave de todo. La "cascada" es el algoritmo/reglas que usa el navegador para decidir qué estilo CSS se aplica cuando hay conflictos.

Imagina que tienes dos reglas que apuntan al mismo `<h1>`:

```css
h1 { color: blue; }

h1 { color: red;} /* ¿Cuál gana? */
```

El navegador decide cuál gana basándose en tres factores (en orden de importancia):

1. **Especificidad (Specificity)**: ¿Qué selector es más "específico"? Un selector de `id` (`#menu`) le gana a un selector de clase (`.menu-item`), y un selector de clase le gana a un selector de etiqueta (`h1`).
2. **Importancia (Importance)**: Un estilo marcado como `!important` (ej. `color: red !important;`) le gana a todo. **¡Evita usar esto!** Es una mala práctica que genera caos. Los estilos en línea (atributo `style`) son el segundo más importante.
3. **Orden de Fuente (Source Order)**: Si dos reglas tienen la _misma especificidad_ (como en el ejemplo de arriba), **gana la última que fue leída**. En nuestro ejemplo, el `<h1>` sería red porque esa regla vino después. Esta es la razón por la que el orden de tus archivos CSS importa.

## 3. Selectores: El Corazón de CSS

Los selectores son la parte más poderosa de CSS. Son el "pegamento" que conecta tu CSS con tu HTML. Hay muchos tipos, pero nos concentraremos en los más importantes.

### a. Selector de Etiqueta (o Tipo)

Apunta a _todas las instancias_ de una etiqueta HTML en la página.

```css
/* Esto afectará a TODOS los párrafos <p> de la página */
p {
    font-family: Arial, sans-serif;
}
/* Esto afectará a TODO el contenido visible */
body {
    background-color: #f0f0f0;
}
```

**Cuándo usarlo**: Para estilos generales, base (ej. estilizar `body`, `h1`, `p` por defecto).

### b. Selector de Clase (Class)

Este es el selector que usarás el 90% del tiempo. Apunta a cualquier elemento que tenga un atributo `class` específico. Las clases son **reutilizables**.

**En HTML (¡puedes usar MÚLTIPLES clases!)**:

```html
<p class="destacado">Este párrafo es importante.</p>
<div class="destacado">Esta caja también es importante.</div>
<p class="destacado urgente">¡PELIGRO!</p>
```

**En CSS (se usa un punto `.`)**:

```css
/* Apunta a CUALQUIER elemento con class="destacado" */
.destacado {
    background-color: yellow;
    font-weight: bold;
}

/* Apunta a CUALQUIER elemento con class="urgente" */
.urgente {
    color: red;
    border: 1px solid red;
}
```

El último párrafo (`<p class="destacado urgente">`) recibirá **ambos** conjuntos de estilos: un fondo amarillo, texto en negrita, color rojo y un borde rojo.

### c. Selector de ID

Apunta a un solo elemento que tenga un atributo `id` específico. Los ID deben ser **únicos** en la página (solo un elemento puede tener ese ID).

**En HTML**:

```html
<div id="menu-principal">...</div>
```

**En CSS (se usa una almohadilla, gato o hashtag `#`)**:

```css
/* Apunta SOLO al elemento con id="menu-principal" */
#menu-principal {
    border: 1px solid black;
}
```

**Clase vs. ID: ¿Cuándo usar cuál?**

- Usa **Clase** (`.clase`) para todo lo que quieras _reutilizar_. Es tu herramienta principal de estilizado (ej. `.boton`, `.tarjeta-producto`, `.foto-perfil`).
- Usa ID (`#id`) para un elemento _único_ en la página (ej. `#header-principal`, `#formulario-contacto`). También se usa para "anclas" de página (ej. `index.html#seccion-contacto`) y para ser seleccionado por JavaScript.

### d. Selector de Descendiente (Contexto)

Te permite seleccionar un elemento solo cuando está _dentro_ de otro. Es increíblemente útil.

```css
/* Selecciona SOLO los <a> que estén DENTRO de un <footer> */
footer a {
    color: blue;
}

/* Selecciona SOLO los <li> que estén DENTRO de un .menu */
.menu li {
    padding: 5px;
}
```

Esto no afectará a los `<a>` que estén en tu `<header>` o `<main>`, solo a los del `<footer>`.

## 4. El Modelo de Caja (The Box Model)

**¡El concepto más importante de CSS!** El navegador trata a **cada elemento HTML como una caja rectangular**. El Modelo de Caja describe cómo se construye esa caja.

La caja se compone de cuatro capas, de adentro hacia afuera:

1. **Content (Contenido)**: El texto, la imagen, etc. Aquí es donde se aplican `width` (ancho) y `height` (alto).
2. **Padding (Relleno)**: El espacio _interno_ entre el contenido y el borde. Es el "acolchado" de la caja. Si le das _padding_ a un botón, el texto se aleja de los bordes.
3. **Border (Borde)**: La línea que rodea al padding y al contenido. Puedes definir su grosor, estilo (sólido, punteado) y color.
4. **Margin (Margen)**: El espacio _externo_ y transparente fuera del borde. Es la "distancia social" de la caja. Se usa para empujar a otras cajas y crear _espacio entre elementos_.

**Pro-Tip: ¡El `box-sizing` es tu mejor amigo!**

Por defecto, cuando defines `width: 100px;`, CSS aplica ese ancho solo al **Contenido**. Si luego añades `padding: 10px;` y `border: 2px;`, el ancho total de tu caja será:`100px (contenido) + 20px (padding izq/der) + 4px (borde izq/der) = 124px`.

_**¡Esto es confuso y causa muchos problemas de layout!**_

**La solución**: Dile al navegador que calcule el modelo de caja de forma más intuitiva con `box-sizing: border-box;`. Esta regla le dice al navegador: "Si defino `width: 100px;`, quiero que el ancho total de la caja (incluyendo padding y borde) sea `100px`". El navegador entonces ajustará el espacio del contenido _hacia adentro automáticamente_.

```css
/* Este es el "reset" de CSS más útil que existe: */
* {
    box-sizing: border-box;
}
```

_(El selector `*` es el "selector universal", selecciona todos los elementos de la página)_.

## 5. Propiedades CSS Esenciales

### a. Unidades de Medida

¿Qué significa `10px` o `80%`?

- **`px` (Píxel)**: Una unidad absoluta. Un píxel en la pantalla. Es bueno para cosas que quieres que tengan un tamaño fijo, como un `border: 1px;`.
- **`%` (Porcentaje)**: Una unidad relativa. Es un porcentaje del elemento padre. Si un `<div>` tiene width: 80%; y está dentro de un `<body>` de `1000px`, el `<div>` medirá `800px`. Esencial para _responsive design_.
- **`em`**: Relativo al `font-size` del _elemento padre_. Si un `<p>` tiene `font-size: 16px`, un `padding: 2em;` dentro de él será `32px`.
- **`rem` (Root EM)**: La mejor unidad para texto y espaciado. Es relativo al `font-size` de la etiqueta `<html>` (la raíz). Por defecto, esto es `16px`. Si defines tu `font-size` como `1.2rem`, será `1.2 * 16px = 19.2px`. Su gran ventaja es que si un usuario cambia el tamaño de fuente en su navegador (por accesibilidad), ¡todo tu layout escalará perfectamente!

### b. Propiedades Comunes

| Tipo | Propiedad | Descripción| Ejemplo de Valor|
| --- | --- | --- | --- |
| **Texto** | `color` | Cambia el color del texto.| `red`, `#333`, `#f0a8c2`, `rgb(50, 50, 50)` |
| | `font-family` | Cambia la fuente (tipografía) del texto. | `Arial`, `sans-serif`, `"Times New Roman"` |
| | `font-size` | Cambia el tamaño del texto.| `16px`, `2rem`, `120%` |
| | `font-weight` | Cambia el grosor del texto. | `normal` (400), `bold` (700) |
| | `text-align` | Alinea el texto horizontalmente. | `left`, `center`, `right` |
| | `text-decoration` | Decora el texto. (Se usa mucho para quitar el subrayado de los enlaces). | `none`, `underline` |
| | `line-height` | Define la altura de la línea de texto (mejora legibilidad). |`1.6` (significa 1.6 veces el font-size), `2rem` |
| **Caja (Layout)** | `background-color` | Cambia el color de fondo de la caja. | `blue`, `#f4f4f4` |
| | `width` | Define el ancho del contenido de la caja (o de la caja total, si usas `border-box`). | `500px`, `80%`, `auto` |
| | `height` | Define el alto del contenido de la caja. | `200px` |
| | `padding`| Define el relleno interno. | `10px` (4 lados), `10px 20px` (arr/ab, izq/der) |
| | `margin` | Define el margen externo. | `10px`, `10px 20px`, `0 auto` (truco para centrar) |
| | border | Define el borde. (Es un atajo para grosor, estilo y color). | `1px solid black`, `2px dotted #f00` |
| | `border-radius` | Redondea las esquinas de la caja. | `10px` (esquinas redondeadas), 50% (¡un círculo!) |
| | `display` | Define cómo se muestra una caja. | `block` (ocupa todo el ancho), `inline` (fluye con el texto), `inline-block` (como texto, pero respeta width/height), `none` (oculta el elemento) |

## 6. ¡A la Práctica! (Archivo `style.css`)

Ahora, vamos a aplicar lo aprendido. Crea un archivo llamado `style.css` **en la misma carpeta que tu** `index.html`.

### Paso 1: Archivo `style.css` (El CSS)

Crea este archivo primero. Contiene todos nuestros estilos.

```css
/*
  MÓDULO 3 - PRÁCTICA 2
  Este es nuestro archivo de estilos (style.css)
*/

/* Selector de Etiqueta:
  Estilos base para toda la página.
  Afecta a la etiqueta <body>.
*/
body {
    background-color: #f4f4f4; /* Un gris muy claro de fondo */
    color: #333; /* Un gris oscuro para el texto, más suave que el negro puro */
    font-family: Arial, "Helvetica Neue", Helvetica, sans-serif; /* Una fuente limpia, "sans-serif" es la opción de respaldo */
    line-height: 1.6; /* Espacio entre líneas de texto para mejor legibilidad (1.6 veces el tamaño de la fuente) */
}

/* Selector de Etiqueta:
  Estilos para el título principal.
*/
h1 {
    color: #005a9c; /* Un color azul oscuro para el título */
    text-align: center; /* Centramos el título */
}

/* Selector de Clase: .container
  Esta será nuestra clase principal para centrar el contenido.
  La hemos aplicado a <main> en el HTML.
*/
.container {
    width: 80%; /* La caja ocupará el 80% del ancho de la pantalla */
    max-width: 900px; /* Pero como máximo, tendrá 900px (para pantallas grandes) */
    
    /* EL TRUCO PARA CENTRAR:
      'margin: auto;' o 'margin: 0 auto;' le dice al navegador 
      'margin-top: 0', 'margin-bottom: 0', y que calcule 
      'margin-left' y 'margin-right' automáticamente. 
      Si el 'width' es menor al 100%, el navegador divide el espacio 
      sobrante por igual a ambos lados, centrando la caja.
    */
    margin: 20px auto; /* 20px de margen arriba/abajo, y centrado izq/der */
    
    background-color: #ffffff; /* Fondo blanco para el contenedor principal */
    padding: 20px; /* 20px de espacio interno (relleno) en los 4 lados */
    border-radius: 8px; /* Esquinas ligeramente redondeadas */
}

/* Selector de Clase: .profile-pic
  Esta clase la usaremos para la foto de perfil.
  La hemos aplicado a la etiqueta <img>.
*/
.profile-pic {
    /* Convertimos la imagen en un círculo perfecto.
      ¡Esto solo funciona si la imagen original es cuadrada!
      Nuestro placeholder no es cuadrado, así que se verá ovalado.
    */
    border-radius: 50%;
    
    border: 4px solid #ddd; /* Un borde gris claro */
    
    /* 'display: block' y 'margin: auto' es la forma de centrar
      una imagen (que por defecto es 'inline', y 'margin: auto' no funciona
      en elementos inline).
    */
    display: block;
    margin: 20px auto; /* 20px arriba/abajo, y centrado izq/der */
}


/* Selector de Etiqueta (Descendiente):
  Esto selecciona solo los <a> QUE ESTÉN DENTRO de un <footer>
  No afectará a otros enlaces que podamos tener.
*/
footer a {
    color: #005a9c; /* Mismo azul que el título */
    text-decoration: none; /* ¡Quitamos el subrayado feo! */
}

/*
  Pseudo-clase: :hover
  Esto define un estilo especial que se aplica solo cuando el 
  usuario pasa el puntero del mouse por encima del elemento.
*/
footer a:hover {
    text-decoration: underline; /* Le devolvemos el subrayado solo al pasar el mouse */
}

/*
  Selector de Etiqueta:
  Centramos el texto del footer.
*/
footer {
    text-align: center;
    margin-top: 30px; /* Un margen superior para separarlo del contenido */
    font-size: 0.9em; /* Texto un poco más pequeño (90% del tamaño del body) */
}
```

### Paso 2: Archivo `index.html`

Este es el archivo HTML del Módulo 2, pero con 3 líneas añadidas (marcadas con comentarios) para vincular el CSS y aplicar las clases.

```html
<!-- 
  MÓDULO 3 - PRÁCTICA 2
  Este es nuestro archivo HTML del Módulo 2,
  ¡PERO MODIFICADO para incluir el CSS!
-->

<!DOCTYPE html>
<html lang="es">
<head>
    <!-- Metadatos para el navegador -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Página de Perfil</title>
    
    <!-- MODIFICACIÓN 1: Vinculamos la hoja de estilos -->
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Mi Nombre (Ej. Alex Cruz)</h1>
    </header>

    <!-- MODIFICACIÓN 2: Añadimos la clase "container" al <main> -->
    <main class="container">
        
        <section>
            <h2>Sobre Mí</h2>
            <p>
                Hola, soy un desarrollador en formación apasionado por la tecnología. 
                Estoy aprendiendo HTML y CSS para construir aplicaciones web increíbles.
            </p>
            
            <img 
                class="profile-pic"
                src="https://placehold.co/400x200/e8e8e8/555?text=Mi+Foto+de+Perfil&font=noto-sans" 
                alt="Una foto de perfil genérica"
            >
        </section>

        <section>
            <h2>Mis Habilidades</h2>
            <ul>
                <li>HTML5 (Semántico)</li>
                <li>CSS3 (¡Próximamente!)</li>
                <li>Git (Control de versiones)</li>
            </ul>
        </section>

    </main>

    <footer>
        <p>
            Encuéntrame en 
            <a href="https://www.github.com" target="_blank">GitHub</a>.
        </p>
        <p>
            &copy; 2025 Mi Página. Todos los derechos reservados.
        </p>
    </footer>

</body>
</html>
```

## 7. Recursos Adicionales

CSS es, sin duda, la parte más profunda y artística del desarrollo web. ¡Nunca dejas de aprender trucos nuevos! Aquí tienes los mejores sitios para dominarlo:

- [MDN Web Docs - Guía de CSS](https://developer.mozilla.org/es/docs/Web/CSS): Tu biblia. La referencia más completa y precisa para cada propiedad de CSS.
- [CSS-Tricks - Guía de Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/): (En inglés) Flexbox es la herramienta moderna #1 para layouts. Esta guía visual es la más famosa y útil para aprenderlo.
- [Flexbox Froggy](https://flexboxfroggy.com/#es): Un juego interactivo para aprender Flexbox de forma divertida. ¡Altamente recomendado!
- [Grid Garden](https://cssgridgarden.com/#es): El juego hermano de Flexbox Froggy, pero para aprender CSS Grid (la otra gran herramienta de layout).
- [W3Schools - Tutorial de CSS](https://www.w3schools.com/css/): Ideal para ejemplos rápidos y probar cosas en vivo.
- [Adobe Color](https://color.adobe.com/es/create/color-wheel): Una herramienta profesional para crear paletas de colores armónicas para tus sitios web.

---

**¡Felicidades!** Si guardas ambos archivos (`index.html` actualizado y `style.css`) y refrescas tu `index.html` en el navegador, tu página ahora debería verse profesional, centrada, con colores y espaciado.

Has aprendido la trinidad del desarrollo front-end básico:

1. **HTML**: La estructura.
2. **CSS**: La presentación.

En el Módulo 4, veremos el inicio del tercer pilar: la **interacción**, aprendiendo a construir **formularios HTML** para que el usuario pueda enviarnos datos.
