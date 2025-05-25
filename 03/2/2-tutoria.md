# Tutoría: Logs y Depuración con Log4j2 y SLF4j

## 1. Bienvenida y Sondeo Inicial

- "¿Cuál creen que es la principal desventaja de usar `System.out.println()` para el seguimiento de la aplicación en producción?"
- "¿Qué papel juega SLF4j en un sistema de logging, y cómo se diferencia de Log4j2?"
- "¿Para qué sirven los niveles de log y cómo los aplicarían para priorizar la información en una aplicación?"

## 2. Taller Práctico: Configuración y Uso de Logging Básico

- **Objetivo**: Los estudiantes configurarán Log4j2 en un proyecto Java sencillo y lo usarán para emitir mensajes de log con diferentes niveles mientras simulan una aplicación de gestión de tareas.
- **Descripción del Mini-Proyecto: "Gestor de Tareas Simple"**
Crearemos una aplicación de consola muy básica que simula la gestión de tareas. La idea es que la aplicación registre cada acción importante (creación de tarea, finalización, eliminación) y cualquier error que pueda ocurrir.
- **Actividad Guiada**:
  - **Paso 1: Creación del Proyecto**. El docente guiará a los estudiantes para crear un **nuevo proyecto Maven**. Nombre del proyecto sugerido: `simple-task-manager`.
- **Paso 2: Añadir Dependencias al `pom.xml`**. Los estudiantes añadirán las dependencias necesarias de SLF4j API y Log4j2 Core/SLF4j-impl al archivo `pom.xml`.

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>2.0.17</version>
      </dependency>
      <dependency>
          <groupId>org.apache.logging.log4j</groupId>
          <artifactId>log4j-slf4j2-impl</artifactId>
          <version>2.24.3</version>
      </dependency>
      <dependency>
          <groupId>org.apache.logging.log4j</groupId>
          <artifactId>log4j-core</artifactId>
          <version>2.24.3</version>
      </dependency>
  </dependencies>
  ```

- **Paso 3: Creación del Archivo de Configuración de Log4j2**. Los estudiantes crearán el archivo `log4j2.xml` dentro de la carpeta `src/main/resources`.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <Configuration status="WARN">
      <Appenders>
          <Console name="Console" target="SYSTEM_OUT">
              <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
          </Console>
          <File name="File" fileName="logs/app.log">
              <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
          </File>
      </Appenders>
      <Loggers>
          <Root level="info">
              <AppenderRef ref="Console"/>
              <AppenderRef ref="File"/>
          </Root>
      </Loggers>
  </Configuration>
  ```

- **Paso 4: Implementación del Logger en Código Java**. En la clase `App.java` (o `Main.java` según la convención del proyecto), los estudiantes instanciarán un logger y lo usarán para simular acciones del gestor de tareas:

  ```java
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import java.util.Scanner;

  public class App {
      // Se obtiene una instancia de Logger para esta clase
      private static final Logger logger = LoggerFactory.getLogger(App.class);
      private static int taskIdCounter = 0; // Para simular IDs de tareas

      public static void main(String[] args) {
          logger.info("Aplicación Simple Task Manager iniciada."); // Nivel INFO para inicio de la app

          Scanner scanner = new Scanner(System.in);
          int option;

          do {
              System.out.println("\n--- Menú Gestor de Tareas ---");
              System.out.println("1. Crear nueva tarea");
              System.out.println("2. Completar tarea");
              System.out.println("3. Eliminar tarea");
              System.out.println("4. Salir");
              System.out.print("Elija una opción: ");

              try {
                  option = Integer.parseInt(scanner.nextLine());

                  switch (option) {
                      case 1:
                          System.out.print("Ingrese descripción de la tarea: ");
                          String description = scanner.nextLine();
                          createTask(description);
                          break;
                      case 2:
                          System.out.print("Ingrese ID de la tarea a completar: ");
                          int completeTaskId = Integer.parseInt(scanner.nextLine());
                          completeTask(completeTaskId);
                          break;
                      case 3:
                          System.out.print("Ingrese ID de la tarea a eliminar: ");
                          int deleteTaskId = Integer.parseInt(scanner.nextLine());
                          deleteTask(deleteTaskId);
                          break;
                      case 4:
                          logger.info("Saliendo de la aplicación Simple Task Manager.");
                          break;
                      default:
                          logger.warn("Opción no válida seleccionada: {}", option); // Nivel WARN para opciones inválidas
                          System.out.println("Opción no válida. Intente de nuevo.");
                  }
              } catch (NumberFormatException e) {
                  logger.error("Error: Entrada no numérica. Detalles: {}", e.getMessage(), e); // Nivel ERROR para errores de entrada
                  System.out.println("Entrada no válida. Por favor, ingrese un número.");
                  option = 0; // Para que el bucle continúe
              } catch (Exception e) {
                  logger.fatal("Se ha producido un error inesperado en el menú principal. Detalles: {}", e.getMessage(), e); // Nivel FATAL para errores críticos no manejados
                  System.out.println("Ocurrió un error inesperado. Consulte los logs.");
                  option = 0;
              }

          } while (option != 4);

          scanner.close();
      }

      public static void createTask(String description) {
          taskIdCounter++;
          logger.info("Tarea creada con ID: {} y descripción: {}", taskIdCounter, description); // Uso de sustitución de parámetros
          System.out.println("Tarea '" + description + "' creada con éxito (ID: " + taskIdCounter + ").");
      }

      public static void completeTask(int taskId) {
          if (taskId > 0 && taskId <= taskIdCounter) {
              logger.debug("Intentando completar tarea con ID: {}", taskId); // Nivel DEBUG para detalles de ejecución
              System.out.println("Tarea con ID " + taskId + " marcada como completada.");
          } else {
              logger.error("Error: Intentando completar tarea con ID inválido: {}", taskId); // Nivel ERROR
              System.out.println("No se encontró tarea con ID " + taskId + " para completar.");
          }
      }

      public static void deleteTask(int taskId) {
          if (taskId > 0 && taskId <= taskIdCounter) {
              logger.debug("Intentando eliminar tarea con ID: {}", taskId);
              System.out.println("Tarea con ID " + taskId + " eliminada.");
          } else {
              logger.error("Error: Intentando eliminar tarea con ID inválido: {}", taskId);
              System.out.println("No se encontró tarea con ID " + taskId + " para eliminar.");
          }
      }
  }
  ```

