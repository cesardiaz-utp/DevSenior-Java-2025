# Proyecto API RESTful Completa con Spring Boot

## 1. El Proyecto: API RESTful para Gestión de Cursos

El objetivo de este proyecto es consolidar los conocimientos adquiridos en esta clase y las anteriores mediante la construcción de una API RESTful completa para la gestión de cursos.

### 1.1 Entidad Principal: El Curso

La API debe manejar la información de los cursos. Cada curso debe tener los siguientes atributos:

* Un identificador único (ID).
* Nombre del curso.
* Código del curso (que idealmente debería ser único).
* Una descripción.
* Fecha de inicio.
* Fecha de fin.
* Número de créditos.

### 1.2 Operaciones RESTful (CRUD + Búsqueda)

La API debe exponer los siguientes endpoints REST para las operaciones sobre los recursos "Curso":

* `GET /api/cursos`: Obtener la lista completa de todos los cursos.
* `GET /api/cursos/{id}`: Obtener los detalles de un curso específico por su ID.
* `GET /api/cursos/buscar?nombre={nombre}`: Buscar cursos cuyo nombre contenga un texto dado.
* `POST /api/cursos`: Crear un nuevo curso.
* `PUT /api/cursos/{id}`: Actualizar la información de un curso existente identificado por su ID.
* `DELETE /api/cursos/{id}`: Eliminar un curso por su ID.

### 1.3 Integración de Capas

Se debe aplicar la arquitectura por capas vista en clase:

* **Controlador** (`@RestController`): Maneja las peticiones HTTP.
* **Capa de Servicio** (`@Service`): Contiene la lógica de negocio para los cursos.
* **Capa de Persistencia** (`@Repository`): Puede ser una lista en memoria o, idealmente, usar Spring Data JPA con una base de datos en memoria como H2 para simular la persistencia real. Las dependencias entre capas deben inyectarse correctamente (`@Autowired`, inyección por constructor).

### 1.4 Validación de Datos

La API debe validar que los datos de entrada al crear o actualizar un curso sean correctos (por ejemplo, nombre no vacío, créditos positivos, fechas válidas).

### 1.5 Documentación Automática

La API debe generar documentación automática para facilitar su comprensión y uso por otros desarrolladores.

**Su desafío es construir esta API integrando todos estos elementos, aplicando los conceptos que explicaremos a continuación.**

## 2. Validación con `javax.validation` (Bean Validation)

### 2.1 ¿Por Qué Validar los Datos que Recibimos?

Validar los datos de entrada es fundamental para:

1. **Garantizar la Integridad**: Asegurar que solo se guarden datos válidos y consistentes.
2. **Aumentar la Seguridad**: Prevenir errores inesperados o agujeros de seguridad.
3. **Mejorar la Usabilidad de la API**: Proporcionar feedback claro e inmediato al cliente de la API.

### 2.2 Bean Validation: El Estándar en Java

Java ofrece un estándar para la validación de beans. Spring Boot se integra perfectamente con este estándar. Las reglas de validación se definen mediante **anotaciones directamente en los atributos de las clases Java**.

### 2.3 Anotaciones de Validación Comunes

Algunas anotaciones útiles incluyen:

* `@NotNull`: Asegura que el atributo no sea `null`.
* `@NotEmpty`: Asegura que una cadena, colección o array no sea `null` ni esté vacío.
* `@NotBlank`: Asegura que una cadena no sea `null`, ni esté vacía, y que contenga al menos un carácter que no sea espacio en blanco.
* `@Size(min=X, max=Y)`: Valida el tamaño de una cadena, colección o array.
* `@Min(value)`: Valida que un valor numérico sea mayor o igual que `value`.
* `@Max(value)`: Valida que un valor numérico sea menor o igual que `value`.
* `@Email`: Valida que una cadena tiene formato de email válido.
* `@Pattern(regexp="...")`: Valida que una cadena cumple con una expresión regular específica.
* Anotaciones para Fechas/Tiempo:
  * `@Future`: La fecha/tiempo debe ser en el futuro.
  * `@Past`: La fecha/tiempo debe ser en el pasado.

### 2.4 Activando la Validación en los Controladores

Para que Spring ejecute estas validaciones, se debe añadir la anotación `@Valid` al parámetro del método del controlador que recibe el cuerpo de la petición (`@RequestBody`).

