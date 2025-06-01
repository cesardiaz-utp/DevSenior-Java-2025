# Ejercicio Autónomo: Sistema de Gestión de Películas

## Objetivos del Ejercicio

Este ejercicio está diseñado para que **consolides y apliques** todos los conceptos clave que hemos visto hasta ahora en un proyecto real. Al finalizar, habrás demostrado tu habilidad para:

- **Persistencia de Datos Robusta**: Interactuar con una base de datos **PostgreSQL** utilizando **Spring Data JPA**, incluyendo el manejo de entidades y relaciones básicas.
- **Diseño API Profesional**: Crear **DTOs** eficientes para la transferencia de datos, aplicando validaciones de forma declarativa con **Spring Boot Validation** y personalizando el formato JSON con las potentes **anotaciones de Jackson**.
- **Desarrollo Ágil con Lombok**: Utilizar **Lombok** para reducir el código boilerplate en tus entidades y DTOs, mejorando la legibilidad y mantenibilidad.
- **Manejo de Errores Completo**: Implementar un sistema de manejo global de excepciones con `@ControllerAdvice` y `@ExceptionHandler` para interceptar y estandarizar las respuestas de error (errores de validación, recursos no encontrados, conflictos, errores inesperados).
- **API RESTful Completa**: Desarrollar controladores y servicios que expongan una **API RESTful** con operaciones CRUD (Crear, Leer, Actualizar, Borrar) y capacidades de filtrado.
- **Comunicación HTTP Precisa**: Utilizar `ResponseEntity` en tus controladores para devolver códigos de estado HTTP exactos y cuerpos de respuesta controlados, asegurando que tu API se comunique de forma clara y estándar.

## Instrucciones Detalladas

Crea una API RESTful que te permita gestionar un catálogo de películas. Debes implementar una **Entidad** `Movie`, su `Repository`, un `Service` con lógica de negocio, un `Controller` REST, **DTOs** específicos para peticiones y respuestas, y tu `GlobalExceptionHandler` personalizado.

### Paso 1: Configuración del Proyecto y Dependencias

1. Crea un nuevo proyecto **Spring Boot** (versión 3.4.5 recomendada) usando Spring Initializr o tu IDE.
2. Añade las siguientes dependencias a tu `pom.xml`:
    - `spring-boot-starter-web`: Para desarrollar APIs REST (incluye Jackson).
    - `spring-boot-starter-data-jpa`: Para interactuar con bases de datos relacionales.
    - `spring-boot-starter-validation`: Para las validaciones de DTOs.
    - `org.postgresql:postgresql`: El driver de PostgreSQL.
    - `org.projectlombok:lombok`: Para reducir el boilerplate.
    - `spring-boot-starter-test`: Para pruebas (ya viene por defecto).
    - `org.springdoc:springdoc-openapi-starter-webmvc-ui`: Para generar la documentación de Swagger UI y probar tu API fácilmente.
3. Configura `src/main/resources/application.properties` para PostgreSQL:
    - Asegúrate de tener una base de datos PostgreSQL creada (ej., `moviemanagementdb`) y un usuario/contraseña.

      ```Properties
      # Configuración de PostgreSQL
      spring.datasource.url=jdbc:postgresql://localhost:5432/moviemanagementdb
      spring.datasource.username=tu_usuario_postgres
      spring.datasource.password=tu_contrasena_postgres
      spring.datasource.driverClassName=org.postgresql.Driver

      # Configuración de JPA/Hibernate
      spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
      spring.jpa.hibernate.ddl-auto=update # 'update' para actualizar esquema, 'create' para recrear cada vez
      spring.jpa.show-sql=true # Para ver las sentencias SQL en consola

      ```

### Paso 2: Definición de la Entidad Movie

1. Crea un paquete `model.entity`.
2. Dentro, define la clase `Movie` utilizando **Lombok** para getters/setters/constructores:
    - Anótala con `@Entity` y `@Table(name = "movies")`.
    - Utiliza `@Data`, `@NoArgsConstructor`, `@AllArgsConstructor` de Lombok.
    - **Atributos**:
      - `id`: `Long`, autogenerado (`@Id`, `@GeneratedValue(strategy = GenerationType.IDENTITY)`).
      - `title`: `String`, no nulo, único (`@Column(nullable = false, unique = true)`).
      - `director`: `String`, no nulo.
      - `genre`: `String`, no nulo.
      - `releaseYear`: `Integer`, no nulo.
      - `imdbRating`: `Double`, puede ser nulo (es opcional).

