# Módulo 4: Formularios HTML - El Inicio de la Interacción

En los módulos anteriores, hemos trabajado en un monólogo: nosotros creamos contenido (HTML) y le damos estilo (CSS) para que el usuario lo _consuma_. El flujo de información ha sido en una sola dirección: de nosotros hacia el usuario.

Los formularios HTML cambian esto. Son el componente fundamental que permite al usuario _hablarnos de vuelta_. Son el inicio de la **interacción** y el primer paso para convertir un sitio web estático en una **aplicación web dinámica**.

Casi todo lo que haces en la web que implica enviar información, lo haces a través de un formulario (`form`):

- Iniciar sesión
- Escribir un término de búsqueda en Google
- Publicar un post en una red social
- Comprar un producto

## 1. ¿Qué es un Formulario HTML?

Un formulario es un conjunto de elementos especiales de HTML que, juntos, actúan como un contenedor para recopilar datos del usuario.

El proceso tiene dos partes:

1. **Front-End (Lo que hacemos hoy)**: Es el formulario HTML visible y sus estilos CSS. Es la "hoja de papel" que el usuario rellena.
2. **Back-End (Lo que NO hacemos hoy)**: Es un programa en un servidor que recibe los datos, los procesa (ej. los guarda en una base de datos, envía un email) y envía una respuesta. Es el "buzón" y la "oficina" que procesa la carta.

Hoy nos enfocamos en construir una "hoja de papel" perfecta.

## 2. Las Etiquetas Clave del Formulario

Construir un formulario es como construir con bloques de LEGO. Estas son las piezas esenciales.

### a. `<form>`: El Contenedor

Es la etiqueta padre que envuelve todos los controles del formulario.

```html
<form action="/enviar-datos" method="POST">
    <!-- Aquí dentro van todos los labels, inputs, etc. -->
</form>
```

- `action`: Le dice al navegador **A DÓNDE** enviar los datos (usualmente una URL en un servidor). Como no tenemos servidor, a menudo se deja vacío o con un `#`.
- `method`: Le dice al navegador **CÓMO** enviar los datos.
  - `GET`: Envía los datos de forma visible en la URL (ej. `google.com/search?q=gatos`). Se usa para búsquedas, no para datos sensibles.
  - `POST`: Envía los datos de forma "oculta" en el cuerpo de la petición. Es el estándar para enviar formularios, inicios de sesión, etc.
  
### b. `<label>` y la Accesibilidad

Esta es la etiqueta más importante para la **accesibilidad** y la **usabilidad**. Es el texto que describe un campo.

```html
<!-- LA FORMA CORRECTA Y PROFESIONAL -->

<!-- El 'for' del label coincide con el 'id' del input -->
<label for="nombreUsuario">Nombre:</label>
<input type="text" id="nombreUsuario">
```

**¿Por qué es crucial?**

1. **Accesibilidad**: Los lectores de pantalla leen el `label` cuando el usuario enfoca el `input`, diciéndole qué debe rellenar.
2. **Usabilidad**: ¡Prueba a hacer clic en el texto "Nombre:"! Verás que el cursor se activa automáticamente en la caja de texto. Esto crea un área de clic más grande, lo cual es vital en dispositivos móviles.

#### ¡Nunca hagas esto! (Mala práctica)

```html
Nombre: <input type="text">
```

### c. `<input>`: El Caballo de Batalla

Es la etiqueta más versátil. Su comportamiento cambia drásticamente según su atributo `type`.

```html
<!-- Un campo de texto simple -->
<label for="nombre">Nombre:</label>
<input type="text" id="nombre" name="nombre_usuario" placeholder="Ej. Alex Cruz">

<!-- Un campo de email (el navegador lo valida) -->
<label for="correo">Email:</label>
<input type="email" id="correo" name="email_usuario" required>

<!-- Un campo de contraseña (oculta el texto) -->
<label for="clave">Contraseña:</label>
<input type="password" id="clave" name="password">
```

- `id`: Es el identificador _único_ para vincularlo con el `<label>`.
- `name`: **¡Importante!** Este es el nombre que usa el servidor para identificar el dato. Si no pones `name`, ¡el dato no se envía!
- `placeholder`: Texto de ayuda grisado que desaparece al escribir.
- `required`: Un atributo booleano que le dice al navegador "este campo no puede estar vacío".

### d. `<textarea>`: Para Texto Largo

Para campos de "Mensaje", "Comentario" o "Biografía", un `<input>` es muy pequeño. Usamos `<textarea>`.

```html
<label for="mensaje">Tu Mensaje:</label>
<textarea id="mensaje" name="mensaje_usuario" rows="5"></textarea>
```

- `rows="5"` le da una altura inicial (5 líneas de texto). Se puede controlar mejor con CSS.

### e. `<button>`: El Envío

Finalmente, necesitamos un botón para que el usuario envíe el formulario.

```html
<!-- La forma moderna y preferida -->
<button type="submit">Enviar Mensaje</button>
```

