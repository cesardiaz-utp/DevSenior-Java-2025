# Ejercicio Autoguiado: Sistema de Gestión de Proyectos Simples

## Objetivos del Ejercicio

Al completar este ejercicio, el estudiante será capaz de:

- **Aplicar Maven**: Crear y configurar un proyecto Maven desde cero, incluyendo la gestión de dependencias básicas.
- **Diseñar POO con Relaciones**: Modelar entidades (`Project` y `Task`) con una relación uno-a-muchos, utilizando arrays de tamaño fijo para la contención de datos.
- **Manejar Arrays**: Implementar lógica para agregar, buscar y manipular elementos en arrays de tamaño fijo, incluyendo el manejo de la capacidad.
- **Implementar Excepciones Personalizadas**: Crear y utilizar excepciones personalizadas (`checked` y `unchecked`) para manejar situaciones de error específicas del dominio de la aplicación.
- **Control de Flujo con Excepciones**: Aplicar `try-catch` y `throws` para manejar y propagar excepciones de manera adecuada en la jerarquía de llamadas de métodos.
- **Desarrollar una Aplicación de Consola**: Crear una aplicación de consola funcional para interactuar con el sistema de gestión, demostrando los casos de uso exitosos y los escenarios de error.

## Instrucciones Detalladas

### Paso 1: Configuración del Proyecto Maven

1. **Crear Proyecto Maven**:
    - Abre VS Code y, desde la Paleta de Comandos (`Ctrl + Shift + P`), selecciona `Maven: Create Maven Project`.
    - Elige el `maven-archetype-quickstart`.
    - Define el `groupId: com.devsenior.projects`
    - Define el `artifactId: project-management`
    - Asegúrate de que las propiedades `maven.compiler.source` y `maven.compiler.target` en tu `pom.xml` estén configuradas a 21 (o la versión de JDK que estés usando).

### Paso 2: Diseño de las Entidades y Excepciones

1. Clases de Entidad (en `src/main/java/com/projects/model`):
    - Clase `Task`:
      - **Atributos**:
        - `taskId: String` (unique ID within a project)
        - `description: String`
        - `priority: String` (e.g., "LOW", "MEDIUM", "HIGH")
        - `completed: boolean` (default `false`)
      - **Métodos**:
        - Constructor: `Task(String taskId, String description, String priority)`
        - Getters para todos los atributos.
        - `setCompleted(boolean completed)`: Para cambiar el estado de la tarea.
        - `toString()`: Para una representación legible de la tarea.
        - Implementa `equals()` y `hashCode()` basado en `taskId`.
    - Clase `Project`:
      - **Atributos**:
        - `projectId: String` (unique ID)
        - `name: String` (Project name)
        - `description: String`
        - `tasks`: `Task[]` (Un array para almacenar tareas de este proyecto, ej. `new Task[5];` - elige un tamaño fijo adecuado para tus pruebas).
        - `taskCount: int` (Para llevar el conteo de tareas actualmente almacenadas en el array interno de este proyecto).
      - **Métodos**:
        - Constructor: `Project(String projectId, String name, String description)`
        - Getters para todos los atributos.
        - `addTask(Task task)`:
          - Valida la `task` y sus atributos. Lanza `InvalidDataException` si son inválidos.
          - Verifica si el array `tasks` de este proyecto está lleno. Si sí, lanza `ArrayFullException`.
          - Verifica si una tarea con el mismo `taskId` ya existe en este proyecto. Si sí, lanza `IdAlreadyExistsException`.
          - Añade la tarea al array y actualiza `taskCount`.
        - `markTaskCompleted(String taskId)`:
          - Valida `taskId`. Lanza `InvalidDataException`.
          - Busca la tarea por taskId en el array `tasks` de este proyecto. Si no la encuentra, lanza `TaskNotFoundException`.
          - Si la encuentra, marca la tarea como `completed = true`. Si ya estaba completada, lanza `TaskAlreadyCompletedException`.
        - `findTask(String taskId)`:
          - Valida `taskId`. Lanza `InvalidDataException`.
          - Busca la tarea por `taskId` en el array tasks de este proyecto. Si no la encuentra, lanza `TaskNotFoundException`.
          - Retorna la `Task` encontrada.
        - `listTasks()`: Imprime todas las tareas en el array de este proyecto que no sean `null`.
        - Implementa `equals()` y `hashCode()` basado en `projectId`.
        - `toString()`: Para una representación legible del proyecto y sus tareas.
