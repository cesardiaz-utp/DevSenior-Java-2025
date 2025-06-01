# Java Senior con IA

## Modulo 07 - Seguridad, JWT y APIs Avanzadas en Spring Boot

### Clase 1 - Manejo de Excepciones Globales y Validaciones Personalizadas

### [Contenido](1-contenido.md)

1. Respuestas en el Protocolo HTTP y Cómo Usarlas en Servicios REST
    1. Estructura de una respuesta HTTP
    2. Categorías de Códigos de Estado HTTP
    3. La Importancia de la Precisión
2. Uso de `ResponseEntity` para Devolver Distintos Mensajes de Respuesta
3. Creación de un Manejador Global con `@ControllerAdvice` y `@ExceptionHandler`
    1. `@ControllerAdvice`
    2. `@ExceptionHandler(TipoDeExcepcion.class)`
    3. Ejemplo de Implementación de un `GlobalExceptionHandler` (Esqueleto)
4. Formato Unificado de Respuesta de Error
    1. Formato Propuesto
    2. Implementación de la Clase `ApiErrorResponse`
    3. Integración en el `GlobalExceptionHandler`
    4. Beneficios de la Estandarización
    5. Validaciones con `spring-boot-starter-validation`
5. Personalización de DTOs con la Librería Jackson
    1. ¿Por qué personalizar con Jackson?
    2. Anotaciones Jackson Comunes para DTOs
6. Ejercicio Práctico en Clase

### [Tutoría](2-tutoria.md)

1. Inicio y Contextualización
2. Revisión de Flujos de Error
3. Refactorización del Manejador Global
4. Taller Práctico (`/register` Endpoint)
5. Cierre

#### [Ejercicio autónomo](3-ejercicio.md)

#### [Ejercicios adicionales](4-ejercicios_adicionales.md)