- `type="submit"`: Este es el valor mágico. Le dice al botón "Cuando me hagan clic, busca el `<form>` en el que vives y envíalo" (según su `action` y `method`).
- **Alternativa (Antigua)**: `<input type="submit" value="Enviar Mensaje">`. Funciona igual, pero `<button>` es más flexible porque puedes poner iconos e imágenes dentro.

## 3. Referencia Rápida: Etiquetas de Formulario

Aquí tienes una tabla de referencia con los elementos más comunes que encontrarás al construir formularios, incluyendo algunos que no usamos en la práctica pero que son fundamentales.

### a. Elementos Principales de Formulario

| Etiqueta | Atributos Clave | Descripción |
| --- | --- | --- |
| `<form>` | `action`, `method` |El contenedor principal que envuelve todos los controles del formulario. |
| `<label>` | `for` | La etiqueta de texto que describe un control. El for se vincula al id de un input para accesibilidad. |
| `<input>` | `type`, `id`, `name`, `placeholder`, `required` | El elemento de entrada de datos más versátil. Su comportamiento depende del atributo type. |
| `<textarea>` | `id`, `name`, `rows`| Un campo de texto de múltiples líneas, para mensajes o comentarios largos. |
| `<button>` | `type` | Un botón. `type="submit"` envía el formulario. `type="button"` no hace nada (para JavaScript). |
| `<select>` | `id`, `name` | Un control para crear un menú desplegable (dropdown). |
| `<option>` | `value` | Representa una opción individual dentro de un `<select>`. El value es lo que se envía al servidor. |
| `<fieldset>` | | `Agrupa visual y semánticamente un conjunto de campos relacionados (ej. "Dirección de Envío"). |
| `<legend>` | | El título para un `<fieldset>`. |

### b. Atributos type Comunes para `<input>`

La etiqueta `<input>` es un camaleón. El atributo type lo cambia todo.

| type | Descripción |
| --- | --- |
| `type="text"` | El campo de texto por defecto, de una sola línea. |
| `type="email` | "Un campo de texto que valida si el contenido parece un email (en navegadores modernos). |
| `type="password"` | Oculta los caracteres a medida que el usuario escribe. |
| `type="number"` | Un campo para números, a menudo muestra flechas para incrementar/decrementar. |
| `type="checkbox"` | Una casilla de verificación (para múltiples opciones, "marcar todas las que apliquen"). |
| `type="radio"` | Un botón de opción (para una sola opción de un grupo; todos los radios del grupo deben compartir el mismo name). |
| `type="date"` | Muestra un selector de calendario nativo del navegador. |
| `type="file"` | Permite al usuario seleccionar un archivo de su dispositivo para subirlo. |
| `type="submit"` | (Forma antigua) Un botón que envía el formulario. Se prefiere `<button type="submit">`. |

## 4. ¡A la Práctica! (Creando 3 Archivos)

Vamos a crear una nueva página de contacto. Esto implicará 3 pasos:

1. Crear un **NUEVO** archivo: `contacto.html`.
2. **MODIFICAR** nuestro `index.html` para añadir un menú de navegación.
3. ****AÑADIR** nuevos estilos a nuestro `style.css` para el menú y el formulario.

### Archivo 1 (NUEVO): `contacto.html`

Crea este archivo en la misma carpeta que `index.html` y `style.css`.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contacto - Mi Página</title>
    <!-- ¡Vinculamos la MISMA hoja de estilos! -->
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <header>
        <h1>Mi Nombre (Ej. Alex Cruz)</h1>
        
        <!-- Añadimos un menú de navegación -->
        <nav>
            <ul>
                <li><a href="index.html">Inicio</a></li>
                <li><a href="contacto.html">Contacto</a></li>
            </ul>
        </nav>
    </header>

    <!-- Reutilizamos la clase .container para centrar el contenido -->
    <main class="container">
        
        <section>
            <h2>Formulario de Contacto</h2>
            <p>Envíame un mensaje y me pondré en contacto contigo.</p>

            <!-- Este es nuestro formulario -->
            <form action="#" method="GET" class="formulario-contacto">
                
                <!-- Agrupamos cada par de label/input para estilizarlo fácil -->
                <div class="form-group">
                    <label for="nombre">Nombre:</label>
                    <input type="text" id="nombre" name="nombre" required>
                </div>
                
                <div class="form-group">
                    <label for="email">Email:</label>
                    <input type="email" id="email" name="email" required>
                </div>
                
                <div class="form-group">
                    <label for="mensaje">Mensaje:</label>
                    <textarea id="mensaje" name="mensaje" rows="6" required></textarea>
                </div>
                
                <!-- El botón de envío -->
                <button type="submit">Enviar</button>
                
            </form>
        </section>

    </main>

    <footer>
        <p>
            Encuéntrame en 
            <a href="[https://www.github.com](https://www.github.com)" target="_blank">GitHub</a>.
        </p>
        <p>
            &copy; 2025 Mi Página. Todos los derechos reservados.
        </p>
    </footer>