Ejemplo:

```java
@PostMapping("/api/cursos")
public Curso createCurso(@Valid @RequestBody Curso curso) {
    return cursoService.save(curso);
}
```

Deberían hacer lo mismo en su endpoint `PUT` para actualizar un curso.

### 2.5 Manejo de Errores de Validación

Por defecto, si la validación falla debido a `@Valid`, Spring lanza una excepción (`MethodArgumentNotValidException`). Spring Boot la captura por ustedes y devuelve una respuesta HTTP 400 Bad Request estándar al cliente, incluyendo detalles sobre qué campos fallaron la validación y por qué (los mensajes que definieron). Para una API más profesional, podrían personalizar este manejo de errores usando `@ControllerAdvice`.

### 2.6 Asistencia de Cursor AI

* **Sugerir Anotaciones**: Mientras definen los atributos en su clase `Curso`, describan los requisitos (ej. "Este campo no puede estar vacío", "Este número debe ser positivo"). Cursor AI les sugerirá las anotaciones de validación apropiadas.
* **Añadir** `@Valid`: Cuando escriban un método de controlador que usa `@RequestBody`, Cursor AI a menudo les recordará añadir `@Valid`.
* **Explicar Errores**: Si se encuentran con un error de validación al probar su API, pueden pegar el error en el chat de Cursor AI y pedirle que les explique por qué ocurrió.

## 3 Documentación Automática con Springdoc OpenAPI (Swagger)

### 3.1 ¿Por Qué Documentar su API?

Una API sin documentación es casi inútil para otros desarrolladores. La documentación explica:

* Qué recursos están disponibles.
* Qué URIs y verbos HTTP usar para cada operación.
* Qué parámetros espera cada endpoint.
* La estructura esperada del cuerpo de la petición y la respuesta.

### 3.2 Configuración

Añadir la dependencia de Springdoc OpenAPI:

#### Maven

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.8.5</version>
</dependency>
```

#### Gradle

```groovy
implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.8.5'
```

Después de añadir la dependencia, la documentación estará disponible en:

* <http://localhost:8080/v3/api-docs>
* <http://localhost:8080/swagger-ui.html>

### 3.3 Anotaciones de Documentación

* `@Tag`: Agrupa endpoints relacionados. Usada en una clase `@RestController` para agrupar los endpoints relacionados en Swagger UI.
* `@Operation`: Describe una operación HTTP específica. Proporciona un resumen y una descripción detallada.
* `@Parameter`: Añade detalles a un parámetro en la firma de un método (`@PathVariable`, `@RequestParam`, etc.).
* `@RequestBody`: Describe el cuerpo de la petición esperado. Pueden describir su propósito, si es requerido y referenciar el esquema del modelo (`@Schema)`).
* `@ApiResponses` y `@ApiResponse`: Documentan las posibles respuestas HTTPque puede devolver una operación (códigos de estado como 200, 201, 400, 404, 500) y describir qué significan y qué contenido devuelven. Se aplica a nivel de método
* `@Schema`: Usada en la clase modelo o en DTOs para añadir descripciones a sus propiedades. Ayuda a Swagger UI a mostrar la estructura de los datos.

```java
package com.example.apicursos.controller;

import io.swagger.v3.oas.annotations.tags.Tag;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.parameters.RequestBody;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.Schema;

// ...

@Tag(name = "Cursos", description = "API para la gestión de cursos académicos")
@RestController
@RequestMapping("/api/cursos")
public class CursoController {

    @Operation(summary = "Obtener un curso por su ID", 
            description = "Retorna la información detallada de un curso específico")
    @ApiResponses(value = {
        @ApiResponse(responseCode = "200", description = "Curso encontrado exitosamente",
                  content = @Content(schema = @Schema(implementation = Curso.class))),
        @ApiResponse(responseCode = "404", description = "Curso no encontrado - El ID proporcionado no existe"),
        @ApiResponse(responseCode = "400", description = "Solicitud inválida - Por ejemplo, si el ID no es numérico")
    })
    @GetMapping("/{id}")
    public ResponseEntity<Curso> getCursoById(
        @Parameter(description = "ID del curso a buscar")
        @PathVariable Long id) {
            // Lógica del controlador
    }

