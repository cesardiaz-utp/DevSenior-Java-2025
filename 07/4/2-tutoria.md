# Tutoría: Java Records, MapStruct, el Desarrollo Ágil y Scrum

## 1. Bienvenida y Recopilación de Dudas Rápidas

Hoy exploraremos cómo modernizar nuestros DTOs con Java Records, cómo simplificar el mapeo de objetos con MapStruct y cómo organizar nuestro trabajo de desarrollo con Desarrollo Ágil y Scrum. ¿Tienen alguna duda inicial sobre estos temas?

## 2. Presentación del Proyecto de la Tutoría

Para esta tutoría, trabajaremos sobre los proyectos de los ejercicios adicionales de las Clases 2/3 (Sistema de Gestión de Eventos y Participantes, o Sistema de Gestión de Proyectos y Tareas). El objetivo es refactorizar algunos DTOs para usar `Record` clases, integrar MapStruct para el mapeo y aplicar conceptos de Scrum al proceso de desarrollo.

## 3. Refactorización con Java `Record`s y Mapeo con `MapStruct`

- **Descripción Detallada**:
  - **Introducción a Records**: Iniciaremos explicando en detalle qué son los `record`s, sus beneficios clave (concisión, inmutabilidad por defecto, reducción de _boilerplate code_) y cuándo son más adecuados para usar (principalmente como DTOs de transporte de datos y clases de valor simples). Discutiremos la sintaxis y los métodos generados automáticamente.
  - **Refactorizar DTOs con Records**: Elegiremos al menos dos DTOs existentes de los ejercicios anteriores que sean buenos candidatos para convertirse en `record`s (ej., `EventDTO`, `ParticipantDTO`, `ProjectDTO`, `TaskDTO`, o DTOs de solicitud/respuesta específicos). Realizaremos la refactorización en vivo, mostrando cómo las propiedades se convierten en componentes y cómo las anotaciones de validación (`@NotBlank`, `@NotNull`, `@Positive`, etc.) y las de OpenAPI (`@Schema`, si ya las aplicaron o si se van a aplicar en el proyecto final) se trasladan a los componentes del `record`.
  - **Introducción a MapStruct y Configuración**: Explicaremos la necesidad de un mapeador de objetos como MapStruct para transformar entre entidades y DTOs, destacando sus ventajas (rendimiento, seguridad en tiempo de compilación). Añadiremos las dependencias necesarias en el `pom.xml` y configuraremos el `maven-compiler-plugin`.
  - **Implementación de Mappers con MapStruct**: Crearemos interfaces de mapper utilizando `@Mapper` para al menos dos entidades (ej., `EventMapper`, `ParticipantMapper`). Demostraremos cómo definir métodos de mapeo (`toEntity`, `toDto`, `toDtoList`) y cómo MapStruct maneja los records de forma nativa. Cubriremos el uso de `@Mapping` para campos con nombres diferentes o para ignorar propiedades.
  - **Actualizar Servicios y Controladores**: Modificaremos los servicios y controladores para inyectar y utilizar las interfaces de mapper generadas por MapStruct. Demostraremos cómo el servicio ahora delega la conversión entre entidades y DTOs al mapper, simplificando su lógica.
- **Pruebas**: Realizaremos pruebas con Postman/Insomnia (o Swagger UI si ya está configurado) para verificar que la serialización y deserialización de los `record`s y que el mapeo con MapStruct funcionan correctamente al crear, actualizar y consultar recursos, y que las validaciones se activan y son manejadas por el `GlobalExceptionHandler`.

## 4. Exploración de Desarrollo Ágil y Scrum

- **Descripción Detallada**:
  - **Introducción a lo Ágil**: Discutiremos el Manifiesto Ágil y sus principios, contrastándolos con metodologías más tradicionales.
  - **Roles y Eventos de Scrum**: Explicaremos los roles de Product Owner, Scrum Master y Equipo de Desarrollo, su autonomía y responsabilidades específicas. Describiremos los eventos clave: Sprint Planning, Daily Scrum, Sprint Review y Sprint Retrospective, analizando su propósito y duración fija (_time-boxing_).
  - **Artefactos de Scrum y Trabajo en Grupo**: Explicaremos el Product Backlog, Sprint Backlog y el Incremento. Mostraremos ejemplos concretos de cómo se podrían aplicar estos artefactos a nuestro proyecto actual de APIs. Haremos énfasis en cómo Scrum fomenta la **colaboración, la auto-organización y la responsabilidad compartida** dentro del equipo.
- **Actividad Práctica / Discusión Dirigida**:
  - Tomaremos una nueva característica para nuestro proyecto de API (ej., "Permitir a los participantes calificar eventos") y, en un formato de discusión guiada, plantearemos:
    - ¿Cómo se formularía esta característica como una Historia de Usuario para el Product Backlog, incluyendo criterios de aceptación claros?
    - ¿Qué tipo de tareas de desarrollo (diseño de endpoints, lógica de negocio, etc.) se derivarían de esta historia para el Sprint Backlog?
    - Si estuviéramos en un Daily Scrum al implementar esta característica, ¿qué elementos del progreso, planes o impedimentos se compartirían?
    - ¿Cómo se presentaría esta característica terminada en un Sprint Review y qué tipo de retroalimentación se buscaría de los stakeholders (ej., ¿el endpoint es intuitivo para el frontend?, ¿maneja los errores esperados?)?

## Cierre y Próximos Pasos

En esta clase, hemos explorado cómo modernizar nuestro código con Java Records para DTOs más concisos e inmutables, cómo simplificar el tedioso mapeo de objetos con MapStruct, y hemos comprendido los fundamentos del Desarrollo Ágil y la metodología Scrum, esenciales para la gestión efectiva de proyectos de software en equipos.

La siguiente fase será el Proyecto Final del Módulo 7, donde integrarán TODO lo aprendido en las Clases 1 a 4 (incluyendo la seguridad JWT, manejo de excepciones, validaciones, documentación, paginación, filtrado, Java Records y MapStruct) para construir una aplicación Spring Boot completa y robusta, siguiendo posiblemente algunos principios de organización ágil.

¡Felicidades por completar este módulo!
