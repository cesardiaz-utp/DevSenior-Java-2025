# Ejercicios adicionales

## Ejercicio 1: Sistema de Gestión de Eventos y Participantes

Construirás una API RESTful para gestionar Eventos y sus Participantes. Un evento puede tener múltiples participantes, y un participante puede inscribirse en múltiples eventos (una relación de muchos a muchos). El sistema permitirá administrar eventos, registrar participantes y gestionar sus inscripciones.

**Requisitos Funcionales:**

1. Gestión de Eventos:
    - **Registrar un nuevo evento**: Se debe poder crear un evento con un nombre, fecha, lugar y capacidad máxima. El nombre del evento debe ser único.
    - **Consultar un evento por ID**: Obtener los detalles de un evento específico, incluyendo la lista de participantes inscritos.
    - **Consultar todos los eventos**: Listar todos los eventos, mostrando también la cantidad de participantes actuales y la capacidad restante.
    - **Actualizar un evento**: Modificar la información de un evento existente (nombre, fecha, lugar, capacidad).
    - **Eliminar un evento**: Borrar un evento del sistema. Solo se podrá eliminar si no tiene participantes inscritos.
2. Gestión de Participantes:
    - **Registrar un nuevo participante**: Se debe poder crear un participante con un nombre, email (único) y número de teléfono.
    - **Consultar un participante por ID**: Obtener los detalles de un participante específico, incluyendo los eventos en los que está inscrito.
    - **Consultar todos los participantes**: Listar todos los participantes registrados en el sistema.
    - **Actualizar un participante**: Modificar la información de un participante existente.
    - **Eliminar un participante**: Borrar un participante del sistema.
3. Gestión de Inscripciones (Relación Evento-Participante):
    - **Inscribir un participante en un evento**: Registrar a un participante existente en un evento existente. Debe validarse que el evento no haya excedido su capacidad máxima. Un participante no puede inscribirse dos veces en el mismo evento.
    - **Cancelar la inscripción de un participante**: Desvincular a un participante de un evento.
    - **Consultar participantes de un evento**: Obtener la lista de todos los participantes inscritos en un evento específico.
    - **Consultar eventos de un participante**: Obtener la lista de todos los eventos en los que un participante específico está inscrito.
4. Manejo de Errores y Validaciones:
    - **Validaciones de entrada**: La API debe validar los datos de entrada (campos obligatorios, formatos de email, capacidades positivas, etc.).
    - **Recurso no encontrado**: Si se solicita un evento o participante por un ID que no existe, la API debe informar el recurso no encontrado.
    - **Conflicto de datos**: Si se intenta crear un evento con un nombre ya existente, o un participante con un email ya registrado, la API debe indicar un conflicto.
    - **Restricciones de eliminación**: Si se intenta eliminar un evento que tiene participantes, o un participante que está inscrito en eventos, la API debe rechazar la operación con un mensaje explicativo.
    - **Capacidad excedida**: Si se intenta inscribir a un participante en un evento que ya alcanzó su capacidad máxima, la API debe rechazar la inscripción.
    - **Inscripción duplicada**: Si se intenta inscribir a un participante en un evento en el que ya está, la API debe indicar un conflicto.
    - **Errores inesperados**: Cualquier otro error interno del servidor debe ser manejado de forma genérica y segura.
    - **Formato de respuesta de error**: Todas las respuestas de error deben seguir un formato estándar y consistente.

## Ejercicio 2: Sistema de Gestión de Proyectos y Tareas

Desarrollarás una API RESTful para gestionar Proyectos y Tareas. Cada tarea pertenecerá a un único proyecto. Los usuarios podrán administrar proyectos, crear y consultar tareas, y gestionar el estado de las tareas. Este sistema es fundamental para organizar el trabajo en equipos.

**Requisitos Funcionales:**

1. Gestión de Proyectos:
    - **Registrar un nuevo proyecto**: Se debe poder crear un proyecto con un nombre, descripción y una fecha de inicio. El nombre del proyecto debe ser único.
    - **Consultar un proyecto por ID**: Obtener los detalles de un proyecto específico, incluyendo la lista de tareas asociadas.
    - **Consultar todos los proyectos**: Listar todos los proyectos registrados.
    - **Actualizar un proyecto**: Modificar la información de un proyecto existente.
    - **Eliminar un proyecto**: Borrar un proyecto del sistema. Si un proyecto tiene tareas asociadas, todas sus tareas también deben eliminarse (comportamiento en cascada).
2. Gestión de Tareas:
    - **Crear una nueva tarea**: Registrar una tarea con un título, descripción (opcional), fecha límite y un estado (ej. "Pendiente", "En Progreso", "Completada"). La tarea debe estar asociada a un proyecto existente.
    - **Consultar una tarea por ID**: Obtener los detalles de una tarea específica, incluyendo la información del proyecto al que pertenece.
    - **Consultar todas las tareas**: Listar todas las tareas, mostrando también el proyecto asociado a cada una.
    - **Actualizar una tarea**: Modificar la información de una tarea existente, incluyendo su título, descripción, fecha límite o estado. No se debe permitir cambiar el proyecto al que pertenece una tarea.
    - **Eliminar una tarea**: Borrar una tarea del sistema.
3. Consultas y Filtros:
    - **Consultar tareas por proyecto**: Obtener todas las tareas asociadas a un proyecto específico (buscando por el ID del proyecto).
    - **Consultar tareas por estado**: Obtener todas las tareas que se encuentren en un estado particular (ej. "Pendientes" o "Completadas").
4. Manejo de Errores y Validaciones:
    - **Validaciones de entrada**: La API debe validar los datos de entrada para proyectos y tareas (ej. campos obligatorios, fechas válidas, estados permitidos).
    - **Asociación a recurso existente**: Al crear una tarea, el proyecto al que se asocia debe existir. Si el proyecto no existe, la operación debe fallar.
    - **Recurso no encontrado**: Si se solicita un proyecto o tarea por un ID que no existe, la API debe informar el recurso no encontrado.
    - **Conflicto de datos**: Si se intenta crear un proyecto con un nombre ya existente, la API debe indicar un conflicto.
    - **Errores inesperados**: Cualquier otro error interno del servidor debe ser manejado de forma genérica y segura.
    - **Formato de respuesta de error**: Todas las respuestas de error deben seguir un formato estándar y consistente.