    @PostMapping
    @Operation(summary = "Crear un nuevo curso")
    public Curso createCurso(
            @RequestBody(description = "Datos del nuevo curso a crear", required = true, 
                        content = @Content(schema = @Schema(implementation = Curso.class))) 
            @Valid @org.springframework.web.bind.annotation.RequestBody Curso curso) { 
        // Lógica del controlador
    }

    // ... otros métodos del controlador ...
}
```

```java
package com.example.apicursos.model;

import io.swagger.v3.oas.annotations.media.Schema; 

@Entity
@Table(name = "cursos")
@Schema(description = "Representa un curso académico")
public class Curso {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Schema(description = "Identificador único generado automáticamente", example = "101")
    private Long id;

    @NotBlank
    @Size(min = 3, max = 100)
    @Schema(description = "Nombre completo del curso", example = "Programación Avanzada en Java")
    private String nombre;

    @NotBlank
    @Size(min = 2, max = 10)
    @Schema(description = "Código único del curso", example = "PROG401") 
    private String codigo;

    // ... otras propiedades con @Schema ...

    // ... Constructores, Getters, Setters, toString ...
}
```

### 3.4 Asistencia de Cursor AI

Si escriben `@Operation` antes de un método, Cursor AI a menudo les sugerirá automáticamente añadir atributos comunes como summary y description. Si escriben `@Parameter` o `@RequestBody`, les sugerirá atributos como description y required. Si seleccionan una clase modelo, pueden pedirle a Cursor AI que les ayude a añadir anotaciones @Schema a los campos existentes o generar ejemplos de valores para el example atributo. Pueden preguntar a Cursor AI el propósito de cualquier anotación de Swagger.

## 4. Refactorización Final y Pulido con IA

### 4.1 Importancia de una Revisión Final

Una vez que la funcionalidad (CRUD), validación y documentación básica están implementadas, es una buena práctica revisar y pulir el código para asegurar su calidad a largo plazo.

### 4.2 Refactorización y Limpieza con Asistencia de Cursor AI

Cursor AI es una herramienta valiosa en este paso:

* **Análisis General**: Seleccionen una clase completa (ej. su `CursoService` o `CursoController`) y usen el chat de AI (Cmd/Ctrl + L): "Review this code for potential code smells and suggest improvements for clarity and maintainability."
* **Simplificar Lógica**: Si un método les parece muy largo o complejo, seleccionen el código y pidan: "Simplify this Java code block." o "Suggest refactoring options for this method."
* **Mejorar Nombres**: Utilicen la función de renombrar de Cursor AI (Shift + F6). Pídanle a la IA sugerencias si no están seguros de un buen nombre.
* **Generar Comentarios/Javadoc**: Si hay lógica que no es obvia o métodos importantes, pídanle a Cursor AI que genere comentarios o Javadoc para ellos.
* **Detectar Duplicados**: Usen la función de análisis de código de Cursor AI para encontrar fragmentos de código duplicado que podrían ser consolidados.
* **Validar Documentación**: Revisen su Swagger UI. Si algo no se ve bien o falta información, identifiquen la sección en su código y pídanle a Cursor AI que les ayude a añadir o corregir la anotación de Swagger correspondiente.

--

## 5. Conclusión y Próximos Pasos

Han aprendido dos aspectos fundamentales para construir APIs RESTful robustas: la **validación de datos** para asegurar la calidad y seguridad, y la **documentación automática** para hacer su API usable por otros.

Ahora, el desafío es integrar estos conceptos con su conocimiento previo de Spring Boot para completar el proyecto de la **API RESTful para Gestión de Cursos**.

* Apliquen la arquitectura por capas (Controlador -> Servicio -> Repositorio).
* Implementen las operaciones CRUD + búsqueda en esas capas.
* Añadan validaciones a la entidad `Curso` y actívenlas en el controlador usando `@Valid`.
* Configuren Springdoc OpenAPI y añadan anotaciones (`@Tag`, `@Operation`, `@Parameter`, `@RequestBody`, `@ApiResponses`, `@Schema`) para documentar su API.
* Usen Cursor AI en cada paso para asistir en la codificación, validación y documentación.
* Realicen una revisión final y pulan su código con la ayuda de la IA.

¡Este proyecto les permitirá integrar gran parte de lo que hemos visto hasta ahora y les dará una base sólida para construir APIs reales!
