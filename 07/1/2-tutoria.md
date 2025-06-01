# Tutoría : Manejo de Excepciones Globales y Personalización de DTOs

## 1. Inicio y Contextualización

Bienvenida y objetivos: Inicia la sesión aclarando los objetivos: consolidar validaciones y manejo de errores, y elevar la calidad de las respuestas API.

## 2. Revisión de Flujos de Error

Guía una discusión sobre los tres tipos principales de errores:

- **Errores de Validación (`400 Bad Request`)**: Recuérdales cómo `@Valid` lanza `MethodArgumentNotValidException` y cómo el `GlobalExceptionHandler` la captura y formatea.
- **Recursos No Encontrados (`404 Not Found`)**: Explica la práctica de lanzar `ResourceNotFoundException` y cómo un manejador específico la intercepta para devolver un `404`. Proporciona el código de ejemplo para `ResourceNotFoundException` y su `ExceptionHandler`.
- **Excepciones Técnicas/Inesperadas (`500 Internal Server Error`)**: Enfatiza el rol del manejador genérico para `Exception.class` como "último recurso" y la importancia de **loggear** el error completo en el servidor mientras se envía un mensaje genérico al cliente.

## 3. Refactorización del Manejador Global

- Simula (o guía) una "refactorización" con Cursor AI del `GlobalExceptionHandler`. El objetivo es mejorar la **organización, claridad y consistencia** de las respuestas de error.
- Discute los puntos clave: agrupación lógica de los manejadores por tipo de error (4xx, 5xx), mensajes de error claros para el cliente y el desarrollador, y el patrón uniforme de retorno de `ResponseEntity<ApiErrorResponse>`.

## 4. Taller Práctico (`/register` Endpoint)

### Preparación para el Taller

1. **Verificación de Entorno**: Pide a los estudiantes que abran sus proyectos Spring Boot. Confirma que tienen las dependencias necesarias (`spring-boot-starter-web`, `spring-boot-starter-validation`, `springdoc-openapi-ui`, `lombok`).
2. **Confirmación de Código Previsto**:
    - Asegúrate de que ya tienen creadas las clases:
      - `UsuarioRegistroDTO` (con validaciones y anotaciones Jackson).
      - `ApiErrorResponse` (el formato unificado de error).
      - `GlobalExceptionHandler` (con manejadores para `MethodArgumentNotValidException` y `Exception.class`, y opcionalmente `ResourceNotFoundException`).
      - `AuthController` (con el método `/register` usando `@Valid @RequestBody UsuarioRegistroDTO`).
    - Si algún estudiante falta alguna de estas partes, repasar brevemente los puntos clave o darles el esqueleto de código.
3. **Inicio de la Aplicación**: Pídeles que inicien su aplicación Spring Boot.
4. **Acceso a Swagger UI**: Indícales que abran su navegador y accedan a la documentación de su API en `http://localhost:8080/swagger-ui.html`. Esto será nuestra herramienta principal para enviar peticiones y observar respuestas.

### Pruebas Guiadas Paso a Paso

Guía a los estudiantes a través de cada escenario de prueba. Para cada uno, describe la petición, haz la pregunta sobre el resultado esperado, y luego realiza la petición en vivo (o pídeles que la hagan) para verificar. Fomenta la observación y el análisis de la respuesta JSON.

#### Escenario 1: Registro Exitoso (Datos Válidos)

- **Objetivo**: Confirmar que el endpoint funciona correctamente con datos válidos y observar el efecto de las anotaciones Jackson en el DTO (si se devuelve el DTO en el éxito).
- **Tutor dice / Demuestra**:
  - "Vamos a comenzar con el escenario ideal: un registro de usuario con datos completamente válidos."
  - "En Swagger UI (o Postman), busquen el endpoint POST /api/auth/register."
  - "Utilicen el siguiente JSON para la petición. Observen el campo nombre_de_usuario si usaron `@JsonProperty` en su DTO, y `user_name_old` o `nombre_completo_usuario` si usaron `@JsonAlias`."
  - **JSON de Petición (Ejemplo)**:

    ```JSON
    {
      "nombre_de_usuario": "usuarioPerfecto", // Usando el nombre del @JsonProperty
      "user_name_old": "otroNombreValido",   // Si el cliente envia un @JsonAlias
      "email": "perfecto@ejemplo.com",
      "password": "contrasenaSegura123"
    }
    ```

  - **Pregunta al Estudiante**: "¿Qué código de estado HTTP y qué tipo de mensaje esperan recibir?"
