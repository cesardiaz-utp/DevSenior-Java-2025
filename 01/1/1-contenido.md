# Clase 1: Configuración Profesional del Entorno y Primer Proyecto

**¡Bienvenido a tu primer paso en el mundo de la programación Java!** En esta clase, no solo te familiarizarás con los conceptos fundamentales de Java, sino que también configurarás tu propio espacio de trabajo profesional.

## Objetivos de Aprendizaje de la Clase

Al finalizar esta clase, serás capaz de:

- Instalar y configurar las herramientas esenciales para desarrollar en Java (JDK, VSCode, Git).
- Comprender la estructura básica de un programa Java y su flujo de ejecución.
- Crear, compilar y ejecutar tu primer programa "Hola Mundo" en Java.
- Manejar los comandos básicos de Git para control de versiones.

## 1. Preparando el Terreno: Tu Entorno de Desarrollo Java

Un entorno de desarrollo bien configurado es la base para cualquier programador. Aquí aprenderás a instalar las herramientas clave:

### 1.1. Instalación del JDK (Java Development Kit)

El **JDK** es el kit de desarrollo de Java. Incluye el **compilador de Java** (`javac`), el **entorno de ejecución de Java (JRE)** y otras herramientas necesarias para escribir y ejecutar aplicaciones Java.

