# Proyecto del Módulo 7: Sistema de Gestión de Campus Universitario

Este es el momento de aplicar y consolidar todos los conocimientos adquiridos en el módulo. Vas a construir una API RESTful completa y robusta para un Sistema de Gestión de Campus Universitario, integrando seguridad, manejo de errores, validaciones, documentación y gestión eficiente de datos.

Este proyecto te permitirá demostrar tu capacidad para desarrollar una API de nivel profesional con Spring Boot.

## Descripción del Proyecto

Desarrollarás una API RESTful para gestionar un sistema de Campus Universitario, incluyendo las siguientes entidades principales:

- Estudiantes (Students)
- Cursos (Courses)
- Profesores (Professors)
- Inscripciones (Enrollments): Representa la relación entre Estudiantes y Cursos.

## Requisitos Funcionales y Técnicos

### 1. Autenticación y Autorización

- **Usuarios Internos**: Gestionar usuarios del sistema (nombre de usuario, contraseña) para autenticación basada en JWT. Estos usuarios pueden persistirse en la base de datos.
- **Login JWT**: Debe existir un endpoint de autenticación que reciba credenciales de usuario y, si son válidas, genere y devuelva un JWT válido.
- **Roles**:
  - `ROLE_ADMIN`: Deberá tener acceso completo para realizar operaciones de creación, lectura, actualización y eliminación (CRUD) en todas las entidades del sistema (Estudiantes, Cursos, Profesores, e Inscripciones).
  - `ROLE_STUDENT`: Deberá poder consultar su propio perfil, visualizar los cursos en los que está inscrito y gestionar sus propias inscripciones/cancelaciones de cursos disponibles. No deberá tener permisos para acceder o modificar los datos de otros estudiantes.
  - **Asignación de Roles**: Los usuarios internos deben tener roles asignados y estos roles deben incluirse como claims dentro del JWT.
- **Filtrado JWT**: Se debe implementar un mecanismo de filtro para interceptar, validar y establecer el contexto de seguridad con el JWT en cada solicitud que requiera protección.

### 2. Modelado de Datos

- **Entidades**: Define las entidades `Student`, `Course`, `Professor`, `Enrollment` con sus atributos relevantes.
  - `Student`: Incluirá atributos como identificador, nombre, dirección de correo electrónico (que debe ser única), número de estudiante (también único), entre otros que consideres pertinentes.
  - `Course`: Deberá tener un identificador, nombre, un código de curso (único), descripción, la capacidad máxima de estudiantes y una referencia al profesor asociado.
  - `Professor`: Contendrá un identificador, nombre, departamento al que pertenece y dirección de correo electrónico (única).
  - `Enrollment`: Representará la inscripción de un estudiante en un curso, incluyendo un identificador, referencias al estudiante y al curso, la fecha de inscripción y el estado de la inscripción (ej., "ACTIVO", "COMPLETADO", "RETIRADO").

- **Relaciones**:
  - `Student` y `Course`: Se relacionarán a través de la entidad `Enrollment` en una relación de muchos a muchos.
  - `Professor` y `Course`: Establecerán una relación de uno a muchos (un profesor puede impartir varios cursos, pero cada curso es impartido por un único profesor).

- **DTOs con Java Records**: Utiliza **Java Records** para modelar la mayoría de tus Data Transfer Objects (DTOs), tanto para las solicitudes (cuerpos de entrada) como para las respuestas de tu API (ej., DTOs para crear o actualizar un estudiante, para listar detalles de un curso, etc.). Esto incluye el DTO utilizado para el login.

### 3. Operaciones CRUD para Entidades Principales

Implementa las operaciones CRUD completas para cada tipo de entidad, aplicando las reglas de seguridad basadas en roles definidas:

- **Gestión de Estudiantes**:
  - Se debe permitir el registro de nuevos estudiantes (solo `ROLE_ADMIN`).
  - La consulta de estudiantes puede ser individual (por ID) o de una lista completa. La consulta individual puede ser para `ROLE_ADMIN` o para el estudiante (`ROLE_STUDENT`) que accede a su propio perfil. La lista completa es solo para `ROLE_ADMIN`.
  - Se debe permitir la actualización de la información de un estudiante (solo `ROLE_ADMIN`, o `ROLE_STUDENT` para su propio perfil).
  - La eliminación de estudiantes solo será posible para `ROLE_ADMIN`.

- **Gestión de Cursos**:
  - La creación de nuevos cursos será exclusiva para `ROLE_ADMIN`.
  - La consulta de cursos (individual por ID o lista completa de disponibles) será accesible para `ROLE_ADMIN` y `ROLE_STUDENT`.
  - La actualización y eliminación de cursos solo podrá ser realizada por `ROLE_ADMIN`.

- **Gestión de Profesores**:
  - Todas las operaciones CRUD (crear, consultar, actualizar, eliminar) para profesores serán exclusivas para `ROLE_ADMIN`.

