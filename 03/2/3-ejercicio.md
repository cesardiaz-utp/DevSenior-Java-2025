# Ejercicio Autónomo: Sistema de Gestión de Clínica Veterinaria

## Objetivos del Ejercicio

Al completar este ejercicio, serás capaz de:

- **Diseñar y aplicar conceptos de POO** (clases, atributos, métodos, encapsulamiento, relaciones entre objetos) para modelar un sistema veterinario.
- **Implementar manejo de excepciones** para gestionar escenarios anómalos (ej. paciente no encontrado, datos inválidos en registros).
- **Integrar logging** (Log4j2/SLF4j) en todas las capas de la aplicación para rastrear el flujo de ejecución, registrar eventos importantes y diagnosticar errores.
- **Crear y lanzar excepciones personalizadas** (`Custom Exceptions`) para errores específicos de la lógica de negocio (ej. `PatientNotFoundException`, `InvalidDataException`, `ClinicFullException`).
- **Utilizar Maven** para la gestión de dependencias y la construcción del proyecto.
- **Aplicar buenas prácticas** en la escritura de código (nombres en inglés, comentarios en español, sustitución de parámetros en logs).

## Instrucciones Detalladas

Construirás una aplicación de consola para gestionar un inventario básico de pacientes (animales) y sus visitas en una clínica veterinaria. La aplicación permitirá registrar pacientes, añadir visitas, buscar pacientes y listar todos los registros.

### Paso 1: Creación del Proyecto Maven