2. Clases de Excepción (en `src/main/java/com/projects/exceptions`):
    - **Verificadas (`checked`)**:
      - `ProjectNotFoundException`: Cuando un proyecto no se encuentra.
      - `TaskNotFoundException`: Cuando una tarea no se encuentra dentro de un proyecto.
      - `TaskAlreadyCompletedException`: Cuando se intenta completar una tarea que ya está completada.
      - `ArrayFullException`: Cuando se intenta añadir un proyecto o una tarea y el array correspondiente ya está lleno.
    - **No Verificadas (`unchecked`)**:
      - `InvalidDataException`: Para cuando los datos de entrada (ej. ID nulo, descripción vacía, prioridad inválida) no son válidos.
      - `IdAlreadyExistsException`: Cuando se intenta agregar un proyecto o una tarea con un ID que ya existe (a nivel de `ProjectManager` para proyectos, y a nivel de `Project` para tareas).
    - Asegúrate de que cada excepción tenga al menos un constructor que reciba un `String message`.

### Paso 3: Implementación de la Lógica de Negocio

1. Clase `ProjectManager` (en `src/main/java/com/projects/manager`):
    - **Atributos**:
      - `projects: Project[]` (array de proyectos, de tamaño fijo, ej. `new Project[5];` - elige un tamaño fijo adecuado).
      - `projectCount: int` (para llevar el conteo de proyectos actualmente almacenados en el array).
    - **Métodos**:
      - `addProject(Project project)`:
        - Valida el `project` y sus atributos. Lanza `InvalidDataException`.
        - Verifica si el array `projects` está lleno. Lanza `ArrayFullException`.
        - Verifica si ya existe un proyecto con el mismo `projectId`. Lanza `IdAlreadyExistsException`.
        - Añade el proyecto al array y actualiza `projectCount`.
      - `findProject(String projectId)`:
        - Valida `projectId`. Lanza `InvalidDataException`.
        - Busca el proyecto por `projectId` en el array. Si no lo encuentra, lanza `ProjectNotFoundException`.
        - Retorna el `Project` encontrado.
      - `addTaskToProject(String projectId, Task task)`:
        - Valida `projectId` y `task`. Lanza `InvalidDataException`.
        - Busca el proyecto usando `findProject(projectId)`. Si el proyecto no existe, `findProject` lanzará `ProjectNotFoundException`.
        - Si el proyecto es encontrado, llama al método `addTask()` del `Project` encontrado. Observa cómo esto podría lanzar `InvalidDataException`, `ArrayFullException` o `IdAlreadyExistsException` (provenientes de la clase `Project`).
      - `markTaskCompletedInProject(String projectId, String taskId)`:
        - Valida `projectId` y `taskId`. Lanza `InvalidDataException`.
        - Busca el proyecto usando `findProject(projectId)`. Si no existe, `findProject` lanzará `ProjectNotFoundException`.
        - Si el proyecto es encontrado, llama al método `markTaskCompleted()` del `Project` encontrado. Esto podría lanzar `TaskNotFoundException` o `TaskAlreadyCompletedException`.
      - `listAllProjects()`: Imprime todos los proyectos en el array que no sean `null`. Para cada proyecto, llama a su método `listTasks()` para mostrar sus tareas asociadas.

### Paso 4: Pruebas en la Clase Principal `App.java`

Modificar `src/main/java/com/projects/App.java`:

- Crea una instancia de `ProjectManager`.
- Implementa una secuencia de llamadas a los métodos de `ProjectManager` dentro de bloques `try-catch`. Esto es crucial para manejar las excepciones y demostrar la robustez del sistema.
- **Casos Exitosos**:
  - Añadir al menos dos proyectos válidos.
  - Añadir varias tareas a cada uno de esos proyectos (asegúrate de que los IDs de tareas sean únicos _dentro de cada proyecto_).
  - Marcar algunas tareas como completadas.
  - Buscar un proyecto y una tarea específica.
  - Listar todos los proyectos y sus tareas.