- **Gestión de Inscripciones**:
  - La inscripción de un estudiante en un curso será una operación para `ROLE_STUDENT`. Se deben incluir validaciones para la capacidad del curso y para evitar inscripciones duplicadas.
  - La consulta de todas las inscripciones será exclusiva para `ROLE_ADMIN`.
  - La consulta de una inscripción específica o la actualización de su estado será para `ROLE_ADMIN`.
  - La cancelación de una inscripción podrá ser realizada por `ROLE_ADMIN` o por el `ROLE_STUDENT` que realizó la inscripción.

### 4. Manejo de Excepciones Globales

- Implementa un `GlobalExceptionHandler` utilizando `@ControllerAdvice` para centralizar el manejo de errores. Este manejador deberá devolver un DTO `ErrorResponse` consistente con detalles claros sobre el problema.
- Manejar, al menos, escenarios que resultarían en los siguientes tipos de errores, asegurando que la API devuelva códigos HTTP apropiados y mensajes claros:
  - **Errores de validación de datos de entrada**: Cuando los datos enviados en una solicitud no cumplen con las reglas de validación (ej., campos obligatorios ausentes, formatos incorrectos).
  - **Recursos no encontrados**: Cuando se intenta acceder o manipular una entidad (estudiante, curso, profesor, inscripción) que no existe en la base de datos.
  - **Conflictos de datos**: Cuando una operación intentaría crear un recurso duplicado (ej., un estudiante con un email ya registrado, un curso con un código ya existente) o cuando se intenta eliminar un recurso que tiene dependencias activas (ej., un curso con estudiantes inscritos).
  - **Acceso denegado**: Cuando un usuario autenticado intenta realizar una acción para la cual no tiene los permisos o roles necesarios.
  - **Violaciones de reglas de negocio**: Cuando una operación no puede completarse debido a una regla de negocio específica de la aplicación (ej., intentar inscribir a un estudiante en un curso que ya ha alcanzado su capacidad máxima).
  - **Errores internos inesperados del servidor**: Para cualquier otra excepción no controlada que pueda ocurrir en la aplicación, asegurando una respuesta genérica y segura.

### 5. Validaciones Personalizadas

- Crea y aplica al menos **tres reglas de validación personalizadas** que no puedan ser cubiertas por las anotaciones estándar de Bean Validation. Estas reglas deben reflejar lógica de negocio específica y aplicarse en tus DTOs. Para cada validación personalizada, deberás definir una anotación y su clase validadora correspondiente. Algunos ejemplos de requisitos para estas validaciones incluyen:
  - Asegurar que el email de un estudiante pertenezca a un dominio específico de la universidad (ej., @alumnos.universidad.edu).
  - Validar que el código de un curso siga un patrón alfanumérico predefinido (ej., dos o tres letras seguidas de tres números, como "MAT-101" o "ING-205").
  - Verificar que un curso tenga capacidad disponible antes de permitir una nueva inscripción, considerando el número actual de inscritos.

### 6. Paginación y Filtrado Avanzado

- Implementa capacidades de **paginación** para todos los endpoints que devuelvan colecciones o listas de recursos (ej., listado de estudiantes, cursos, profesores, inscripciones). Esto permitirá a los clientes solicitar porciones específicas de datos.
- Además, añade funcionalidades de filtrado avanzado que permitan a los clientes consultar los recursos basándose en múltiples criterios. Los estudiantes deben decidir los criterios de filtrado más relevantes para cada entidad (ej., filtrar estudiantes por nombre o email, cursos por código o profesor asociado, inscripciones por estudiante o curso). Deberás utilizar las capacidades de Spring Data JPA para construir estas consultas dinámicas.

### 7. Documentación de API con OpenAPI/Swagger UI

- Integra **SpringDoc OpenAPI** en tu proyecto para generar automáticamente una documentación interactiva y visual de tu API.
- Configura la información básica de la API (título, versión, descripción general).
- Configura el esquema de seguridad **Bearer Authentication** para que Swagger UI permita a los usuarios introducir y enviar JWTs en las solicitudes a endpoints protegidos.
- Anota todos los controladores y endpoints con las anotaciones relevantes de OpenAPI (`@Tag`, `@Operation`, `@ApiResponse`, `@Parameter`, `@Schema`, `@SecurityRequirement`) para proporcionar una documentación clara, completa y utilizable por otros desarrolladores.
- Asegúrate de que los modelos de datos (DTOs), especialmente los que hayas implementado como `Record`s, estén bien documentados con `@Schema` para describir sus propiedades y ejemplos.

### 8. Buenas Prácticas y Calidad del Código

- **Organización**: Mantén una estructura de paquetes clara y lógica (ej., controller, service, repository, entity, dto, exception, config, validation, specification).
- **Comentarios**: El código debe estar bien comentado, explicando la lógica compleja, algoritmos y propósito de métodos/clases.
- **Nombres**: Utiliza nombres de variables, métodos y clases significativos, legibles y que reflejen su propósito.
- **Consistencia**: Asegura que las respuestas HTTP y los mensajes de error devueltos por la API sigan un formato consistente en toda la aplicación.
- **Transaccionalidad**: Asegúrate de que las operaciones que impliquen múltiples cambios en la base de datos sean transaccionales (@Transactional) para garantizar la integridad de los datos.