1. Abre tu terminal o línea de comandos.
2. Crea un nuevo proyecto Maven utilizando el arquetipo `maven-archetype-quickstart` o créalos el aistente de VSCode:

    ```Bash
    mvn archetype:generate -DgroupId=com.devsenior.veterinary -DartifactId=veterinary-clinic -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Abre el proyecto `veterinary-clinic` en Visual Studio Code (o tu IDE preferido).

### Paso 2: Configuración de Dependencias en `pom.xml`

1. Abre el archivo `pom.xml` dentro de la carpeta `veterinary-clinic`.
2. Asegúrate de que la **versión de Java** configurada sea la 21 (o superior) y añade las propiedades para las versiones de logging.

    ```XML
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <slf4j.version>2.0.17</slf4j.version>
        <log4j.version>2.24.3</log4j.version>
    </properties>
    ```

3. Añade las dependencias para SLF4j API, Log4j2 SLF4j Binding y Log4j2 Core dentro de la sección `<dependencies>`.

    ```XML
    <dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-slf4j2-impl</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>${log4j.version}</version>
        </dependency>
    </dependencies>
    ```

4. Guarda los cambios en pom.xml.

### Paso 3: Creación del Archivo de Configuración de Log4j2 (`log4j2.xml`)

1. Dentro de tu proyecto, crea la carpeta `src/main/resources`.
2. Dentro de `src/main/resources`, crea un nuevo archivo llamado `log4j2.xml`.
3. Copia el siguiente contenido. Esta configuración imprimirá logs en la consola y en un archivo llamado `clinic.log` dentro de una carpeta `logs`, con rotación diaria.

    ```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration status="WARN">
        <Appenders>
            <Console name="ConsoleAppender" target="SYSTEM_OUT">
                <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
            </Console>

            <RollingFile name="FileAppender" fileName="logs/clinic.log"
                        filePattern="logs/$${date:yyyy-MM}/clinic-%d{MM-dd-yyyy}-%i.log.gz">
                <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
                <Policies>
                    <TimeBasedTriggeringPolicy interval="1" modulate="true"/>
                    <SizeBasedTriggeringPolicy size="10 MB"/>
                </Policies>
                <DefaultRolloverStrategy max="5"/>
            </RollingFile>
        </Appenders>

        <Loggers>
            <Root level="debug">
                <AppenderRef ref="ConsoleAppender"/>
                <AppenderRef ref="FileAppender"/>
            </Root>
        </Loggers>
    </Configuration>
    ```

### Paso 4: Creación de Clases de Modelo y Excepciones Personalizadas

En la carpeta `src/main/java/com/devsenior/veterinary/` (o subcarpetas como `model`, `exception`):

1. **Clase `Patient.java`: Representa un paciente (animal)**.
    - **Atributos (privados)**:
      - `String patientId` (único, generado automáticamente o ingresado)
      - `String name`
      - `String species`
      - `int age`
      - `String ownerName`
      - `String ownerPhone`.
    - **Constructor**: Para inicializar todos los atributos. El `patientId` podría generarse internamente en el constructor (ej. con un contador o `UUID.randomUUID().toString()`).
    - Getters para todos los atributos.
    - Setters para `name`, `age`, `ownerName`, `ownerPhone` (ya que pueden cambiar).
    - **Validaciones en setters/constructor**: Por ejemplo, `age` no debe ser negativo, los nombres no deben ser nulos o vacíos. Lanza `IllegalArgumentException` estándar de Java si la validación falla.
    - **Método `toString()`**: Para una representación legible del paciente.
    - **Logging**: En el constructor y setters para registrar creación y cambios.

    ```Java
    package com.devsenior.veterinary.model; // Puedes crear un paquete 'model'

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import java.util.UUID; // Para generar IDs únicos si se desea

    public class Patient {
        private static final Logger logger = LoggerFactory.getLogger(Patient.class);

        private String patientId;
        private String name;
        private String species;
        private int age;
        private String ownerName;
        private String ownerPhone;

        public Patient(String name, String species, int age, String ownerName, String ownerPhone) {
            // Se genera un ID único para el paciente al momento de su creación.
            this.patientId = UUID.randomUUID().toString();
            // Se loguea la creación de un nuevo objeto Patient.
            logger.debug("Creando nuevo paciente con ID: {}", this.patientId);

            setName(name);
            setSpecies(species);
            setAge(age);
            setOwnerName(ownerName);
            setOwnerPhone(ownerPhone);
        }

        // Getters
        public String getPatientId() { return patientId; }
        public String getName() { return name; }
        public String getSpecies() { return species; }
        public int getAge() { return age; }
        public String getOwnerName() { return ownerName; }
        public String getOwnerPhone() { return ownerPhone; }

        // Setters con validación y logging
        public void setName(String name) {
            if (name == null || name.trim().isEmpty()) {
                logger.warn("Intento de establecer nombre vacío o nulo para paciente ID {}.", this.patientId);
                throw new IllegalArgumentException("El nombre del paciente no puede estar vacío.");
            }
            logger.debug("Nombre del paciente {} actualizado a {}", this.patientId, name);
            this.name = name;
        }

        public void setSpecies(String species) {
            if (species == null || species.trim().isEmpty()) {
                logger.warn("Intento de establecer especie vacía o nula para paciente ID {}.", this.patientId);
                throw new IllegalArgumentException("La especie no puede estar vacía.");
            }
            logger.debug("Especie del paciente {} actualizada a {}", this.patientId, species);
            this.species = species;
        }

        public void setAge(int age) {
            if (age < 0) {
                logger.warn("Intento de establecer edad negativa para paciente ID {}: {}", this.patientId, age);
                throw new IllegalArgumentException("La edad no puede ser negativa.");
            }
            logger.debug("Edad del paciente {} actualizada a {}", this.patientId, age);
            this.age = age;
        }

        public void setOwnerName(String ownerName) {
            if (ownerName == null || ownerName.trim().isEmpty()) {
                logger.warn("Intento de establecer nombre de propietario vacío o nulo para paciente ID {}.", this.patientId);
                throw new IllegalArgumentException("El nombre del propietario no puede estar vacío.");
            }
            logger.debug("Nombre del propietario del paciente {} actualizado a {}", this.patientId, ownerName);
            this.ownerName = ownerName;
        }

        public void setOwnerPhone(String ownerPhone) {
            if (ownerPhone == null || ownerPhone.trim().isEmpty()) {
                logger.warn("Intento de establecer teléfono de propietario vacío o nulo para paciente ID {}.", this.patientId);
                throw new IllegalArgumentException("El teléfono del propietario no puede estar vacío.");
            }
            logger.debug("Teléfono del propietario del paciente {} actualizado a {}", this.patientId, ownerPhone);
            this.ownerPhone = ownerPhone;
        }

        @Override
        public String toString() {
            return "Paciente [ID: " + patientId.substring(0, 8) + "..., Nombre: '" + name + "', Especie: '" + species + "', Edad: " + age + ", Propietario: '" + ownerName + "', Teléfono: '" + ownerPhone + "']";
        }
    }
    ```

2. **Clase `Visit.java`**: Representa una visita del paciente a la clínica.
    - **Atributos (privados)**:
      - `String visitId` (único)
      - `String patientId` (ID del paciente asociado)
      - `LocalDate visitDate`
      - `String reason`
      - `String diagnosis`
      - `double cost`
    - **Constructor**: Para inicializar. `visitId` puede ser generado automáticamente.
    - Getters para todos los atributos.
    - Setters para `diagnosis` y `cost` (ya que podrían actualizarse después de la visita inicial).
    - **Validaciones en setters/constructor**: Por ejemplo, `cost` no debe ser negativo, `reason` no debe ser nulo/vacío.
    - **Método `toString()`**: Para una representación legible de la visita.
    - **Logging**: En el constructor y setters.

    ```Java
    package com.devsenior.veterinary.model;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import java.time.LocalDate; // Para manejar fechas de forma más robusta
    import java.util.UUID;

    public class Visit {
        private static final Logger logger = LoggerFactory.getLogger(Visit.class);

        private String visitId;
        private String patientId; // ID del paciente al que pertenece esta visita
        private LocalDate visitDate;
        private String reason;
        private String diagnosis;
        private double cost;

        public Visit(String patientId, LocalDate visitDate, String reason, String diagnosis, double cost) {
            this.visitId = UUID.randomUUID().toString();
            logger.debug("Creando nueva visita con ID: {} para paciente ID: {}", this.visitId, patientId);

            if (patientId == null || patientId.trim().isEmpty()) {
                logger.warn("Intento de crear visita sin Patient ID.");
                throw new IllegalArgumentException("El ID del paciente es obligatorio para una visita.");
            }
            this.patientId = patientId;

            if (visitDate == null) {
                logger.warn("Intento de crear visita sin fecha.");
                throw new IllegalArgumentException("La fecha de la visita es obligatoria.");
            }
            this.visitDate = visitDate;

            setReason(reason);
            setDiagnosis(diagnosis);
            setCost(cost);
        }

        // Getters
        public String getVisitId() { return visitId; }
        public String getPatientId() { return patientId; }
        public LocalDate getVisitDate() { return visitDate; }
        public String getReason() { return reason; }
        public String getDiagnosis() { return diagnosis; }
        public double getCost() { return cost; }

        // Setters con validación y logging
        public void setReason(String reason) {
            if (reason == null || reason.trim().isEmpty()) {
                logger.warn("Intento de establecer razón de visita vacía o nula para visita ID {}.", this.visitId);
                throw new IllegalArgumentException("La razón de la visita no puede estar vacía.");
            }
            logger.debug("Razón de visita {} actualizada para visita ID {}.", reason, this.visitId);
            this.reason = reason;
        }

        public void setDiagnosis(String diagnosis) {
            // La diagnosis puede ser nula inicialmente, pero si se establece, no puede estar vacía
            if (diagnosis != null && diagnosis.trim().isEmpty()) {
                logger.warn("Intento de establecer diagnóstico vacío para visita ID {}.", this.visitId);
                throw new IllegalArgumentException("El diagnóstico no puede estar vacío si se proporciona.");
            }
            logger.debug("Diagnóstico {} actualizado para visita ID {}.", diagnosis, this.visitId);
            this.diagnosis = diagnosis;
        }

        public void setCost(double cost) {
            if (cost < 0) {
                logger.warn("Intento de establecer costo negativo para visita ID {}: {}", this.visitId, cost);
                throw new IllegalArgumentException("El costo no puede ser negativo.");
            }
            logger.debug("Costo {} actualizado para visita ID {}.", cost, this.visitId);
            this.cost = cost;
        }

        @Override
        public String toString() {
            return "Visita [ID: " + visitId.substring(0, 8) + "..., Fecha: " + visitDate + ", Razón: '" + reason + "', Diagnóstico: '" + diagnosis + "', Costo: $" + String.format("%.2f", cost) + "]";
        }
    }
    ```

3. **Clases de Excepción Personalizadas**:
    - `PatientNotFoundException.java`: Se lanza cuando se intenta buscar un paciente por ID que no existe.

      ```Java
      package com.devsenior.veterinary.exception; // Puedes crear un paquete 'exception'

      public class PatientNotFoundException extends RuntimeException {
          public PatientNotFoundException(String patientId) {
              super("Paciente con ID " + patientId + " no encontrado en el sistema.");
          }
      }
      ```

    - `InvalidDataException.java`: Se lanza cuando se intenta crear o actualizar un registro (paciente o visita) con datos que no cumplen las reglas de negocio (ej. nombre vacío, edad negativa). Esta excepción puede unificar varias `IllegalArgumentException` para nuestra lógica de negocio.

      ```Java
      package com.devsenior.veterinary.exception;

      public class InvalidDataException extends RuntimeException {
          public InvalidDataException(String message) {
              super(message);
          }
          public InvalidDataException(String message, Throwable cause) {
              super(message, cause);
          }
      }
      ```

    - `ClinicFullException.java`: Se lanza si se intenta registrar un paciente y el array de pacientes está lleno.

      ```Java
      package com.devsenior.veterinary.exception;

      public class ClinicFullException extends RuntimeException {
          public ClinicFullException(int capacity) {
              super("La clínica ha alcanzado su capacidad máxima de " + capacity + " pacientes.");
          }
      }
      ```

### Paso 5: Implementación de la Lógica de la Clínica (`ClinicManager.java`)

Esta clase central gestionará las colecciones de pacientes y visitas utilizando Arrays.

1. Crea la clase `ClinicManager.java` (sugerido en `src/main/java/com/devsenior/veterinary/service/`).
    - **Atributos (privados)**:
      - `Patient[] patients`: Un array para almacenar objetos `Patient`. Define un tamaño fijo, por ejemplo, `MAX_PATIENTS = 10`.
      - `Visit[][] patientVisits`: Un array bidimensional o un array de arrays para almacenar las visitas. Podrías tener `Visit[MAX_PATIENTS][MAX_VISITS_PER_PATIENT]`, o para simplificar, un `Visit[]` global y que cada `Visit` tenga el `patientId` asociado.
      - `int patientCount`: Un contador para llevar el número actual de pacientes en el array.
      - `int visitCount`: Un contador para llevar el número total de visitas (simplificado para el array global de visitas).
    - **Métodos**:
      - `registerPatient(Patient patient)`: Añade un nuevo paciente.
        - Debe buscar un espacio disponible en el array `patients`.
        - Si el array está lleno, lanza `ClinicFullException`.
        - Si el `patientId` (generado por `Patient` o asignado aquí) ya existe (requerirá una búsqueda secuencial), loguea una advertencia y no lo añade (o permite actualizar ciertos datos, logueando el cambio).
        - **Lanza** `InvalidDataException` si el paciente es `null` o sus datos son inválidos (aprovechando las validaciones del constructor de `Patient`).
      - `findPatientById(String patientId)`: Busca y retorna un `Patient` por su ID. Lanza `PatientNotFoundException` si no existe. (Requiere búsqueda secuencial en el array patients).
      - `addVisit(String patientId, Visit visit)`: Registra una nueva visita para un paciente existente.
        - Primero, verifica que el `patientId` exista (usando `findPatientById`).
        - Añade la visita a un array global de visitas o, si optas por el array bidimensional, al array de visitas de ese paciente (esto último es más complejo con arrays fijos). Para simplificar, podemos tener un `Visit[] visits` global y simplemente añadir la visita, confiando en que `visit.getPatientId()` la asocie.
        - **Lanza** `PatientNotFoundException` si el `patientId` no existe.
        - **Lanza** `InvalidDataException` si los datos de la visita son inválidos (aprovechando las validaciones del constructor de `Visit`).
      - `getPatientVisits(String patientId)`: Retorna un **nuevo array** de `Visit[]` con todas las visitas de un paciente específico. (Requiere iterar sobre el array global de visitas). **Lanza** `PatientNotFoundException` si el paciente no existe.
      - `listAllPatients()`: Retorna un **nuevo array** de `Patient[]` con todos los pacientes registrados. Deberá loguear (`DEBUG`) el estado actual.
    - **Logging en cada método**: Cada operación (registro, búsqueda, visita, listado) debe registrarse con el nivel adecuado (`INFO` para operaciones exitosas, `DEBUG` para detalles internos, `WARN` para problemas menores, `ERROR` para fallos, `FATAL` para errores irrecuperables).

    ```Java
    package com.devsenior.veterinary.service;

    import com.devsenior.veterinary.model.Patient;
    import com.devsenior.veterinary.model.Visit;
    import com.devsenior.veterinary.exception.PatientNotFoundException;
    import com.devsenior.veterinary.exception.InvalidDataException;
    import com.devsenior.veterinary.exception.ClinicFullException;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    // Para simular listas dinámicas con Arrays.
    import java.util.Arrays;

    public class ClinicManager {
        private static final Logger logger = LoggerFactory.getLogger(ClinicManager.class);

        private static final int MAX_PATIENTS = 5; // Capacidad máxima de pacientes
        private static final int MAX_VISITS = 20;  // Capacidad máxima de visitas en total (simplificado)

        private Patient[] patients;
        private int patientCount; // Número actual de pacientes registrados

        private Visit[] visits;
        private int visitCount; // Número actual de visitas registradas

        public ClinicManager() {
            this.patients = new Patient[MAX_PATIENTS];
            this.patientCount = 0;
            this.visits = new Visit[MAX_VISITS];
            this.visitCount = 0;
            logger.info("Gestor de clínica veterinaria inicializado con capacidad para {} pacientes y {} visitas.", MAX_PATIENTS, MAX_VISITS);
        }

        /**
         * Registra un nuevo paciente en la clínica.
        * @param patient El objeto Patient a registrar.
        * @throws ClinicFullException Si la clínica está llena.
        * @throws InvalidDataException Si los datos del paciente son inválidos.
        */
        public void registerPatient(Patient patient) {
            logger.debug("Intentando registrar paciente: {}", (patient != null ? patient.getName() : "nulo"));
            if (patient == null) {
                logger.error("Intento de registrar un paciente nulo.");
                throw new InvalidDataException("El paciente no puede ser nulo.");
            }

            // Validar si ya existe un paciente con el mismo ID
            // (Si Patient generó el ID, esta búsqueda es para detectar colisiones si el ID no es UUID)
            // Si el ID es UUID, la colisión es casi imposible, pero se valida por si acaso.
            for (int i = 0; i < patientCount; i++) {
                if (patients[i].getPatientId().equals(patient.getPatientId())) {
                    logger.warn("Intento de registrar paciente con ID {} ya existente. Actualizando datos si es necesario.", patient.getPatientId());
                    // Opción: Actualizar el paciente existente en lugar de lanzar excepción.
                    // Para este ejercicio, asumimos que no se puede registrar uno con el mismo ID ya existente.
                    throw new InvalidDataException("Ya existe un paciente con el ID: " + patient.getPatientId());
                }
            }

            if (patientCount >= MAX_PATIENTS) {
                logger.error("Fallo al registrar paciente: Capacidad máxima de pacientes alcanzada ({}).", MAX_PATIENTS);
                throw new ClinicFullException(MAX_PATIENTS);
            }

            patients[patientCount] = patient;
            patientCount++;
            logger.info("Paciente '{}' con ID {} registrado con éxito. Total pacientes: {}", patient.getName(), patient.getPatientId().substring(0, 8), patientCount);
        }

        /**
         * Busca un paciente por su ID.
        * @param patientId El ID del paciente a buscar.
        * @return El objeto Patient encontrado.
        * @throws PatientNotFoundException Si el paciente no se encuentra.
        */
        public Patient findPatientById(String patientId) {
            logger.debug("Buscando paciente con ID: {}", patientId);
            for (int i = 0; i < patientCount; i++) {
                if (patients[i] != null && patients[i].getPatientId().equals(patientId)) {
                    logger.info("Paciente con ID {} encontrado: {}", patientId, patients[i].getName());
                    return patients[i];
                }
            }
            logger.error("Paciente con ID {} no encontrado.", patientId);
            throw new PatientNotFoundException(patientId);
        }

        /**
         * Añade una visita a un paciente existente.
        * @param patientId El ID del paciente al que se le añade la visita.
        * @param visit La visita a añadir.
        * @throws PatientNotFoundException Si el paciente no existe.
        * @throws InvalidDataException Si los datos de la visita son inválidos o la capacidad de visitas está llena.
        */
        public void addVisit(String patientId, Visit visit) {
            logger.debug("Intentando añadir visita para paciente ID {} con visita ID {}.", patientId, (visit != null ? visit.getVisitId().substring(0, 8) : "nulo"));

            // Primero, verificar que el paciente exista
            try {
                findPatientById(patientId); // Esto lanzará PatientNotFoundException si no existe
            } catch (PatientNotFoundException e) {
                logger.error("Fallo al añadir visita: {}", e.getMessage());
                throw e; // Relanzar la excepción
            }

            if (visit == null) {
                logger.error("Intento de añadir una visita nula para paciente ID {}.", patientId);
                throw new InvalidDataException("La visita no puede ser nula.");
            }

            // Validar que la visita realmente corresponda al patientId
            if (!visit.getPatientId().equals(patientId)) {
                logger.error("Error: La visita con ID {} no corresponde al paciente ID {}.", visit.getVisitId().substring(0,8), patientId);
                throw new InvalidDataException("La visita no corresponde al paciente especificado.");
            }

            if (visitCount >= MAX_VISITS) {
                logger.error("Fallo al añadir visita: Capacidad máxima de visitas alcanzada ({}).", MAX_VISITS);
                throw new InvalidDataException("La clínica ha alcanzado su capacidad máxima de visitas.");
            }

            visits[visitCount] = visit;
            visitCount++;
            logger.info("Visita (ID: {}) añadida para paciente ID {}. Razón: '{}'. Total visitas: {}", visit.getVisitId().substring(0, 8), patientId, visit.getReason(), visitCount);
        }

        /**
         * Obtiene un array de todas las visitas de un paciente específico.
        * @param patientId El ID del paciente.
        * @return Un array de objetos Visit[] con las visitas del paciente.
        * @throws PatientNotFoundException Si el paciente no existe.
        */
        public Visit[] getPatientVisits(String patientId) {
            logger.debug("Obteniendo visitas para paciente con ID: {}", patientId);
            // Verificar que el paciente exista
            try {
                findPatientById(patientId); // Esto lanzará PatientNotFoundException si no existe
            } catch (PatientNotFoundException e) {
                logger.warn("Intento de obtener visitas para paciente no existente: {}", patientId);
                throw e; // Relanzar la excepción
            }

            // Contar cuántas visitas tiene este paciente para crear un array del tamaño justo
            int count = 0;
            for (int i = 0; i < visitCount; i++) {
                if (visits[i] != null && visits[i].getPatientId().equals(patientId)) {
                    count++;
                }
            }

            Visit[] patientSpecificVisits = new Visit[count];
            int index = 0;
            for (int i = 0; i < visitCount; i++) {
                if (visits[i] != null && visits[i].getPatientId().equals(patientId)) {
                    patientSpecificVisits[index++] = visits[i];
                }
            }
            logger.info("Encontradas {} visitas para el paciente con ID {}.", count, patientId);
            return patientSpecificVisits;
        }

        /**
         * Lista todos los pacientes registrados en la clínica.
        * @return Un array de objetos Patient[] con todos los pacientes.
        */
        public Patient[] listAllPatients() {
            logger.debug("Listando todos los pacientes registrados. Total: {}", patientCount);
            // Retorna una copia del array de pacientes que realmente están ocupados.
            return Arrays.copyOf(patients, patientCount);
        }
    }
    ```

### Paso 6: Implementación de la Interfaz de Usuario en `App.java`

Modifica la clase `App.java` para interactuar con el `ClinicManager`.

1. Abre el archivo `App.java` (ubicado en `src/main/java/com/devsenior/veterinary/App.java`).
2. Implementa un menú de consola que permita:
    - **Registrar un nuevo paciente**: Solicita todos los datos del paciente.
    - **Añadir una visita**: Solicita el `patientId` (existente) y los datos de la visita.
    - **Buscar paciente por ID**: Muestra los detalles del paciente.
    - **Listar visitas de un paciente**: Solicita `patientId` y muestra sus visitas.
    - **Listar todos los pacientes**: Muestra un resumen de todos los pacientes.
    - **Salir**.
3. Asegúrate de que toda la interacción y los posibles errores del usuario (ej. entrada no numérica) sean manejados con bloques `try-catch` y logueados con los niveles apropiados. Captura las excepciones personalizadas (`PatientNotFoundException`, `InvalidDataException`, `ClinicFullException`) y loguéalas con sus mensajes y _stack traces_.

```Java
package com.devsenior.veterinary;

