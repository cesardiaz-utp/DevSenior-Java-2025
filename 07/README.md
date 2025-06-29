# Java Senior con IA

## Modulo 07 - Seguridad, JWT y APIs Avanzadas en Spring Boot

En este módulo el estudiante aprenderá a asegurar aplicaciones Java con Spring Security, implementar autenticación basada en tokens JWT (JSON Web Token), gestionar errores globalmente, y construir APIs más sólidas, organizadas y preparadas para producción. Todo será reforzado mediante refactorización asistida por Cursor AI para mantener código limpio, seguro y mantenible.

### [Clase 01](1) - Manejo de Excepciones Globales y Validaciones Personalizadas

- **Objetivos de aprendizaje**:
  - Entender los tipos de mensaje de respuesta de una API RESTful,
  - Implementar un sistema centralizado de manejo de errores.
  - Estandarizar las respuestas de error en toda la API.
  - Validar entradas de usuario y mostrar mensajes significativos.

- **Contenidos**
  - Restuestas en el protocolo HTTP y como usarlas en servicios REST.
  - Uso de ResponseEntity para devolver distintos mensajes de respuesta.
  - Creación de manejador global con `@ControlerAdvice` y `@ExceptionHandler`.
  - Formato unificado de respuesta (`timestamp`, `mensaje`, `estado`, `ruta`).
  - Validaciones con `@Valid`, `@NotBlank`, `@Size`, `@Email`, etc.
  - Respuestas automáticas y personalizadas con `BindingResult`.

### [Clase 02](2) - Fundamentos de Seguridad en Aplicaciones Spring Boot

- **Objetivos de aprendizaje**:
  - Comprender los fundamentos de la seguridad en aplicaciones web modernas.
  - Implementar mecanismos básicos de autenticación y autorización con Spring Security.
  - Conocer el ciclo de vida de una solicitud protegida.

- **Contenido**
  - ¿Qué es Spring Security? Componentes básicos y flujo de autenticación.
  - Seguridad por defecto en Spring Boot.
  - Configuración de seguridad personalizada con `SecurityFilterChain`.
  - Rutas públicas vs. rutas protegidas (`antMatchers`, `authorizeHttpRequests`, etc.).
  - Inyección de usuarios en memoria con `UserDetailsService`.

- **Ejercicio práctico**:
  - Proteger una API básica (/api/cursos) y permitir solo el acceso autenticado a rutas específicas.

### [Clase 03](3) - Implementación de Autenticación con JWT (JSON Web Tokens)

- **Objetivos de aprendizaje**:
  - Integrar JWT como mecanismo de autenticación sin sesiones.
  - Implementar generación, validación y uso de tokens en APIs REST.

- **Contenido**
  - ¿Qué es un JWT? Encabezado, payload y firma.
  - Flujo completo de autenticación sin estado:
    Inicio de sesión → generación del token → uso del token en peticiones →
validación automática.
  - Implementación de filtros personalizados para validar JWT en cada solicitud.
  - Configuración de cabeceras (`Authorization: Bearer TOKEN`) y extracción del
usuario.
  
- **Ejercicio práctico**:
  - Implementar un endpoint /login que devuelva un JWT al autenticarse y proteger
el acceso a /api/privado con dicho token.

### [Clase 04](4) - Java Records, MapStruct, el Desarrollo Ágil y Scrum

- **Objetivos de aprendizaje**:
  - Comprender y utilizar las clases `Record` de Java para crear DTOs inmutables y concisos en Spring Boot.
  - Aplicar `Record` clases en controladores y servicios de Spring Boot.
  - Implementar el mapeo de objetos utilizando MapStruct para transformar entre entidades y DTOs (incluyendo DTOs basados en Records).
  - Comprender los principios fundamentales del Desarrollo Ágil.
  - Explicar la metodología Scrum, sus roles, ceremonias y artefactos.
  - Identificar cómo el desarrollo Ágil y Scrum se aplican en el contexto de proyectos de desarrollo de APIs y software en general.

- **Contenido**:
  - Introducción a Java Records: sintaxis, ventajas y ejemplos prácticos para DTOs en Spring Boot.
  - Uso de Records en controladores y servicios, integración con validaciones y serialización JSON.
  - Mapeo eficiente entre entidades y DTOs usando MapStruct, ventajas frente a mapeo manual.
  - Principios del Desarrollo Ágil y el Manifiesto Ágil.
  - Scrum: roles (Product Owner, Scrum Master, Development Team), eventos (Sprint, Planning, Daily, Review, Retrospective) y artefactos (Product Backlog, Sprint Backlog, Increment).
  - Beneficios de aplicar Scrum y Ágil en equipos de desarrollo de APIs y software.