- **Paso 5: Verificación.** Los estudiantes ejecutarán la aplicación y verificarán la salida en la consola y la creación/contenido del archivo `logs/app.log`. Deberán observar cómo los mensajes se registran con diferentes niveles y cómo las excepciones son capturadas en el log.

## 3. Ejercicio Complementario: Logging de Excepciones y Sustitución de Parámetros

- **Objetivo**: Profundizar en el logging de excepciones y la importancia de la sustitución de parámetros para un logging eficiente.
- **Actividad**:
  - **Escenario**: Dada la función `Integer.parseInt(String s)` utilizada en el `main`, los estudiantes deberán:
    - Asegurarse de que el `catch (NumberFormatException e)` en el `main` capture la excepción correctamente y la registre usando `logger.error("Error: Entrada no numérica. Detalles: {}", e.getMessage(), e);` para que se imprima el **stack trace completo** en el log.
  - **Desafío**: Añadir una nueva función `findTask(String taskIdString)` que intente convertir `taskIdString` a un entero. Si la conversión falla, esta función debe lanzar una `IllegalArgumentException` (no es necesario crear una `CustomException` compleja, solo instanciar `IllegalArgumentException`) y asegurar que el `main` la capture y la loguee adecuadamente.

    ```java
    // Ejemplo de función a implementar o modificar 
    public static void findTask(String taskIdString) {
        logger.debug("Buscando tarea con ID en formato String: {}", taskIdString);
        try {
            int taskId = Integer.parseInt(taskIdString); // Lógica de búsqueda de tarea simulada
            
            if (taskId > 0 && taskId <= taskIdCounter) {
                System.out.println("Tarea con ID " + taskId + " encontrada.");
                logger.info("Tarea con ID {} encontrada.", taskId);
            } else {
                System.out.println("No se encontró tarea con ID " + taskId + ".");
                logger.warn("No se encontró tarea con ID {}. Posible ID inválido.", taskId);
            }
        } catch (NumberFormatException e) {
            // Lanzar una nueva excepción para que el main la capture
            logger.error("Error de formato de número al buscar tarea: {}", e.getMessage(), e);
            throw new IllegalArgumentException("El ID de tarea debe ser un número válido.", e);
        }
    }
    ```
  
- **Discusión**: ¿Por qué es mejor usar `logger.error(mensaje, excepcion)` en lugar de `logger.error(mensaje + excepcion.getMessage())`? ¿Cuál es la ventaja de `logger.info("Mensaje con parametro: {}", variable);` sobre la concatenación de `Strings`?

## 4. Diagnóstico de Problemas: Debugger vs. Logs

- **Objetivo**: Entender cuándo usar el debugger y cuándo los logs para diagnosticar problemas.
- **Actividad Grupal/Discusión**:
  - **Escenario**: Se presenta un pequeño fragmento de código con un error lógico sutil (ej. un bucle infinito en ciertas condiciones, o un cálculo incorrecto).
  - **Pregunta Abierta**: "¿Cómo abordarían la depuración de este problema? ¿Usarían el depurador, los logs o ambos? ¿Por qué?".
    - **Debugger**: Ideal para seguir el flujo de ejecución **paso a paso**, inspeccionar variables en tiempo real, breakpoints condicionales. Útil para errores lógicos complejos o cuando se necesita ver el estado exacto en un punto específico.
    - **Logs**: Esencial en **entornos de producción** donde el debugger no es una opción. Permite entender el flujo general de la aplicación, identificar cuellos de botella, monitorear el comportamiento de un sistema distribuido y registrar eventos importantes. Útil para errores que ocurren esporádicamente o en escenarios que son difíciles de replicar con el debugger.

## 5. Cierre y Preguntas Adicionales

- ¿Qué nivel de log usarían para registrar el inicio y fin de una operación crítica?
- Si están desarrollando una nueva funcionalidad, ¿qué nivel de log sería útil para ver todos los detalles de ejecución sin afectar el rendimiento de la aplicación en producción?
- Mencionen una situación donde el debugger sería claramente superior a los logs para identificar un problema.