import com.devsenior.veterinary.model.Patient;
import com.devsenior.veterinary.model.Visit;
import com.devsenior.veterinary.service.ClinicManager;
import com.devsenior.veterinary.exception.PatientNotFoundException;
import com.devsenior.veterinary.exception.InvalidDataException;
import com.devsenior.veterinary.exception.ClinicFullException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.time.LocalDate;
import java.time.format.DateTimeParseException;
import java.util.InputMismatchException;
import java.util.Scanner;

public class App {
    private static final Logger logger = LoggerFactory.getLogger(App.class);
    private static final Scanner scanner = new Scanner(System.in);
    private static final ClinicManager clinicManager = new ClinicManager();

    public static void main(String[] args) {
        logger.info("Iniciando aplicación de gestión de clínica veterinaria.");
        loadInitialData(); // Cargar algunos datos para facilitar las pruebas

        int choice;
        do {
            displayMenu();
            try {
                choice = scanner.nextInt();
                scanner.nextLine(); // Consumir el salto de línea
                logger.debug("Opción de menú seleccionada: {}", choice);

                switch (choice) {
                    case 1:
                        registerPatient();
                        break;
                    case 2:
                        addVisitToPatient();
                        break;
                    case 3:
                        findPatient();
                        break;
                    case 4:
                        listPatientVisits();
                        break;
                    case 5:
                        listAllPatients();
                        break;
                    case 6:
                        logger.info("Saliendo de la aplicación.");
                        System.out.println("¡Hasta pronto!");
                        break;
                    default:
                        logger.warn("Opción de menú inválida: {}", choice);
                        System.out.println("Opción inválida. Por favor, intente de nuevo.");
                }
            } catch (InputMismatchException e) {
                logger.error("Error de entrada: Se esperaba un número. Detalles: {}", e.getMessage(), e);
                System.out.println("Entrada no válida. Por favor, ingrese un número.");
                scanner.nextLine(); // Limpiar el buffer del scanner
                choice = 0; // Para que el bucle continúe
            } catch (Exception e) {
                logger.fatal("Ocurrió un error inesperado en el bucle principal de la aplicación. Detalles: {}", e.getMessage(), e);
                System.out.println("Ocurrió un error grave. Consulte los logs.");
                choice = 0;
            }
        } while (choice != 6);

        scanner.close();
    }

