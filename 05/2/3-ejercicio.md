# Ejercicio Autónomo: Extendiendo el Servicio de Libros y Practicando

**Objetivo del Ejercicio:**

Aplicarás los conceptos de la capa de servicio, depuración y refactorización con Cursor AI para añadir una nueva funcionalidad al `LibroService` que hemos trabajado.

**Contexto:**

Actualmente, nuestro `LibroService` permite listar, buscar, agregar y eliminar libros de una lista en memoria. Queremos añadir una funcionalidad que permita marcar un libro como "leído".

---

**Pasos a Seguir:**

Este ejercicio te guiará a través de la adición de una nueva característica, aplicando las habilidades que hemos practicado en la tutoría. ¡Recuerda usar **Cursor AI** como tu asistente en cada paso!

**Paso 1: Añadir el Estado "Leído" al Modelo `Libro`**

* **Tarea:** Modifica la clase `Libro.java` para incluir una nueva propiedad que indique si el libro ha sido leído o no. Un `boolean` es una buena opción para esto (ej. `boolean leido;`).
* **Ayuda de Cursor AI:** Puedes pedirle a Cursor AI que te genere automáticamente los métodos getter y setter para este nuevo campo si aún no tienes Lombok configurado, o simplemente añade la línea `boolean leido;` y usa la sugerencia de "Add getter and setter" si aparece.

**Paso 2: Extender la Interfaz y la Implementación del Servicio**

* **Tarea:** Define un nuevo método en la interfaz `LibroService` que permita marcar un libro como leído, probablemente identificándolo por su título o ISBN (elige el que te parezca más práctico).  *Ejemplo: `void marcarComoLeido(String identificador);`*
* **Tarea:** Implementa este nuevo método en la clase `LibroServiceImpl`. La lógica debe ser:
    1.  Recibir el identificador del libro.
    2.  Buscar el libro correspondiente en la `listaLibros` en memoria.
    3.  Si el libro es encontrado, actualiza su estado `leido` a `true`.
* **Ayuda de Cursor AI:**
    * Al añadir el método en la interfaz, Cursor AI te sugerirá automáticamente implementarlo en `LibroServiceImpl`. Acepta la sugerencia.
    * Dentro del método `marcarComoLeido`, puedes pedirle a Cursor AI que te ayude con la lógica de búsqueda en la lista (similar a `buscarPorTitulo`) y la actualización del estado. Por ejemplo: "In this method, find a Libro in `listaLibros` by its title and set its `leido` property to true".

**Paso 3: Añadir un Endpoint en el Controlador**

* **Tarea:** Crea un nuevo método en `LibroController.java` que maneje una petición HTTP para marcar un libro como leído. Un método `PUT` es semánticamente apropiado para actualizar un recurso existente. Puedes usar un `@PathVariable` para recibir el identificador del libro en la URL.
* **Tarea:** Dentro de este nuevo método del controlador, llama al método `marcarComoLeido` de tu `libroService` inyectado.
* **Ayuda de Cursor AI:**
    * Usa la generación de código inline (Cmd/Ctrl + K) o el chat para pedirle a Cursor AI que te genere el boilerplate para un endpoint `PUT` en un controlador Spring. Por ejemplo: "Create a Spring Boot PUT endpoint in this controller at `/api/libros/{titulo}/marcarLeido`".
    * Cursor AI te ayudará a autocompletar la llamada al método del servicio.

**Paso 4: Practicar la Depuración del Nuevo Flujo**

* **Tarea:** Establece puntos de ruptura en tu nuevo método del controlador y en el método `marcarComoLeido` en `LibroServiceImpl`.
* **Tarea:** Inicia la aplicación en modo Debug en Cursor AI.
* **Tarea:** Usa una herramienta como Postman (o `curl`) para enviar una petición `PUT` al nuevo endpoint (asegúrate de que el libro que intentas marcar como leído ya exista en tu lista en memoria, quizás agregándolo primero con el endpoint POST existente).
* **Tarea:** Utiliza los controles de depuración (Step Over, Step Into) para seguir el flujo de ejecución desde el controlador hasta el servicio.
* **Tarea:** Inspecciona las variables en cada punto de ruptura para confirmar que el identificador del libro se está pasando correctamente y que el estado `leido` se actualiza en el objeto `Libro` dentro de la lista.
* **Ayuda de Cursor AI:** Si te pierdes con los controles de depuración o la ventana de variables, usa el chat de Cursor AI para preguntar cómo realizar una acción específica (ej. "How do I use Step Into in the debugger?", "Show me the value of the 'libro' variable").

**Paso 5: Refactorizar y Mejorar con Cursor AI**

* **Tarea:** Revisa el código que has escrito para el método `marcarComoLeido`. ¿Hay alguna parte que se vea un poco compleja?
* **Tarea:** Pídele a Cursor AI que sugiera mejoras para este método. Por ejemplo: "Suggest ways to simplify the `marcarComoLeido` method in LibroServiceImpl."
* **Tarea:** Si Cursor AI sugiere una refactorización (como extraer un método, simplificar una condición, etc.), analiza la sugerencia. ¿Entiendes por qué la sugiere? ¿Es una mejora real? Si estás convencido, aplica la refactorización con la ayuda de Cursor AI.
* **Tarea (Opcional):** Utiliza la función de detección de duplicados de Cursor AI para escanear tu código y ver si encuentras algo que podrías simplificar.

**Paso 6: Documentar el Nuevo Método (Opcional)**

* **Tarea:** Añade comentarios Javadoc a tu nuevo método `marcarComoLeido` explicando qué hace, sus parámetros y posibles resultados.
* **Ayuda de Cursor AI:** Puedes pedirle a Cursor AI que te genere un borrador de Javadoc para el método. Selecciona el método y usa el chat: "Generate Javadoc for this method."

---

**Para Finalizar (y si te sientes aventurero):**

* Modifica el método `listarLibros` para que solo devuelva los libros que *no* han sido leídos por defecto, añadiendo un parámetro opcional para incluir todos los libros.
* Depura este flujo para asegurar que el filtro funciona correctamente.

---

¡Recuerda que este ejercicio es para que practiques de forma autónoma! Utiliza Cursor AI como tu asistente personal de programación. No dudes en preguntar, pedir explicaciones y solicitar sugerencias.

¡Mucho éxito!