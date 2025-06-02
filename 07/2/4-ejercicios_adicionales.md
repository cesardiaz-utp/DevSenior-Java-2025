# Ejercicios adicionales

Se tomarán los ejercicios adicionales de la clase anterior y debes aplicar los conceptos de esta clase.

## Ejercicio 1: Sistema de Gestión de Eventos y Participantes

Construirás una API RESTful para gestionar Eventos y sus Participantes. Un evento puede tener múltiples participantes, y un participante puede inscribirse en múltiples eventos (una relación de muchos a muchos). El sistema permitirá administrar eventos, registrar participantes y gestionar sus inscripciones, aplicando las reglas de seguridad aprendidas en la Clase 1.

**Requisitos Funcionales con Seguridad**:

1. **Usuarios y Roles**:
    - Para este ejercicio, los usuarios y sus roles (`ROLE_USER`, `ROLE_ADMIN`) se gestionarán en memoria a través de la configuración de `SecurityConfig` o `MyUserDetailsService`, tal como se explicó en la Clase.
    - La autenticación se realizará mediante **HTTP Basic**.
2. **Gestión de Eventos**:
    - Registrar un nuevo evento (`POST /api/events`): Solo accesible para usuarios con el rol `ROLE_ADMIN`. El nombre del evento debe ser único.
    - Consultar un evento por ID (`GET /api/events/{id}`): Accesible públicamente (sin autenticación).
    - Consultar todos los eventos (`GET /api/events`): Accesible públicamente (sin autenticación).
    - Actualizar un evento (`PUT /api/events/{id}`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
    - Eliminar un evento (`DELETE /api/events/{id}`): Solo accesible para usuarios con el rol `ROLE_ADMIN`. Solo se podrá eliminar si no tiene participantes inscritos.
3. **Gestión de Participantes**:
    - Registrar un nuevo participante (`POST /api/participants`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`). El email del participante debe ser único.
    - Consultar un participante por ID (`GET /api/participants/{id}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - Consultar todos los participantes (`GET /api/participants`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
    - Actualizar un participante (`PUT /api/participants/{id}`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
    - Eliminar un participante (`DELETE /api/participants/{id}`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
4. **Gestión de Inscripciones (Relación Evento-Participante)**:
    - **Inscribir un participante en un evento** (`POST /api/events/{eventId}/participants/{participantId}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`). Debe validarse que el evento no haya excedido su capacidad máxima. Un participante no puede inscribirse dos veces en el mismo evento.
    - **Cancelar la inscripción de un participante** `(DELETE /api/events/{eventId}/participants/{participantId}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - **Consultar participantes de un evento** (`GET /api/events/{eventId}/participants`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
    - **Consultar eventos de un participante** (`GET /api/participants/{participantId}/events`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
5. **Manejo de Errores y Validaciones**:
    - **Validaciones de entrada**: La API debe validar los datos de entrada (campos obligatorios, formatos de email, capacidades positivas, etc.).
    - **Recurso no encontrado**: Si se solicita un evento o participante por un ID que no existe, la API debe informar el recurso no encontrado (ej., `404 Not Found`).
    - **Conflicto de datos**: Si se intenta crear un evento con un nombre ya existente, o un participante con un email ya registrado, la API debe indicar un conflicto (ej., `409 Conflict`).
    - **Restricciones de eliminación**: Si se intenta eliminar un evento que tiene participantes, o un participante que está inscrito en eventos, la API debe rechazar la operación con un mensaje explicativo (ej., `400 Bad Request` o `409 Conflict`).
    - **Capacidad excedida**: Si se intenta inscribir a un participante en un evento que ya alcanzó su capacidad máxima, la API debe rechazar la inscripción (ej., `400 Bad Request`).
    - **Inscripción duplicada**: Si se intenta inscribir a un participante en un evento en el que ya está, la API debe indicar un conflicto (ej., `409 Conflict`).
    - **Errores inesperados**: Cualquier otro error interno del servidor debe ser manejado de forma genérica y segura (ej., `500 Internal Server Error`).
    - **Formato de respuesta de error**: Todas las respuestas de error deben seguir un formato estándar y consistente.

## Ejercicio 2: Sistema de Gestión de Proyectos y Tareas

**Descripción del Ejercicio**:

Desarrollarás una API RESTful para gestionar Proyectos y Tareas. Cada tarea pertenecerá a un único proyecto. Los usuarios podrán administrar proyectos, crear y consultar tareas, y gestionar el estado de las tareas, aplicando las reglas de seguridad aprendidas en la Clase.

**Requisitos Funcionales con Seguridad**:

1. **Usuarios y Roles**:
    - Para este ejercicio, los usuarios y sus roles (`ROLE_USER`, `ROLE_ADMIN`) se gestionarán en memoria a través de la configuración de `SecurityConfig` o `MyUserDetailsService`, tal como se explicó en la Clase.
    - La autenticación se realizará mediante **HTTP Basic**.
2. **Gestión de Proyectos**:
    - **Registrar un nuevo proyecto** (`POST /api/projects`): Solo accesible para usuarios con el rol `ROLE_ADMIN`. El nombre del proyecto debe ser único.
    - **Consultar un proyecto por ID** (`GET /api/projects/{id}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - **Consultar todos los proyectos** (`GET /api/projects`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - **Actualizar un proyecto** (`PUT /api/projects/{id}`): Solo accesible para usuarios con el rol `ROLE_ADMIN`.
    - **Eliminar un proyecto** (`DELETE /api/projects/{id}`): Solo accesible para usuarios con el rol `ROLE_ADMIN.` Si un proyecto tiene tareas asociadas, todas sus tareas también deben eliminarse (comportamiento en cascada).
3. **Gestión de Tareas**:
    - **Crear una nueva tarea** (`POST /api/tasks`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`). La tarea debe estar asociada a un proyecto existente.
    - **Consultar una tarea por ID** (`GET /api/tasks/{id}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - **Consultar todas las tareas** (`GET /api/tasks`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - **Actualizar una tarea** (`PUT /api/tasks/{id}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`). No se debe permitir cambiar el proyecto al que pertenece una tarea.
    - **Eliminar una tarea** (`DELETE /api/tasks/{id}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
4. **Consultas y Filtros**:
    - **Consultar tareas por proyecto** (`GET /api/projects/{projectId}/tasks`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
    - **Consultar tareas por estado** (`GET /api/tasks/by-status?status={estado}`): Accesible para cualquier usuario autenticado (`ROLE_USER` o `ROLE_ADMIN`).
5. **Manejo de Errores y Validaciones**:
    - **Validaciones de entrada**: La API debe validar los datos de entrada para proyectos y tareas (ej., campos obligatorios, fechas válidas, estados permitidos).
    - **Asociación a recurso existente**: Al crear una tarea, el proyecto al que se asocia debe existir. Si el proyecto no existe, la operación debe fallar (ej., `404 Not Found` o `400 Bad Request`).
    - **Recurso no encontrado**: Si se solicita un proyecto o tarea por un ID que no existe, la API debe informar el recurso no encontrado (ej., `404 Not Found`).
    - **Conflicto de datos**: Si se intenta crear un proyecto con un nombre ya existente, la API debe indicar un conflicto (ej., `409 Conflict`).
    - **Errores inesperados**: Cualquier otro error interno del servidor debe ser manejado de forma genérica y segura (ej., `500 Internal Server Error`).
    - **Formato de respuesta de error**: Todas las respuestas de error deben seguir un formato estándar y consistente.