    private static void displayMenu() {
        System.out.println("\n--- Menú de Gestión de Clínica Veterinaria ---");
        System.out.println("1. Registrar Nuevo Paciente");
        System.out.println("2. Añadir Visita a Paciente");
        System.out.println("3. Buscar Paciente por ID");
        System.out.println("4. Listar Visitas de un Paciente");
        System.out.println("5. Listar Todos los Pacientes");
        System.out.println("6. Salir");
        System.out.print("Elija una opción: ");
    }

    private static void registerPatient() {
        logger.debug("Iniciando proceso para registrar nuevo paciente.");
        System.out.print("Ingrese Nombre del paciente: ");
        String name = scanner.nextLine();
        System.out.print("Ingrese Especie: ");
        String species = scanner.nextLine();
        System.out.print("Ingrese Edad: ");
        int age = readIntInput();
        System.out.print("Ingrese Nombre del Propietario: ");
        String ownerName = scanner.nextLine();
        System.out.print("Ingrese Teléfono del Propietario: ");
        String ownerPhone = scanner.nextLine();

        try {
            Patient patient = new Patient(name, species, age, ownerName, ownerPhone);
            clinicManager.registerPatient(patient);
            System.out.println("Paciente registrado con éxito. ID: " + patient.getPatientId().substring(0, 8) + "...");
            logger.info("Paciente '{}' (ID: {}) registrado correctamente.", name, patient.getPatientId().substring(0, 8));
        } catch (InvalidDataException | IllegalArgumentException e) {
            logger.error("Error al registrar paciente: {}", e.getMessage(), e);
            System.out.println("Error al registrar paciente: " + e.getMessage());
        } catch (ClinicFullException e) {
            logger.error("Error al registrar paciente: {}", e.getMessage(), e);
            System.out.println("Error: " + e.getMessage());
        } catch (Exception e) {
            logger.error("Error inesperado al registrar paciente: {}", e.getMessage(), e);
            System.out.println("Ocurrió un error inesperado al registrar el paciente.");
        }
    }