- **Ejecución y Observación**: Realiza la petición.
- **Resultado Esperado**:
  - **Código de Estado**: `200 OK`
  - **Cuerpo de la Respuesta**: "Usuario registrado exitosamente: usuarioPerfecto" (o similar, dependiendo de tu implementación).
- **Discusión**: "¡Excelente! Esto confirma que la validación pasó y que nuestro controlador procesó la petición. Si hubiéramos devuelto el DTO de usuario, notarían que la `password` no aparecería debido a `@JsonIgnore.`"

#### Escenario 2: Fallo de Validación - Campos Requeridos Vacíos

- **Objetivo**: Demostrar cómo nuestro `GlobalExceptionHandler` captura los errores de `@NotBlank` y los formatea.
- **Tutor dice / Demuestra**:
  - "Ahora, vamos a simular un error común: el cliente envía datos incompletos."
  - "En Swagger UI, hagan una petición con `username` y `email` vacíos."
  - **JSON de Petición (Ejemplo)**:

      ```JSON
      {
        "nombre_de_usuario": "",
        "email": "",
        "password": "pass"
      }
      ```

  - **Pregunta al Estudiante**: "¿Qué código de estado HTTP y qué formato de respuesta esperan ver ahora? ¿Recuerdan qué excepción se lanza en estos casos y qué manejador la procesa?"
- **Ejecución y Observación**: Realiza la petición.
- **Resultado Esperado**:
  - **Código de Estado**: `400 Bad Request`
  - **Cuerpo de la Respuesta (`ApiErrorResponse`)**:

    ```JSON
    {
        "timestamp": "YYYY-MM-DDTHH:mm:ss",
        "status": 400,
        "error": "Bad Request",
        "message": "Errores de validación: username: El nombre de usuario es obligatorio y no puede estar vacío.; email: El correo electrónico es obligatorio.; password: La contraseña debe tener al menos 6 caracteres.",
        "path": "/api/auth/register"
    }
    ```

- **Discusión**: "¡Perfecto! Vemos el `400 Bad Request` y nuestro `ApiErrorResponse` estandarizado. El mensaje consolida todos los errores de validación, lo que es muy útil para el cliente. Esto fue manejado por nuestro `@ExceptionHandler(MethodArgumentNotValidException.class)`."

#### Escenario 3: Fallo de Validación - Longitud y Formato Incorrecto

- **Objetivo**: Reforzar el manejo de `@Size` y `@Email` por el `GlobalExceptionHandler`.
- **Tutor dice / Demuestra**:
  - "Cambiemos la naturaleza de los errores. Ahora, enviaremos un `username` muy corto, una `password` que no cumpla la longitud mínima y un `email` con formato inválido."
  - **JSON de Petición (Ejemplo)**:

    ```JSON
    {
      "nombre_de_usuario": "us",
      "email": "esto-no-es-un-email",
      "password": "abc"
    }
    ```

  - **Pregunta al Estudiante**: "¿Qué esperan ver como respuesta? ¿Cambiará el código de estado o el formato de la respuesta?"
- **Ejecución y Observación**: Realiza la petición.
- **Resultado Esperado**:
  - **Código de Estado**: `400 Bad Request`
  - Cuerpo de la Respuesta (`ApiErrorResponse`): Contiene un mensaje detallando todos los errores de `Size` y `Email`.
- **Discusión**: "Como ven, el `GlobalExceptionHandler` maneja consistentemente cualquier fallo de validación, sin importar el tipo de anotación que haya fallado. El cliente siempre recibe una respuesta `400 Bad Request` con el formato esperado."

#### Escenario 4: Jackson `@JsonAlias` en Acción (Si implementaste `@JsonAlias`)

- **Objetivo**: Demostrar la flexibilidad de Jackson para aceptar múltiples nombres de campo en la entrada JSON.
- **Tutor dice / Demuestra**:
  - "Si en su `UsuarioRegistroDTO` usaron `@JsonAlias` en el campo username (ej., `@JsonAlias`({`"user_name_old"`, `"nombre_completo_usuario"`})), vamos a probarlo.
  - "Envíen una petición donde el nombre de usuario venga con uno de los alias, no con el nombre del campo Java (`username`) ni el del `@JsonProperty`."
  - **JSON de Petición (Ejemplo)**:

    ```JSON
    {
      "nombre_completo_usuario": "aliasUsuario", // Usando uno de los @JsonAlias
      "email": "alias@ejemplo.com",
      "password": "passwordSegura123"
    }
    ```

  - **Pregunta al Estudiante**: "¿Esperan que esta petición sea exitosa o falle? ¿Por qué?"
