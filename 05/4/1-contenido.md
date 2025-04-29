# Proyecto API RESTful Completa con Spring Boot

## El Proyecto: API RESTful para Gestión de Cursos
Su objetivo para consolidar los conocimientos de esta clase y las anteriores es construir una API RESTful completa para la gestión de Cursos.

* **Entidad Principal**: El Curso

    Su API debe manejar la información de Cursos. Un Curso debe tener al menos los siguientes atributos:
    * Un identificador único (ID).
    * Nombre del curso.
    * Código del curso (que idealmente debería ser único).
    * Una descripción.
    * Fecha de inicio.
    * Fecha de fin.
    * Número de créditos.

* **Operaciones RESTful (CRUD + Búsqueda)**

    La API debe exponer endpoints REST para las operaciones típicas sobre los recursos "Curso":
    * `GET /api/cursos`: Obtener la lista completa de todos los cursos.
    * `GET /api/cursos/{id}`: Obtener los detalles de un curso específico por su ID.
    * `GET /api/cursos/buscar?nombre={nombre}`: Buscar cursos cuyo nombre contenga un texto dado.
    * `POST /api/cursos`: Crear un nuevo curso.
    * `PUT /api/cursos/{id}`: Actualizar la información de un curso existente identificado por su ID.
    * `DELETE /api/cursos/{id}`: Eliminar un curso por su ID.

* **Integración de Capas**
    
    Deberán aplicar la arquitectura por capas vista:
    * Un Controlador (`@RestController`) para manejar las peticiones HTTP.
    * Una Capa de Servicio (`@Service`) con la lógica de negocio para los Cursos.
    * Una Capa de Persistencia (`@Repository`), que puede ser una lista en memoria para este ejercicio o, idealmente, usar Spring Data JPA con una base de datos en memoria como H2 para simular la persistencia real. Deberán inyectar las dependencias correctamente entre capas (@Autowired, constructor injection).

* **Validación de Datos (Tema clave de la clase)**
    
    La API debe validar que los datos de entrada al crear o actualizar un curso son correctos (ej. nombre no vacío, créditos positivos, fechas válidas).

* **Documentación Automática (Tema clave de la clase)**
    
    La API debe generar documentación automática para que sea fácil de entender y usar por otros desarrolladores.

**Su desafío es construir esta API integrando todos estos elementos, aplicando los conceptos que explicaremos a continuación.**

---

## Validación con `javax.validation` (Bean Validation)
* **¿Por Qué Validar los Datos que Recibimos?**

    Imaginen que su API recibe datos directamente del exterior (una aplicación web, móvil, otro servicio). ¡No pueden confiar ciegamente en que esos datos sean correctos! Validar los datos de entrada es fundamental para:
    1. **Garantizar la Integridad**: Asegurar que solo se guardan datos válidos y consistentes en su sistema (ej. base de datos). Un curso debe tener un nombre, no puede tener créditos negativos, etc.
    1. **Aumentar la Seguridad**: Prevenir que datos mal formados o maliciosos causen errores inesperados, corrompan datos o abran agujeros de seguridad (ej. inyecciones).
    1. **Mejorar la Usabilidad de la API**: Dar un feedback claro e inmediato al cliente de la API cuando envía datos incorrectos, en lugar de que la petición falle más adelante en el proceso con un error confuso.

* **Bean Validation: El Estándar en Java**

    Java tiene un estándar para la validación de beans (clases Java). Spring Boot se integra perfectamente con este estándar (si añaden la dependencia de validación a su proyecto). La idea es simple y poderosa: definen reglas de validación usando **anotaciones directamente en los atributos de sus clases Java**.