    private static void addVisitToPatient() {
        logger.debug("Iniciando proceso para añadir visita.");
        System.out.print("Ingrese ID del paciente: ");
        String patientId = scanner.nextLine();
        System.out.print("Ingrese Fecha de la visita (YYYY-MM-DD): ");
        String dateStr = scanner.nextLine();
        LocalDate visitDate;
        try {
            visitDate = LocalDate.parse(dateStr);
        } catch (DateTimeParseException e) {
            logger.error("Error de formato de fecha para visita: {}. Entrada: {}", e.getMessage(), dateStr, e);
            System.out.println("Formato de fecha inválido. Use YYYY-MM-DD.");
            return;
        }

        System.out.print("Ingrese Razón de la visita: ");
        String reason = scanner.nextLine();
        System.out.print("Ingrese Diagnóstico (opcional, presione Enter si no aplica): ");
        String diagnosis = scanner.nextLine();
        System.out.print("Ingrese Costo de la visita: ");
        double cost = readDoubleInput();

        try {
            Visit visit = new Visit(patientId, visitDate, reason, diagnosis.isEmpty() ? null : diagnosis, cost);
            clinicManager.addVisit(patientId, visit); // Se pasa el patientId dos veces, una para la lógica de búsqueda y otra para el objeto Visit.
            System.out.println("Visita añadida con éxito para el paciente con ID: " + patientId.substring(0, 8) + "...");
            logger.info("Visita (ID: {}) añadida para paciente ID {}.", visit.getVisitId().substring(0, 8), patientId.substring(0, 8));
        } catch (PatientNotFoundException | InvalidDataException | IllegalArgumentException e) {
            logger.error("Error al añadir visita: {}", e.getMessage(), e);
            System.out.println("Error: " + e.getMessage());
        } catch (Exception e) {
            logger.error("Error inesperado al añadir visita: {}", e.getMessage(), e);
            System.out.println("Ocurrió un error inesperado al añadir la visita.");
        }
    }

