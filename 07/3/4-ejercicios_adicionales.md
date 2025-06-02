# Ejercicios adicionales

Estos ejercicios te desafían a tomar los sistemas de gestión de Eventos y Participantes, y Proyectos y Tareas (que ya resolviste en la Clase 2), y a implementar en ellos la autenticación y autorización basada en JWT (JSON Web Tokens), tal como se vio en la Clase 3.

Se asume que la estructura de la API (modelos, servicios, controladores, validaciones básicas de negocio) de los ejercicios de la Clase 1 ya está implementada. El enfoque aquí es la integración de la seguridad JWT.

## Ejercicio 1: Sistema de Gestión de Eventos y Participantes

**Descripción**: Modifica tu API RESTful de gestión de Eventos y Participantes para que la autenticación y autorización se realicen utilizando JWT.

**Requisitos de Seguridad**:

1. **Autenticación de Usuarios con JWT**:
    - Implementa un endpoint de autenticación (`POST /api/auth/authenticate`) que reciba credenciales de usuario (username/password).
    - Si las credenciales son válidas (contra los usuarios en base de datos configurados en `UserDetailsService`), la API debe generar y devolver un JSON Web Token (JWT) en la respuesta.
    - Si las credenciales son inválidas, la API debe devolver un `401 Unauthorized`.
2. **Validación de JWT en Solicitudes Protegidas**:
    - Configura Spring Security para que todas las solicitudes a endpoints protegidos requieran un JWT válido en el encabezado Authorization: `Bearer <token>`.
    - Implementa un filtro (`JwtRequestFilter`) que intercepte las solicitudes, extraiga y valide el JWT (verificando firma, expiración y usuario).
    - Si el JWT es válido, la solicitud debe proceder. Si es inválido, ausente o expirado, la API debe devolver un `401 Unauthorized` o `403 Forbidden`.
3. **Autorización Basada en Roles con JWT**:
    - Los roles (`ROLE_USER`, `ROLE_ADMIN`) deben ser cargados por el `UserDetailsService` y incluidos como claims dentro del JWT al generarlo.
    - Aplica las siguientes reglas de autorización utilizando los roles extraídos del JWT:
      - **Gestión de Eventos** (`POST /api/events`, `PUT /api/events/{id}`, `DELETE /api/events/{id}`): Solo accesibles para usuarios con el rol `ROLE_ADMIN`.
      - **Gestión de Participantes** (`POST /api/participants`, `PUT /api/participants/{id}`, `DELETE /api/participants/{id}`): Solo accesibles para usuarios con el rol `ROLE_ADMIN`.
      - **Consultar todos los participantes** (`GET /api/participants`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
      - **Consultar participantes de un evento** (`GET /api/events/{eventId}/participants`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
      - **Otros Endpoints** (ej., `GET /api/events/{id}`, `GET /api/events`, `POST /api/events/{eventId}/participants/{participantId}`, `DELETE /api/events/{eventId}/participants/{participantId}`, `GET /api/participants/{id}`, `GET /api/participants/{participantId}/events)`: Accesibles para cualquier usuario autenticado (con un JWT válido, rol `ROLE_USER` o `ROLE_ADMIN`).

## Ejercicio 2: Sistema de Gestión de Proyectos y Tareas

**Descripción**: Modifica tu API RESTful de gestión de Proyectos y Tareas para que la autenticación y autorización se realicen utilizando JWT.

**Requisitos de Seguridad**:

1. **Autenticación de Usuarios con JWT**:
    - Implementa un endpoint de autenticación (`POST /api/auth/authenticate`) que reciba credenciales de usuario (username/password).
    - Si las credenciales son válidas (contra los usuarios en base de datos configurados en `UserDetailsService`), la API debe generar y devolver un **JSON Web Token** (JWT) en la respuesta.
    - Si las credenciales son inválidas, la API debe devolver un `401 Unauthorized`.
2. **Validación de JWT en Solicitudes Protegidas**:
    - Configura Spring Security para que todas las solicitudes a endpoints protegidos requieran un JWT válido en el encabezado `Authorization: Bearer <token>`.
    - Implementa un filtro (`JwtRequestFilter`) que intercepte las solicitudes, extraiga y valide el JWT (verificando firma, expiración y usuario).
    - Si el JWT es válido, la solicitud debe proceder. Si es inválido, ausente o expirado, la API debe devolver un `401 Unauthorized` o `403 Forbidden`.
3. **Autorización Basada en Roles con JWT**:
    - Los roles (`ROLE_USER`, `ROLE_ADMIN`) deben ser cargados por el `UserDetailsService` e incluidos como claims dentro del JWT al generarlo.
    - Aplica las siguientes reglas de autorización utilizando los roles extraídos del JWT:
      - **Gestión de Proyectos** (`POST /api/projects`, `PUT /api/projects/{id}`, `DELETE /api/projects/{id}`): Solo accesibles para usuarios con el rol `ROLE_ADMIN`.
      - **Otros Endpoints** (ej., `GET /api/projects/{id}`, `GET /api/projects`, `POST /api/tasks`, `GET /api/tasks/{id}`, `GET /api/tasks`, `PUT /api/tasks/{id}`, `DELETE /api/tasks/{id}`, `GET /api/projects/{projectId}/tasks`, `GET /api/tasks/by-status?status={estado}`): Accesibles para cualquier usuario autenticado (con un JWT válido, rol `ROLE_USER` o `ROLE_ADMIN`).
