# Tutoría: Pruebas Unitarias con JUnit 5 y Mockito

## 1. Bienvenida y Sondeo Inicial

- **Bienvenida**: ¡Es genial tenerlos aquí! La clase de hoy es fundamental para que puedan escribir código robusto y de calidad.
- **Repaso rápido de objetivos**: En esta sesión, buscamos que se sientan cómodos escribiendo y entendiendo pruebas unitarias. Nuestro enfoque será práctico, aplicando JUnit y Mockito al proyecto que les presenté.
- **Sondeo inicial**: Me gustaría escuchar de ustedes:
  - ¿Qué aspecto de la Clase 3 les resultó más desafiante? ¿JUnit 5, Mockito, o quizás la configuración inicial del proyecto?
  - ¿Lograron configurar el proyecto de "Gestión de Tareas Simples" en VS Code y copiar las clases base?
  - ¿Pudieron empezar a escribir alguna prueba o encontraron algún obstáculo al ejecutar `mvn test`?

## 2. Proyecto Práctico: Sistema de Gestión de Tareas Simples

### Requisitos Funcionales

La clase principal que vamos a desarrollar y probar será `TaskManager`.

- **Crear Tarea**: Se puede crear una tarea nueva con un título y una descripción.
- **Listar Tareas**: Se pueden obtener todas las tareas existentes.
- **Completar Tarea**: Una tarea se puede marcar como completada.
- **Eliminar Tarea**: Una tarea se puede eliminar de la lista.
- **Generación de ID**: Cada tarea debe tener un ID único.

### Estructura del Proyecto (Maven y VS Code)

Mantendremos la estructura estándar de Maven, lo que facilitará la gestión de dependencias y la ejecución de pruebas en VS Code.

- `src/main/java/com/cursojava/tasks/model/Task.java`: Clase que representa una tarea.
- `src/main/java/com/cursojava/tasks/service/TaskManager.java`: Clase que gestiona las tareas (nuestra unidad bajo prueba).
- `src/main/java/com/cursojava/tasks/util/IdGenerator.java`: Una clase auxiliar que usaremos para generar IDs únicos (esta será una dependencia que _mockearemos_).
- `src/test/java/com/cursojava/tasks/service/TaskManagerTest.java`: Nuestra clase de pruebas para `TaskManager`.

### Detalle de las Clases y su Implementación Base

Para empezar, crearemos las clases básicas con la funcionalidad mínima necesaria.

1. **Clase `Task` (Modelo)**: En `src/main/java/com/cursojava/tasks/model/Task.java`

    ```Java
    package com.cursojava.tasks.model;

    import java.util.Objects; // Necesario para equals y hashCode

    public class Task {
        private String id;
        private String title;
        private String description;
        private boolean completed;

        public Task(String id, String title, String description) {
            if (id == null || id.trim().isEmpty()) {
                throw new IllegalArgumentException("El ID de la tarea no puede ser nulo o vacío.");
            }
            if (title == null || title.trim().isEmpty()) {
                throw new IllegalArgumentException("El título de la tarea no puede ser nulo o vacío.");
            }
            this.id = id;
            this.title = title;
            this.description = description;
            this.completed = false; // Por defecto, una tarea no está completada
        }

        // Getters
        public String getId() {
            return id;
        }

        public String getTitle() {
            return title;
        }

        public String getDescription() {
            return description;
        }

        public boolean isCompleted() {
            return completed;
        }

        // Setters
        public void setTitle(String title) {
            if (title == null || title.trim().isEmpty()) {
                throw new IllegalArgumentException("El título de la tarea no puede ser nulo o vacío.");
            }
            this.title = title;
        }

        public void setDescription(String description) {
            this.description = description;
        }

        public void setCompleted(boolean completed) {
            this.completed = completed;
        }

        @Override
        public String toString() {
            return "Task{" +
                  "id='" + id + '\'' +
                  ", title='" + title + '\'' +
                  ", description='" + description + '\'' +
                  ", completed=" + completed +
                  '}';
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Task task = (Task) o;
            return Objects.equals(id, task.id); // Tareas son iguales si sus IDs son iguales
        }

        @Override
        public int hashCode() {
            return Objects.hash(id);
        }
    }
    ```

