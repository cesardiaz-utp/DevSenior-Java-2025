# Ejercicio Autónomo: Pruebas Unitarias para el Sistema de Gestión de Clínica Veterinaria

Continuaremos trabajando sobre el proyecto del Sistema de **Gestión de Clínica Veterinaria** que desarrollaste en el ejercicio anterior. Ahora, el objetivo no es añadir nuevas funcionalidades, sino **garantizar la calidad y robustez del código existente** a través de pruebas unitarias. Esto implica probar las clases de modelo (`Patient`, `Visit`) y, crucialmente, la lógica de negocio en `ClinicManager`, simulando sus dependencias cuando sea necesario.

## Objetivos del Ejercicio

Al completar este ejercicio, serás capaz de:

- **Aplicar JUnit 5** para escribir pruebas unitarias efectivas para clases de modelo y de lógica de negocio.
- **Diseñar casos de prueba** que cubran tanto el comportamiento esperado (`happy path`) como los escenarios de error (manejo de excepciones).
- **Utilizar anotaciones de JUnit 5** como `@Test`, `@DisplayName`, `@BeforeEach`, `@AfterEach`.
- **Aplicar aserciones de JUnit 5** (`Assertions.assertEquals`, `Assertions.assertTrue`, `Assertions.assertThrows`, etc.) para verificar el comportamiento del código.
- **Introducir Mockito** para crear objetos simulados (mocks) y verificar interacciones (verify).
- **Escribir pruebas unitarias** para métodos que interactúan con otras "dependencias" (incluso si en nuestro caso son arrays, simularemos escenarios donde otras clases se comportarían de forma específica).
- **Integrar las pruebas** dentro del proyecto Maven.

## Instrucciones Detalladas

### Paso 1: Configuración de Dependencias de JUnit y Mockito en `pom.xml`