- **Descarga**:
  - Visita la página oficial de Oracle (requiere cuenta) o OpenJDK (recomendado por su flexibilidad y licencias más abiertas, como Adoptium/Eclipse Temurin):
    - [Adoptium/Eclipse Temurin](https://adoptium.net/) (Recomendado, elige la versión **LTS** más reciente, ej. Java 21).
    - Descarga el instalador adecuado para tu sistema operativo (Windows x64, macOS x64/ARM, Linux).
- **Instalación**:
  - **Windows**: Ejecuta el instalador y sigue las instrucciones. Es recomendable instalar en la ruta por defecto.
  - **macOS**: Para el `.dmg`, arrastra el icono del JDK a la carpeta de Aplicaciones. Para Homebrew (`brew install openjdk@XX`), sigue las instrucciones de la terminal.
  - **Linux**: Descomprime el archivo `.tar.gz` en una ubicación como `/opt/java` o `/usr/lib/jvm`.
- **Configuración de Variables de Entorno**:
  - `JAVA_HOME`: Es una variable de entorno que apunta a la ruta donde está instalado el JDK.
    - **Windows**:
      1. Busca "Editar las variables de entorno del sistema".
      2. En la pestaña "Opciones avanzadas", haz clic en "Variables de entorno...".
      3. En "Variables del sistema", haz clic en "Nueva...".
      4. Nombre de la variable: `JAVA_HOME`.
      5. Valor de la variable: La ruta de tu instalación del JDK (ej. `C:\Program Files\Java\jdk-XX`).
      6. Aceptar.
    - **macOS/Linux**: Edita tu archivo de configuración de shell (`.bashrc`, `.zshrc`, `.profile`).

      ```Bash
      export JAVA_HOME="/path/to/your/jdk-XX" # Reemplaza XX con tu versión
      export PATH="$JAVA_HOME/bin:$PATH"
      ```

      Luego, ejecuta `source ~/.bashrc` (o tu archivo correspondiente) para aplicar los cambios.
  - `PATH`: Asegúrate de que la carpeta `bin` del JDK esté en tu variable `PATH` para poder ejecutar los comandos de Java desde cualquier directorio. (Normalmente, la configuración de `JAVA_HOME` y la adición de `$JAVA_HOME/bin` al `PATH` lo hacen automáticamente en la mayoría de los instaladores o scripts).
- **Verificación**: Abre una nueva terminal o símbolo del sistema y escribe:

  ```Bash
  java -version
  javac -version
  ```

  Deberías ver la versión del JDK instalada.

### 1.2. Visual Studio Code - Tu Editor de Código Ideal

**VSCode** es un editor de código ligero pero potente, con un excelente soporte para Java a través de extensiones.

- **Descarga e Instalación**:
  - Visita [code.visualstudio.com](code.visualstudio.com).
  - Descarga el instalador para tu sistema operativo y sigue las instrucciones.
- **Extensiones Esenciales para Java**:
  - Abre VSCode, ve al ícono de extensiones (cuadrados en el menú lateral izquierdo, o `Ctrl+Shift+X`).
  - Busca e instala el **"Java Extension Pack"** de Microsoft. Este paquete incluye:
    - Language Support for Java™ by Red Hat
    - Debugger for Java
    - Maven for Java
    - Gradle for Java
    - Test Runner for Java
    - Project Manager for Java
  - Otras extensiones recomendadas:
    - GitLens (para una mejor integración con Git).
    - Material Icon Theme (para iconos de archivos).
    - Prettier (para formatear código).
- **Configuración Básica en VSCode**: VSCode debería detectar tu JDK automáticamente. Si no, puedes ir a `File > Preferences > Settings` (o `Ctrl + ,`) y buscar `"java.configuration.runtimes"` para configurar manualmente la ruta de tu JDK.

### 1.3. Git y GitHub: Control de Versiones y Colaboración

**Git** es un sistema de control de versiones distribuido que te permite rastrear cambios en tu código. **GitHub** es una plataforma web para alojar repositorios Git y facilitar la colaboración.

- **Instalación de Git**:
  - Visita [git-scm.com/downloads](git-scm.com/downloads).
  - Descarga e instala la versión para tu sistema operativo. En Windows, puedes dejar las opciones por defecto.
- **Configuración Inicial de Git**: Abre tu terminal (o la terminal integrada de VSCode) y configura tu nombre de usuario y email (estos se usarán en tus `commits`):

  ```Bash
  git config --global user.name "Tu Nombre"
  git config --global user.email "tuemail@ejemplo.com"
  ```

- **Conexión a GitHub**:
  - **Crear una cuenta en GitHub**: Si no tienes una, regístrate en [github.com](github.com).
  - **Autenticación (SSH o HTTPS)**:
    - **SSH (Recomendado)**: Genera un par de claves SSH en tu máquina (`ssh-keygen`) y añade tu clave pública a tu cuenta de GitHub. Esto evita pedir contraseña constantemente.
    - **HTTPS**: Git Credentials Manager para Windows o el asistente de autenticación de GitHub en VSCode te ayudará a gestionar las credenciales.
- **Comandos Básicos de Git (Fundamentales)**:
  - `git init`: Inicializa un nuevo repositorio Git en el directorio actual.

    ```Bash
    cd mi-proyecto-java
    git init
    ```

  - `git add <nombre_archivo>` o `git add .`: Añade archivos al "área de staging" (prepara los archivos para el próximo commit).

    ```Bash
    git add HelloWorld.java
    git add . # Añade todos los cambios en el directorio actual
    ```

  - `git commit -m "Mensaje"`: Guarda los cambios "staged" en el historial del repositorio local con un mensaje descriptivo.

    ```Bash
    git commit -m "feat: Primer programa Hola Mundo"
    ```

  - `git remote add origin <URL_repositorio>`: Vincula tu repositorio local con un repositorio remoto en GitHub.

    ```Bash
    git remote add origin <https://github.com/tu_usuario/nombre_repositorio.git>
    ```

  - `git push origin <nombre_rama>`: Sube los commits del repositorio local al repositorio remoto (ej. `git push origin main`).

    ```Bash
    git push origin main
    ```

  - `git pull origin <nombre_rama>`: Descarga los cambios del repositorio remoto a tu repositorio local.

    ```Bash
    git pull origin main
    ```

## 2. Introducción a la Programación y Java

Antes de sumergirnos en el código, entendamos qué significa programar y cómo Java se ajusta a este concepto.

### 2.1. ¿Qué es la Programación?

Programar es dar instrucciones a una computadora para que realice tareas específicas. Estas instrucciones deben ser claras, precisas y en un lenguaje que la computadora entienda.

- **Algoritmo**: Es una secuencia finita y ordenada de pasos para resolver un problema o realizar una tarea. Piensa en una receta de cocina: son pasos para lograr un plato.

![Huevo Frito](https://imagenes.20minutos.es/files/image_1920_1080/uploads/imagenes/2021/11/02/huevo-frito.jpeg)

- **Pensamiento Algorítmico**: Es la habilidad de descomponer un problema grande en pasos más pequeños y manejables, y luego organizar esos pasos lógicamente para llegar a una solución. ¡Es la base de la programación!

[![como hacer un sandwish de mantequilla de mani y mermelada](https://markdown-videos-api.jorgenkh.no/youtube/cDA3_5982h8)](https://youtu.be/cDA3_5982h8?si=Xmwa_V04HYXLnt4g)

### 2.2. ¿Que es Java?

Java es un lenguaje de programación y una plataforma informática creada por Sun Microsystems en 1995. Es un lenguaje orientado a objetos, lo que significa que se basa en conceptos de "objetos" que pueden contener datos y código para manipular esos datos.

Una de las características más destacadas de Java es su capacidad de ser ejecutado en diferentes plataformas sin necesidad de modificar el código fuente, gracias a su lema **Write Once, Run Anywhere** (Escribe una vez, ejecuta en cualquier lugar). Esto se logra mediante la compilación del código Java en un formato intermedio llamado bytecode, que luego es interpretado por la Java Virtual Machine (JVM) en cualquier dispositivo.

Java se utiliza ampliamente en el desarrollo de aplicaciones web, aplicaciones móviles (especialmente en Android), sistemas empresariales y mucho más. Además, es conocido por su robustez, seguridad y portabilidad.

#### Caracteristicas

1. **Portabilidad**: Gracias a la Java Virtual Machine (JVM), el código Java puede ejecutarse en cualquier plataforma que tenga una JVM instalada, sin necesidad de recompilar.
2. **Orientado a objetos**: Java utiliza el paradigma de programación orientada a objetos, lo que facilita la creación de software modular y reutilizable.
3. **Multihilo**: Java soporta la programación multihilo, lo que permite la ejecución de múltiples hilos de ejecución simultáneamente, mejorando el rendimiento de las aplicaciones.
4. **Seguridad**: Java tiene características de seguridad integradas, como la gestión de memoria automática y la verificación de bytecode, que ayudan a prevenir errores y vulnerabilidades.
5. **Robustez**: Java es conocido por su robustez, ya que maneja bien los errores en tiempo de ejecución y tiene una gestión de memoria eficiente.
6. **Distribuido**: Java facilita la creación de aplicaciones distribuidas, gracias a su soporte para tecnologías como RMI (Remote Method Invocation) y CORBA (Common Object Request Broker Architecture).
7. **Interpretado y compilado**: El código Java se compila en bytecode, que luego es interpretado por la JVM. Esto permite que el código sea independiente de la plataforma.
8. **Alto rendimiento**: Aunque Java es interpretado, las JVM modernas utilizan técnicas de compilación Just-In-Time (JIT) para mejorar el rendimiento.

#### Compilación y ejecución de aplicaciones Java

![Compilación y ejecución de aplicaciones Java](https://cdn.javarush.com/images/article/fef10693-b1f3-479a-a02e-29414cdc2a79/1024.jpeg)

La compilación y ejecución de aplicaciones Java sigue un proceso bien definido que se puede resumir en los siguientes pasos:

1. **Escribir el código fuente**: Se crea un archivo con extensión .java que contiene el código fuente del programa.
2. **Compilar el código fuente**: Utilizando el compilador javac del JDK (Java Development Kit), el código fuente se convierte en bytecode, que es un formato intermedio independiente de la plataforma.
3. **Ejecutar el bytecode**: La JVM (Java Virtual Machine) interpreta y ejecuta el bytecode.

#### Modelo de Liberación de Versiones de Java

- Liberaciones Semestrales:
  - Desde 2017, Java adopta un ciclo de liberación de versiones cada seis meses. Esto significa que hay dos versiones nuevas de Java cada año, una en marzo y otra en septiembre.
  - Este modelo permite a los desarrolladores acceder a nuevas características y mejoras de manera más rápida y frecuente.
- Versiones LTS (Long-Term Support):
  - Cada tres años, una de las versiones liberadas se designa como LTS. Las versiones LTS reciben soporte a largo plazo, lo que incluye actualizaciones de seguridad y correcciones de errores durante un período extendido.
  - Las versiones LTS son ideales para entornos de producción donde la estabilidad y el soporte a largo plazo son cruciales. Ejemplos de versiones LTS son Java 8, Java 11, Java 17 y la más reciente, Java 21.
- Versiones No-LTS:
  - Las versiones que no son LTS reciben soporte y actualizaciones solo hasta la siguiente liberación, es decir, durante seis meses. Estas versiones permiten a los desarrolladores experimentar con nuevas características y mejoras antes de que se incluyan en una versión LTS.
- Características en Previsión (Preview Features):
  - Algunas nuevas características se introducen inicialmente como "preview features". Esto permite a los desarrolladores probar y dar feedback sobre estas características antes de que se incluyan oficialmente en una versión futura.
  - Las características en previsión no están habilitadas por defecto y deben activarse explícitamente durante la compilación y ejecución.

**Ver**: [Versiones de Java](https://en.wikipedia.org/wiki/Java_version_history)

### 2.3. Desentrañando Java: JVM, JRE y JDK

El ecosistema de Java puede parecer confuso al principio con estas siglas, pero son fundamentales:

- **JVM (Java Virtual Machine - Máquina Virtual de Java)**:
  - Es el "corazón" de Java. La JVM es un programa que permite a los programas Java ejecutarse en cualquier dispositivo.
  - Convierte el "bytecode" (el código compilado de Java) en instrucciones entendibles por el hardware de tu computadora.
  - La magia de "Write Once, Run Anywhere" (Escribe una vez, ejecuta en cualquier lugar) se debe a la JVM.
- **JRE (Java Runtime Environment - Entorno de Ejecución de Java)**:
  - Es un paquete que contiene la JVM y las bibliotecas (clases predefinidas) necesarias para ejecutar aplicaciones Java.
  - Si solo quieres ejecutar programas Java y no desarrollarlos, solo necesitas el JRE.
- **JDK (Java Development Kit - Kit de Desarrollo de Java)**:
  - Es un superconjunto del JRE. Incluye el JRE y todas las herramientas de desarrollo necesarias para crear, compilar y depurar aplicaciones Java.
  - Contiene el compilador de Java (javac), el depurador (jdb), y otras utilidades.
- **En resumen**:
  ![JDK, JRE y JVM](https://codigojava.online/wp-content/uploads/2022/11/JDK-768x384.png)

  - **JDK** = JRE + Herramientas de Desarrollo (para el desarrollador).
  - **JRE** = JVM + Bibliotecas estándar (para ejecutar aplicaciones).
  - **JVM** = Intérprete de bytecode (el motor que corre el código).

## 3. Tu Primer Programa en Java: "Hola Mundo"

Ahora, ¡manos a la obra con tu primer programa! La tradición dicta empezar con un "Hola Mundo".

### 3.1. Creando el Archivo `HelloWorld.java`

1. **Crea una carpeta de trabajo**: En tu computadora, crea una carpeta para tus proyectos Java, por ejemplo: `C:\java_projects\module-01\class-01`.
2. **Abre VSCode**: Ve a `File > Open Folder...` y selecciona la carpeta `class-01`.
3. **Nuevo Archivo**: En el explorador de archivos de VSCode (panel izquierdo), haz clic en el icono de "Nuevo Archivo" y nómbralo `HelloWorld.java`. **¡Importante! El nombre del archivo debe coincidir exactamente con el nombre de la clase pública dentro del archivo (sensible a mayúsculas y minúsculas).**
4. **Escribe el Código**: Copia y pega el siguiente código en `HelloWorld.java`:

    ```Java
    // Nombre del archivo: HelloWorld.java
    // Este es tu primer programa en Java
    public class HelloWorld { // Declaración de la clase principal
        // Este es el método principal (punto de entrada del programa)
        public static void main(String[] args) {
            // La siguiente línea imprime "¡Hola, Mundo desde Java!" en la consola
            System.out.println("¡Hola, Mundo desde Java!");
        }
    }
    ```

- `public class HelloWorld`: Declara una clase pública llamada `HelloWorld`. En Java, todo el código reside dentro de clases.
- `public static void main(String[] args)`: Este es el método `main`, el punto de entrada de tu programa Java. Cuando ejecutas un programa Java, la JVM busca y ejecuta este método primero.
  - `public`: Significa que el método es accesible desde cualquier parte.
  - `static`: Significa que puedes llamar a este método sin necesidad de crear un objeto de la clase `HelloWorld`.
  - `void`: Indica que el método no devuelve ningún valor.
  - `main`: Es el nombre estándar para el método principal.
  - `String[] args`: Es un arreglo de cadenas de texto que puede recibir argumentos de línea de comandos (lo veremos más adelante).
- `System.out.println("¡Hola, Mundo desde Java!");`: Esta línea es la instrucción que hace que el programa imprima texto en la consola.
  - `System`: Es una clase estándar de Java.
  - `out`: Es un objeto dentro de `System` que representa la salida estándar (normalmente la consola).
  - `println()`: Es un método del objeto `out` que imprime el texto entre paréntesis y luego un salto de línea.
- `//` y `/*... */`: Son comentarios. El texto después de `//` hasta el final de la línea es un comentario. El texto entre `/* y */` es un comentario de bloque. Los comentarios son ignorados por el compilador y se usan para documentar tu código.

### 3.2. Entendiendo la Sintaxis Básica de Java

Veamos algunas reglas fundamentales del lenguaje Java:

- **Sentencias (Statements)**: Son las instrucciones que le das a la computadora para que realice una acción. La mayoría de las sentencias en Java terminan con un **punto y coma** (`;`).

  ```Java
  int edad = 30; // Esta es una sentencia
  System.out.println("Hola"); // Esta es otra sentencia
  ```

- **Bloques de Código (`{ }`)**: Son grupos de una o más sentencias encerrados entre llaves `{ }`. Los bloques se usan para agrupar código que pertenece a una clase, un método, una estructura condicional (`if`, `else`), un bucle (`for`, `while`), etc. Definen el alcance de las variables locales.

  ```Java
  public class MiClase { // Este es el bloque de la clase

      public static void main(String[] args) { // Este es el bloque del método main
          // Sentencias dentro del bloque main
          if (true) { // Este es el bloque de un if
              // Sentencias dentro del bloque if
          } // Fin del bloque if
      } // Fin del bloque main
  } // Fin del bloque de la clase
  ```

- Java es **sensible a mayúsculas y minúsculas**. Esto significa que `miVariable` es diferente de `mivariable`, `System` es diferente de `system`, y `main` es diferente de `Main`. Debes escribir las palabras clave, nombres de variables y nombres de clases exactamente como están definidas.

- **Palabras Clave (Keywords)**: Java tiene un conjunto de [palabras reservadas](https://www.abrirllave.com/java/palabras-clave.php) que tienen un significado especial para el compilador (ej. `public`, `static`, `void`, `class`, `int`, `if`, `for`, etc.). No puedes usar estas palabras clave como nombres de variables, métodos o clases.

- **Identificadores**: Son los nombres que le das a tus variables, métodos, clases, etc. Deben seguir ciertas reglas (empezar con letra, `_` o `$`, no contener espacios, no ser palabras clave). Se recomienda usar nombres descriptivos.

### 3.3. Documentando tu Código: Los Comentarios

Los comentarios son notas que los programadores añaden a su código para hacerlo más claro y comprensible, tanto para ellos mismos en el futuro como para otros desarrolladores. El compilador de Java **ignora por completo** los comentarios; no afectan cómo se ejecuta el programa.

Hay tres tipos principales de comentarios en Java:

- **Comentarios de una sola línea**: Comienzan con dos barras inclinadas (`//`) y continúan hasta el final de la línea. Son útiles para explicaciones cortas o para comentar una sola línea de código.

  ```Java
  int edad = 30; // Declara una variable para almacenar la edad
  // System.out.println("Esta línea está comentada y no se ejecutará");
  ```

- **Comentarios de bloque (o multilínea)**: Comienzan con `/*` y terminan con `*/`. Pueden abarcar varias líneas y son útiles para explicaciones más largas o para comentar secciones completas de código temporalmente.

  ```Java
  /*
  Este es un comentario de bloque.
  Puede usarse para describir la funcionalidad
  de una sección de código o un método.
  */
  double precioTotal = precioUnitario * cantidad;
  ```

- **Comentarios de documentación (Javadoc)**: Comienzan con `/**` y terminan con `*/`. Se usan para generar documentación automática del código.

  ```Java
  /**
   * Este método suma dos números enteros.
   * @param a El primer número a sumar.
   * @param b El segundo número a sumar.
   * @return La suma de a y b.
   */
  public static int sumar(int a, int b) {
      return a + b;
  }
  ```

**¿Por qué usar comentarios?**

- **Claridad**: Explican la lógica compleja o las decisiones de diseño.
- **Documentación**: Ayudan a otros (y a tu futuro yo) a entender rápidamente qué hace el código.
- **Depuración**: Puedes "comentar" temporalmente líneas de código para probar si son la causa de un error.

Acostúmbrate a usar comentarios de forma regular para mantener tu código bien documentado y fácil de entender.

### 3.4. Proceso de Compilación y Ejecución

Java es un lenguaje compilado e interpretado. Primero, el código fuente (`.java`) se compila a bytecode (`.class`), y luego la JVM interpreta y ejecuta ese bytecode.

- **Desde la Consola (Terminal Integrada de VSCode)**:
  1. Abre la terminal integrada de VSCode (`Ctrl+ñ` o `View > Terminal`).
  2. Asegúrate de estar en el directorio donde guardaste `HelloWorld.java` (ej. cd `C:\java_projects\module-01\class-01`).
  3. **Compilación**: Ejecuta el compilador de Java:

      ```Bash
      javac HelloWorld.java
      ```

      Si no hay errores, este comando creará un archivo llamado `HelloWorld.class` en el mismo directorio. Este archivo `.class` contiene el bytecode.
  4. **Ejecución**: Ejecuta el bytecode utilizando la JVM:

      ```Bash
      java HelloWorld
      ```

      Deberías ver la salida: `¡Hola, Mundo desde Java!`
- **Desde VSCode (Método más simple con las extensiones)**:
  1. Abre `HelloWorld.java` en el editor.
  2. Las extensiones de Java en VSCode añaden un botón "Run" (Ejecutar) o "Debug" (Depurar) en la parte superior del editor o al lado del método `main`.
  3. Haz clic en el botón "Run" o en el icono de "Play" verde en la barra superior.
  4. VSCode compilará y ejecutará el programa automáticamente, mostrando la salida en la ventana "TERMINAL" de VSCode.

## Resumen de la Clase

- Has configurado un entorno de desarrollo completo con **JDK**, **VSCode** y **Git/GitHub**.
- Comprendes la función de la **JVM**, **JRE** y **JDK** en el ecosistema de Java.
- Has escrito tu primer programa en Java, el famoso **"Hola Mundo"**, y sabes cómo compilarlo y ejecutarlo tanto desde la consola como desde VSCode.
- Te has familiarizado con los comandos básicos de Git para el control de versiones.

## Videos adicionales (Recomendados)

### Por esto te cuesta programar

[![Por esto te cuesta programar](https://markdown-videos-api.jorgenkh.no/youtube/c3NRsitewTc)](https://youtu.be/c3NRsitewTc?si=lXzsR94-WEHCDQcK)

## El problema de "Aprender" programación

[![El problema de "Aprender" programación](https://markdown-videos-api.jorgenkh.no/youtube/d1XlxVm2sA0)](https://youtu.be/d1XlxVm2sA0?si=f1DqMOPcOuSv7vu2)

## 5 Errores al Aprender a Programar

[![5 Errores al Aprender a Programar](https://markdown-videos-api.jorgenkh.no/youtube/kgxWf1GFyVI)](https://youtu.be/kgxWf1GFyVI?si=M0oq1u3OJAMx-gbJ)

## Lógica de programación

[![Lógica de programación](https://i.ytimg.com/vi/OyPJpud974E/maxresdefault.jpg)](https://www.youtube.com/watch?v=OyPJpud974E&list=PLeJNEiFH8nIBf9UxeJ1WvjdztOKKrdtOI)

¡Felicidades! Has dado un gran paso en tu viaje de programación en Java. Ahora, ¡a practicar!
