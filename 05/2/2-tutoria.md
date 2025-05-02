# Tutoría: Refuerzo en Servicios, Modularidad y Depuración Asistida

## Objetivos de la Sesión

El objetivo de hoy no es solo escribir código, sino **entender cómo se ejecuta**. Cuando una petición llega a tu aplicación Spring, ¿qué pasa exactamente? ¿En qué orden se llaman los métodos? ¿Qué datos se pasan de una capa a otra? Aprender a depurar te dará superpoderes para responder estas preguntas y solucionar problemas rápidamente.

## Actividad 1: Preparando la Depuración y Puntos de Ruptura (15 minutos)

**Concepto:** La depuración (debugging) es el proceso de ejecutar tu programa paso a paso para encontrar y corregir errores. Un **punto de ruptura (breakpoint)** es una marca que pones en una línea de código para decirle al depurador que detenga temporalmente la ejecución ahí.

**Por qué es importante:** Establecer puntos de ruptura en puntos clave (como la entrada de un controlador o el inicio de un método de servicio) nos permite "congelar" la aplicación y ver su estado interno en ese momento.

**Pasos a seguir:**

1. **Abre tu Proyecto:** Asegúrate de tener el proyecto Spring Boot abierto.
2. **Identifica Métodos Clave:** Piensa en una operación específica que quieras seguir, por ejemplo, agregar un libro (`POST /api/libros`). Identifica el método en el controlador que maneja esta petición (`LibroController.addLibro()`) y el método en el servicio al que llama (`LibroService.agregarLibro()`).
3. **Establece Puntos de Ruptura:**
    * Haz clic en el margen izquierdo de la línea donde comienza el cuerpo del método `addLibro` en `LibroController`. Debería aparecer un círculo rojo.
    * Haz clic en el margen izquierdo de la línea donde comienza el cuerpo del método `agregarLibro` en `LibroService`. Debería aparecer otro círculo rojo.
4. **Inicia la Aplicación en Modo Debug:** En la barra de herramientas superior, busca el botón de iniciar con un icono de "bicho" (bug) o que dice "Debug" y haz clic en él. La aplicación se iniciará, pero estará esperando peticiones.

**Qué esperamos lograr:** Que la aplicación se inicie correctamente en modo depuración y que los puntos de ruptura estén activos, indicando que el depurador pausará la ejecución en esas líneas cuando se alcancen.

## Actividad 2: Rastreo del Flujo de Ejecución con Controles de Depuración (30 minutos)

**Concepto:** Una vez que la ejecución se detiene en un punto de ruptura, usamos los controles de depuración para movernos por el código. Estos controles nos permiten ver exactamente el camino que sigue el programa.

**Por qué es importante:** Rastrear el flujo nos ayuda a confirmar que las llamadas a métodos entre capas (controlador -> servicio -> modelo) ocurren como esperamos y en el orden correcto.

**Pasos a seguir:**

1. **Realiza una Petición:** Usa una herramienta como Postman o `curl` para enviar una petición `POST` a `http://localhost:8080/api/libros` con un cuerpo JSON que represente un nuevo libro (por ejemplo: `{"isbn": "12345", "titulo": "Mi Nuevo Libro", "autor": "Un Autor"}`).
2. **La Ejecución se Detiene:** Observa cómo la ejecución se detiene en el primer punto de ruptura que estableciste en `LibroController.addLibro()`. La línea actual se resaltará.
3. **Usa los Controles de Depuración:**
    * **Step Over (F8):** Avanza línea por línea *dentro* del método actual (`addLibro`).
    * **Step Into (F7):** Usa F7 para "entrar" en el método `agregarLibro` del servicio. La ejecución ahora estará pausada dentro del método del servicio.
    * **Step Out (Shift+F8):** Si te adentras demasiado en código interno, usa esta opción para salir del método actual.
