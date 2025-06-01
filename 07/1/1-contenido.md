# Clase 1: Manejo de Excepciones Globales y Validaciones Personalizadas

¡Hola a todos!

¡Bienvenidos al **Módulo 7**! En esta etapa de su aprendizaje, no solo construimos APIs que funcionan, sino que las elevamos a un nivel profesional: **APIs robustas, seguras y confiables**. Esta primera clase es fundamental, ya que nos sumergiremos en cómo manejar los errores de manera elegante y cómo asegurar que los datos que recibe nuestra API sean válidos desde el primer momento.

## Objetivos de Aprendizaje

Al terminar esta clase, no solo sabrán qué hacer cuando algo sale mal, sino que serán expertos en:

- **Comunicación Clara con HTTP**: Entenderán los diferentes **códigos de estado HTTP** y cómo usarlos para que su API "hable" de forma precisa con cualquier cliente.
- **Manejo Centralizado de Errores**: Implementarán un sistema que captura y procesa todas las excepciones de su API desde un único lugar, evitando la repetición de código y manteniendo la coherencia.
- **Respuestas de Error Estándar**: Aprenderán a devolver los errores siempre en un mismo formato, haciendo que su API sea predecible y fácil de usar por otros desarrolladores.
- **Personalización Avanzada de DTOs**: Controlarán con detalle la serialización (Java a JSON) y deserialización (JSON a Java) de sus Data Transfer Objects (DTOs) utilizando las potentes anotaciones de la librería **Jackson**.

## 1. Respuestas en el Protocolo HTTP y Cómo Usarlas en Servicios REST