### Paso 3: Creación de DTOs con Validaciones y Jackson

1. Crea un paquete `model.dto`.
2. `MovieRequestDTO` (Para creación y actualización):
    - Representa los datos que un cliente enviará para crear o modificar una película.
    - Utiliza `@Data`, `@NoArgsConstructor`, `@AllArgsConstructor` de **Lombok**.
    - **Atributos y Validaciones (Spring Boot Validation)**:
      - `title`: `@NotBlank(message = "El título es obligatorio."), @Size(min = 2, max = 255, message = "El título debe tener entre 2 y 255 caracteres.")`.
      - `director`: `@NotBlank(message = "El director es obligatorio."), @Size(min = 2, max = 100, message = "El director debe tener entre 2 y 100 caracteres.")`.
      - `genre`: `@NotBlank(message = "El género es obligatorio."), @Pattern(regexp = "^(Action|Comedy|Drama|Horror|Sci-Fi|Thriller|Animation|Documentary)$", message = "Género no válido. Debe ser uno de los predefinidos.")`.
      - `releaseYear`: `@NotNull(message = "El año de lanzamiento es obligatorio."), @Min(value = 1888, message = "El año de lanzamiento no puede ser anterior a 1888 (inicio del cine)."), @Max(value = 2025, message = "El año de lanzamiento no puede ser posterior al año actual (2025).")`.
      - `imdbRating`: `@Min(value = 1.0, message = "La calificación IMDb debe ser al menos 1.0."), @Max(value = 10.0, message = "La calificación IMDb no puede exceder 10.0.")`. (No `@NotNull` para que sea opcional).
    - **Anotaciones Jackson**:
      - A nivel de clase: `@JsonInclude(JsonInclude.Include.NON_NULL)` (para que campos nulos no se incluyan en el JSON).
      - En el atributo `releaseYear`: `@JsonProperty("year_released")`.
      - En el atributo imdbRating: `@JsonProperty("imdb_score")`.
3. `MovieResponseDTO` (Para respuestas):
    - Representa los datos que tu API enviará de vuelta al cliente.
    - Utiliza `@Data`, `@NoArgsConstructor`, `@AllArgsConstructor` de **Lombok**.
    - **Atributos**: `id`, `title`, `director`, `genre`, `releaseYear`, `imdbRating`.
    - **Anotaciones Jackson**:
      - A nivel de clase: `@JsonInclude(JsonInclude.Include.NON_NULL)`.
      - En los atributos `releaseYear` y `imdbRating`: Las mismas @`JsonProperty` (`year_released`, `imdb_score`) para consistencia.
    - Crea un constructor que reciba una entidad `Movie` y mapee sus campos a este DTO.

### Paso 4: Interfaz del Repositorio `MovieRepository` (Spring Data JPA)

1. Crea un paquete `repository`.
2. Define la interfaz `MovieRepository`:
    - Extiende `JpaRepository<Movie, Long>`.
    - Añade métodos de consulta personalizados (Spring Data JPA los implementa automáticamente):
      - `List<Movie> findByDirectorContainingIgnoreCase(String director);`
      - `List<Movie> findByGenreIgnoreCase(String genre);`
      - `Optional<Movie> findByTitle(String title);` (Necesario para verificar títulos duplicados).

### Paso 5: Lógica de Negocio en el Servicio `MovieService`

1. Crea un paquete `service`.
2. Define la **interface** `MovieService`

    ```Java
    public interface MovieService {
        MovieResponseDTO createMovie(MovieRequestDTO movieDto);
        MovieResponseDTO getMovieById(Long id);
        List<MovieResponseDTO> getAllMovies(String director, String genre);
        MovieResponseDTO updateMovie(Long id, MovieRequestDTO movieDto);
        void deleteMovie(Long id);
    }
    ```