1. Abre el archivo `pom.xml` de tu proyecto `veterinary-clinic`.
2. Dentro de la sección `<dependencies>`, asegura que tienes las dependencias de SLF4j y Log4j2.
3. **Añade la sección** `<dependencyManagement>` para incluir el BOM de JUnit 5. Esto te permite especificar la versión del BOM una sola vez y luego omitir las versiones en las dependencias individuales de JUnit.
4. **Añade las dependencias de JUnit 5 y Mockito** dentro de la sección `<dependencies>` con el `scope` adecuado (`test)`.

    ```XML
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <slf4j.version>2.0.17</slf4j.version>
        <log4j.version>2.24.3</log4j.version>
        <junit.version>5.12.2</junit.version>
        <mockito.version>5.18.0</mockito.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>${junit.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

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

        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>${mockito.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    ```

5. Guarda los cambios en `pom.xml`. Maven debería descargar las nuevas dependencias automáticamente.

### Paso 2: Creación de la Estructura de Pruebas

1. En tu proyecto Maven, las pruebas unitarias se colocan en la carpeta `src/test/java/`.
2. Dentro de `src/test/java/`, replica la estructura de paquetes de tu código fuente. Por ejemplo, si tienes `com.cursojava.veterinary.model` y `com.cursojava.veterinary.service`, crea `src/test/java/com/cursojava/veterinary/model` y `src/test/java/com/cursojava/veterinary/service`.

### Paso 3: Desarrollo de Pruebas Unitarias

Ahora, escribirás las clases de prueba para cada componente clave.

#### 3.1. Pruebas para Clases de Modelo (`PatientTest.java` y `VisitTest.java`)

- Crea un archivo `PatientTest.java` en `src/test/java/com/cursojava/veterinary/model/`.
- Crea un archivo `VisitTest.java` en `src/test/java/com/cursojava/veterinary/model/`.
- Para cada clase (`Patient`, `Visit`), escribe pruebas para:
  - **Creación de objetos**: Asegúrate de que los constructores inicializan los atributos correctamente.
  - **Getters**: Verifica que los getters retornen los valores esperados.
  - **Setters y validaciones**:
    - Prueba los setters con valores válidos y verifica que los atributos se actualicen.
    - Prueba los setters con valores inválidos (ej. edad negativa para `Patient`, costo negativo para `Visit`, cadenas vacías o nulas) y asegúrate de que lancen la excepción esperada (`IllegalArgumentException` o tu `InvalidDataException` si la refactorizaste para eso). Usa `Assertions.assertThrows()`.
    - **Método `toString()`**: Verifica que la representación de cadena sea la esperada (útil para depuración, aunque no es una prueba crítica de lógica de negocio).

```Java
package com.cursojava.veterinary.model;

import com.cursojava.veterinary.exception.InvalidDataException; // Si refactorizaste a esta excepción
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class PatientTest {

    @Test
    @DisplayName("Debe crear un paciente correctamente con datos válidos")
    void shouldCreatePatientWithValidData() {
        // Arrange
        String name = "Fido";
        String species = "Perro";
        int age = 3;
        String ownerName = "Carlos Ruiz";
        String ownerPhone = "555-1234";

        // Act
        Patient patient = new Patient(name, species, age, ownerName, ownerPhone);

        // Assert
        assertNotNull(patient.getPatientId(), "El ID del paciente no debería ser nulo");
        assertFalse(patient.getPatientId().isEmpty(), "El ID del paciente no debería estar vacío");
        assertEquals(name, patient.getName(), "El nombre del paciente no coincide");
        assertEquals(species, patient.getSpecies(), "La especie del paciente no coincide");
        assertEquals(age, patient.getAge(), "La edad del paciente no coincide");
        assertEquals(ownerName, patient.getOwnerName(), "El nombre del propietario no coincide");
        assertEquals(ownerPhone, patient.getOwnerPhone(), "El teléfono del propietario no coincide");
    }

    @Test
    @DisplayName("Debe lanzar IllegalArgumentException al crear paciente con edad negativa")
    void shouldThrowExceptionWhenCreatingPatientWithNegativeAge() {
        // Arrange
        String name = "Luna";
        String species = "Gato";
        int age = -1; // Edad inválida
        String ownerName = "Maria Gomez";
        String ownerPhone = "555-5678";

        // Act & Assert
        // Se espera que el constructor lance una IllegalArgumentException
        IllegalArgumentException thrown = assertThrows(IllegalArgumentException.class, () -> {
            new Patient(name, species, age, ownerName, ownerPhone);
        }, "Se esperaba IllegalArgumentException por edad negativa");

        assertTrue(thrown.getMessage().contains("negativa"), "El mensaje de error debe indicar edad negativa");
    }

    @Test
    @DisplayName("Debe actualizar la edad del paciente correctamente")
    void shouldUpdatePatientAgeCorrectly() {
        // Arrange
        Patient patient = new Patient("Leo", "León", 1, "Zoo", "999-9999");
        int newAge = 2;

        // Act
        patient.setAge(newAge);

        // Assert
        assertEquals(newAge, patient.getAge(), "La edad no se actualizó correctamente");
    }

    // ... Más pruebas para otros setters y validaciones ...
}
```

#### 3.2. Pruebas para la Lógica de Negocio (`ClinicManagerTest.java`)

- Crea un archivo `ClinicManagerTest.java` en `src/test/java/com/cursojava/veterinary/service/`.
- Para `ClinicManager`, las pruebas son más complejas porque la clase gestiona arrays y lanza excepciones específicas.
- **Configuración (Arrange)**: Utiliza `@BeforeEach` para inicializar una nueva instancia de `ClinicManager` antes de cada prueba, asegurando un estado limpio.
- **Pruebas de Registro de Pacientes**:
  - Prueba `registerPatient()` con un paciente válido. Verifica que el `patientCount` aumente y que el paciente se pueda encontrar.
  - Prueba `registerPatient()` con un paciente nulo o con datos inválidos (aprovechando las excepciones de `Patient`).
  - Prueba `registerPatient()` cuando la clínica esté llena (alcanzando `MAX_PATIENTS`) y verifica que lance `ClinicFullException`.
- **Pruebas de Búsqueda de Pacientes (`findPatientById`)**:
  - Prueba a buscar un paciente existente y verifica que sea el correcto.
  - Prueba a buscar un paciente que no existe y verifica que lance `PatientNotFoundException`.
- **Pruebas de Añadir Visitas (`addVisit`)**:
  - Prueba a añadir una visita a un paciente existente y verifica que la visita se pueda recuperar.
  - Prueba a añadir una visita a un paciente que no existe y verifica que lance `PatientNotFoundException`.
  - Prueba a añadir una visita con datos inválidos (ej. costo negativo) y verifica que lance la excepción adecuada.
  - Prueba a añadir una visita cuando el array de visitas esté lleno (si implementaste una capacidad para ello).
- **Pruebas de Obtener Visitas (`getPatientVisits`)**:
  - Prueba a obtener visitas de un paciente con varias visitas.
  - Prueba a obtener visitas de un paciente sin visitas (debería retornar un array vacío).
  - Prueba a obtener visitas de un paciente que no existe (debería lanzar `PatientNotFoundException`).
- **Pruebas de Listado (`listAllPatients`)**:
  - Prueba `listAllPatients()` cuando no hay pacientes.
  - Prueba `listAllPatients()` cuando hay varios pacientes.

#### 3.3. Uso de Mockito (Opcional Avanzado, pero recomendable para practicar)

Aunque `ClinicManager` usa directamente arrays internos, para simular un escenario donde `ClinicManager` podría depender de un "repositorio" externo o de una "clase de validación" más compleja, podríamos introducir un mock.

##### Escenario para Mocking (Ejemplo)

Imagina que `ClinicManager` tuviera una dependencia de una interfaz `PatientIdGenerator` para generar los IDs de los pacientes, o de un `ValidatorService` para validar los datos. Aunque tu `Patient` ya genera un UUID o el `ClinicManager` puede hacerlo, si tuvieras algo como:

```Java
// Interfaz simulada para la generación de IDs
public interface IdGenerator {
    String generateId();
}

// Dentro de ClinicManager
private final IdGenerator idGenerator; // Inyectada en el constructor

public ClinicManager(IdGenerator idGenerator) {
    this.idGenerator = idGenerator;
    // ...
}

public void registerPatient(Patient patient) {
    // String newId = idGenerator.generateId();
    // patient.setPatientId(newId); // Si el ID se asignara así
    // ...
}
```

- **¿Cómo probar esto con Mockito?**
  - Crea un mock de `IdGenerator`: `IdGenerator mockIdGenerator = mock(IdGenerator.class);`
  - Define el comportamiento del mock: `when(mockIdGenerator.generateId()).thenReturn("mocked-id-123");`
  - Instancia `ClinicManager` con el mock: `ClinicManager manager = new ClinicManager(mockIdGenerator);`
  - Luego, prueba `registerPatient` y verifica que `generateId()` fue llamado: `verify(mockIdGenerator).generateId();`

##### Para el ejercicio actual

Como `ClinicManager` solo usa arrays y objetos de modelo directamente, el mocking no es estrictamente necesario para su lógica interna. Sin embargo, para practicar, puedes modificar `ClinicManager` ligeramente para que reciba una dependencia mockeable en su constructor (ej. una interfaz `Logger` personalizada, o una interfaz `PatientValidator` si externalizaras las validaciones de `Patient`).

##### Alternativa de Mocking más sencilla para este ejercicio

Podrías crear una versión "mock" de un `Patient` o `Visit` para verificar cómo `ClinicManager` interactúa con ellos, aunque las clases `Patient` y `Visit` son modelos simples y suelen ser probadas directamente. La mayor utilidad de Mockito aquí sería si `ClinicManager` tuviera "colaboradores" de servicio más complejos.

**Recomendación**: Para este ejercicio, concéntrate en probar `ClinicManager` asumiendo que los objetos `Patient` y `Visit` que le pasas son reales. Si quieres un reto extra de Mockito, puedes intentar:
    - **Verificar interacciones**: Por ejemplo, si en `addVisit` el `ClinicManager` llamara un método específico de la instancia de `Visit` (ej. `visit.markAsProcessed()`), podrías mockear `Visit` y verificar esa llamada. Sin embargo, en el diseño actual, `ClinicManager` solo lee y almacena `Visits`.
    - **Spying en `Patient` o `Visit` (avanzado)**: Si quisieras verificar que, por ejemplo, el método `setStock` en `Patient` (si `Patient` fuera una dependencia de `ClinicManager` de otra forma) fue llamado.

### Paso 4: Ejecución de Pruebas

1. Abre tu terminal en la raíz del proyecto `veterinary-clinic`.
2. Ejecuta todas las pruebas unitarias con Maven:

    ```Bash
    mvn test
    ```

3. Observa la salida en la consola. Deberías ver un resumen de las pruebas ejecutadas, cuántas pasaron y cuántas fallaron.
4. Si tienes fallos, revisa el stack trace para identificar la causa y corregir tu código o tu prueba.

## Criterios de Evaluación (Autoevaluación)

Tu ejercicio será evaluado en base a los siguientes puntos:

- Las dependencias de JUnit 5 y Mockito se añadieron correctamente al `pom.xml` con el `scope` `test`.
- Se creó la estructura de carpetas `src/test/java/` y los paquetes correspondientes.
- Se creó la clase `PatientTest.java` y contiene pruebas para:
  - Constructor de `Patient` (datos válidos).
  - Getters de `Patient`.
  - Setters de `Patient` (datos válidos).
  - Setters/Constructor de `Patient` con datos inválidos, verificando que lancen la excepción correcta (`IllegalArgumentException` o `InvalidDataException`).
- Se creó la clase `VisitTest.java` y contiene pruebas para:
  - Constructor de `Visit` (datos válidos).
  - Getters de `Visit`.
  - Setters de `Visit` (datos válidos).
  - Setters/Constructor de `Visit` con datos inválidos, verificando que lancen la excepción correcta.
- Se creó la clase `ClinicManagerTest.java` y contiene pruebas para:
  - Inicialización de `ClinicManager` usando `@BeforeEach`.
  - `registerPatient()` (caso de éxito, verificación de aumento de contador y capacidad de encontrar el paciente).
  - `registerPatient()` con paciente `null` o datos inválidos (verificando `InvalidDataException` o `IllegalArgumentException`).
  - `registerPatient()` cuando la clínica está llena (verificando `ClinicFullException`).
  - `findPatientById()` (paciente existente).
  - `findPatientById()` (paciente no existente, verificando `PatientNotFoundException`).
  - `addVisit()` (caso de éxito).
  - `addVisit()` para paciente no existente (verificando `PatientNotFoundException`).
  - `addVisit()` con datos de visita inválidos (verificando la excepción adecuada).
  - `getPatientVisits()` (paciente con varias visitas).
  - `getPatientVisits()` (paciente sin visitas).
  - `getPatientVisits()` (paciente no existente, verificando `PatientNotFoundException`).
  - `listAllPatients()` (cuando hay pacientes y cuando no).
- Todos los métodos de prueba tienen nombres descriptivos (ej. `shouldDoSomethingWhenCondition`).
- Se utilizan las anotaciones `@DisplayName` para nombres legibles en los reportes de prueba.
- Las aserciones de JUnit 5 (`assertEquals`, `assertNotNull`, `assertTrue`, `assertThrows`, etc.) se usan apropiadamente.
- (Opcional Avanzado) Se intentó utilizar Mockito para simular alguna dependencia o verificar una interacción, aunque sea un escenario simplificado, y se entendió su propósito.
- Todas las pruebas se ejecutan exitosamente con `mvn test`.