* **Anotaciones de Validación Comunes** (`jakarta.validation.constraints.*` o `javax.validation.constraints.*`)

    Aplicarán estas anotaciones a los atributos de la clase que representa los datos que esperan recibir en el cuerpo de una petición POST o PUT (en nuestro caso, la clase `Curso`). Pueden añadir un mensaje personalizado a la mayoría de las anotaciones usando el atributo `message`.

    * `@NotNull`: Asegura que el atributo no sea `null`. Útil para objetos o tipos envoltorio (`Integer`, `Boolean`, `String`, `LocalDate`).
    * `@NotEmpty`: Asegura que una cadena, colección o array no sea `null` ni esté vacía. (Para String, requiere al menos un carácter).
    * `@NotBlank`: Asegura que una cadena no sea `null`, ni esté vacía, y que contenga al menos un carácter que no sea espacio en blanco. Ideal para campos de texto obligatorios.
    * `@Size(min=X, max=Y)`: Valida el tamaño de una cadena, colección o array. Pueden especificar un tamaño mínimo y/o máximo.
        * Ejemplo: `@Size(min = 3, max = 100, message = "El nombre debe tener entre 3 y 100 caracteres")`
    * `@Min(value)`: Valida que un valor numérico (byte, short, int, long, float, double, BigDecimal, BigInteger y sus envoltorios) sea mayor o igual que `value`.
        * Ejemplo: `@Min(value = 1, message = "Los créditos deben ser al menos 1")`
    * `@Max(value)`: Valida que un valor numérico sea menor o igual que value.
        * Ejemplo: `@Max(value = 30, message = "Un curso no puede tener más de 30 créditos")`
    * `@Email`: Valida que una cadena tiene formato de email válido.
    * `@Pattern(regexp="...")`: Valida que una cadena cumple con una expresión regular específica. Muy flexible para formatos personalizados (códigos postales, números de teléfono, códigos de curso con formato especial).
        * Ejemplo: `@Pattern(regexp = "^[A-Z]{2,4}\\d{3}$", message = "Formato de código de curso inválido (ej. CS101)")`
    * Anotaciones para Fechas/Tiempo:
        * `@Future`: La fecha/tiempo debe ser en el futuro.
        * `@Past`: La fecha/tiempo debe ser en el pasado.
        * `@FutureOrPresent`: La fecha/tiempo debe ser en el futuro o presente.
        * `@PastOrPresent`: La fecha/tiempo debe ser en el pasado o presente.
        * Ejemplo: 
            * `@NotNull(message = "La fecha de inicio no puede ser nula")`
            * `@FutureOrPresent(message = "La fecha de inicio debe ser en el presente o futuro")`

* **Activando la Validación en sus Controladores** (`@Valid`)

    Para que Spring ejecute estas validaciones definidas con anotaciones en su clase `Curso` cuando llega una petición HTTP, necesitan añadir la anotación `@Valid` al parámetro del método del controlador que recibe el cuerpo de la petición (`@RequestBody`).

    Por ejemplo, en su endpoint `POST` para crear un curso:

    ```java

    // En su CursoController.java
    import jakarta.validation.Valid; // ¡Importa esta anotación!
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestBody;
    // ... otros imports ...

    // ... dentro de la clase CursoController ...

    @PostMapping("/api/cursos") // Ejemplo de mapeo
    public Curso createCurso(@Valid @RequestBody Curso curso) { // <-- @Valid aquí activa la validación
        // Si el objeto 'curso' recibido NO cumple las anotaciones @NotBlank, @Size, etc.
        // definidas en la clase Curso, Spring automáticamente detendrá la ejecución
        // antes de que esta línea se ejecute y devolverá un error 400 Bad Request
        // al cliente de la API.
        return cursoService.save(curso); // Esto solo se ejecuta si la validación PASA
    }
    ```

    Deberían hacer lo mismo en su endpoint `PUT` para actualizar un curso.

* **Manejo de Errores de Validación**

    Por defecto, si la validación falla debido a `@Valid`, Spring lanza una excepción (`MethodArgumentNotValidException`). Spring Boot la captura por ustedes y devuelve una respuesta HTTP 400 Bad Request estándar al cliente, incluyendo detalles sobre qué campos fallaron la validación y por qué (los mensajes que definieron). Para una API más profesional, podrían personalizar este manejo de errores usando `@ControllerAdvice`.

* **Asistencia de Cursor AI:**

    * **Sugerir Anotaciones**: Mientras definen los atributos en su clase `Curso`, describan los requisitos (ej. "Este campo no puede estar vacío", "Este número debe ser positivo"). Cursor AI les sugerirá las anotaciones de validación apropiadas.
    * **Añadir** `@Valid`: Cuando escriban un método de controlador que usa `@RequestBody`, Cursor AI a menudo les recordará añadir `@Valid`.
    * **Explicar Errores**: Si se encuentran con un error de validación al probar su API, pueden pegar el error en el chat de Cursor AI y pedirle que les explique por qué ocurrió.

---

## Documentación Automática con Springdoc OpenAPI (Swagger)

* **¿Por Qué Documentar su API?**

    Una API sin documentación es casi inútil para otros desarrolladores. La documentación explica:
    * Qué recursos están disponibles.
    * Qué URIs y verbos HTTP usar para cada operación.
    * Qué parámetros espera cada endpoint (path variables, query params, request body).
    * Cuál es la estructura esperada del cuerpo de la petición.
    * Cuál es la estructura de la respuesta exitosa y los posibles códigos de error.

    Una buena documentación es esencial para la adopción y el uso correcto de su API.