3. Crea un paquete `service.impl`.
4. Define la **clase** `DefaultMovieService` que implementa `MovieService`:
    - Anótala con `@Service`.
    - Inyecta `MovieRepository` (se recomienda inyección por constructor).
    - **Métodos**:
      - `MovieResponseDTO createMovie(MovieRequestDTO movieDto)`:
        - Primero, verifica si ya existe una película con el mismo título usando `movieRepository.findByTitle(movieDto.getTitle())`. Si existe, **lanza una** `MovieAlreadyExistsException`.
        - Mapea `movieDto` a entidad `Movie`, guarda, mapea entidad guardada a `MovieResponseDTO` y retorna.
      - `MovieResponseDTO getMovieById(Long id)`:
        - Busca por ID. Si no encuentra la película, lanza una `ResourceNotFoundException`.
      - `List<MovieResponseDTO> getAllMovies(String director, String genre)`:
        - Si solo `director` está presente (no es `null` ni vacío), usa `findByDirectorContainingIgnoreCase`.
        - Si solo `genre` está presente, usa `findByGenreIgnoreCase`.
        - Si ambos están presentes, puedes priorizar uno (ej., `director`) o si no hay ninguno, retorna todas las películas.
        - Mapea la lista de entidades `Movie` a una lista de `MovieResponseDTO` y retorna.
      - `MovieResponseDTO updateMovie(Long id, MovieRequestDTO movieDto)`:
        - Busca por ID (lanza `ResourceNotFoundException` si no existe).
        - Actualiza los campos de la entidad `Movie` con los datos del `movieDto`.
        - Guarda la entidad actualizada, y retorna el `MovieResponseDTO` actualizado.
      - `void deleteMovie(Long id)`:
        - Busca por ID (lanza `ResourceNotFoundException` si no existe).
        - Elimina la película.

### Paso 6: Excepciones Personalizadas

1. Crea un paquete `exception`.
2. `ResourceNotFoundException`:
    - Extiende `RuntimeException`.
    - Constructor que reciba un `message`.
3. `MovieAlreadyExistsException`:
    - Extiende `RuntimeException`.
    - Constructor que reciba un `message`.

### Paso 7: Manejo Global de Excepciones (Centralizado)

1. Crea un paquete `config.error`.
2. `ApiErrorResponse`: (Usa la misma clase que vimos en clase, con `timestamp`, `status`, `error`, `message`, `path` y `@JsonFormat` en `timestamp`).
3. `GlobalExceptionHandler`:
    - Anótala con `@ControllerAdvice`.
    - Inyecta `HttpServletRequest` si es necesario para obtener el `path` de la solicitud.
    - Implementa los siguientes `@ExceptionHandler` métodos, que deben retornar `ResponseEntity<ApiErrorResponse>`:
      - `handleValidationErrors(MethodArgumentNotValidException ex, HttpServletRequest request)`:
        - Captura errores de `@Valid`.
        - Extrae todos los `FieldError` del `BindingResult` de la excepción.
        - Consolida los mensajes de error en un único string.
        - Retorna `400 Bad Request` con un `ApiErrorResponse` detallado.
      - `handleResourceNotFoundException(ResourceNotFoundException ex, HttpServletRequest request)`:
        - Captura `ResourceNotFoundException`.
        - Retorna `404 Not Found` con un `ApiErrorResponse` que contenga el mensaje de la excepción.
      - `handleMovieAlreadyExistsException(MovieAlreadyExistsException ex, HttpServletRequest request)`:
        - Captura `MovieAlreadyExistsException`.
        - Retorna `409 Conflict` con un `ApiErrorResponse` que contenga el mensaje de la excepción.
      - `handleGenericException(Exception ex, HttpServletRequest request)`:
        - El "catch-all" para cualquier otra excepción inesperada.
        - **IMPORTANTE**: Este método debe loggear el stack trace completo de la excepción en la consola (ex.printStackTrace() o un logger profesional).
        - Retorna `500 Internal Server Error` con un `ApiErrorResponse` que contenga un mensaje genérico y seguro para el cliente (ej., "Ha ocurrido un error inesperado en el servidor. Por favor, intente más tarde.").

### Paso 8: Controlador REST MovieController

1. Crea un paquete `controller`.
2. Define la clase `MovieController`:
    - Anótala con `@RestController` y `@RequestMapping("/api/movies")`.
    - Inyecta `MovieService`.
    - **Endpoints**:
      - `POST /api/movies`:
        - **Método**: `createMovie(@Valid @RequestBody MovieRequestDTO movieDto)`
        - **Retorno**: `ResponseEntity<MovieResponseDTO>` con `HttpStatus.CREATED`.
      - `GET /api/movies/{id}`:
        - **Método**: `getMovieById(@PathVariable Long id)`
        - **Retorno**: `ResponseEntity<MovieResponseDTO>` con `HttpStatus.OK`.
      - `GET /api/movies`:
        - **Método**: `getAllMovies(@RequestParam(required = false) String director, @RequestParam(required = false) String genre)`
        - **Retorno**: `ResponseEntity<List<MovieResponseDTO>>` con `HttpStatus.OK`.
      - `PUT /api/movies/{id}`:
        - **Método**: `updateMovie(@PathVariable Long id, @Valid @RequestBody MovieRequestDTO movieDto)`
        - **Retorno**: `ResponseEntity<MovieResponseDTO>` con `HttpStatus.OK`.
      - `DELETE /api/movies/{id}`:
        - **Método**: `deleteMovie(@PathVariable Long id)`
        - **Retorno**: `ResponseEntity<Void>` con `HttpStatus.NO_CONTENT`.