    private static void findPatient() {
        logger.debug("Iniciando proceso de búsqueda de paciente.");
        System.out.print("Ingrese ID del paciente a buscar: ");
        String patientId = scanner.nextLine();

        try {
            Patient patient = clinicManager.findPatientById(patientId);
            System.out.println("Paciente encontrado: " + patient);
            logger.info("Búsqueda exitosa para paciente ID {}: {}", patientId, patient.getName());
        } catch (PatientNotFoundException e) {
            logger.warn("Búsqueda fallida: {}", e.getMessage());
            System.out.println("Error: " + e.getMessage());
        } catch (Exception e) {
            logger.error("Error inesperado al buscar paciente: {}", e.getMessage(), e);
            System.out.println("Ocurrió un error inesperado al buscar el paciente.");
        }
    }

    private static void listPatientVisits() {
        logger.debug("Iniciando proceso de listado de visitas de un paciente.");
        System.out.print("Ingrese ID del paciente para listar sus visitas: ");
        String patientId = scanner.nextLine();

        try {
            Visit[] visits = clinicManager.getPatientVisits(patientId);
            if (visits.length == 0) {
                System.out.println("El paciente no tiene visitas registradas o el ID es incorrecto.");
                logger.info("No se encontraron visitas para el paciente ID {}.", patientId);
            } else {
                System.out.println("\n--- Visitas del Paciente " + patientId.substring(0, 8) + "... ---");
                for (Visit visit : visits) {
                    System.out.println(visit);
                }
                logger.info("Listadas {} visitas para el paciente ID {}.", visits.length, patientId.substring(0, 8));
            }
        } catch (PatientNotFoundException e) {
            logger.warn("Error al listar visitas: {}", e.getMessage());
            System.out.println("Error: " + e.getMessage());
        } catch (Exception e) {
            logger.error("Error inesperado al listar visitas: {}", e.getMessage(), e);
            System.out.println("Ocurrió un error inesperado al listar las visitas.");
        }
    }