</body>
</html>
```

### Archivo 2 (MODIFICADO): `index.html`

Ahora, vuelve a tu `index.html` y añádele el mismo `<nav>...</nav>` dentro del `<header>`, justo después del `<h1>`.

```html
<!-- En index.html, dentro de <header> -->
<header>
    <h1>Mi Nombre (Ej. Alex Cruz)</h1>
    
    <!-- LÍNEAS NUEVAS -->
    <nav>
        <ul>
            <li><a href="index.html">Inicio</a></li>
            <li><a href="contacto.html">Contacto</a></li>
        </ul>
    </nav>
    
</header>
<!-- ... el resto de tu index.html sigue igual ... -->
```

### Archivo 3 (AÑADIR A): `style.css`

Abre tu archivo `style.css` existente. **NO BORRES NADA**. Simplemente, añade todo este nuevo bloque de código al final del archivo.

```css
/*
=========================================
  ESTILOS DEL MÓDULO 4 (AÑADIR AL FINAL)
=========================================
*/

/* --- Estilos de Navegación (Menu) --- */

nav {
    background-color: #333; /* Fondo oscuro para el menú */
    padding: 10px 0;
    margin-bottom: 20px; /* Separa el menú del título (en pantallas pequeñas) */
}

nav ul {
    list-style: none; /* Quitamos las viñetas */
    padding: 0;
    margin: 0;
    text-align: center; /* Centramos los elementos del menú */
}

nav ul li {
    display: inline-block; /* Hacemos que los 'li' se pongan uno al lado del otro */
    margin: 0 15px;
}

nav ul li a {
    color: white; /* Texto blanco */
    text-decoration: none; /* Sin subrayado */
    font-weight: bold;
    padding: 5px 10px;
    border-radius: 5px;
}

/* :hover para el menú */
nav ul li a:hover {
    background-color: #555; /* Un gris más claro al pasar el mouse */
}


/* --- Estilos del Formulario --- */

.formulario-contacto {
    margin-top: 20px;
}

/* El 'div' que agrupa label e input */
.form-group {
    margin-bottom: 20px; /* Espacio entre cada campo del formulario */
}

.form-group label {
    display: block; /* Hace que el label ocupe su propia línea */
    font-weight: bold;
    margin-bottom: 5px; /* Pequeño espacio entre el label y el input */
}

/* Estilo para los campos de texto y el textarea */
.form-group input[type="text"],
.form-group input[type="email"],
.form-group textarea {
    width: 100%; /* ¡El input ocupa el 100% del ancho de su contenedor! */
    padding: 10px; /* Relleno interno para que sea fácil de clickear y ver */
    border: 1px solid #ccc; /* Borde sutil */
    border-radius: 5px; /* Esquinas redondeadas */
    
    /* Esto soluciona un problema del 'box-sizing' con los inputs */
    box-sizing: border-box; 
}

/* Estilo para el botón de envío */
button[type="submit"] {
    background-color: #005a9c; /* Mismo azul que el título h1 */
    color: white;
    font-weight: bold;
    font-size: 1.1em;
    padding: 12px 20px;
    border: none; /* Sin borde */
    border-radius: 5px;
    cursor: pointer; /* Cambia el cursor a una manito */
    
    /* Transición suave para el :hover */
    transition: background-color 0.3s ease;
}

/* :hover para el botón */
button[type="submit"]:hover {
    background-color: #004070; /* Un azul un poco más oscuro */
}
```

## 5. Recursos Adicionales para Profundizar

Los formularios son un tema sorprendentemente profundo, especialmente cuando comienzas a estilizarlos (lo cual puede ser complicado). Aquí tienes algunos recursos excelentes para seguir aprendiendo:

- [MDN - Tu Primer Formulario HTML](https://developer.mozilla.org/es/docs/Learn/Forms/Your_first_form): La guía oficial de Mozilla para empezar desde cero.
- [MDN - Estilizar Formularios HTML](https://developer.mozilla.org/es/docs/Learn_web_development/Extensions/Forms/Styling_web_forms): Una guía detallada sobre cómo dar estilo a los diferentes tipos de controles de formulario.
- [W3Schools - Referencia de `<form>`](https://www.w3schools.com/tags/tag_form.asp): Una referencia rápida de todos los atributos y etiquetas.
- [web.dev (Google) - Aprende sobre Formularios](https://web.dev/learn/forms/): Este es un curso muy moderno y completo.
- [MDN - Estilizado Avanzado de Formularios](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling): Este profundiza en técnicas más complejas.

---

¡Felicidades! Ahora tienes un sitio web de dos páginas, enlazadas por un menú de navegación, y una de ellas tiene un formulario con un estilo profesional.

Intenta esto: Abre `contacto.html` en tu navegador, rellena el formulario y presiona "Enviar".

**¿Qué pasó?** La página probablemente se recargó y puede que veas los datos que escribiste en la barra de direcciones (ej. `contacto.html?nombre=Alex&...`). Esto es porque usamos method="GET".

Hemos llegado al límite de lo que HTML y CSS pueden hacer. Construimos el coche, pero no tenemos el motor.

Para _realmente_ hacer algo con esos datos (validarlos, enviarlos a un email, guardarlos, mostrar un mensaje de "¡Gracias!" sin recargar la página), necesitamos el tercer pilar: **JavaScript**.

¡Has completado la base fundamental del desarrollo web!
