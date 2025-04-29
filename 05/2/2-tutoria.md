## Tutoría:  Refuerzo en servicios, modularidad y depuración asistida

### Objetivos de la Sesión:

El objetivo de hoy no es solo escribir código, sino **entender cómo se ejecuta**. Cuando una petición llega a tu aplicación Spring, ¿qué pasa exactamente? ¿En qué orden se llaman los métodos? ¿Qué datos se pasan de una capa a otra? Aprender a depurar te dará superpoderes para responder estas preguntas y solucionar problemas rápidamente.

---

### Actividad 1: Preparando la Depuración y Puntos de Ruptura (15 minutos)

**Concepto:** La depuración (debugging) es el proceso de ejecutar tu programa paso a paso para encontrar y corregir errores. Un **punto de ruptura (breakpoint)** es una marca que pones en una línea de código para decirle al depurador que detenga temporalmente la ejecución ahí.

**Por qué es importante:** Establecer puntos de ruptura en puntos clave (como la entrada de un controlador, el inicio de un método de servicio) nos permite "congelar" la aplicación y ver su estado interno en ese momento.

**Pasos a seguir en Cursor AI:**

1.  **Abre tu Proyecto:** Asegúrate de tener el proyecto Spring Boot (con el `LibroController` y `LibroService` que creamos) abierto en Cursor AI.
2.  **Identifica Métodos Clave:** Piensa en una operación específica que quieras seguir, por ejemplo, agregar un libro (`POST /api/libros`). Identifica el método en el controlador que maneja esta petición (`LibroController.addLibro()`) y el método en el servicio al que llama (`LibroService.agregarLibro()`).
3.  **Establece Puntos de Ruptura:**
    * Haz clic en el margen izquierdo de la línea donde comienza el cuerpo del método `addLibro` en `LibroController`. Debería aparecer un círculo rojo.
    * Haz clic en el margen izquierdo de la línea donde comienza el cuerpo del método `agregarLibro` en `LibroService`. Debería aparecer otro círculo rojo.
4.  **Inicia la Aplicación en Modo Debug:** En la barra de herramientas superior de Cursor AI (o en el menú `Run`), busca el botón de iniciar con un icono de "bicho" (bug) o que dice "Debug" y haz clic en él. La aplicación se iniciará, pero estará esperando peticiones.

**Qué esperamos lograr:** Que la aplicación se inicie correctamente en modo depuración y que los círculos rojos (puntos de ruptura) estén activos, indicando que el depurador pausará la ejecución en esas líneas cuando se alcancen.

---

### Actividad 2: Rastreo del Flujo de Ejecución con Controles de Depuración (30 minutos)

**Concepto:** Una vez que la ejecución se detiene en un punto de ruptura, usamos los controles de depuración para movernos por el código. Estos controles nos permiten ver exactamente el camino que sigue el programa.

**Por qué es importante:** Rastrear el flujo nos ayuda a confirmar que las llamadas a métodos entre capas (controlador -> servicio -> modelo) ocurren como esperamos y en el orden correcto.

**Pasos a seguir en Cursor AI:**

1.  **Realiza una Petición:** Abre una herramienta como Postman o usa `curl` (si te sientes cómodo con la terminal) para enviar una petición `POST` a `http://localhost:8080/api/libros` con un cuerpo JSON que represente un nuevo libro (por ejemplo: `{"isbn": "12345", "titulo": "Mi Nuevo Libro", "autor": "Un Autor"}`).
2.  **La Ejecución se Detiene:** Observa cómo la ventana de Cursor AI se enfoca y la ejecución se detiene en el primer punto de ruptura que estableciste en `LibroController.addLibro()`. La línea actual se resaltará.
3.  **Usa los Controles de Depuración:**
    * **Step Over (F8):** Utiliza F8 para avanzar línea por línea *dentro* del método actual (`addLibro`). Observa cómo el puntero avanza.
    * Cuando llegues a la línea donde se llama a `libroService.agregarLibro(libro);`, **NO uses F8**.
    * **Step Into (F7):** Usa F7 para "entrar" en el método `agregarLibro` del servicio. Verás cómo la ejecución salta del controlador al método `agregarLibro` en `LibroServiceImpl`. La ejecución ahora está pausada dentro del método del servicio.
    * Continúa usando F8 para avanzar línea por línea dentro del método `agregarLibro`.
    * Si te adentras demasiado en código interno de Java o que no te interesa depurar, puedes usar **Step Out (Shift+F8)** para salir del método actual y volver a la línea justo después de donde lo llamaste.