* **OpenAPI Specification (Swagger) y Springdoc**

    **OpenAPI Specification** (antes conocido como Swagger Specification) es un estándar universal para describir APIs RESTful de forma que sean legibles tanto por humanos como por máquinas.

    **Springdoc OpenAPI** es una librería que se integra con Spring Boot para **generar automáticamente la documentación de su API** en formato OpenAPI, basándose en su código (controladores, modelos, anotaciones). También proporciona una interfaz gráfica muy útil, Swagger UI, para visualizar y probar su API directamente desde el navegador.

* **Configuración: ¡Es Muy Fácil!**

    Solo necesitan añadir una dependencia a su proyecto Spring Boot.

    Si usan Maven (archivo `pom.xml`):

    ```xml
    <dependency>
        <groupId>org.springdoc</groupId>
        <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
        <version>2.8.5</version>
    </dependency>
    ```
    
    Si usan Gradle (archivo `build.gradle`):
    ```groovy
    implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.8.5' // Usen la versión más reciente
    ```

    Después de añadir la dependencia y reconstruir el proyecto (recargar en Maven/Gradle en su IDE), ejecuten su aplicación Spring Boot.

    **¡Listo!** Su documentación se generará automáticamente y podrán acceder a ella en:
    * http://localhost:8080/v3/api-docs (formato JSON o YAML)
    * http://localhost:8080/swagger-ui.html (la interfaz gráfica de Swagger UI)

* **Anotaciones de Documentación** (`io.swagger.v3.oas.annotations.*`)

    Aunque Springdoc genera mucha documentación basándose solo en su código (rutas, verbos, parámetros, modelos), pueden enriquecerla y hacerla más clara usando anotaciones específicas de Swagger en su controlador y modelos.

    * `@Tag`: Usada en una clase `@RestController` para agrupar los endpoints relacionados en Swagger UI.

        ```java
        import io.swagger.v3.oas.annotations.tags.Tag;
        // ...
        @RestController
        @RequestMapping("/api/cursos")
        @Tag(name = "Cursos", description = "API para la gestión de cursos académicos") // Documenta este controlador
        public class CursoController { ... }
        ```

    * `@Operation`: Usada en un método handler de controlador para describir una operación HTTP específica. Proporciona un resumen y una descripción detallada.

        ```java
        import io.swagger.v3.oas.annotations.Operation;
        // ...
        @GetMapping("/{id}")
        @Operation(summary = "Obtener un curso por su ID", description = "Retorna la información detallada de un curso específico")
        public ResponseEntity<Curso> getCursoById(@PathVariable Long id) { ... }
        ```

    * `@Parameter`: Usada para añadir detalles a un parámetro en la firma de un método handler (`@PathVariable`, `@RequestParam`, etc.).

        ```java
        import io.swagger.v3.oas.annotations.Parameter;

        @GetMapping("/{id}")
        @Operation(summary = "Obtener curso por ID")
        public ResponseEntity<Curso> getCursoById(
            @Parameter(description = "ID del curso a buscar") // Documenta el parámetro 'id'
            @PathVariable Long id) { ... }
        ```

    * `@RequestBody`: Usada en el parámetro `@RequestBody` de un método handler para describir el cuerpo de la petición esperado. Pueden describir su propósito, si es requerido y referenciar el esquema del modelo (`@Schema`).

        ```java
        import io.swagger.v3.oas.annotations.parameters.RequestBody;
        import io.swagger.v3.oas.annotations.media.Content;
        import io.swagger.v3.oas.annotations.media.Schema;
        // ...
        @PostMapping
        @Operation(summary = "Crear un nuevo curso")
        public Curso createCurso(
            @RequestBody(description = "Datos del nuevo curso a crear", required = true, // Describe el cuerpo
                        content = @Content(schema = @Schema(implementation = Curso.class))) // Referencia el esquema del modelo Curso
            @Valid @org.springframework.web.bind.annotation.RequestBody Curso curso) { ... } // @RequestBody de Spring
        ```

    * `@ApiResponses` y `@ApiResponse`: Usadas para documentar las posibles respuestas HTTP que puede devolver una operación (códigos de estado como 200, 201, 400, 404, 500) y describir qué significan y qué contenido devuelven. Se aplica a nivel de método.

        ```java
        import io.swagger.v3.oas.annotations.responses.ApiResponse;
        import io.swagger.v3.oas.annotations.responses.ApiResponses;
        import io.swagger.v3.oas.annotations.media.Content;
        import io.swagger.v3.oas.annotations.media.Schema;
        // ...
        @GetMapping("/{id}")
        @Operation(summary = "Obtener curso por ID")
        @ApiResponses(value = { // Define las posibles respuestas para este endpoint
            @ApiResponse(responseCode = "200", description = "Curso encontrado exitosamente",
                        content = @Content(schema = @Schema(implementation = Curso.class))), // Describe el 200 OK
            @ApiResponse(responseCode = "404", description = "Curso no encontrado - El ID proporcionado no existe"), // Describe el 404 Not Found
            @ApiResponse(responseCode = "400", description = "Solicitud inválida - Por ejemplo, si el ID no es numérico") // Pueden añadir este si manejan errores de formato de ID
            // Si usan @Valid y manejan errores de validación, podrían añadir un 400 específico aquí
        })
        public ResponseEntity<Curso> getCursoById(@PathVariable Long id) { ... }
        ```

    * `@Schema`: Usada en la clase modelo (ej. Curso) o en DTOs para añadir descripciones a sus propiedades. Ayuda a Swagger UI a mostrar la estructura de los datos.

        ```java
        package com.example.apicursos.model;

        import io.swagger.v3.oas.annotations.media.Schema; // Importar anotación
        // ... otros imports y anotaciones JPA/Validation ...

        @Entity
        @Table(name = "cursos")
        @Schema(description = "Representa un curso académico") // Describe la entidad a nivel general
        public class Curso {

            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            @Schema(description = "Identificador único generado automáticamente", example = "101") // Describe el campo ID
            private Long id;

            @NotBlank
            @Size(min = 3, max = 100)
            @Schema(description = "Nombre completo del curso", example = "Programación Avanzada en Java") // Describe el nombre
            private String nombre;

            @NotBlank
            @Size(min = 2, max = 10)
            @Schema(description = "Código único del curso", example = "PROG401") // Describe el código
            private String codigo;

            // ... otras propiedades con @Schema ...

            // ... Constructores, Getters, Setters, toString ...
        }
        ```
    * **Asistencia de Cursor AI**: Si escriben @Operation antes de un método, Cursor AI a menudo les sugerirá automáticamente añadir atributos comunes como summary y description. Si escriben @Parameter o @RequestBody, les sugerirá atributos como description y required. Si seleccionan una clase modelo, pueden pedirle a Cursor AI que les ayude a añadir anotaciones @Schema a los campos existentes o generar ejemplos de valores para el example atributo. Pueden preguntar a Cursor AI el propósito de cualquier anotación de Swagger.