4. **Inspecciona Variables:** Mientras la ejecución está pausada, mira la ventana "Variables" en la interfaz de depuración. Observa los valores de las variables locales y verifica que los datos se pasaron correctamente del controlador al servicio.

**Ejercicio Práctico:**

* Depura el flujo de la petición `POST /api/libros` utilizando los controles de depuración para seguir la ejecución desde el controlador hasta el servicio.
* Repite el proceso para la petición `GET /api/libros/{titulo}` (llamando a `buscarPorTitulo`).

**Qué esperamos lograr:** Que te sientas cómodo usando los controles de depuración para seguir el flujo de ejecución y que puedas inspeccionar el estado de las variables en diferentes puntos.

## Actividad 3: Revisión del Uso de @Autowired y Principios de Bajo Acoplamiento (15 minutos)

**Concepto:** `@Autowired` es la forma en que Spring inyecta las dependencias. Usar inyección de dependencias y programar a interfaces promueve el **bajo acoplamiento**, lo que significa que los componentes dependen lo menos posible unos de otros.

**Por qué es importante:** El bajo acoplamiento hace que tu código sea más flexible, fácil de probar y más fácil de modificar en el futuro sin romper otras partes.

**Pasos a seguir:**

1. **Visualiza Dependencias:**
    * Haz clic derecho sobre el nombre de la clase `LibroController` en el editor y busca opciones como "Show References" o "Find All Usages".
2. **Analiza la Inyección:**
    * Observa que la dependencia es a la *interfaz* `LibroService`, no a la clase concreta `LibroServiceImpl`.
3. **Discute el Bajo Acoplamiento:** Imagina que decides cambiar tu implementación de `LibroService` para usar una base de datos real. Mientras la nueva clase implemente la interfaz `LibroService`, el `LibroController` no necesita cambiar.

**Ejercicio Práctico:**

* Identifica dónde se usa `LibroService`.
* Examina el constructor del `LibroController` y reflexiona sobre cómo `@Autowired` demuestra el bajo acoplamiento.

**Qué esperamos lograr:** Que comprendas cómo `@Autowired` establece conexiones entre tus beans y cómo programar a interfaces reduce el acoplamiento.

## Actividad 4: Diagnóstico y Simplificación con Herramientas de Análisis (30 minutos)

**Concepto:** Las herramientas de análisis de código pueden ayudarte a identificar "código sucio" (code smells) como duplicación o complejidad innecesaria.

**Por qué es importante:** Simplificar y refactorizar el código lo hace más fácil de leer, entender, mantener y extender.

**Pasos a seguir:**

1. **Identificar Código Duplicado:** Usa herramientas de análisis para buscar duplicación en tu proyecto.
2. **Obtener Sugerencias de Simplificación:** Selecciona un método y pide sugerencias de mejora.
3. **Refactorización Asistida:**
    * **Renombrar:** Usa la función de renombrar para mejorar la claridad de nombres.
    * **Extraer Método:** Si un método parece largo, selecciona un bloque de código y extrae un nuevo método.

**Ejercicio Práctico:**

* Busca código duplicado en tu `LibroServiceImpl`.
* Practica la refactorización "Renombrar" y "Extraer Método".

**Qué esperamos lograr:** Que aprendas a identificar áreas de mejora en tu código y practiques refactorizaciones comunes.

## Conclusiones y Preguntas Abiertas (10 minutos)

* **Recapitulación:** Hemos cubierto la importancia de la arquitectura por capas, practicado la depuración, revisado cómo `@Autowired` promueve el bajo acoplamiento y utilizado herramientas de análisis para mejorar el código.
* **La Próxima Vez:** Exploraremos cómo conectar la capa de acceso a datos a una base de datos.
* **Reflexión Final:** La depuración es una habilidad esencial. La arquitectura por capas es un patrón fundamental. Y las herramientas de análisis pueden potenciar tu productividad, pero siempre debes pensar críticamente en sus sugerencias.
* **Espacio para Preguntas:** Abre la palabra para cualquier duda que haya quedado.