2. **Clase `IdGenerator` (Utilidad - Colaborador a Mockear)**: En `src/main/java/com/cursojava/tasks/util/IdGenerator.java`:

    ```Java
    package com.cursojava.tasks.util;

    import java.util.UUID;

    public class IdGenerator {
        public String generateUniqueId() {
            return UUID.randomUUID().toString();
        }
    }
    ```

3. **Clase `TaskManager` (Unidad Bajo Prueba)**: En `src/main/java/com/cursojava/tasks/service/TaskManager.java`:

    ```Java
    package com.cursojava.tasks.service;

    import com.cursojava.tasks.model.Task;
    import com.cursojava.tasks.util.IdGenerator;
    import java.util.ArrayList;
    import java.util.List;
    import java.util.Optional; // Para el método findTaskById

    public class TaskManager {
        private final int MAX_TASKS = 50;
        private final Task[] tasks;
        private final IdGenerator idGenerator; // Nuestra dependencia

        private int taskCount;

        // Constructor que recibe el IdGenerator (para poder pasar un mock)
        public TaskManager(IdGenerator idGenerator) {
            this.idGenerator = idGenerator;

            this.tasks = new Task[MAX_TASKS];
            this.taskCount = 0;
        }

        /**
         * Crea y añade una nueva tarea.
        * Genera un ID único para la tarea.
        * @param title El título de la tarea.
        * @param description La descripción de la tarea.
        * @return La tarea creada.
        * @throws IllegalArgumentException si el título es nulo o vacío.
        */
        public Task createTask(String title, String description) {
            if (title == null || title.trim().isEmpty()) {
                throw new IllegalArgumentException("El título no puede ser nulo o vacío.");
            }
            String id = idGenerator.generateUniqueId(); // Usamos nuestro colaborador
            Task newTask = new Task(id, title, description);

            this.tasks[this.taskCount++] = newTask;

            return newTask;
        }

        /**
         * Obtiene una tarea por su ID.
        * @param id El ID de la tarea.
        * @return Contiene la tarea si se encuentra, o null si no.
        */
        public Task findTaskById(String id) {
            if (id == null || id.trim().isEmpty()) {
                throw new IllegalArgumentException("El ID de búsqueda no puede ser nulo o vacío.");
            }

            for(int i = 0; i < this.taskCount, i++) {
                if(tasks[i].getId().equals(id)) {
                    return tasks[i];
                }
            }

            return null;
        }

        /**
         * Marca una tarea como completada.
        * @param id El ID de la tarea a completar.
        * @return true si la tarea fue encontrada y marcada como completada, false en caso contrario.
        */
        public boolean completeTask(String id) {
            Task taskOpt = findTaskById(id);
            if (taskOpt != null) {
                taskOpt.setCompleted(true);
                return true;
            }
            return false;
        }

        /**
         * Elimina una tarea por su ID.
        * @param id El ID de la tarea a eliminar.
        * @return true si la tarea fue encontrada y eliminada, false en caso contrario.
        */
        public boolean deleteTask(String id) {
            if (id == null || id.trim().isEmpty()) {
                throw new IllegalArgumentException("El ID de eliminación no puede ser nulo o vacío.");
            }

            boolean found = false;
            for(int i = 0; i < this.taskCount; i++) {
                if(found) {
                    this.tasks[i - 1] = this.tasks[i];
                }
                else if(this.tasks[i].getId().equals(id)) {
                    found = true;
                }
            }
            if(found) {
              this.taskCount--;
            }

            return found;
        }

        /**
         * Obtiene una lista inmutable de todas las tareas.
        * @return Una lista de tareas.
        */
        public Task[] getAllTasks() {
            return this.tasks; // Retorna una copia para evitar modificación externa
        }

        /**
         * Resetea el gestor de tareas, eliminando todas las tareas.
        * Útil para limpiar el estado en pruebas.
        */
        public void clearTasks() {
            tasks = new Tasks[MAX_TASKS];
            this.taskCount = 0;
        }
    }
    ```

