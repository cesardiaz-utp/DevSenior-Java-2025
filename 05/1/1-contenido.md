# Introducción a Spring Boot y Estructura de Proyectos

## Antes de Empezar: Preparación del Entorno

Para seguir esta clase y practicar, asegúrate de tener instalado lo siguiente:

* **Java Development Kit (JDK):** Versión 17 o superior (se recomienda una versión LTS como 17 o 21).
* **Entorno de Desarrollo Integrado (IDE):**
  * Cursor AI (si lo utilizas)
  * IntelliJ IDEA Community o Ultimate
  * VS Code con extensiones Java (Extension Pack for Java)
* **Navegador Web:** Cualquier navegador moderno (Chrome, Firefox, Edge, Safari).
* **Herramienta para probar APIs (Opcional pero Recomendado):**
  * Postman
  * Insomnia
  * O simplemente usar la línea de comandos con `curl`.

### 1. Bienvenida e Introducción

¡Hola a todos/as!

En esta primera clase, nos sumergiremos en el mundo de **Spring Boot**, la forma más popular y rápida de crear aplicaciones Java robustas y listas para producción.

**Objetivos de Aprendizaje de Hoy:**

Al finalizar esta sesión, serás capaz de:

* Comprender qué es Spring Boot, sus ventajas y por qué es tan utilizado.
* Generar y configurar tu primer proyecto Spring Boot.
* Entender y aplicar una estructura de carpetas profesional.
* Describir los principios básicos de las APIs REST.
* Construir y probar un endpoint REST básico que retorne datos en JSON.
* Utilizar herramientas de IA (como Cursor AI) para asistirte en tareas de desarrollo.

**Agenda del Día:**

1. ¿Qué es Spring Boot?
2. Creación del proyecto con Spring Initializr.
3. Estructura recomendada de carpetas.
4. Configuración básica (`application.properties`).
5. Introducción a REST y creación de nuestra primera API GET.
6. Asistencia con herramientas de IA (Cursor AI).
7. Conclusiones y próximos pasos.

### 2. ¿Qué es Spring Boot y por qué usarlo?

Anteriormente, desarrollar con Java EE o incluso con el Spring Framework clásico podía ser complejo debido a la gran cantidad de configuración necesaria (XMLs, despliegue en servidores externos, gestión de dependencias manual...).

**Spring Boot** nació para simplificar drásticamente el desarrollo de aplicaciones Spring. Piensa en él como una capa por encima del Spring Framework tradicional que te permite "arrancar" (boot) rápidamente.

**Conceptos Clave de Spring Boot:**

* **"Convention over Configuration" (Convención sobre Configuración):** En lugar de forzarte a configurar todo, Spring Boot asume sensiblemente lo que necesitas basándose en las dependencias que añades. Si necesitas algo diferente, puedes sobreescribir las convenciones fácilmente.
* **Auto-configuración (Auto-configuration):** Basado en las Jars (librerías) que tienes en tu proyecto, Spring Boot configura automáticamente muchos aspectos de tu aplicación (por ejemplo, configura una base de datos si ve que tienes el driver, configura un servidor web si ve que tienes `spring-boot-starter-web`, etc.).
* **Servidores Embebidos (Embedded Servers):** No necesitas instalar y configurar un servidor de aplicaciones externo (como Tomcat, Jetty o WildFly). Spring Boot incluye servidores embebidos (Tomcat es el default con `spring-boot-starter-web`), permitiéndote empaquetar tu aplicación completa como un único archivo JAR ejecutable. Simplemente ejecutas `java -jar tu-app.jar`.
* **Starters:** Son conjuntos de dependencias pre-configuradas. Por ejemplo, `spring-boot-starter-web` incluye todo lo necesario para construir aplicaciones web y APIs REST (Spring MVC, Tomcat embebido, Jackson para JSON, etc.). Simplifican enormemente la gestión del `pom.xml` (Maven) o `build.gradle` (Gradle).
* **Actuator (Mención):** Proporciona puntos finales (endpoints) listos para usar que te permiten monitorear y gestionar tu aplicación en producción (salud, métricas, información de entorno, etc.).

**Ventajas Clave de Usar Spring Boot:**

* **Mayor Productividad:** Desarrollas más rápido gracias a la auto-configuración y los starters.
* **Ideal para Microservicios:** Su diseño JAR ejecutable y su ligereza lo hacen perfecto para construir microservicios.
* **Aprovecha el Ecosistema Spring:** Tienes acceso a todos los proyectos de Spring (Data, Security, Cloud, etc.).
* **Gran Comunidad y Documentación:** Es una tecnología madura con mucho soporte en línea.