![Errores de navegación](https://rockcontent.com/es/wp-content/uploads/sites/3/2019/02/errores-de-navegacio%CC%81n.png.webp)

Una API RESTful es, en esencia, una conversación entre un cliente (como una aplicación móvil o un navegador) y un servidor. En esta conversación, el **protocolo HTTP** (Hypertext Transfer Protocol) actúa como el idioma común. Los **códigos de estado HTTP** son las señales de tráfico que indican el resultado de una solicitud. Usarlos correctamente es crucial para la **usabilidad y la predictibilidad** de su API. Imaginen que están hablando con alguien y, en lugar de responder "_sí_", "_no_" o "_no te entendí_", simplemente se quedan en silencio. ¡Sería frustrante! Los códigos HTTP evitan esa frustración.

### 1.1. Estructura de una respuesta HTTP

![Respuesta HTTP](https://mdn.github.io/shared-assets/images/diagrams/http/overview/http-response.svg)

Las respuestas están formadas por los siguientes campos:

- La versión del protocolo HTTP que están usando.
- Un código de estado, indicando si la petición ha sido exitosa, o no, y debido a que.
- Un mensaje de estado, una breve descripción del código de estado.
- Cabeceras HTTP, como las de las peticiones.
- Opcionalmente, el recurso que se ha pedido.

### 1.2. Categorías de Códigos de Estado HTTP

![Categorias codigos estado HTTP](https://static.semrush.com/blog/uploads/media/f5/7c/f57c8f9295d20dc481e81ae1a782c13c/original.png)

- **`1xx` (Informativos)**: Son códigos provisionales que indican que la solicitud ha sido recibida y el proceso continúa. Por ejemplo, `100 Continue` le dice al cliente que siga enviando el resto de la solicitud. En el contexto de APIs REST usuales, no los verán a menudo como respuesta final.
- **`2xx` (Éxito)**: ¡Aquí es donde queremos estar! Estos códigos confirman que la acción solicitada por el cliente fue recibida, entendida y procesada con éxito.
  - `200 OK`: Este es el rey del éxito. Se usa cuando la solicitud fue exitosa y la respuesta contiene la información solicitada. Es el código por defecto para la mayoría de las operaciones `GET` y `PUT`. Por ejemplo, al obtener un recurso o al actualizarlo sin crear uno nuevo.
  - `201 Created`: ¡Una estrella ha nacido! Este código es el ideal para las operaciones `POST` que resultan en la creación de un nuevo recurso en el servidor. La respuesta debería incluir la ubicación del nuevo recurso (en un encabezado `Location`) y, a menudo, el recurso recién creado en el cuerpo.
  - `204 No Content`: A veces, el éxito no necesita palabras (o datos). Este código se usa cuando la solicitud fue exitosa, pero no hay información adicional que enviar en el cuerpo de la respuesta. Es perfecto para operaciones `DELETE` donde el recurso se eliminó correctamente y no hay nada que devolver.
- **`3xx` (Redirección)**: Estos códigos le indican al cliente que necesita realizar una acción adicional para completar la solicitud, generalmente ir a otra URL. Aunque son fundamentales en la navegación web (ej., cuando una página se mueve permanentemente), son menos comunes en las APIs REST puras, a menos que se trate de un caso muy específico de reubicación de recursos.
- **`4xx` (Errores del Cliente)**: ¡Aquí está la responsabilidad del cliente! Estos códigos señalan que el problema está en la solicitud enviada por el cliente. Una API bien diseñada debe usar estos códigos para dar feedback claro.
  - `400 Bad Request`: La solicitud enviada por el cliente es incorrecta. Esto puede ser por una sintaxis inválida (ej., un JSON mal formado) o porque los parámetros de la solicitud no cumplen con las reglas (ej., valores esperados, campos obligatorios, validaciones fallidas). Es un código muy común que usaremos para indicar fallos de validación.
  - `401 Unauthorized`: El cliente no ha proporcionado credenciales de autenticación válidas o le faltan. Esto significa que el cliente no está identificado o su identificación es incorrecta.
  - `403 Forbidden`: El servidor entendió la solicitud, pero se niega a autorizarla. A diferencia de `401`, aquí el cliente está identificado, pero no tiene los permisos necesarios para acceder a ese recurso o realizar esa acción. Es una cuestión de autorización.
  - `404 Not Found`: El recurso solicitado por el cliente no existe en el servidor. Cuando intentan obtener un producto por un ID que no existe, esta es la respuesta esperada.
  - `409 Conflict`: La solicitud no pudo ser completada porque hay un conflicto con el estado actual del recurso. Un ejemplo clásico es intentar crear un recurso que ya existe y que, por definición, debe ser único (ej., un nombre de usuario que ya está registrado).
- **`5xx` (Errores del Servidor)**: ¡La responsabilidad es nuestra! Estos códigos indican que el problema ocurrió en el servidor, no en la solicitud del cliente.
  - `500 Internal Server Error`: El código más genérico y menos deseado. Significa que el servidor encontró una condición inesperada que le impidió cumplir con la solicitud. Generalmente, es un error inesperado no capturado por lógica de negocio específica. Deberíamos esforzarnos por evitar que este código sea la respuesta por defecto, y en su lugar, dar más especificidad si es posible.
  - `503 Service Unavailable`: El servidor no está listo para manejar la solicitud. Esto puede ser por sobrecarga, mantenimiento programado o que el servidor esté caído temporalmente.

### 1.3. La Importancia de la Precisión

Usar el código HTTP correcto no es solo una buena práctica, es una forma de que su API sea **autodescriptiva** y fácil de integrar. Los clientes pueden reaccionar automáticamente a ciertos códigos (ej., si ven un `401`, saben que deben pedir al usuario que inicie sesión de nuevo).

## 2. Uso de `ResponseEntity` para Devolver Distintos Mensajes de Respuesta

En un controlador de Spring, si un método simplemente devuelve un objeto POJO (Plain Old Java Object), Spring por defecto lo serializará a JSON (o XML) y lo devolverá con un código de estado `200 OK`. Sin embargo, a menudo necesitamos más control: queremos devolver un `201 Created` al crear un recurso, un `204 No Content` al borrarlo, o un `400 Bad Request` si la entrada es inválida. Aquí es donde `ResponseEntity` se convierte en su mejor aliado.

`ResponseEntity<T>` es una clase en Spring Framework que representa la **respuesta HTTP completa**. Esto significa que pueden controlar explícitamente:

- **El Cuerpo de la Respuesta (`T`)**: El objeto que se serializará y enviará en el cuerpo de la respuesta (ej., un objeto `Usuario`, un mensaje de error).
- **Los Encabezados HTTP**: Pueden añadir o modificar encabezados HTTP (ej., `Location` para `201 Created`).
- **El Código de Estado HTTP**: Lo más importante para esta clase, ya que pueden especificar el código numérico exacto (ej., `HttpStatus.OK`, `HttpStatus.CREATED`, `HttpStatus.NOT_FOUND`).

```Java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.net.URI; // Para usar URI en el encabezado Location

@RestController
@RequestMapping("/ejemplos")
public class EjemploResponseEntityController {

    // --- Éxito (2xx) ---

    // GET: 200 OK
    @GetMapping("/datos")
    public ResponseEntity<String> obtenerDatos() {
        String datos = "¡Esto son mis datos!";
        return ResponseEntity.ok(datos); // Es igual a ResponseEntity.status(HttpStatus.OK).body(datos);
    }

    // POST: 201 Created
    @PostMapping("/recursos")
    public ResponseEntity<String> crearRecurso(@RequestBody String nuevoRecurso) {
        // Lógica para guardar el recurso...
        String idRecursoCreado = "abc123xyz"; // ID ficticio del recurso creado
        // Es buena práctica devolver también la URI del nuevo recurso en el encabezado Location
        return ResponseEntity.created(URI.create("/ejemplos/recursos/" + idRecursoCreado))
                             .body("Recurso '" + nuevoRecurso + "' creado con ID: " + idRecursoCreado);
    }

    // DELETE: 204 No Content
    @DeleteMapping("/recursos/{id}")
    public ResponseEntity<Void> eliminarRecurso(@PathVariable String id) {
        // Lógica para eliminar el recurso...
        boolean eliminado = true; // Simulación de eliminación exitosa
        if (eliminado) {
            return ResponseEntity.noContent().build(); // No hay cuerpo en la respuesta
        } else {
            return ResponseEntity.notFound().build(); // Si no se encuentra para eliminar
        }
    }

    // --- Errores del Cliente (4xx) ---

    // POST/PUT: 400 Bad Request (Parámetros inválidos)
    @PostMapping("/proceso")
    public ResponseEntity<String> procesarAlgo(@RequestBody int valor) {
        if (valor < 0) {
            // Usamos badRequest() para un 400
            return ResponseEntity.badRequest().body("El valor no puede ser negativo.");
        }
        return ResponseEntity.ok("Valor procesado: " + valor);
    }

    // GET: 404 Not Found
    @GetMapping("/usuarios/{id}")
    public ResponseEntity<String> obtenerUsuario(@PathVariable String id) {
        // Lógica para buscar usuario...
        String usuario = null; // Simulación de usuario no encontrado
        if (usuario == null) {
            // Usamos notFound() para un 404
            return ResponseEntity.notFound().build();
        }
        return ResponseEntity.ok(usuario);
    }

    // --- Errores del Servidor (5xx) ---

    // GET: 500 Internal Server Error (Por una excepción no controlada en el método)
    @GetMapping("/error-inesperado")
    public ResponseEntity<String> causarError() {
        // Esto lanzará una excepción que nuestro GlobalExceptionHandler podría capturar
        throw new RuntimeException("¡Algo salió muy mal en el servidor!");
    }
}
```

**Ventaja Clave**: `ResponseEntity` es la herramienta definitiva para modelar con precisión las respuestas de su API, permitiéndoles ir más allá del simple `200 OK` y comunicar de forma efectiva el resultado de cada operación. Esto es fundamental para construir APIs que no solo sean funcionales, sino también **precisas, estandarizadas y fáciles de consumir** por cualquier cliente o desarrollador externo. Es un pilar para la profesionalización de sus APIs.

## 3. Creación de un Manejador Global con `@ControllerAdvice` y `@ExceptionHandler`

Sin un mecanismo centralizado para manejar las excepciones, sus controladores se llenarían de bloques `try-catch` repetitivos. Imaginen tener que escribir el mismo código para capturar un `ResourceNotFoundException` en 20 métodos diferentes de 5 controladores distintos. _¡Sería una pesadilla de mantenimiento y un caldo de cultivo para inconsistencias!_

Aquí es donde entra en juego la magia de `@ControllerAdvice` y `@ExceptionHandler`. Juntos, permiten crear una **capa de manejo de errores global** que intercepta las excepciones a medida que ocurren, las procesa y devuelve una respuesta HTTP estandarizada, todo desde un único lugar.

### 3.1. `@ControllerAdvice`

- **Propósito**: Esta anotación marca una clase como un "asistente" global para los controladores. Imagínenlo como un director de orquesta que supervisa el comportamiento de todos (o un grupo específico) de los músicos (controladores). Puede manejar excepciones, procesar datos antes de que lleguen a los controladores (con `@InitBinder`), o modificar las respuestas de los controladores (con `@ResponseBodyAdvice`).
- **Ámbito**: Por defecto, un `@ControllerAdvice` aplica a todos los controladores de la aplicación. Si necesitan restringirlo, pueden usar atributos como `basePackages = {"com.miapi.miproyecto.controller"}` o `assignableTypes = {MySpecificController.class}`.
- **Ubicación**: Generalmente, se crea una clase separada en un paquete como `config.exception` o `config.error`.

### 3.2. `@ExceptionHandler(TipoDeExcepcion.class)`

- **Propósito**: Esta anotación se coloca sobre un método dentro de una clase `@ControllerAdvice` (o, menos comúnmente para el manejo global, dentro de un controlador individual). Su función es específica: le indica a Spring que **este método particular debe ser invocado** cada vez que una excepción del `TipoDeExcepcion` especificado (o cualquier subclase de él) sea lanzada por cualquier método dentro de los controladores que están bajo la influencia de este `@ControllerAdvice`.
- **Mecanismo de Selección**: Cuando una excepción es lanzada, Spring busca el `@ExceptionHandler` más específico que pueda manejar esa excepción. Por ejemplo, si tienen un manejador para `RuntimeException` y otro para `IllegalArgumentException` (que es una subclase de `RuntimeException`), Spring elegirá el manejador para `IllegalArgumentException` si esa es la excepción lanzada. Si no encuentra uno más específico, recurrirá al manejador de la superclase.
- **Parámetros del Método**: Un método `@ExceptionHandler` puede recibir como parámetros:
  - La excepción que fue lanzada (el `TipoDeExcepcion` especificado en la anotación).
  - `HttpServletRequest` o `WebRequest`: Para obtener información sobre la solicitud HTTP que causó el error (como la URI, los encabezados, etc.).
  - `HttpHeaders`: Para acceder a los encabezados de la solicitud.
  - Y otros, dependiendo de la necesidad.
- **Retorno del Método**: El método `@ExceptionHandler` puede devolver cualquier cosa que un método de controlador normal pueda devolver: un `ResponseEntity`, un `String` (para resolución de vistas, aunque no es común en REST), o incluso `void` (si solo quieren loggear la excepción). Para APIs REST, `ResponseEntity` es casi siempre la opción correcta para tener control sobre el código de estado y el cuerpo de la respuesta.

### 3.3. Ejemplo de Implementación de un `GlobalExceptionHandler` (Esqueleto)

```Java
package com.miapi.miproyecto.error; // Donde definimos nuestros errores y manejadores

import jakarta.servlet.http.HttpServletRequest; // Necesario para obtener la URI de la solicitud
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

// @ControllerAdvice indica que esta clase proporcionará manejo de excepciones global.
@ControllerAdvice
public class GlobalExceptionHandler {

    // Este @ExceptionHandler capturará cualquier excepción de tipo RuntimeException
    // que ocurra en cualquier controlador, a menos que haya un manejador más específico.
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<ApiErrorResponse> handleRuntimeException(RuntimeException ex, HttpServletRequest request) {
        // MUY IMPORTANTE: En un entorno de producción, loggeen la excepción completa
        // usando un sistema de logging (ej. SLF4J con Logback) para depuración.
        System.err.println("RuntimeException inesperada en la ruta " + request.getRequestURI() + ": " + ex.getMessage());
        ex.printStackTrace(); // Esto es útil en desarrollo para ver el stack trace completo.

        // Creamos una respuesta de error estandarizada para el cliente.
        ApiErrorResponse errorResponse = new ApiErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR, // Código 500: Error genérico del servidor
            "Ocurrió un error inesperado en el servidor.", // Mensaje amigable para el cliente
            request.getRequestURI() // La ruta de la solicitud donde ocurrió el error
        );
        return new ResponseEntity<>(errorResponse, HttpStatus.INTERNAL_SERVER_ERROR);
    }

    // Un manejador genérico para cualquier otra clase de Exception que no haya sido
    // capturada por un @ExceptionHandler más específico. Este es el "último recurso".
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiErrorResponse> handleGeneralException(Exception ex, HttpServletRequest request) {
        System.err.println("Excepción general no manejada en " + request.getRequestURI() + ": " + ex.getMessage());
        ex.printStackTrace();

        ApiErrorResponse errorResponse = new ApiErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR, // Código 500
            "Ha ocurrido un error inesperado. Por favor, intente más tarde.",
            request.getRequestURI()
        );
        return new ResponseEntity<>(errorResponse, HttpStatus.INTERNAL_SERVER_ERROR);
    }

    // Aquí irán más @ExceptionHandler para excepciones específicas (ej. validación, recursos no encontrados)
}
```

**Ventajas del Manejo Global**:

- **Centralización Total**: Todo el código de manejo de errores reside en un solo lugar, lo que simplifica la auditoría y modificación.
- **Coherencia de Respuestas**: Garantiza que todas las respuestas de error de su API sigan el mismo formato y usen los códigos HTTP apropiados.
- **Código Limpio en Controladores**: Libera a sus controladores de la tediosa tarea de manejar excepciones, permitiendo que se centren puramente en la lógica de negocio.
- **Mantenibilidad Mejorada**: Añadir un nuevo tipo de error o cambiar el formato de las respuestas de error es tan simple como añadir un nuevo método `@ExceptionHandler` o modificar la clase `ApiErrorResponse`.
- `Depuración Simplificada`: Con un formato de error unificado y el registro adecuado en los manejadores, depurar problemas se vuelve mucho más eficiente.

## 4. Formato Unificado de Respuesta de Error

Para que una API sea realmente profesional y fácil de consumir, no basta con devolver el código de estado HTTP correcto. Los clientes de su API (sean otros servicios, aplicaciones móviles o frontends web) necesitan información adicional y estructurada para entender la causa del error y actuar en consecuencia. Un mensaje de error poco claro o un formato inconsistente pueden llevar a frustración y errores en la integración.

Por ello, la **estandarización del formato de las respuestas de error** es una práctica fundamental. Significa que, sin importar la causa o el tipo de error, la estructura JSON que el cliente reciba siempre será la misma, variando solo los valores de sus campos.

### 4.1. Formato Propuesto

Un formato común y altamente efectivo para las respuestas de error incluye los siguientes campos:

- `timestamp`:
  - **Propósito**: Indica la fecha y hora exactas en que el error ocurrió en el servidor. Es crucial para depuración y para correlacionar los errores reportados por los clientes con los logs de su aplicación.
  - **Implementación**: Generalmente se usa `java.time.LocalDateTime` o `java.time.Instant` en Java. Al serializar a JSON, es una buena práctica formatearlo como una cadena siguiendo el estándar **ISO 8601** (ej., `"2025-06-01T14:30:00"`), lo que garantiza la interoperabilidad.
- `status`:
  - **Propósito**: El código de estado HTTP numérico que corresponde al error (ej., `400` para "Bad Request", `404` para "Not Found", `500` para "Internal Server Error"). Es redundante con el código de estado de la respuesta HTTP en sí, pero es útil incluirlo en el cuerpo para mayor claridad y facilidad de análisis del JSON.
  - **Implementación**: Un `Integer` que guarda el valor numérico del `HttpStatus` (ej., `HttpStatus.BAD_REQUEST.value()`).
- `error`:
  - **Propósito**: Una descripción textual breve del código de estado HTTP. Es la "razón" asociada al código de estado (ej., "Bad Request", "Not Found", "Internal Server Error").
  - **Implementación**: Un `String` que se obtiene de `HttpStatus.getReasonPhrase()`.
- `message`:
  - **Propósito**: El mensaje más detallado y específico sobre la causa del error. Este mensaje debe ser útil para el desarrollador que consume la API y, si es posible, para el usuario final (si la API es directamente consumida por un frontend). Debe explicar qué salió mal (ej., "El usuario con ID 'XYZ' no fue encontrado", "La contraseña es demasiado corta", "El nombre de usuario ya está en uso").
  - **Implementación**: Un `String` que se genera dinámicamente en el método `ExceptionHandler`, a menudo tomando el mensaje de la excepción original o consolidando varios mensajes de validación.
- `path`:
  - **Propósito**: La URI (Uniform Resource Identifier) de la solicitud que generó el error. Ayuda a identificar rápidamente qué endpoint de la API fue invocado cuando ocurrió el problema.
  - **Implementación**: Un `String` que se obtiene de `HttpServletRequest.getRequestURI()`.

### 4.2. Implementación de la Clase `ApiErrorResponse`

Crearemos una clase POJO (Plain Old Java Object) que sirva como el modelo para nuestras respuestas de error.

```Java
package com.miapi.miproyecto.error; // Un paquete común para errores

import java.time.LocalDateTime; // O java.time.Instant para un timestamp más universal
import com.fasterxml.jackson.annotation.JsonFormat; // Para formatear la fecha
import org.springframework.http.HttpStatus; // Para usar los estados HTTP

public class ApiErrorResponse {

    // @JsonFormat asegura que la fecha se serialice a un formato legible por humanos
    // ISO 8601 es el estándar para fechas en APIs REST
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss")
    private LocalDateTime timestamp;

    private Integer status; // Código de estado HTTP numérico (ej: 400, 404)
    private String error;   // La frase del estado HTTP (ej: "Bad Request", "Not Found")
    private String message; // Mensaje detallado del error para el cliente
    private String path;    // La URI de la solicitud que causó el error

    // Constructor que inicializa los campos
    public ApiErrorResponse(HttpStatus status, String message, String path) {
        this.timestamp = LocalDateTime.now(); // Obtiene la fecha y hora actual
        this.status = status.value();         // Convierte el enum HttpStatus a su valor numérico
        this.error = status.getReasonPhrase(); // Obtiene la frase descriptiva del HttpStatus
        this.message = message;
        this.path = path;
    }

    // Getters para que Jackson pueda serializar los campos a JSON
    public LocalDateTime getTimestamp() {
        return timestamp;
    }

    public Integer getStatus() {
        return status;
    }

    public String getError() {
        return error;
    }

    public String getMessage() {
        return message;
    }

    public String getPath() {
        return path;
    }

    // Los setters no son estrictamente necesarios si solo vamos a construir el objeto
    // en el constructor y luego serializarlo, pero pueden añadirlos si lo necesitan.
}
```

### 4.3. Integración en el `GlobalExceptionHandler`

Ahora, modificaremos los métodos `handle` de nuestro `GlobalExceptionHandler` para que devuelvan instancias de `ApiErrorResponse`, asegurando que todas las respuestas de error sigan este formato.

```Java
// Modificación dentro de GlobalExceptionHandler.java
// Asegúrate de que este archivo está en el mismo paquete o importado correctamente
// import com.miapi.miproyecto.error.ApiErrorResponse;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    // Manejador para cualquier excepción general (el "catch-all")
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiErrorResponse> handleGenericException(Exception ex, HttpServletRequest request) {
        System.err.println("Excepción general no manejada: " + ex.getMessage());
        ex.printStackTrace(); // Vital para depuración en desarrollo/testing

        ApiErrorResponse errorResponse = new ApiErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR, // 500 Internal Server Error
            "Ha ocurrido un error inesperado en el servidor. Por favor, intente más tarde.",
            request.getRequestURI()
        );
        return new ResponseEntity<>(errorResponse, HttpStatus.INTERNAL_SERVER_ERROR);
    }

    // Podemos añadir más manejadores específicos aquí.
    // Por ejemplo, si tuvieran una excepción personalizada como ResourceNotFoundException:
    // @ResponseStatus(HttpStatus.NOT_FOUND) // También se puede usar @ResponseStatus directamente
    // @ExceptionHandler(ResourceNotFoundException.class)
    // public ApiErrorResponse handleResourceNotFoundException(ResourceNotFoundException ex, HttpServletRequest request) {
    //     return new ApiErrorResponse(
    //         HttpStatus.NOT_FOUND,
    //         ex.getMessage(),
    //         request.getRequestURI()
    //     );
    // }
}
```

### 4.4. Beneficios de la Estandarización

- **Predecibilidad para Clientes**: Los desarrolladores que consumen su API siempre sabrán qué estructura de error esperar, lo que simplifica enormemente la implementación del manejo de errores en su lado.
- **Depuración Eficiente**: La información como `timestamp` y `path` facilita la correlación de errores reportados por los clientes con los logs del servidor.
- **Consistencia en la API**: Su API presentará un frente profesional y coherente, sin importar el tipo de error que ocurra.
- **Facilita la Automatización**: Los sistemas automatizados pueden parsear fácilmente las respuestas de error y reaccionar de manera inteligente.

### 4.5. Validaciones con `spring-boot-starter-validation`

La validación de datos de entrada es la primera y una de las más importantes líneas de defensa para la integridad de su aplicación. Antes de que cualquier dato llegue a su lógica de negocio o base de datos, debe ser verificado para asegurar que cumple con todas las reglas y expectativas. Esto previene datos corruptos, errores en tiempo de ejecución y posibles vulnerabilidades de seguridad. Spring Boot facilita enormemente esto gracias a su integración con la especificación Jakarta Bean Validation (JSR 380).

Cuando un objeto anotado con `@Valid` no cumple con una o más de sus reglas de validación, Spring automáticamente lanza una excepción específica: `MethodArgumentNotValidException`. Esta excepción es la que nuestro manejador global de excepciones (`@ControllerAdvice`) debe interceptar y procesar para devolver una respuesta de error estandarizada y detallada al cliente.

## 5. Personalización de DTOs con la Librería Jackson

**Jackson** es la librería predeterminada en Spring Boot para la **serialización** (convertir objetos Java a JSON) y **deserialización** (convertir JSON a objetos Java). Aunque Jackson funciona automáticamente, sus anotaciones les permiten un **control fino** sobre cómo se realizan estas conversiones, lo cual es vital para el diseño de APIs flexibles, seguras y eficientes.

### 5.1. ¿Por qué personalizar con Jackson?

- **Consistencia de Nombres**: Adaptarse a convenciones de nombrado JSON (ej., `snake_case`) aunque su Java use `camelCase`.
- **Seguridad y Privacidad**: Excluir campos sensibles (ej., contraseñas) del JSON de salida.
- **Flexibilidad de Entrada**: Aceptar diferentes nombres de campo en el JSON de entrada para el mismo campo Java.
- **Respuestas Limpias**: Omitir campos nulos o vacíos para un JSON más compacto.
- **Formatos Específicos**: Controlar el formato de fechas, números, etc.

### 5.2. Anotaciones Jackson Comunes para DTOs

1. `@JsonProperty("nombre_en_json")`:
    - **Uso**: Se coloca sobre un campo, getter, setter o parámetro de constructor.
    - **Función**: Renombra el campo en el JSON. Si en Java es `userName`, pero en el JSON quiere que sea `"user_name"`.

    ```Java
    public class UserProfileDTO {
        @JsonProperty("user_name") // En JSON, este campo será "user_name"
        private String username;
        private String email;
    }
    ```

2. `@JsonIgnore`:
    - **Uso**: Se coloca sobre un campo, getter o setter.
    - **Función**: Excluye el campo completamente de la serialización (Java a JSON) y la deserialización (JSON a Java). Ideal para información sensible o interna.

    ```Java
    public class UsuarioLoginDTO {
        private String username;
        @JsonIgnore // La contraseña no se mostrará en el JSON de respuesta ni se leerá del JSON de entrada (si se usa para respuesta también)
        private String password;
    }
    ```

3. `@JsonIgnoreProperties({"campo1", "campo2"})`:
    - **Uso**: Se coloca a nivel de clase.
    - **Función**: Ignora una lista de propiedades. Útil cuando el JSON entrante tiene campos que no te interesan mapear al DTO.

4. ``@JsonInclude(JsonInclude.Include.NON_NULL)``:
    - **Uso**: Se puede aplicar a nivel de clase o de campo.
    - **Función**: Controla cuándo un campo se incluye en el JSON de salida. `NON_NULL` es muy común: el campo solo se serializa si su valor no es `null`. Otros valores incluyen `NON_EMPTY` (no incluye null, cadenas vacías, colecciones vacías) o `NON_DEFAULT` (no incluye el valor por defecto del tipo).

    ```Java
    @JsonInclude(JsonInclude.Include.NON_NULL) // Los campos nulos no aparecerán en el JSON de esta clase
    public class PedidoDTO {
        private String id;
        private String descripcion;
        private Double descuento; // Campo opcional, puede ser nulo
    }
    ```

5. `@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "formato_fecha")`:
    - **Uso**: Sobre campos de fecha y hora (`Date`, `LocalDate`, `LocalDateTime`).
    - **Función**: Personaliza el formato de las fechas y horas en la **serialización** a JSON.

    ```Java
    public class CitaDTO {
        private String paciente;

        @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd/MM/yyyy HH:mm:ss")
        private LocalDateTime fechaHoraCita;
    }
    ```

6. `@JsonAlias({"nombreAlternativo1", "nombreAlternativo2"})`:
    - **Uso**: Sobre un campo en el DTO.
    - **Función**: Durante la **deserialización** (JSON a Java), permite que un campo Java sea mapeado desde varios nombres de campo en el JSON entrante. Útil para mantener la compatibilidad hacia atrás si cambian los nombres de los campos en su API.

    ```Java
    public class ContactoDTO {
        @JsonAlias({"nombre_completo", "full_name"}) // Acepta "nombre_completo" o "full_name" del JSON
        private String fullName;
        private String email;
    }
    ```

7. `@JsonCreator` con `@JsonProperty` en Constructores:
    - **Uso**: Sobre un constructor.
    - **Función**: Indica a **Jackson** qué constructor usar para la deserialización y cómo mapear los parámetros del constructor a los campos del JSON. Es clave para trabajar con DTOs inmutables (con campos `final` y sin setters).

    ```Java
    import com.fasterxml.jackson.annotation.JsonCreator;
    import com.fasterxml.jackson.annotation.JsonProperty;

    public class CredencialesDTO {
        private final String username;
        private final String password;

        @JsonCreator // Jackson usará este constructor para construir el objeto desde JSON
        public CredencialesDTO(@JsonProperty("username") String username,
                              @JsonProperty("password") String password) {
            this.username = username;
            this.password = password;
        }
        // Solo getters, no setters (es inmutable)
    }
    ```

**Impacto en su API**: Al dominar estas anotaciones, sus DTOs no solo transportarán datos, sino que se convertirán en contratos de comunicación precisos y robustos, mejorando la seguridad, la flexibilidad y la calidad de su API.

## 6. Ejercicio Práctico en Clase

Vamos a aplicar el manejo de excepciones global y la personalización de DTOs con Jackson en un ejemplo de registro de usuarios.

### Configura tu Proyecto Spring Boot

- Asegúrate de tener un proyecto Spring Boot base.
- Verifica las dependencias: `spring-boot-starter-web`, `spring-boot-starter-validation`, `springdoc-openapi-ui` (para ver la API) y `lombok` (opcional, pero útil).

### Crea la Clase `UsuarioRegistroDTO` con Validaciones y Anotaciones Jackson

- Crea un paquete `dto`.
- Define la clase `UsuarioRegistroDTO` con los campos `username`, `email`, y `password`.
- **Anotaciones de Validación**: Aplica `@NotBlank`, `@Size` para username y password, y `@Email` para email. Asegúrate de que cada una tenga un message personalizado.
- **Anotaciones Jackson**:
  - **A nivel de clase**: `@JsonInclude(JsonInclude.Include.NON_NULL)`.
  - En el campo `password`: Añade `@JsonIgnore` para que nunca se serialice en respuestas.
  - En el campo `username`: Añade `@JsonProperty("username")` y `@JsonAlias({"user_name_old", "nombre_completo_usuario"})`. Esto hará que al serializarlo se llame `nombre_de_usuario`, y al deserializarlo pueda venir como `username`, `user_name_old` o `nombre_completo_usuario`.
- Genera constructores, getters y setters (o usa `@Data` de Lombok).

### Crea el `ApiErrorResponse`

- Crea un paquete `exception.handler`.
- Define la clase `ApiErrorResponse` tal como se explicó en los contenidos, con `timestamp`, `status`, `error`, `message` y `path`.

### Crea el `GlobalExceptionHandler`

- Crea la clase `GlobalExceptionHandler` en `exception.handler`.
- Anótala con `@ControllerAdvice`.
- Implementa los dos métodos `@ExceptionHandler`:
  - Para `MethodArgumentNotValidException.class`: Extrae los errores de validación, los concatena en un mensaje y devuelve un `ApiErrorResponse` con `HttpStatus.BAD_REQUEST (400)`.
  - Para `Exception.class`: Actúa como "catch-all", devolviendo un `ApiErrorResponse` con `HttpStatus.INTERNAL_SERVER_ERROR (500)` y un mensaje genérico. **¡No olvides `ex.printStackTrace()`!**

### Crea un Controlador de Prueba (`AuthController`)

- Crea un paquete `controller`.
- Define la clase `AuthController` con `@RestController` y `@RequestMapping("/api/auth")`.
- Crea un método `POST` para la ruta `/register`.
- El método debe aceptar el `UsuarioRegistroDTO` en el cuerpo de la solicitud, usando `@Valid @RequestBody`.
- Dentro del método, si la validación pasa, devuelve un `ResponseEntity.ok("Usuario registrado exitosamente: " + usuarioDTO.getUsername())`.

### Prueba tu API

- Inicia tu aplicación Spring Boot.
- Accede a la documentación de Swagger UI en `http://localhost:8080/swagger-ui.html`.
- Utiliza Swagger UI o un cliente REST (Postman, Insomnia, VSCode RestClient) para hacer solicitudes POST a `http://localhost:8080/api/auth/register`:
  - **Éxito**: Envía un JSON con datos válidos para `username` (prueba enviando `username`, `user_name_old`, `nombre_completo_usuario`), `email` y `password`. Observa la respuesta `200 OK`. Nota que `password` no se mostrará en la respuesta JSON.
  - **Validación Fallida**: Envía JSONs con errores en `username`, `email` y `password` (vacíos, muy cortos, formato inválido). Espera respuestas `400 Bad Request` con tu `ApiErrorResponse` detallado.
  - **Error Inesperado (Simulado)**: Modifica _temporalmente_ el método `/register` para que lance una `RuntimeException`. Espera un `500 Internal Server Error` con el mensaje genérico de tu `ApiErrorResponse`. **¡Recuerda deshacer este cambio!**