## 3. Explorando el Proyecto "Gestión de Tareas Simples" y Patrón AAA

- **Revisión del Proyecto**: Primero, haremos un recorrido rápido por las clases `Task`, `IdGenerator`, y `TaskManager` que ya tienen en sus VS Code. Nos enfocaremos en `TaskManager`, nuestra **unidad bajo prueba**, y cómo `IdGenerator` es un **colaborador** del que depende.
- **Diseño de Pruebas con AAA**: Vamos a tomar un requisito de nuestro `TaskManager` y, como equipo, aplicaremos el patrón **Arrange, Act, Assert** para diseñar la estructura de una prueba. Por ejemplo, pensaremos en cómo probar `createTask()` o `findTaskById()`.
- **Ejemplo en Código (Si hay tiempo)**: Si el grupo se siente cómodo, podemos escribir juntos la estructura básica de una prueba para `TaskManager` en `TaskManagerTest.java`, enfocándonos en la preparación (`@BeforeEach`) y la primera prueba `@Test` para `createTask()`.

## 4. Profundizando en JUnit 5 y Aserciones

- **Manos a la Obra con JUnit**: Basándonos en `TaskManager`, vamos a escribir y ejecutar pruebas para:
  - **Comprobar el éxito**: Pruebas para `createTask()` o `getAllTasks()` para asegurar que las operaciones se realizan correctamente y los datos son los esperados. Usaremos `assertEquals()`, `assertNotNull()`, `assertTrue()` / `assertFalse()`.
  - **Probar el manejo de errores**: Verificaremos que `createTask()` o `findTaskById()` lanzan la excepción correcta cuando se les dan entradas inválidas (por ejemplo, título nulo o ID vacío). Aquí, `assertThrows()` será nuestro mejor amigo.
- **Discusión Abierta**: Resolveremos dudas específicas sobre las anotaciones de JUnit 5 (`@DisplayName` para nombres claros de prueba) y cómo elegir la aserción adecuada para cada escenario.

## 5. Dominando Mockito para Aislamiento

- **El Problema de `IdGenerator`**: Nos centraremos en `TaskManager.createTask()`, que depende de `IdGenerator`. Explicaremos por qué no queremos usar el `IdGenerator` real en nuestras pruebas unitarias (IDs impredecibles, lentitud, etc.).
- **Creación y Stubbing de Mocks**: En `TaskManagerTest`, utilizaremos la anotación `@Mock` para crear una versión simulada de `IdGenerator`. Luego, con `when().thenReturn()`, "programaremos" nuestro `mockIdGenerator` para que devuelva IDs específicos y predecibles cuando `createTask()` lo llame.
- **Verificación de Interacciones**: Usaremos `verify()` para confirmar que `TaskManager` realmente llamó a `mockIdGenerator.generateUniqueId()` y que lo hizo el número de veces esperado. También veremos `never()` para escenarios donde un método no debería ser llamado.
- **Ejercicios Adicionales con Mockito**:
  - Crearemos una prueba para `createTask()` donde `IdGenerator` arroja una excepción para ver cómo `TaskManager` (hipotéticamente) la manejaría.
  - Discutiremos otros escenarios donde `verify()` es crucial (por ejemplo, asegurarnos de que no se llama un método de un colaborador en un caso de error).

## 6. Cierre y Preguntas Adicionales

- **Resumen de los Puntos Clave**: Hoy hemos visto cómo el patrón AAA organiza nuestras pruebas, cómo JUnit nos ayuda a verificar el comportamiento esperado, y cómo Mockito nos permite aislar nuestra unidad de prueba, lo cual es vital para pruebas unitarias de verdad.
- **Espacio Abierto para Preguntas**: Este es su momento para preguntar cualquier cosa que aún les cause inquietud sobre JUnit, Mockito o el proyecto de tareas. No hay preguntas tontas.
- **Próximos Pasos**:
  - Los animo a que sigan escribiendo más pruebas para las funcionalidades restantes de `TaskManager` (completar, eliminar, obtener todas).
  - Experimenten con diferentes escenarios de éxito y error para cada método.
  - ¡Sigan practicando la ejecución de `mvn test` en VS Code!