**Comparación Breve (Alto Nivel):**

* **vs. Spring Framework Clásico:** Boot simplifica enormemente la configuración que era manual o en XML en el Spring clásico.
* **vs. Java EE/Jakarta EE:** Boot ofrece una alternativa más ligera y rápida de configurar y desplegar, sin necesidad de servidores de aplicaciones robustos.
* **vs. Otros frameworks (Node.js, Python/Django/Flask):** Spring Boot ofrece la solidez y el rendimiento de Java con una experiencia de desarrollo ágil que compite favorablemente con frameworks dinámicos, manteniendo la ventaja del tipado fuerte de Java.
* **vs. Alternativas Java Ligeras (Quarkus, Micronaut):** Estas son alternativas más recientes, a menudo optimizadas para tiempos de arranque y uso de memoria reducidos, populares en entornos serverless y microservicios. Spring Boot sigue siendo el líder del mercado por su madurez, ecosistema y facilidad de uso general.

### 3. Creación del Proyecto con Spring Initializr y Apertura en IDE

La forma más sencilla y recomendada de iniciar un proyecto Spring Boot es usando **Spring Initializr**, una herramienta web que genera el esqueleto básico por ti.

#### Pasos para Crear el Proyecto

1. Abre tu navegador y ve a: `https://start.spring.io/`
2. **Configura tu proyecto:**
    * **Project:** Selecciona **Maven** o Gradle (usaremos Maven en la demo).
    * **Language:** Selecciona **Java**.
    * **Spring Boot Version:** Elige la **última versión estable** recomendada (normalmente la que no tiene sufijo como SNAPSHOT o M).
    * **Project Metadata:**
        * **Group:** Un identificador único para tu organización (ej: `com.midominio.curso`).
        * **Artifact:** El nombre de tu proyecto (ej: `introduccion-springboot`).
        * **Name:** Nombre legible del proyecto (suele ser igual al Artifact).
        * **Description:** Una breve descripción.
        * **Package name:** Se genera automáticamente (Group + Artifact).
        * **Packaging:** Selecciona **Jar** (para ejecutar con servidor embebido).
        * **Java:** Elige la **versión LTS** que tengas instalada (17 o 21).
    * **Dependencies:** Haz clic en "Add Dependencies..." y busca y añade **`Spring Web`**. Esta es la dependencia clave para construir aplicaciones web y APIs REST.

3. Haz clic en el botón **Generate** (en la parte inferior). Se descargará un archivo ZIP.

4. **Importar el Proyecto en tu IDE (Cursor AI / IntelliJ / VS Code):**
    * Descomprime el archivo ZIP descargado en una carpeta de tu elección.
    * Abre tu IDE.
    * Selecciona la opción para "Abrir" o "Importar" un proyecto existente.
    * Navega hasta la carpeta donde descomprimiste el proyecto y selecciona la carpeta raíz del proyecto (la que contiene `pom.xml`). El IDE debería reconocerlo automáticamente como un proyecto Maven.
    * Espera a que el IDE descargue las dependencias (esto puede tardar un poco la primera vez).

### 4. Explorando el Proyecto Generado

#### Archivos Clave

* **`pom.xml` (si usaste Maven):** Este archivo define las dependencias y cómo se construye el proyecto. Busca la dependencia `spring-boot-starter-web` que añadimos.
* **`src/main/java/com/tu_paquete_base/TuAplicacionApplication.java`:** Esta es la clase principal. Contiene el método `main()` y la anotación `@SpringBootApplication`.
  * `@SpringBootApplication`: Es una anotación de conveniencia que equivale a `@Configuration`, `@EnableAutoConfiguration` y `@ComponentScan`. Básicamente, le dice a Spring Boot: "Esta es la clase principal, configura automáticamente basándote en las dependencias y escanea componentes (clases con anotaciones Spring como `@Controller`, `@Service`, etc.) dentro de este paquete y subpaquetes".

### 5. Primera Ejecución

1. En tu IDE, busca la clase `TuAplicacionApplication.java`.
2. Haz clic derecho sobre ella y selecciona la opción para "Ejecutar" o "Run".
3. Observa la consola: Deberías ver los logs de Spring Boot, indicando que está arrancando un servidor web (Tomcat) en el puerto 8080.
4. Abre tu navegador y ve a `http://localhost:8080`.

   **Nota:** Verás una página de "Whitelabel Error Page". Esto es completamente esperado y significa que el servidor web está funcionando correctamente, pero aún no tienes ningún endpoint configurado para la raíz (`/`).

5. Detén la aplicación desde el IDE.