### Paso 9: Documentar OpenAPI (Swagger) adecuadamente todos las clases que sea necesario

## Criterios de Evaluación (Autónoma)

Una vez que hayas terminado tu implementación, inicia la aplicación y utiliza Swagger UI (`<http://localhost:8080/swagger-ui.html>`) o un cliente REST (Postman, Insomnia) para verificar cada uno de los siguientes puntos.

- ¿Puedes crear una película con datos válidos y recibir un `201 Created?`
- ¿Puedes obtener una película por su ID y recibir un `200 OK`?
- ¿Puedes obtener todas las películas y recibir un `200 OK` con la lista esperada?
- ¿Puedes actualizar una película existente con datos válidos y recibir un `200 OK`?
- ¿Puedes eliminar una película existente y recibir un `204 No Content`?
- ¿Al intentar crear o actualizar una película con datos inválidos (ej., título vacío, año fuera de rango, género no permitido), recibes un `400 Bad Request`?
- ¿El cuerpo de la respuesta es un `ApiErrorResponse` con un message claro que detalla los errores de validación (`title: El título es obligatorio.; genre: Género no válido.`)?
- ¿Al intentar obtener, actualizar o eliminar una película con un ID que no existe, recibes un `404 Not Found`?
- ¿El cuerpo de la respuesta es un `ApiErrorResponse` con un message que indica "Película con ID X no encontrada."?
- ¿Al intentar crear una película con un título que ya existe, recibes un `409 Conflict`?
- ¿El cuerpo de la respuesta es un `ApiErrorResponse` con un message que indica el conflicto (ej., "La película con título 'Inception' ya existe.")?
- ¿Si causas una excepción inesperada (ej., temporalmente inserta `throw new RuntimeException("Simulated unexpected error");` en tu servicio o controlador y envía una petición válida), recibes un `500 Internal Server Error`?
- ¿El cuerpo de la respuesta es un `ApiErrorResponse` con un mensaje genérico (ej., "Ha ocurrido un error inesperado...")?
- **Crucial**: ¿En la consola de tu IDE, se imprime el _stack trace_ completo de la `RuntimeException` simulada? (¡No olvides quitar la línea `throw new RuntimeException();` después de probar!)
- ¿Puedes obtener películas filtrando solo por `director` (ej., `GET /api/movies?director=Nolan`) y recibir el resultado correcto?
- ¿Puedes obtener películas filtrando solo por `genre` (ej., GET `/api/movies?genre=Sci-Fi`) y recibir el resultado correcto?
- ¿Los nombres de los campos en el JSON de tus peticiones y respuestas (`year_released`, `imdb_score`) coinciden con lo definido en `@JsonProperty`?
- ¿Los campos opcionales (`imdb_score`) no aparecen en el JSON de respuesta cuando son `null` (gracias a `@JsonInclude`)?
- ¿Has utilizado las anotaciones de Lombok (`@Data`, etc.) para mantener tus entidades y DTOs concisos?
- ¿Tu código está organizado en las capas correctas (`controller`, `service`, `repository`, `model`, `exception`, `config`)?
- ¿El código es legible y fácil de entender?

---

¡Felicidades por llegar hasta aquí! Este ejercicio es un paso gigantesco en tu camino para convertirte en un desarrollador Spring Boot de alto nivel. Integrar la base de datos PostgreSQL, las validaciones robustas, la personalización de datos y el manejo de errores de forma global y elegante, es lo que diferencia una API funcional de una API profesional.

Cada vez que te encuentres con un error, no lo veas como un problema, sino como una oportunidad para aprender y depurar como un experto. Has construido las herramientas para entender qué sucede en tu aplicación, y eso es un superpoder.

**¡Sigue adelante con esa misma curiosidad y determinación! Estás construyendo habilidades increíbles que te abrirán muchas puertas. ¡El futuro de tus APIs es prometedor!**
