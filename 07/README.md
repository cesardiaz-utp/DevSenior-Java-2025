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

#### Tutoría

- **Objetivos de la tutoría**:
  - Analizar y reforzar el entendimiento del flujo de seguridad.
  - Configurar correctamente el acceso seguro a rutas según roles o permisos.
- **Actividades**:
  - Implementacion de `UserDetailsService` con conexion a base de datos.
  - Taller práctico: diferenciar rutas públicas y privadas, crear login básico en memoria.
  - Diagnóstico de errores comunes (`403 Forbidden`, `401 Unauthorized`).
  - Casos de estudio: cómo proteger rutas en una API pública y qué endpoints deben ser expuestos sin seguridad.

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

#### Tutoría

- **Objetivos de la tutoría**:
  - Profundizar en la lógica de autenticación sin sesión.
  - Asegurar la correcta configuración del flujo completo de login y validación de token.

- **Actividades**:
  - Simulación completa: login, obtención de token, uso en cliente (Postman, VSCode REST Client).
  - Debug del filtro JWT con breakpoints y validación de claims.
  - ¿cómo almacenar tokens del lado del cliente? Buenas prácticas para proteger APIs públicas

### [Clase 04](4) - Proyecto API Segura – Autenticación, Validación y Control de Acceso

- **Objetivos de aprendizaje**:
  - Integrar seguridad, autenticación JWT, validaciones y manejo de errores en un sistema real.
  - Construir una API segura, funcional y lista para producción.

- **Contenido**:
  - Diseño del proyecto: API RESTful segura para gestión de usuarios o productos.
  - Roles de usuario (ADMIN, USER) y acceso condicional a rutas.
  - Inicio de sesión con generación de JWT, protección de rutas por rol.
  - Validaciones exhaustivas de entradas y manejo de errores personalizado.
  - Documentación básica con Swagger para facilitar pruebas externas.

- **Proyecto en clase**:
  - Sistema de gestión de usuarios con endpoints:
    - /registro
    - /login
    - /usuarios (`GET` solo para ADMIN)
    - /perfil (`GET` solo para el propio usuario autenticado)

#### Tutoría

- **Objetivos de la tutoría**:
  - Evaluar la seguridad, estructura y consistencia del proyecto final del módulo.
  - Realizar pruebas completas de flujo autenticado y protección de rutas.

- **Actividades**:
  - Presentación del flujo completo: login → token → consumo de recursos protegidos.
  - Validación cruzada: pruebas con compañeros para comprobar errores, protección y mensajes.
  - Publicación del proyecto seguro en GitHub con documentación clara.