    private static void listAllPatients() {
        logger.debug("Iniciando proceso de listado de todos los pacientes.");
        Patient[] patients = clinicManager.listAllPatients();
        if (patients.length == 0) {
            System.out.println("No hay pacientes registrados.");
            logger.info("No hay pacientes registrados en la clínica.");
        } else {
            System.out.println("\n--- Pacientes Registrados ---");
            for (Patient patient : patients) {
                System.out.println(patient);
            }
            logger.info("Listados {} pacientes registrados.", patients.length);
        }
    }

    // Métodos auxiliares para leer entrada numérica con manejo básico de errores
    private static double readDoubleInput() {
        while (true) {
            try {
                double value = scanner.nextDouble();
                scanner.nextLine(); // Consumir el salto de línea
                return value;
            } catch (InputMismatchException e) {
                logger.warn("Entrada inválida para número decimal. Detalles: {}", e.getMessage());
                System.out.print("Entrada no válida. Por favor, ingrese un número decimal: ");
                scanner.nextLine(); // Limpiar el buffer
            }
        }
    }

    private static int readIntInput() {
        while (true) {
            try {
                int value = scanner.nextInt();
                scanner.nextLine(); // Consumir el salto de línea
                return value;
            } catch (InputMismatchException e) {
                logger.warn("Entrada inválida para número entero. Detalles: {}", e.getMessage());
                System.out.print("Entrada no válida. Por favor, ingrese un número entero: ");
                scanner.nextLine(); // Limpiar el buffer
            }
        }
    }

