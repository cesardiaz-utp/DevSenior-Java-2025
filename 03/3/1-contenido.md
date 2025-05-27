# Clase 3: Pruebas Unitarias con JUnit 5 y Mockito

¡Hola a todos!

Están a punto de sumergirse en una clase que, sin exagerar, cambiará la forma en que abordan el desarrollo de software. Hasta ahora, han estado construyendo bases sólidas con Maven, el manejo de excepciones y la invaluable habilidad del logging, y eso es genial. Pero la realidad de las aplicaciones profesionales va más allá de que el código "simplemente funcione" en tu máquina.

Hoy, van a adquirir una de las **habilidades más críticas y demandadas** en cualquier rol de desarrollo: la capacidad de **garantizar la calidad** de su código de forma sistemática y automática. Aprenderán a verificar que cada componente de su software es robusto, fiable y se comporta exactamente como debería, incluso cuando se enfrentan a cambios y complejidades. El arte de las pruebas unitarias no es solo una buena práctica; es la **columna vertebral de la confianza** en el desarrollo de software y una **habilidad indispensable** para cualquier equipo profesional.

Estén listos para dar un salto cualitativo en su crecimiento técnico.

## Objetivos de Aprendizaje de la Clase

Al finalizar esta clase, serás capaz de:

- **Comprender qué son las pruebas unitarias** y la vital importancia que tienen en el desarrollo de software moderno.
- Aprender a **escribir pruebas unitarias básicas** utilizando el poderoso framework **JUnit 5**.
- Introducir el concepto de **simulación (mocking)** de dependencias con **Mockito**, una herramienta clave para aislar y probar tu código de forma efectiva.

## 1. Pruebas de Software

Las **pruebas de software** son un conjunto de actividades destinadas a evaluar y verificar que un programa o sistema cumple con los requisitos especificados y funciona correctamente. Su objetivo principal es detectar defectos, asegurar la calidad y validar que el software se comporta como se espera en diferentes escenarios, tanto normales como excepcionales. Las pruebas ayudan a prevenir errores en producción y a mejorar la confiabilidad y mantenibilidad del software.

### 1.1. ¿Qué es un caso de prueba?

- Conjunto de condiciones y variables bajo las cuales el analista (o tester) va a determinar si el requisito de la aplicación es parcial o completamente satisfactorio.
- Existen dos tipos de casos:
  - **Positivos**: Muestran una funcionalidad
  - **Negativos**: Comprueban situaciones en las que hay situaciones de errores.
- Cada requisito debe tener al menos un caso de prueba.
- De cada caso de prueba se debe tener una **entrada conocida** y una **salida esperado**.
- Los casos de prueba deben ser trazables

**Ejemplo**:

- **Requisito**: El precio del producto debe ser mayor o igual a cero.
- Casos de prueba positivos:
  - Precio es igual a 0
  - Precio es igual a 10
  - Precio es igual a 15
- Casos de prueba negativo:
  - Precio es igual a -5

### 1.2. Niveles de Prueba

Se definen como grupos de actividades de prueba que se organizan y se gestionan en conjunto y son realizadas según el nivel de desarrollo en que se encuentre el producto.