4.  **Inspecciona Variables:** Mientras la ejecución está pausada en cualquier punto de ruptura, mira la ventana "Variables" en la interfaz de depuración de Cursor AI.
    * Observa los valores de las variables locales (ej. `libro` en el controlador y en el servicio).
    * Verifica que los datos se pasaron correctamente del controlador al servicio.
    * Puedes expandir objetos para ver sus atributos.

**Ejercicio Práctico:**

* Depura el flujo de la petición `POST /api/libros` utilizando los controles de depuración para seguir la ejecución desde el controlador hasta el servicio.
* En cada punto de ruptura, examina el valor del objeto `libro` para asegurarte de que los datos son correctos.
* Repite el proceso para la petición `GET /api/libros/{titulo}` (llamando a `buscarPorTitulo`). Sigue el flujo y verifica el libro retornado (o `null`).

**Qué esperamos lograr:** Que te sientas cómodo usando los controles de depuración para seguir el flujo de ejecución a través de las capas de la aplicación y que puedas inspeccionar el estado de las variables en diferentes puntos. Esto te da una comprensión real de cómo interactúan las capas.

---

### Actividad 3: Revisión del Uso de @Autowired y Principios de Bajo Acoplamiento (15 minutos)

**Concepto:** `@Autowired` es la forma en que Spring inyecta las dependencias. Usar inyección de dependencias y programar a interfaces promueve el **bajo acoplamiento**, lo que significa que los componentes dependen lo menos posible unos de otros.

**Por qué es importante:** El bajo acoplamiento hace que tu código sea más flexible, fácil de probar y más fácil de modificar en el futuro sin romper otras partes.

**Pasos a seguir en Cursor AI:**

1.  **Visualiza Dependencias:** Cursor AI (basado en VS Code) tiene herramientas para entender las relaciones entre clases.
    * Haz clic derecho sobre el nombre de la clase `LibroController` en el editor.
    * Busca opciones como "Show References", "Go to Definition", o "Find All Implementations/Usages". (Las opciones exactas pueden variar, usa la búsqueda si no las encuentras).
    * **Si Cursor AI tiene una función específica para visualizar gráficos de dependencia, úsala aquí.** (Puedes preguntar a Cursor AI: "How can I visualize class dependencies in this project?").
    * Alternativamente, simplemente revisa el código: observa el constructor del `LibroController` y la línea `@Autowired public LibroController(LibroService libroService)`.
2.  **Analiza la Inyección:**
    * Pregúntate: ¿El `LibroController` sabe cómo se crea una instancia de `LibroService`? La respuesta es no. Solo declara que *necesita* un `LibroService` y Spring se encarga de proporcionárselo. Esto es DI.
    * Observa que la dependencia es a la *interfaz* `LibroService`, no a la clase concreta `LibroServiceImpl`.
3.  **Discute el Bajo Acoplamiento:** Imagina que decides cambiar tu implementación de `LibroService` para usar una base de datos real en lugar de una lista en memoria. Mientras la nueva clase también implemente la interfaz `LibroService`, ¡el `LibroController` no necesita cambiar en absoluto! Esto demuestra el bajo acoplamiento.

**Ejercicio Práctico:**

* Utiliza las herramientas de navegación y análisis de código de Cursor AI para identificar dónde se usa `LibroService` (debería ser en el `LibroController`).
* Examina el constructor del `LibroController` y explícale a un compañero (o a ti mismo) por qué `@Autowired` aquí demuestra el bajo acoplamiento.

**Qué esperamos lograr:** Que comprendas visualmente cómo `@Autowired` establece conexiones entre tus beans y que aprecies cómo programar a interfaces (como `LibroService`) reduce el acoplamiento, haciendo que tu código sea más flexible.