    // Método para cargar algunos datos iniciales para probar
    private static void loadInitialData() {
        logger.info("Cargando datos iniciales de prueba en la clínica.");
        try {
            Patient p1 = new Patient("Buddy", "Perro", 5, "Ana García", "310-1234567");
            clinicManager.registerPatient(p1);
            clinicManager.addVisit(p1.getPatientId(), new Visit(p1.getPatientId(), LocalDate.of(2025, 1, 10), "Chequeo anual", "Saludable", 50.00));
            clinicManager.addVisit(p1.getPatientId(), new Visit(p1.getPatientId(), LocalDate.of(2025, 3, 15), "Tos persistente", "Bronquitis leve", 75.00));

            Patient p2 = new Patient("Kitty", "Gato", 2, "Juan Pérez", "311-9876543");
            clinicManager.registerPatient(p2);
            clinicManager.addVisit(p2.getPatientId(), new Visit(p2.getPatientId(), LocalDate.of(2025, 2, 5), "Vacunación", "Vacunas al día", 60.00));

            logger.info("Datos iniciales cargados con éxito.");
        } catch (Exception e) {
            logger.error("Error al cargar datos iniciales: {}", e.getMessage(), e);
            System.err.println("Error al cargar datos iniciales. Consulte los logs.");
        }
    }
}
```

### Paso 7: Compilación y Ejecución del Proyecto

1. Abre tu terminal en la raíz del proyecto `veterinary-clinic`.
2. Compila el proyecto Maven:

    ```Bash
    mvn clean install
    ```

3. Ejecuta la aplicación:

    ```Bash
    java -jar target/veterinary-clinic-1.0.0-SNAPSHOT.jar
    ```

    _(Nota: Si recibes el error "no main manifest attribute", es probable que necesites configurar el `maven-jar-plugin` en tu `pom.xml` para incluir la clase principal en el JAR ejecutable. Si no, en VS Code, puedes simplemente hacer clic derecho en `App.java` y seleccionar "Run Java".)_

### Paso 8: Prueba y Observación de Logs

1. **Interactúa con la aplicación**:
    - **Registra** varios pacientes (ten en cuenta la capacidad máxima del array).
    - **Añade** varias visitas a un mismo paciente.
    - Intenta **registrar un paciente con datos inválidos** (ej. edad negativa, nombre vacío) para ver la `InvalidDataException` y el log de error.
    - Intenta **registrar más pacientes de la capacidad máxima** para ver la `ClinicFullException` y el log de error.
    - **Intenta añadir una visita a un `patientId` que no existe** para ver la `PatientNotFoundException` y el log de error.
    - Intenta **buscar un paciente por un ID que no existe**.
    - Introduce **texto cuando se espere un número** o una **fecha inválida** para ver el manejo de `InputMismatchException`/`DateTimeParseException` y los logs de error/advertencia.
    - **Lista** todos los pacientes y sus visitas para verificar el estado actual.
    - Finalmente, elige la opción para **salir**.
2. **Verifica la consola**: Observa cómo los mensajes de log aparecen junto con la salida de la aplicación.
3. **Verifica el archivo de log**:
    - Navega a la carpeta `logs` (creada en la raíz del proyecto `veterinary-clinic`).
    - Abre el archivo `clinic.log` (o el archivo con la fecha, si usaste `RollingFileAppender`) con un editor de texto.
    - Examina los logs. Presta especial atención a:
      - Los diferentes **niveles de log** (`INFO`, `WARN`, `ERROR`, `DEBUG`).
      - Cómo se registran las **excepciones personalizadas** con su _stack trace_ completo.
      - El uso de la **sustitución de parámetros** en los mensajes de log.

## Criterios de Evaluación (Autoevaluación)

Marca con una "X" cada punto que hayas completado y verificado con éxito:

- El proyecto Maven `veterinary-clinic` se creó correctamente.
- Las dependencias de SLF4j y Log4j2 se añadieron correctamente al `pom.xml`.
- El archivo `log4j2.xml` se creó en `src/main/resources` y configura el logging a consola y a un archivo.
- Se creó la clase `Patient` con atributos privados, constructor, getters/setters, `toString()`, y logging.
- Se creó la clase `Visit` con atributos privados, constructor, getters/setters, `toString()`, y logging.
- Los setters/constructores de `Patient` y `Visit` incluyen validaciones y lanzan `IllegalArgumentException` o `InvalidDataException` si los valores son inválidos, y registran el evento.
- Se crearon las excepciones personalizadas `PatientNotFoundException`, `InvalidDataException`, y `ClinicFullException`.
- Se creó la clase `ClinicManager` utilizando **Arrays** (`Patient[]` y `Visit[]`) y contadores para gestionar pacientes y visitas.
- El método `registerPatient` en `ClinicManager` añade pacientes, maneja duplicados (si se considera el `patientId` ingresado), lanza `ClinicFullException` si el array está lleno, y `InvalidDataException` por datos inválidos, con logging apropiado.
- El método `findPatientById` en `ClinicManager` busca en el array de pacientes y lanza `PatientNotFoundException` si el paciente no existe, y lo loguea.
- El método `addVisit` en `ClinicManager` registra visitas en el array global de visitas, verificando que el paciente exista, manejando `InvalidDataException` si los datos de la visita son inválidos o si el array de visitas está lleno.
- El método `getPatientVisits` retorna un nuevo array de `Visit[]` que contiene solo las visitas del paciente solicitado.
- El método `listAllPatients` retorna un nuevo array de `Patient[]` con todos los pacientes registrados.
- La clase `App.java` inicializa correctamente un `Logger` de SLF4j.
- El menú de la aplicación permite realizar todas las operaciones (registrar paciente, añadir visita, buscar paciente, listar visitas, listar todos los pacientes).
- La aplicación maneja `InputMismatchException` para entradas no numéricas y `DateTimeParseException` para fechas inválidas, y las loguea.
- Las excepciones personalizadas (`PatientNotFoundException`, `InvalidDataException`, `ClinicFullException`) son capturadas en App.java y sus detalles, incluido el _stack trace_, son logueados en nivel `ERROR`.
- Se utilizan los métodos `logger.info()`, `logger.warn()`, `logger.error()`, `logger.debug()` consistentemente en `Patient`, `Visit`, `ClinicManager` y `App`.
- Todos los mensajes de log utilizan la **sustitución de parámetros** (ej. `logger.info("Mensaje con parametro: {}", variable);`).
- Los mensajes de salida para el usuario están en español, mientras que el código fuente (nombres de clases, métodos, variables) está en inglés.
- La aplicación registra correctamente los mensajes tanto en la **consola** como en el **archivo de log**.