![Niveles de prueba](https://media.licdn.com/dms/image/v2/D4D22AQHjTJlx5VI3eA/feedshare-shrink_800/feedshare-shrink_800/0/1684176304714?e=2147483647&v=beta&t=NsL_4sDsFAdzNmBN4rXFC1dwbz1ErRg2kRsyjA9Ysf0)

#### Pruebas de componente o unitarias

Generalmente realizadas por el desarrollador y automatizadas, estas pruebas se enfocan en los componentes, unidades o módulos, es decir, los elementos más pequeños del software.

Algunos objetivos de estas pruebas son:

- Verificar que los comportamientos funcionales y no funcionales del componente son los diseñados y especificados.
- Encontrar defectos.
- Prevenir la propagación de defectos en otros niveles de prueba.

Estas **son responsabilidad de los desarrolladores**

![Pruebas unitarias](https://static.wixstatic.com/media/a59c1e_ece7506c8b334314bb66c7d78016cf97~mv2.png)

#### Pruebas de integración

Estas pruebas se centran en la interacción entre distintos componentes o sistemas.

Algunos objetivos de estas pruebas son:

- Uno de sus objetivos principales es **encontrar defectos**, que bien pueden estar en las interfaces o en los componentes o sistemas.
- Detectar pérdida de datos.
- Manipulación errónea de datos.
- Los componentes interpretan los datos de entrada de manera diferente.

Estas **pueden ser responsabilidad de los desarrolladores** (generalmente cuando es una prueba de integración de componentes) **o de los testers** (generalmente cuando es una prueba de integración entre sistemas).

![Pruebas de integración](https://static.wixstatic.com/media/a59c1e_080ea72c71894d4d9f3e1828511d4063~mv2.png)

Si se integra un **módulo A** con un **módulo B**, la prueba de integración se centra en la comunicación entre los módulos o sistemas, no en la funcionalidad de estos en sí (aunque no se descarta la posibilidad de encontrar defectos en las funcionalidades).

#### Pruebas de sistema

**Realizadas por los testers**, estas pruebas se enfocan en el comportamiento y las capacidades de los sistemas o productos. En este nivel se toman en cuenta los escenarios _end-to-end_.

![Pruebas de integración](https://static.wixstatic.com/media/a27d24_2e06a71c21db45fb9a34fb64b25229c6~mv2.jpg)

Tienen como objetivo:

- La reducción de riesgos.
- Verificar que los requerimientos funcionales y no funcionales del sistema sean cumplidos.
- Validar que el sistema funciona como se espera.
- Generar confianza en la calidad del sistema.
- Prevenir y encontrar defectos.

El entorno en el que se realizan estas pruebas debe tener condiciones similares al ambiente de producción.

#### Pruebas de aceptación

Este nivel de prueba al igual que el de sistemas, se enfoca en el comportamiento de todo el sistema o producto.

Algunos objetivos son:

- Validar que el sistema se encuentra completo y que funcionará de acuerdo con lo esperado.
- Establecer confianza en la calidad del sistema.
- Verificar que los comportamientos funcionales y no funcionales son los especificados.
- Proveer información que ayude a decidir si el sistema está óptimo para salir a producción.

Estas pruebas tienen cuatro subdivisiones:

- **Pruebas de aceptación del usuario (UAT)**: En estas pruebas los usuarios validan si el sistema satisface sus necesidades.
- **Pruebas de aceptación operativa**: Estas se centran en los aspectos operativos y son realizadas por parte del personal de operaciones o de la administración del sistema.
- **Pruebas de aceptación contractuales y normativas**: Se realizan con el objetivo de generar conformidad en cuanto a contratos y normas. Suele ser realizada por usuarios o testers y sus resultados son auditados por agencias reguladoras.
- **Pruebas Alfa y Beta**: Las pruebas alfa y beta son utilizadas por los desarrolladores de software comercial con el objetivo de obtener retroalimentación de los usuarios y/o clientes. La diferencia entre ellas es que las pruebas alfa se realizan dentro de la misma organización que desarrolla (no por el equipo de desarrollo) y las beta son realizadas por clientes potenciales o existentes en sus propias instalaciones.

### 1.3. Pirámide ideal de pruebas de Software

![Pirámide ideal de pruebas de Software](assets/piramide_ideal.png)

![Desarrollo tradicional vs Desarrollo Agile](https://mauroslv.wordpress.com/wp-content/uploads/2023/06/bb.jpg)

## 2. Introducción a las Pruebas Unitarias: ¿Qué son y por qué son importantes?

Imaginen que están construyendo un coche. No esperarían a terminar de ensamblar todo el vehículo para probar si el motor funciona, o si los frenos responden. ¡Eso sería un desastre! En lugar de eso, prueban cada componente individualmente: el motor en un banco de pruebas, los frenos en un sistema de simulación, las luces, la dirección...

En el desarrollo de software, hacemos algo muy similar con las **pruebas unitarias**.

### 2.1. ¿Qué es una Prueba Unitaria?

Una prueba unitaria es un **fragmento de código** que se escribe para verificar que la **unidad más pequeña y aislada** de tu programa funciona como se espera. ¿Qué es una "unidad" de código? Generalmente, nos referimos a un **método** o una **clase específica**. Queremos asegurar que cada "pieza" individual de nuestro software opera correctamente por sí misma.

### 2.2. ¿Por qué son Cruciales las Pruebas Unitarias?

La importancia de las pruebas unitarias va mucho más allá de simplemente encontrar errores. Son una piedra angular de la calidad del software y un pilar fundamental en metodologías ágiles como **Desarrollo Guiado por Pruebas (TDD - Test-Driven Development)**:
    - **Detección Temprana de Errores**: Encontrar fallos en las etapas iniciales del desarrollo es _mucho más barato y más fácil_ que descubrirlos cuando el software ya está en producción. Piensen en ello como una "alarma" temprana.
    - **Confianza para Refactorizar y Evolucionar**: Este es uno de los mayores beneficios. Imaginen que necesitan cambiar la forma interna de un método (`refactorizar`) o añadir una nueva funcionalidad. Sin pruebas, tendrían miedo de "romper" algo que ya funcionaba. Con una buena suite de pruebas, tienen una **red de seguridad** que les da la confianza para hacer cambios audaces, sabiendo que si algo deja de funcionar, las pruebas se lo dirán de inmediato. Esto fomenta la evolución constante y saludable del código.
    - **Mejoran el Diseño del Código**: Para que un código sea fácil de probar unitariamente, tiende a ser más **modular**, con responsabilidades claras y menos acoplamiento (dependencias) entre sus componentes. Este requisito "oculto" de la testabilidad naturalmente conduce a un mejor diseño de software: más limpio, más mantenible y más extensible.
    - **Documentación Viva y Confiable**: Una prueba unitaria bien escrita actúa como una forma de documentación ejecutable. Muestra cómo se espera que se comporte una clase o un método en diferentes escenarios de uso, incluyendo casos de éxito y de error. A diferencia de la documentación escrita que puede volverse obsoleta, ¡las pruebas siempre están actualizadas con el código porque si no, no pasarían!
    - **Acelera el Ciclo de Desarrollo**: Aunque parezca contradictorio invertir tiempo en escribir pruebas, el feedback instantáneo que obtienes de ellas (¿funciona mi último cambio o lo rompió?) te permite detectar y corregir problemas de manera mucho más eficiente, lo que al final acelera el desarrollo y reduce el tiempo de depuración.

## 3. El Ciclo de una Prueba Unitaria: `Arrange`, `Act`, `Assert` (AAA)

Para que tus pruebas unitarias sean claras, consistentes y fáciles de entender por ti y por otros desarrolladores, se recomienda encarecidamente seguir el patrón **AAA**:

- **`Arrange` (Preparar)**: Esta es la fase donde **configuras el estado inicial** para tu prueba. Piensa en todo lo que necesitas tener listo antes de ejecutar la acción que vas a probar. Es como preparar el escenario de una obra de teatro.
  - **Crear y configurar objetos**: Instancias las clases que vas a probar (la "unidad bajo prueba") y cualquier otra que sea necesaria para el escenario.
  - **Inicializar datos**: Define variables, configura valores de entrada, o epara cualquier dato que el método bajo prueba vaya a necesitar.
  - **Establecer condiciones previas**: Asegúrate de que el entorno esté listo para la ejecución de la prueba.

  **Ejemplo con nuestro sistema de veterinaria**: Si vas a probar el método `registerPatient()` en tu `ClinicManager`, en esta fase podrías crear una instancia de `ClinicManager` y un objeto `Patient` con datos válidos que intentarás registrar.

- **`Act` (Actuar)**: En esta fase, **ejecutas la "unidad" de código que quieres probar**. Esta es la única acción que desencadena el comportamiento que deseas verificar. Mantén esta fase tan concisa como sea posible.
  - Llamas al método específico del objeto que estás testeando.

  **Ejemplo**: Llama a `clinicManager.registerPatient(pacienteValido)`.

- **`Assert` (Verificar)**: Aquí es donde **compruebas si el resultado de la acción fue el esperado**. Utilizas las **aserciones** (afirmaciones) proporcionadas por tu framework de pruebas (JUnit en nuestro caso) para validar que todo salió como debía.
  - **Validar el estado**: Comprueba los valores de los atributos de los objetos después de la acción.
  - **Validar el valor de retorno**: Verifica si el método Act devolvió el valor correcto.
  - **Verificar excepciones**: Asegúrate de que, en ciertos escenarios (como d
  datos inválidos), se lanzó la excepción correcta.

  **Ejemplo**: Asegúrate de que el `clinicManager` ahora tiene un paciente más, o que el `patientId` del paciente registrado no es nulo, o que se lanzó una `InvalidDataException` si intentaste registrar un paciente inválido.

Este patrón AAA hace que tus pruebas sean extremadamente legibles, organizadas y fáciles de mantener, lo cual es fundamental a medida que tu suite de pruebas crece.

## 4. JUnit 5: Tu Framework Esencial para Pruebas en Java

**JUnit 5** es el estándar de facto y el framework más popular para escribir pruebas unitarias en el ecosistema Java. Proporciona un conjunto robusto de herramientas y anotaciones para estructurar, escribir y ejecutar tus pruebas de manera eficiente y moderna.

### 4.1. Arquitectura de JUnit 5: Jupiter, Platform y Vintage

JUnit 5 está compuesto por **tres subproyectos principales**, cada uno con un propósito específico. Entender esta arquitectura te ayudará a elegir las dependencias correctas y aprovechar al máximo el framework:

- **JUnit Platform**: Es la base sobre la que se ejecutan los tests. Proporciona la infraestructura para descubrir, ejecutar y reportar pruebas. Permite que diferentes motores de pruebas (no solo JUnit) se integren y ejecuten en el mismo entorno. Incluye el **Console Launcher** y APIs para integraciones con IDEs y herramientas como Maven o Gradle.

- **JUnit Jupiter**: Es el **motor de pruebas moderno** de JUnit 5. Incluye las nuevas anotaciones (`@Test`, `@BeforeEach`, etc.), el modelo de extensión y las aserciones mejoradas. Cuando escribes pruebas con JUnit 5, normalmente usas Jupiter.

- **JUnit Vintage**: Permite ejecutar pruebas escritas para **JUnit 3 y JUnit 4** en el entorno de JUnit 5. Es útil si tienes un proyecto con pruebas antiguas y quieres migrar gradualmente a JUnit 5 sin perder compatibilidad.

**Resumen visual:**

| Proyecto         | Propósito principal                                         |
|------------------|------------------------------------------------------------|
| JUnit Platform   | Infraestructura de ejecución y descubrimiento de tests      |
| JUnit Jupiter    | Motor y API para escribir pruebas modernas (JUnit 5)        |
| JUnit Vintage    | Motor para ejecutar pruebas antiguas (JUnit 3/4)            |

**¿Qué dependencias necesitas?**

- Para escribir y ejecutar pruebas modernas:  
  - `junit-jupiter-api` (API de anotaciones y aserciones)
  - `junit-jupiter-engine` (motor de ejecución Jupiter)
- Para ejecutar pruebas antiguas (JUnit 3/4):  
  - `junit-vintage-engine`
- Para integración con herramientas/IDEs:  
  - `junit-platform-launcher`, `junit-platform-console`, etc. (normalmente gestionadas por tu build tool)

En la mayoría de los proyectos modernos solo necesitas Jupiter, pero puedes añadir Vintage si tienes pruebas heredadas.

### 4.2. Configuración en Maven (`pom.xml`)

Para que Maven sepa cómo compilar y ejecutar tus pruebas con JUnit 5, necesitas añadir las dependencias correctas en tu archivo `pom.xml`. Una **buena práctica** y una recomendación clave es usar el **BOM (Bill of Materials)** de JUnit 5. Un BOM es un archivo especial (`.pom`) que te permite especificar la versión de un conjunto de librerías (como todas las de JUnit 5) una sola vez. Luego, Maven se encarga de que todas las dependencias individuales de ese conjunto (como `junit-jupiter-api` o `junit-jupiter-engine`) usen esa misma versión, garantizando la compatibilidad y simplificando la gestión de dependencias.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cursojava.veterinary</groupId>
    <artifactId>veterinary-clinic</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <slf4j.version>2.0.17</slf4j.version>
        <log4j.version>2.24.3</log4j.version>
        <junit.jupiter.version>5.12.2</junit.jupiter.version>
        <mockito.version>5.18.0</mockito.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>${junit.jupiter.version}</version>
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
</project>
```

**Una vez más, un recordatorio importante**: Después de modificar el `pom.xml`, guarda los cambios y permite que tu IDE o Maven (ejecutando `mvn clean install` en la terminal) descarguen y resuelvan las nuevas dependencias.

### 4.3. Estructura Básica de una Clase de Prueba

En un proyecto Maven, el código fuente de tu aplicación va en la carpeta `src/main/java/` y el código de tus pruebas va en la carpeta `src/test/java/`. Es una buena práctica y convención replicar la misma estructura de paquetes que tienes en `src/main/java/` dentro de `src/test/java/`.

**Convención de Nombres de Clases de Prueba**: Si tienes una clase de la aplicación llamada `MiClase.java` en `src/main/java/com/miempresa/app/`, su clase de prueba correspondiente se llamará `MiClaseTest.java` y estará ubicada en `src/test/java/com/miempresa/app/`. Esto facilita la localización de las pruebas para cada componente.

### 4.4. Anotaciones Clave de JUnit 5

Las anotaciones son etiquetas especiales (comienzan con `@`) que le proporcionan metadatos a JUnit, indicándole cómo debe tratar tus métodos y clases de prueba.

- `@Test`: ¡Es la anotación más fundamental y la que usarás más a menudo! Marca un método como un **caso de prueba ejecutable**. Cuando ejecutes tus pruebas, JUnit buscará todos los métodos marcados con `@Test` y los ejecutará.

  ```Java
  // Importa la anotación desde org.junit.jupiter.api.Test
  import org.junit.jupiter.api.Test;

  public class EjemploTest {
      @Test
      void miPrimeraPrueba() {
          // Aquí dentro va el código de tu prueba, siguiendo el patrón Arrange, Act, Assert (AAA).
          // Ejemplo:
          // Arrange
          int num1 = 5;
          int num2 = 3;
          // Act
          int suma = num1 + num2;
          // Assert
          // (Veremos las aserciones en la siguiente sección)
      }
  }
  ```

- `@DisplayName("Descripción clara de lo que prueba el método")`: Esta anotación te permite dar un **nombre descriptivo y legible** a tu prueba, que aparecerá en los reportes de ejecución de JUnit. Es mucho más útil que solo ver el nombre del método en el reporte. ¡Siempre úsala para mejorar la claridad de tus reportes de prueba!

  ```Java
  import org.junit.jupiter.api.DisplayName;
  import org.junit.jupiter.api.Test;

  public class CalculadoraTest {
      @Test
      @DisplayName("Debe sumar dos números positivos y retornar el resultado correcto")
      void shouldAddTwoPositiveNumbersAndReturnCorrectResult() {
          // Arrange
          int a = 10;
          int b = 5;
          Calculadora calc = new Calculadora(); // Suponiendo una clase Calculadora

          // Act
          int resultado = calc.sumar(a, b);

          // Assert
          assertEquals(15, resultado); // Aserción de JUnit
      }
  }
  ```

- `@BeforeEach`: Un método con esta anotación se ejecutará **antes de CADA método de prueba** (`@Test`) en la clase. Es increíblemente útil para:
  - **Inicializar objetos o variables** que necesitas en cada prueba, asegurando que cada test comience con un **estado limpio y predecible**.
  - **Evitar la repetición de código** de configuración entre diferentes pruebas.

  ```Java
  import org.junit.jupiter.api.BeforeEach;
  import org.junit.jupiter.api.Test;
  import static org.junit.jupiter.api.Assertions.assertEquals; // Importamos estáticamente la aserción

  public class MiServicioTest {
      private MiServicio miServicio; // La clase que vamos a probar
      private Usuario usuarioPrueba; // Objeto común que usaremos en varias pruebas

      @BeforeEach // Este método se ejecuta antes de cada @Test
      void setUp() {
          miServicio = new MiServicio(); // Cada prueba tendrá una instancia nueva y limpia de MiServicio
          usuarioPrueba = new Usuario("user123", "password"); // Y un Usuario para probar
          System.out.println("-> Ejecutando @BeforeEach: Configurando el entorno para una nueva prueba...");
      }

      @Test
      @DisplayName("Debe permitir el login de un usuario válido")
      void shouldAllowLoginForValidUser() {
          // Arrange (complementado por setUp)
          // Act
          boolean isLoggedIn = miServicio.login(usuarioPrueba.getUsername(), usuarioPrueba.getPassword());
          // Assert
          assertEquals(true, isLoggedIn); // O simplemente assertTrue(isLoggedIn);
          System.out.println("   -> Ejecutando prueba: shouldAllowLoginForValidUser");
      }

      @Test
      @DisplayName("Debe denegar el login con contraseña incorrecta")
      void shouldDenyLoginWithIncorrectPassword() {
          // Arrange (complementado por setUp)
          // Act
          boolean isLoggedIn = miServicio.login(usuarioPrueba.getUsername(), "wrong_password");
          // Assert
          assertEquals(false, isLoggedIn); // O assertFalse(isLoggedIn);
          System.out.println("   -> Ejecutando prueba: shouldDenyLoginWithIncorrectPassword");
      }
      // ... otros métodos @Test ...
  }
  ```

- `@AfterEach`: Un método anotado con `@AfterEach` se ejecutará **después de CADA método de prueba**. Es útil para limpiar recursos que podrías haber asignado (cerrar archivos, desconectar de bases de datos, etc.), aunque en la mayoría de las pruebas unitarias simples y bien aisladas no es tan común como `@BeforeEach`.

  ```Java
  // ... siguiendo el ejemplo anterior ...
  public class MiServicioTest {
      // ... @BeforeEach ...

      @AfterEach
      void tearDown() {
          // Limpiar recursos si es necesario después de cada prueba
          miServicio = null; // Liberar la referencia para el Garbage Collector
          usuarioPrueba = null;
          System.out.println("<- Ejecutando @AfterEach: Limpiando después de la prueba.");
      }
      // ...
  }
  ```

- `@BeforeAll` y `@AfterAll`: Estos son similares a `BeforeEach` y `AfterEach`, pero tienen una diferencia crucial: se ejecutan **una sola vez** para toda la clase de prueba, no para cada método. Los métodos anotados con ellos deben ser `static`. Son útiles para configuraciones muy costosas que se pueden compartir entre todas las pruebas en esa clase, como levantar un servidor de pruebas en memoria o inicializar una conexión global que no cambie el estado entre pruebas.

  ```Java
  import org.junit.jupiter.api.BeforeAll;
  import org.junit.jupiter.api.AfterAll;
  // ...

  public class DatabaseTest {
      // Esta base de datos se inicializará una sola vez para todas las pruebas en esta clase
      private static TestDatabase testDatabase;

      @BeforeAll // Se ejecuta una vez antes de todas las pruebas (debe ser static)
      static void setUpAll() {
          testDatabase = new TestDatabase("jdbc:h2:mem:testdb");
          testDatabase.connect();
          testDatabase.loadSchema();
          System.out.println(">>> Ejecutando @BeforeAll: Conectando a la base de datos de pruebas...");
      }

      @AfterAll // Se ejecuta una vez después de todas las pruebas (debe ser static)
      static void tearDownAll() {
          testDatabase.disconnect();
          System.out.println("<<< Ejecutando @AfterAll: Desconectando la base de datos de pruebas.");
      }

      @Test
      void shouldSaveDataCorrectly() {
          // ... prueba que usa testDatabase ...
      }

      @Test
      void shouldRetrieveDataSuccessfully() {
          // ... otra prueba que usa testDatabase ...
      }
  }
  ```

### 4.5. Aserciones de JUnit 5: Verificando el Comportamiento

Las **aserciones** son los métodos que usas en la fase `Assert` del patrón AAA para verificar que tu código se comporta como esperas. Si una aserción falla, la prueba fallará y JUnit te lo indicará. Si todas las aserciones de una prueba pasan, la prueba se considera exitosa.

Todas las aserciones de JUnit 5 residen en la clase estática `org.junit.jupiter.api.Assertions`. Para usarlas de forma más limpia en tu código, es una práctica común importarlas estáticamente:

`import static org.junit.jupiter.api.Assertions.*;`

Aquí están algunas de las aserciones más comunes y útiles:

- `assertEquals(expected, actual, [message])`:
  - **Propósito**: Verifica que dos valores son iguales. Puede comparar tipos primitivos, objetos (usando su método `equals()`), y cadenas de texto. El `message` es opcional y se mostrará si la aserción falla, lo que es muy útil para depurar.
  - **Ejemplo**:

    ```Java
    // Arrange
    int valorEsperado = 10;

    // Act
    int valorObtenido = miMetodoQueCalcula(); // Asume que este método retorna 10

    // Assert
    assertEquals(valorEsperado, valorObtenido, "El valor calculado no coincide con el esperado.");
    ```

  - **Uso**: Para comprobar valores retornados por métodos, el estado final de los atributos de los objetos después de una operación, etc.
- `assertTrue(condition, [message])`:
  - **Propósito**: Verifica que una **condición booleana es verdadera**.
  - **Ejemplo**:

    ```Java
    // Act
    boolean esAdmin = usuario.isAdmin(); // Asume que el usuario no es admin
    
    // Assert
    assertTrue(esAdmin, "El usuario debería ser administrador."); // Esto fallará
    ```

  - **Uso**: Para verificar estados booleanos, resultados de expresiones lógicas.
- `assertFalse(condition, [message])`:
  - **Propósito**: Verifica que una **condición booleana es falsa**.
  - **Ejemplo**:

    ```Java
    // Act
    boolean estaVacio = miLista.isEmpty(); // Asume que la lista NO está vacía

    // Assert
    assertFalse(estaVacio, "La lista no debería estar vacía."); // Esto fallará
    ```

- `assertNull(object, [message])`:
  - **Propósito**: Verifica que un objeto es `null`.
  - **Ejemplo**:

    ```Java
    // Act
    Object resultadoBusqueda = miServicio.buscarPorId("ID_NO_EXISTENTE");

    // Assert
    assertNull(resultadoBusqueda, "El resultado de la búsqueda debería ser nulo para un ID inexistente.");
    ```

- `assertNotNull(object, [message])`:
  - **Propósito**: Verifica que un objeto **no es** `null`.
  - **Ejemplo**:

    ```Java
    // Act
    Paciente paciente = clinicManager.findPatientById("VALID_ID");

    // Assert
    assertNotNull(paciente, "El paciente no debería ser nulo para un ID válido.");
    ```

- `assertThrows(ExpectedException.class, executable, [message])`:
  - **Propósito**: **¡Fundamental para probar el manejo de errores y validaciones!** Verifica que un bloque de código específico (dado como una expresión Lambda `()->{...}` o método de referencia) lanza una excepción del tipo `ExpectedException.class`. Si la excepción no se lanza, o se lanza una de otro tipo, la prueba falla. Retorna la excepción lanzada, permitiéndote verificar su mensaje o propiedades.
  - **Ejemplo**: Si tu clase `Patient` lanza una `IllegalArgumentException` si la edad es negativa:

    ```Java
    import org.junit.jupiter.api.Test;
    import static org.junit.jupiter.api.Assertions.assertThrows;
    import static org.junit.jupiter.api.Assertions.assertTrue;

    public class PatientTest {
        @Test
        @DisplayName("Debe lanzar IllegalArgumentException al intentar establecer una edad negativa")
        void shouldThrowExceptionWhenSettingNegativeAge() {
            // Arrange
            Patient patient = new Patient("Buddy", "Perro", 5, "Ana", "123-4567");

            // Act & Assert (La acción está dentro del assertThrows)
            IllegalArgumentException thrown = assertThrows(IllegalArgumentException.class, () -> {
                patient.setAge(-1); // Este código DEBE lanzar la excepción
            }, "Se esperaba una IllegalArgumentException por edad negativa, pero no se lanzó.");

            // Opcional: Verificar el mensaje de la excepción lanzada
            assertTrue(thrown.getMessage().contains("negativa"),
                      "El mensaje de error de la excepción debe indicar que la edad es negativa.");
        }
    }
    ```

  - **Uso**: Indispensable para validar que tus validaciones funcionan correctamente y que tu código protege los datos de entrada, o maneja situaciones inesperadas lanzando las excepciones adecuadas.

- `assertDoesNotThrow(executable, [message])`:
  - **Propósito**: Verifica que un bloque de código **no lanza ninguna excepción inesperada**.
  - **Ejemplo**:

    ```Java
    @Test
    @DisplayName("No debe lanzar excepción al establecer una edad válida")
    void shouldNotThrowExceptionWhenSettingValidAge() {
        // Arrange
        Patient patient = new Patient("Buddy", "Perro", 5, "Ana", "123-4567");

        // Act & Assert (La acción está dentro del assertThrows)
        assertDoesNotThrow(() -> {
            patient.setAge(10); // Este código NO debería lanzar excepción
        }, "No se esperaba ninguna excepción al establecer una edad válida.");
    }
    ```

- `assertArrayEquals(expectedArray, actualArray, [message])`:
  - **Propósito**: Compara el contenido de dos arrays para asegurar que son idénticos en orden y valor.
  - **Ejemplo**:

    ```Java
    // Arrange
    int[] expectedNumbers = {1, 2, 3};

    // Act
    int[] actualNumbers = miMetodoQueRetornaArray(); // Asume que retorna {1, 2, 3}

    // Assert
    assertArrayEquals(expectedNumbers, actualNumbers, "Los arrays no coinciden.");
    ```

### 4.6. Convenciones de Nombres para Métodos de Prueba

Un buen nombre de prueba es una forma de documentación en sí mismo. Debe comunicar de forma concisa qué se está probando y bajo qué condiciones.

- **Claridad y Descripción**: Evita nombres genéricos y poco informativos como `test1`, `test_sum`, `prueba_funcion`. Estos no dicen nada sobre el escenario o el comportamiento esperado.
- **Patrón `should[ComportamientoEsperado]When[Condicion]`**: Este patrón es el más recomendado y ampliamente adoptado por su claridad.
  - `shouldReturnTrueWhenInputIsValid()`: "Debe retornar verdadero cuando la entrada es válida."
  - `shouldThrowExceptionWhenNegativeAgeIsProvided()`: "Debe lanzar una excepción cuando se proporciona una edad negativa."
  - `shouldAddPatientSuccessfullyToClinic()`: "Debe añadir un paciente exitosamente a la clínica."
  - `shouldFindExistingPatientById()`: "Debe encontrar un paciente existente por su ID."
  - `shouldNotFindNonExistingPatientById()`: "No debe encontrar un paciente inexistente por su ID."

### 4.7. Ejecución de Pruebas con Maven

Una vez que tengas tus pruebas escritas, puedes ejecutarlas desde la línea de comandos en la raíz de tu proyecto Maven:

```Bash
mvn test
```

Este comando hará varias cosas:

1. Compilará el código fuente de tu aplicación (`src/main/java`).
2. Compilará el código de tus pruebas (`src/test/java`).
3. Ejecutará todas las pruebas unitarias que encuentre en la carpeta `src/test/java/`.
4. Mostrará un resumen en la consola que te indicará cuántas pruebas se ejecutaron, cuántas pasaron, cuántas fallaron y los detalles de cualquier fallo, incluyendo el _stack trace_ para ayudarte a depurar.

Si todas las pruebas pasan, verás un mensaje de "BUILD SUCCESS" y un resumen con `Tests run: X, Failures: 0, Errors: 0, Skipped: 0`. **¡Ese es el objetivo!**

## 5. Mockito: Simulando Dependencias para Pruebas Aisladas

Imagina que tu `ClinicManager` no solo gestiona pacientes y visitas en arrays en memoria, sino que también necesita interactuar con otros "colaboradores" o "servicios" para cumplir su función. Por ejemplo, podría necesitar:

- Un `PatientRepository` para guardar y recuperar la información de los pacientes de una base de datos.
- Un `EmailService` para enviar notificaciones a los dueños de mascotas.
- Un `Logger` para registrar eventos (¡aunque ya usamos SLF4j, Mockito nos permitiría simularlo!).

Si tus pruebas unitarias para `ClinicManager` llamaran a estos servicios **reales**:

- Serían **lentas**, porque tendrían que esperar respuestas de la base de datos o de un servidor de correo.
- Serían **no repetibles**, porque el estado de la base de datos podría cambiar entre ejecuciones de pruebas.
- Podrían tener **efectos secundarios no deseados** (¡enviar correos electrónicos reales!).

Aquí es donde entra **Mockito**.

### 5.1. ¿Por qué y cuándo usar mocks?

Mockito es una librería de código abierto que te permite crear **objetos simulados (mocks)** de los **colaboradores** (otras clases o interfaces) que tu unidad de código bajo prueba utiliza. Estos mocks reemplazan a los objetos reales durante la ejecución de la prueba unitaria, permitiéndote:

- **Controlar su Comportamiento**: Puedes "programar" al mock para que, cuando se le llame a un método específico, retorne un valor predefinido, lance una excepción específica, o no haga nada (en el caso de métodos `void`).
- **Verificar Interacciones**: Puedes confirmar que la unidad bajo prueba (la clase que estás testeando) llamó a ciertos métodos del mock, con qué argumentos y cuántas veces. Esto es crucial para probar cómo tu clase interactúa con sus colaboradores.
- **Aislar la Unidad de Prueba**: El beneficio más importante. Al reemplazar los colaboradores reales con mocks, te aseguras de que si una prueba falla, es por un error en la unidad que estás probando (tu `ClinicManager`), no por un error en uno de sus colaboradores (el `PatientRepository` o  } ). Esto hace que las pruebas sean verdaderamente "unitarias" y sus fallos sean mucho más fáciles de diagnosticar.

### 5.2. Conceptos Clave de Mockito

- **Mock**: Un objeto "falso" o "sustituto" que imita el comportamiento de un objeto real. Lo creas para una dependencia de tu clase bajo prueba. No tiene la lógica del objeto real, solo simula su interfaz.
- **Stubbing (`when().thenReturn()` / `when().thenThrow()` / `doNothing().when()`)**: Es el acto de "programar" el comportamiento de un mock. Le dices al mock qué valor específico debe retornar cuando se le llama a un método, o qué excepción debe lanzar, o simplemente que no haga nada.

  ```Java
  // Si el método devuelve un valor:
  // Cuando mockDatabaseService.findPatientById("P001") sea llamado, debe retornar un paciente específico.
  when(mockDatabaseService.findPatientById("P001")).thenReturn(new Patient("Juan", "Perro", 5, "Ana", "123"));

  // Puedes usar matchers para argumentos genéricos (de org.mockito.ArgumentMatchers):
  // Cuando mockDatabaseService.save() sea llamado con CUALQUIER objeto Patient, retorna un nuevo Patient.
  when(mockDatabaseService.save(any(Patient.class))).thenReturn(new Patient("Nuevo", "Gato", 2, "Mia", "456"));
  ```

  **Importante sobre `any()`**: Si usas un matcher como `any(Class)` (o `anyString()`, `anyInt()`, etc.) para un argumento en un método stubbeado o verificado, ¡debes usar _matchers_ para todos los argumentos de ese método! No puedes mezclar valores literales con matchers en la misma llamada `when()` o `verify()`.

  ```Java
  // Si el método no devuelve nada (void):
  // Cuando mockEmailService.sendEmail() sea llamado con CUALQUIER String para destinatario y mensaje, no hace nada.
  doNothing().when(mockEmailService).sendEmail(anyString(), anyString());

  // Cuando mockDatabaseService.connect() sea llamado, lanza una excepción (simula un error de conexión).
  doThrow(new RuntimeException("Error de conexión")).when(mockDatabaseService).connect();
  ```

- **Verification (`verify()`)**: Es el acto de comprobar si un método del mock fue llamado, cuántas veces, y con qué argumentos específicos. Esto te permite asegurarte de que tu clase bajo prueba interactuó correctamente con sus colaboradores (las dependencias simuladas).

  ```Java
  // Verifica que mockEmailService.sendEmail() fue llamado exactamente una vez con estos argumentos.
  verify(mockEmailService, times(1)).sendEmail("destino@ejemplo.com", "Mensaje de confirmación");

  // Verifica que mockDatabaseService.save() fue llamado una vez con CUALQUIER objeto Patient.
  verify(mockDatabaseService, times(1)).save(any(Patient.class));

  // Verifica que mockLogger.logError() NUNCA fue llamado.
  verify(mockLogger, never()).logError(anyString());

  // Verifica que mockPaymentGateway.processPayment() fue llamado al menos una vez.
  verify(mockPaymentGateway, atLeast(1)).processPayment(anyDouble());
  ```

### 5.3. `@Mock` y `@InjectMocks` (Anotaciones de Mockito con JUnit 5)

Para facilitar la creación e inyección de mocks en tus pruebas de JUnit 5, Mockito proporciona estas anotaciones. Son muy convenientes porque reducen el código boilerplate. Para que funcionen, debes añadir la anotación `@ExtendWith(MockitoExtension.class)` a tu clase de prueba.

- `@Mock`: Se usa en un campo de tu clase de prueba para que Mockito cree automáticamente un objeto mock de ese tipo. Es una alternativa a llamar a `Mockito.mock(MiClase.class)`.
- `@InjectMocks`: Se usa en un campo de la clase que quieres probar (la "unidad bajo prueba"). Mockito intentará crear una instancia real de esta clase e inyectar automáticamente los mocks creados con `@Mock` (o `@Spy`) en sus colaboradores, buscando un constructor adecuado o setters públicos.

### 5.4. Ejemplo Completo de Uso de Mockito con JUnit 5

Para ilustrar el uso de Mockito en tu proyecto `veterinary-clinic`, vamos a considerar una ligera modificación conceptual en `ClinicManager`. Imagina que tu `ClinicManager` no solo maneja los pacientes en memoria, sino que también necesita un P`atientRepository` (una interfaz que simula una interacción con almacenamiento de datos) para guardar y recuperar pacientes de forma persistente.

Primero, crea esta **interfaz sencilla** que simulará un "repositorio" de pacientes. Guárdala en `src/main/java/com/cursojava/veterinary/repository/`:

```Java
package com.cursojava.veterinary.repository;

import com.cursojava.veterinary.model.Patient;

public class PatientRepository {

    private Patient[] patients; // Ejemplo: Si mantienes la lógica con arrays
    private int patientCount;
    private static final int MAX_PATIENTS = 100; // Define un tamaño máximo si usas arrays

    public PatientRepository() {
        this.patients = new Patient[MAX_PATIENTS]; // Inicializa arrays si los usas
        this.patientCount = 0;
    }

    public Patient save(Patient patient) throws ClinicFullException {
        if (patientCount < MAX_PATIENTS) {
            this.patients[patientCount++] = patient;
        } else {
            throw new ClinicFullException("La clínica está llena, no se pueden registrar más pacientes.");
        }
        return patient;
    }

    public Patient findById(String id) {
        for (int i = 0; i < patientCount; i++) {
            if (this.patients[i].getPatientId() != null && this.patients[i].getPatientId().equals(id)) {
                logger.info("Paciente encontrado en array interno: {}", this.patients[i].getName());
                return this.patients[i];
            }
        }
        return null;
    }

    public Patient[] findAll() {
        // Retorna una copia del array de pacientes que realmente están ocupados.
        return Arrays.copyOf(patients, patientCount);
    } 
}
```

Ahora, para el propósito de este ejemplo de Mockito (sin cambiar drásticamente tu `ClinicManager` actual si usa arrays internos), vamos a **simular que** `ClinicManager` recibe este `PatientRepository` en su constructor. Si tu `ClinicManager` actual tiene un constructor sin argumentos, puedes añadir este nuevo constructor **adicionalmente** para las pruebas o usar un método "setter" para pasar el mock.

Modifica tu `ClinicManager` en `src/main/java/com/cursojava/veterinary/service/ClinicManager.java` para que pueda aceptar un `PatientRepository` en su constructor. Mantén tu constructor sin argumentos si lo necesitas para la aplicación principal por ahora.

```Java
package com.cursojava.veterinary.service;

import com.cursojava.veterinary.model.Patient;
import com.cursojava.veterinary.model.Visit;
import com.cursojava.veterinary.exception.ClinicFullException;
import com.cursojava.veterinary.exception.InvalidDataException;
import com.cursojava.veterinary.exception.PatientNotFoundException;
import com.cursojava.veterinary.repository.PatientRepository; // Asegúrate de importar

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ClinicManager {
    private static final Logger logger = LoggerFactory.getLogger(ClinicManager.class);

    // ¡Este es el colaborador que queremos simular en las pruebas!
    private final PatientRepository patientRepository;

    // Constructor principal si tu app aún usa arrays sin repositorio explícito (mantener para compatibilidad)
    public ClinicManager() {
        this(new PatientRepository()); // Una implementación por defecto si no se pasa uno
        logger.info("ClinicManager inicializado con PatientRepositoryInMemory.");
    }

    // Constructor para pruebas o si tu aplicación usa un repositorio real
    public ClinicManager(PatientRepository patientRepository) {
        this.patientRepository = patientRepository;
        logger.info("ClinicManager inicializado con PatientRepository proporcionado.");
    }

    // Métodos de ClinicManager, ahora usando el patientRepository para guardar/buscar
    public Patient registerPatient(Patient patient) throws InvalidDataException, ClinicFullException {
        if (patient == null) {
            logger.error("Intento de registrar paciente nulo.");
            throw new InvalidDataException("No se puede registrar un paciente nulo.");
        }
        // Lógica de negocio (ej. validaciones, asignación de ID si no viene del paciente)
        // Aquí, en lugar de añadir a un array, llamaríamos al repositorio
        Patient savedPatient = patientRepository.save(patient); // El repositorio se encarga de guardar
        logger.info("Paciente registrado: {}", savedPatient.getName());

        return savedPatient;
    }

    public Patient findPatientById(String patientId) throws PatientNotFoundException, InvalidDataException {
        if (patientId == null || patientId.trim().isEmpty()) {
            logger.error("Intento de buscar paciente con ID nulo o vacío.");
            throw new InvalidDataException("El ID del paciente no puede ser nulo o vacío.");
        }
        // Llama al repositorio para encontrar al paciente
        Patient foundPatient = patientRepository.findById(patientId);
        if (foundPatient == null) {
            logger.warn("Paciente con ID {} no encontrado en repositorio.", patientId);
            throw new PatientNotFoundException("Paciente con ID " + patientId + " no encontrado.");
        }
        logger.info("Paciente encontrado en repositorio: {}", foundPatient.getName());
        return foundPatient;
    }

    public Patient[] listAllPatients() {
        Patient[] patients = patientRepository.findAll();
        logger.debug("Listando todos los pacientes registrados. Total: {}", patients.length);
        return patients;
    }

    // ... Otros métodos de ClinicManager ...
}
```

Ahora, la prueba para ClinicManager usando Mockito. Guárdala en src/test/java/com/cursojava/veterinary/service/ClinicManagerTest.java:

```Java
package com.cursojava.veterinary.service;

import com.cursojava.veterinary.exception.InvalidDataException;
import com.cursojava.veterinary.exception.PatientNotFoundException;
import com.cursojava.veterinary.model.Patient;
import com.cursojava.veterinary.repository.PatientRepository; // Importa la interfaz que vamos a mockear
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith; // Necesario para MockitoExtension
import org.mockito.Mock; // Para crear mocks
import org.mockito.junit.jupiter.MockitoExtension; // Extensión de JUnit 5 para Mockito

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.*; // Importa todos los métodos estáticos de Mockito (when, verify, any, etc.)

@ExtendWith(MockitoExtension.class) // Habilita las anotaciones @Mock para crear mocks
class ClinicManagerTest { // Usa este nombre o el que ya tengas para tu ClinicManagerTest

    // Arrange - ¡Aquí declaramos nuestro mock!
    // Mockito creará una implementación falsa de PatientRepository para nuestras pruebas.
    @Mock
    private PatientRepository mockPatientRepository;

    // La instancia real de ClinicManager que queremos probar.
    // La crearemos en el @BeforeEach, pasándole el mock.
    private ClinicManager clinicManager;

    @BeforeEach
    void setUp() {
        // Creamos la instancia de ClinicManager pasándole nuestro mockPatientRepository.
        // Así, ClinicManager usará nuestro mock en lugar de un PatientRepository real.
        clinicManager = new ClinicManager(mockPatientRepository);
        // Aquí puedes añadir cualquier otra configuración común para cada prueba
    }

    @Test
    @DisplayName("Debe registrar un paciente y guardarlo usando el repositorio con éxito")
    void shouldRegisterPatientAndSaveThroughRepositorySuccessfully() throws InvalidDataException {
        // Arrange
        Patient newPatient = new Patient("Fido", "Perro", 3, "Carlos", "555-1234");

        // Stubbing: Define el comportamiento del mockPatientRepository.
        // Cuando se llame a mockPatientRepository.save() con CUALQUIER objeto Patient,
        // Mockito debe retornar el mismo objeto 'newPatient' que se le pasó.
        when(mockPatientRepository.save(any(Patient.class))).thenReturn(newPatient);

        // Act
        Patient savedPatient = clinicManager.registerPatient(newPatient);

        // Assert
        assertNotNull(savedPatient, "El paciente guardado no debería ser nulo");
        assertEquals(newPatient.getName(), savedPatient.getName(), "El nombre del paciente guardado debe coincidir.");

        // Verify: Verifica que el método 'save' del mockPatientRepository fue llamado
        // EXACTAMENTE una vez, con el objeto 'newPatient' como argumento.
        verify(mockPatientRepository, times(1)).save(newPatient);
    }

    @Test
    @DisplayName("Debe lanzar PatientNotFoundException al buscar un paciente que no existe en el repositorio")
    void shouldThrowPatientNotFoundExceptionWhenPatientDoesNotExistInRepository() {
        // Arrange
        String nonExistentId = "ID_NO_EXISTENTE";
        // Stubbing: Cuando se llame a mockPatientRepository.findById() con el ID inexistente,
        // el mock debe retornar 'null', simulando que no se encontró el paciente.
        when(mockPatientRepository.findById(nonExistentId)).thenReturn(null);

        // Act & Assert
        // Esperamos que findPatientById() lance PatientNotFoundException
        assertThrows(PatientNotFoundException.class, () -> {
            clinicManager.findPatientById(nonExistentId);
        }, "Se esperaba PatientNotFoundException para ID no existente.");

        // Verify: Verificamos que findById fue llamado exactamente una vez con el ID no existente.
        verify(mockPatientRepository, times(1)).findById(nonExistentId);
    }

    @Test
    @DisplayName("Debe retornar el paciente correcto al buscar por ID existente en el repositorio")
    void shouldReturnCorrectPatientWhenFindingExistingIdInRepository() throws PatientNotFoundException, InvalidDataException {
        // Arrange
        String existingId = "P123";
        Patient expectedPatient = new Patient("Coco", "Pez", 1, "Laura", "555-9876");
        // Stubbing: Configura el mock para que devuelva el paciente esperado cuando se le pida por su ID
        when(mockPatientRepository.findById(existingId)).thenReturn(expectedPatient);

        // Act
        Patient foundPatient = clinicManager.findPatientById(existingId);

        // Assert
        assertNotNull(foundPatient, "El paciente encontrado no debe ser nulo.");
        assertEquals(expectedPatient.getName(), foundPatient.getName(), "El nombre del paciente debe coincidir.");
        assertEquals(expectedPatient.getSpecies(), foundPatient.getSpecies(), "La especie del paciente debe coincidir.");

        // Verify
        verify(mockPatientRepository, times(1)).findById(existingId);
    }

    @Test
    @DisplayName("Debe lanzar InvalidDataException si el paciente a registrar es nulo")
    void shouldThrowInvalidDataExceptionIfPatientToRegisterIsNull() {
        // Act & Assert
        assertThrows(InvalidDataException.class, () -> {
            clinicManager.registerPatient(null);
        }, "Se esperaba InvalidDataException al registrar paciente nulo.");

        // Verify: Asegúrate de que el método 'save' del mock NO fue llamado,
        // porque la validación de 'ClinicManager' falló antes de interactuar con el repositorio.
        verify(mockPatientRepository, never()).save(any(Patient.class));
    }
}
```

## Puntos Clave para Recordar

- Las **pruebas unitarias** no son un lujo, son una **necesidad** para construir **software de alta calidad, robusto y mantenible**. Son la base de un desarrollo eficiente.
- Siempre organiza tus pruebas usando el patrón **AAA (Arrange, Act, Assert)** para hacerlas claras y fáciles de leer.
- **JUnit 5** es tu framework principal. Utiliza sus anotaciones (`@Test`, `@DisplayName`, `@BeforeEach`) para estructurar tus pruebas y sus poderosas **aserciones** (`assertEquals`, `assertThrows`, `assertTrue`, etc.) para verificar el comportamiento de tu código.
- **Mockito** es indispensable cuando tu unidad de código tiene **colaboradores** (otras clases o interfaces de las que depende). Te permite **simular (mockear)** esos colaboradores, dándote control total sobre su comportamiento simulado y permitiéndote verificar que tu código interactúa con ellos de la manera esperada. Esto es crucial para **aislar tus pruebas** y que un fallo apunte al problema real en la unidad que estás probando.
- Recuerda probar tanto los **"happy paths" (escenarios de éxito)** como los **"error paths" (escenarios donde se esperan excepciones o manejo de errores)**.

## Recursos Adicionales

- **Documentación Oficial de JUnit 5**: [https://junit.org/junit5/docs/current/user-guide/](https://junit.org/junit5/docs/current/user-guide/)
- **Documentación Oficial de Mockito**: [https://site.mockito.org/](https://site.mockito.org/)
- **Tutorial de Mockito en Español**: [https://www.arquitecturajava.com/mockito-tutorial/](https://www.arquitecturajava.com/mockito-tutorial/)
- **Libro: "Effective Unit Testing" por Lasse Koskela** (Manning): Un libro excelente que profundiza en los principios y prácticas de las pruebas unitarias efectivas.

¡Ahora estás listo para aplicar estos conceptos en el ejercicio autónomo y llevar la calidad de tu código al siguiente nivel!