- **Ejecución y Observación**: Realiza la petición.
- **Resultado Esperado**:
  - **Código de Estado**: `200 OK`
  - **Cuerpo de la Respuesta**: Mensaje de éxito.
- **Discusión**: "¡Excelente! Esto demuestra el poder de `@JsonAlias`. Su API puede seguir evolucionando, cambiando nombres de campos, pero manteniendo la compatibilidad hacia atrás con clientes que aún envían nombres antiguos. Jackson se encarga del mapeo automáticamente."

### Escenario 5: Error Inesperado del Servidor (Simulado 500 Internal Server Error)

- **Objetivo**: Demostrar cómo el `GlobalExceptionHandler` captura errores no específicos y la importancia del logging.
- **Tutor dice / Demuestra**:
  - "Finalmente, vamos a simular un error inesperado que podría ocurrir en nuestra lógica de negocio, algo que no hayamos manejado explícitamente."
  - **Paso 1 (Temporal y Crucial)**: "En su `AuthController.java`, en el método `registrarUsuario`, justo después de la línea donde se imprime 'Usuario DTO validado:', agreguen _temporalmente_ la siguiente línea para forzar una excepción:"

    ```Java
    // Dentro de AuthController.java, método registrarUsuario
    // ...
    System.out.println("Usuario DTO validado: " + usuarioDTO.getUsername());
    throw new RuntimeException("¡ERROR SIMULADO: Fallo inesperado en la lógica de negocio!"); // AGREGAR TEMPORALMENTE ESTA LÍNEA
    // ...
    ```

  - **Tutor dice**: "Ahora, envíen una petición con datos **válidos** a `/api/auth/register`. Queremos que la validación pase y que la excepción se lance en la lógica de negocio simulada."
  - **JSON de Petición (Ejemplo)**: Datos válidos (como en el Escenario 1).
- **Pregunta al Estudiante**: "¿Qué código de estado HTTP y qué tipo de respuesta esperan ahora? ¿Qué verán en la consola de su IDE?"
- **Ejecución y Observación**: Realiza la petición.
- **Resultado Esperado**:
  - **Código de Estado**: `500 Internal Server Error`
  - **Cuerpo de la Respuesta (`ApiErrorResponse`)**:

    ```JSON
    {
        "timestamp": "YYYY-MM-DDTHH:mm:ss",
        "status": 500,
        "error": "Internal Server Error",
        "message": "Ha ocurrido un error inesperado en el servidor. Por favor, intente más tarde.", // Mensaje genérico
        "path": "/api/auth/register"
    }
    ```

  - **Consola del IDE**: Muestra la consola del IDE donde se debería ver el stack trace completo de la `RuntimeException` que lanzamos, gracias a `ex.printStackTrace()` en el `GlobalExceptionHandler`.
- **Discusión**: "¡Aquí lo tienen! Recibimos un `500` con un mensaje genérico para el cliente, lo cual es vital por seguridad. Pero, y esto es lo importante, en nuestra consola, tenemos el _stack trace_ completo que nos permite depurar el error. Esto demuestra la robustez de nuestro `GlobalExceptionHandler` para capturar cualquier imprevisto."

- **¡Paso Crítico y Obligatorio!**: "Ahora, y esto es **MUY IMPORTANTE**, **remuevan inmediatamente la línea** `throw new RuntimeException(...)` que acabamos de añadir en `AuthController`. Reinicien su aplicación para asegurar que todo esté limpio."

## 5. Cierre

- **Recapitulación**: "Hemos completado un ciclo completo de interacción con nuestra API, manejando tanto el éxito como diversos tipos de errores de manera controlada y profesional. Hemos visto cómo las validaciones y Jackson nos dan un control increíble sobre nuestros DTOs, y cómo nuestro GlobalExceptionHandler es el guardián de la coherencia de nuestras respuestas de error."
- **Preguntas y Feedback**: "Abro el espacio para cualquier pregunta que tengan sobre el taller, los conceptos, o si encontraron algún problema durante las pruebas."
- **Preparación para la Siguiente Clase**: "Todo lo que hemos consolidado hoy, especialmente el manejo de errores de autenticación y autorización, será fundamental para la próxima clase, donde nos sumergiremos en la seguridad de nuestras APIs con JWT."