---

### Actividad 4: Diagnóstico y Simplificación con Cursor AI (30 minutos)

**Concepto:** Las herramientas de análisis de código y la inteligencia artificial pueden ayudarte a identificar "código sucio" (code smells) como duplicación, complejidad innecesaria o áreas que podrían ser más claras.

**Por qué es importante:** Simplificar y refactorizar el código lo hace más fácil de leer, entender, mantener y extender. La IA puede actuar como un "pair programmer" que te señala áreas de mejora.

**Pasos a seguir en Cursor AI:**

1.  **Identificar Código Duplicado:**
    * Abre el chat de AI en Cursor AI (Cmd/Ctrl + L).
    * Escribe un prompt como: "Find duplicated code in this Spring project related to book operations." o "Analyze the LibroServiceImpl class and point out any code duplication."
    * Revisa los resultados que te dé Cursor AI. ¿Hay secciones de código que se repiten? (Quizás en la validación simple de `agregarLibro`).
2.  **Obtener Sugerencias de Simplificación:**
    * Selecciona un método en `LibroServiceImpl`, por ejemplo, `buscarPorTitulo`.
    * Abre el chat de AI o usa un comando de AI y pide: "Suggest improvements for this `buscarPorTitulo` method in Java Spring." o "Simplify this code block." (Selecciona el cuerpo del método).
    * Analiza las sugerencias de Cursor AI. ¿Te propone usar un bucle diferente? ¿Una forma más concisa de escribir la lógica?
3.  **Refactorización Asistida (Renombrar y Extraer Método):**
    * **Renombrar:** Si crees que el nombre de una variable o método no es claro, usa la función de renombrar de Cursor AI (selecciona el nombre y presiona Shift + F6). Cursor AI es consciente del contexto y puede sugerir nombres mejores o asegurarse de que se renombra en todas partes correctamente.
    * **Extraer Método:** Si un método (quizás `agregarLibro` con la validación) parece un poco largo, selecciona el bloque de código que realiza la validación. Haz clic derecho o busca en la paleta de comandos una opción como "Refactor" -> "Extract Method". Cursor AI puede asistirte en este proceso o sugerir si una extracción es apropiada.
    * **Pregunta a la IA sobre Refactorización:** "Cursor AI, should I extract a method from this part of `agregarLibro`? Why or why not?".

**Ejercicio Práctico:**

* Utiliza el chat de Cursor AI para buscar código duplicado en tu `LibroServiceImpl`.
* Selecciona el método `eliminarLibro` y pide a Cursor AI que sugiera mejoras. ¿Entiendes las sugerencias? ¿Son realmente una mejora?
* Practica la refactorización "Renombrar" en una variable local y observa cómo Cursor AI te ayuda.
* Considera si alguna parte de tu código de servicio actual podría beneficiarse de "Extraer Método" y pídele a Cursor AI su opinión.

**Qué esperamos lograr:** Que aprendas a utilizar las capacidades de análisis y sugerencia de Cursor AI para identificar áreas de mejora en tu código y que practiques refactorizaciones comunes para hacer tu código más limpio y robusto. Comprenderás que la IA es un asistente poderoso, pero la decisión final y la revisión del código son siempre tuyas.

---

### 5. Conclusiones y Preguntas Abiertas (10 minutos)

* **Recapitulación:** Hemos cubierto la importancia de la arquitectura por capas, practicado la depuración del flujo entre controlador y servicio, revisado cómo `@Autowired` y las interfaces promueven el bajo acoplamiento, y utilizado Cursor AI para diagnosticar y refactorizar nuestro código.
* **La Próxima Vez:** En aplicaciones reales, la capa de acceso a datos se conectaría a una base de datos, y nuestros servicios coordinarían las operaciones del repositorio.
* **Reflexión Final:** La depuración es una habilidad esencial. La arquitectura por capas es un patrón fundamental. Y herramientas como Cursor AI pueden potenciar enormemente tu productividad y ayudarte a escribir mejor código, pero siempre pensando críticamente en sus sugerencias.
* **Espacio para Preguntas:** Abre la palabra para cualquier duda que haya quedado.