---

## Refactorización Final y Pulido con IA

* **Importancia de una Revisión Final:**

    Una vez que la funcionalidad (CRUD), validación y documentación básica están implementadas, es una buena práctica revisar y pulir el código para asegurar su calidad a largo plazo.

* **Refactorización y Limpieza con Asistencia de Cursor AI:**

    Cursor AI es una herramienta valiosa en este paso:

    * **Análisis General**: Seleccionen una clase completa (ej. su `CursoService` o `CursoController`) y usen el chat de AI (Cmd/Ctrl + L): "Review this code for potential code smells and suggest improvements for clarity and maintainability."
    * **Simplificar Lógica**: Si un método les parece muy largo o complejo, seleccionen el código y pidan: "Simplify this Java code block." o "Suggest refactoring options for this method."
    * **Mejorar Nombres**: Utilicen la función de renombrar de Cursor AI (Shift + F6). Pídanle a la IA sugerencias si no están seguros de un buen nombre.
    * **Generar Comentarios/Javadoc**: Si hay lógica que no es obvia o métodos importantes, pídanle a Cursor AI que genere comentarios o Javadoc para ellos.
    * **Detectar Duplicados**: Usen la función de análisis de código de Cursor AI para encontrar fragmentos de código duplicado que podrían ser consolidados.
    * **Validar Documentación**: Revisen su Swagger UI. Si algo no se ve bien o falta información, identifiquen la sección en su código y pídanle a Cursor AI que les ayude a añadir o corregir la anotación de Swagger correspondiente.

--

## Conclusión y Próximos Pasos

Han aprendido dos aspectos fundamentales para construir APIs RESTful robustas: la **validación de datos** para asegurar la calidad y seguridad, y la **documentación automática** para hacer su API usable por otros.

Ahora, el desafío es integrar estos conceptos con su conocimiento previo de Spring Boot para completar el proyecto de la **API RESTful para Gestión de Cursos**.
* Apliquen la arquitectura por capas (Controlador -> Servicio -> Repositorio).
* Implementen las operaciones CRUD + búsqueda en esas capas.
* Añadan validaciones a la entidad `Curso` y actívenlas en el controlador usando `@Valid`.
* Configuren Springdoc OpenAPI y añadan anotaciones (`@Tag`, `@Operation`, `@Parameter`, `@RequestBody`, `@ApiResponses`, `@Schema`) para documentar su API.
* Usen Cursor AI en cada paso para asistir en la codificación, validación y documentación.
* Realicen una revisión final y pulan su código con la ayuda de la IA.

¡Este proyecto les permitirá integrar gran parte de lo que hemos visto hasta ahora y les dará una base sólida para construir APIs reales!