- **Casos Fallidos (con captura de excepciones)**:
  - Intenta añadir un proyecto con **datos inválidos** (ej. nombre vacío).
  - Intenta añadir un proyecto con un **ID duplicado**.
  - Intenta añadir **más proyectos de los que el array principal puede contener** (para probar `ArrayFullException`).
  - Intenta añadir una tarea a un **proyecto que no existe**.
  - Intenta añadir una tarea con **datos inválidos** (ej. descripción nula).
  - Intenta añadir una tarea con un **ID duplicado** _dentro del mismo proyecto_.
  - Intenta añadir **más tareas de las que el array interno de un proyecto puede contener** (para probar `ArrayFullException` desde `Project`).
  - Intenta marcar una tarea como completada en un **proyecto inexistente**.
  - Intenta marcar una tarea como completada que **no existe** en el proyecto especificado.
  - Intenta marcar una tarea como completada que **ya lo está**.
  - Intenta buscar un proyecto/tarea que **no existe**.
  - Utiliza `e.getMessage()` para mostrar mensajes claros y amigables al usuario.
  - Puedes usar `e.printStackTrace()` para depuración, pero coméntalo o explica que en un entorno de producción se usaría un sistema de logging.

### Paso 5: Compilación y Ejecución con Maven

- **Compilar el proyecto**:
  - Abre la terminal integrada en VS Code.
  - Navega al directorio raíz de tu proyecto (`project-management`).
  - Ejecuta: `mvn clean install`
- **Ejecutar la aplicación**:
  - Una vez que el build sea exitoso, ejecuta el JAR generado:
    - `java -jar target/project-management-1.0.0-SNAPSHOT.jar`
  - Observa la salida en la terminal, verificando tanto los casos exitosos como los mensajes de error de las excepciones capturadas.

## Criterios de Evaluación (Autónoma)

Utiliza esta lista para autoevaluar tu solución:

1. **Estructura del Proyecto Maven**:
    - ¿El proyecto se crea correctamente con Maven?
    - ¿El `pom.xml` está bien configurado con `groupId`, `artifactId` y las versiones de compilador?
    - ¿Se añaden las dependencias de Log4j (opcional para el ejercicio, pero buena práctica)?
2. **Diseño de Clases y POO**:
    - ¿Las clases `Project` y `Task` están correctamente definidas con sus atributos y métodos?
    - ¿Se implementan `equals()`, `hashCode()` y `toString()` correctamente en ambas clases de entidad?
    - ¿La clase `Project` contiene un array de `Tasks` y gestiona su propio `taskCount`?
    - ¿Hay una clara distinción entre la lógica de `Project` (gestión de sus propias tareas) y `ProjectManager` (gestión de proyectos)?
3. **Manejo de Arrays**:
    - ¿Se utilizan arrays de tamaño fijo (`Project[]` y `Task[]`) para almacenar los datos?
    - ¿Se maneja correctamente el `projectCount` y `taskCount` para saber cuántos elementos hay y la próxima posición disponible?
    - ¿Los métodos que añaden elementos (`addProject`, `addTask`) verifican si el array está lleno antes de intentar añadir?
4. **Implementación de Excepciones Personalizadas**:
    - ¿Se han creado todas las clases de excepción especificadas (`ProjectNotFoundException`, `TaskNotFoundException`, `TaskAlreadyCompletedException`, `ArrayFullException`, `InvalidDataException`, `IdAlreadyExistsException`)?
    - ¿Las excepciones `checked` extienden de `Exception` y las `unchecked` de `RuntimeException`?
    - ¿Cada excepción tiene un constructor que recibe un mensaje?
5. **Lanzamiento y Propagación de Excepciones**:
    - ¿Los métodos en `Project` y `ProjectManager` lanzan las excepciones correctas en las situaciones de error apropiadas?
    - ¿Los métodos que llaman a otros métodos que lanzan checked exceptions (`throws`) las declaran en su firma o las manejan con `try-catch`?
6. **Manejo de Excepciones en `App.java`**:
    - ¿La clase `App.java` tiene bloques `try-catch` para manejar las excepciones?
    - ¿Se demuestran tanto los casos exitosos como los escenarios de error, capturando las excepciones?
    - ¿Se utiliza `e.getMessage()` para proporcionar feedback informativo al usuario?
7. **Consistencia de Nomenclatura**:
    - ¿Todos los nombres de código (paquetes, clases, atributos, métodos, variables, groupId, artifactId) están en **inglés**?
8. **Funcionalidad General**:
    - ¿La aplicación de consola ejecuta los comandos de prueba sin errores inesperados y produce la salida esperada?

¡Este ejercicio es un excelente paso para consolidar tus conocimientos y prepararte para conceptos más avanzados en Java!

¡Mucho éxito!
