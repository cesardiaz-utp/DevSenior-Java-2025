# Tutoría: Consolidación del Entorno y Flujo de Trabajo con Git

## 1. Bienvenida y Sondeo Inicial

* **Objetivo**: Evaluar el estado general de la clase y establecer la agenda.
* Saludo y recordatorio del propósito de la tutoría (espacio para resolver dudas, practicar más a fondo).
* Preguntas abiertas:
  * "¿Quién tuvo algún problema significativo con la instalación del JDK, VSCode o Git?"
  * "¿Pudieron ejecutar el programa 'Hola Mundo' sin problemas?"
  * "¿Hay algún concepto de la clase (JVM, JRE, JDK, comandos Git) que necesite ser repasado?"
* Explicar que se priorizará la resolución de problemas de entorno y la práctica de Git.

## 2. Asistencia Personalizada y Resolución de Errores Comunes de Instalación

* **Objetivo**: Garantizar que cada estudiante tenga un entorno de desarrollo base funcional.
* **Enfoque**: El tutor se moverá por los estudiantes (físicamente o en salas de reunión/pantallas compartidas si es remoto) para revisar sus configuraciones.
* **Puntos Clave a Revisar y Errores Comunes**:
  * **Verificación General**: Pedir a los estudiantes que abran una nueva terminal (no la integrada de VSCode inicialmente) y ejecuten:
    * `java -version`
    * `javac -version`
    * `git --version`
    * `code --version` (para verificar VSCode en el PATH, opcional)
  * **Problemas con JDK/Variables de Entorno**:
    * `'java' no se reconoce como un comando interno o externo...`: Sugiere que `JAVA_HOME` o `PATH` no están configurados correctamente. Guiar a los estudiantes a revisar y configurar.
    * **Versión incorrecta o JDK no detectado por VSCode**: Revisar `java.configuration.runtimes` en la configuración de VSCode.
* **Problemas con Git/GitHub**:
  * `'git' no se reconoce...`: Git no está instalado o no está en el `PATH`.
  * Problemas al clonar/hacer `push`/`pull` a GitHub: Revisar si la clave SSH fue añadida a GitHub, si el token de acceso personal es válido (para HTTPS), o si el repositorio remoto está configurado correctamente (`git remote -v`).
* **VSCode**: Asegurarse de que el "Java Extension Pack" esté instalado y habilitado.
* **Estrategia**: Si un error es común, el tutor puede hacer una pequeña demostración para todo el grupo. Para errores individuales, se brinda soporte personalizado.

## 3. Ejercicios Prácticos Guiados de Git y GitHub

Fortalecer la comprensión del flujo de trabajo con Git y familiarizarse con comandos esenciales.

### Actividad 1: Clonación de un Repositorio Remoto

* El tutor compartirá la URL de un repositorio de ejemplo en GitHub (previamente creado, con un `README.md` simple y quizás un `HolaMundo.java`).
* **Instrucción**: "Clonen este repositorio en una nueva carpeta en su máquina local."

  ```bash
  git clone [URL_del_repositorio]
  cd [nombre_del_repositorio]
  ```

  **Discusión**: Explicar la diferencia entre `git clone` (crea un nuevo repositorio local a partir de uno remoto) y `git pull` (actualiza un repositorio local existente con cambios del remoto).

### Actividad 2: Creación de Ramas y Primer Commit en una Rama

* **Explicación**: Introducir el concepto de "ramas" (`branches`) como vías de desarrollo paralelas. Son útiles para trabajar en nuevas características o errores sin afectar el código principal (`main`/`master`).
* **Instrucción**: "Creen una nueva rama para trabajar en una nueva funcionalidad y cambien a ella."

  ```bash
  git branch feature/mi-primera-funcionalidad
  git checkout feature/mi-primera-funcionalidad # O en un solo paso: git checkout -b feature/mi-primera-funcionalidad
  ```

* **Instrucción**: "Realicen un pequeño cambio en el archivo `README.md` o en el `HolaMundo.java` (ej. añadir una línea de comentario) y realicen un commit en esta nueva rama."

  ```bash
  git add .
  git commit -m "feat: Añadida nueva línea en README"
  ```

### Actividad 3: Subir Rama Remota y Conmutación

* **Instrucción**: "Suban esta nueva rama a GitHub."

  ```bash
  git push origin feature/mi-primera-funcionalidad
  ```

  **Verificación**: Pedir a los estudiantes que verifiquen en GitHub.com que la nueva rama ha sido subida.

* **Instrucción**: "Vuelvan a la rama principal (main/master)."

  ```bash
  git checkout main # o master
  ```

  **Discusión**: Discutir brevemente el proceso de pull request como la forma de integrar los cambios de una rama en otra (se profundizará en módulos futuros).

### 4. Creación del Primer Repositorio Individual y Buenas Prácticas (Preparación para el Ejercicio Autónomo)

Guiar a los estudiantes en la creación de su propio repositorio para el ejercicio semanal, fomentando la organización.

* **Discusión**
  * Importancia de una estructura de carpetas organizada para el curso (ej. `java-course/module-01/class-01`).
  * Buenas prácticas de nombres para repositorios, carpetas y archivos Java.
  * Uso de archivos `.gitignore` (breve mención para ignorar archivos compilados `.class` o de configuración del IDE).
* **Demostración / Guía**
  * Cómo crear un nuevo repositorio **vacío** en GitHub (sin README ni .gitignore inicial).
  * Cómo clonar ese repositorio vacío en la máquina local.
  * Cómo organizar el proyecto dentro de esa carpeta clonada.
  * Énfasis en los commits descriptivos.

### 5. Cierre y Preguntas Adicionales

* **Recapitulación de los temas clave cubiertos**: instalación exitosa, comandos Git básicos, creación de ramas.
* Recordatorio del ejercicio propuesto para la semana (se explicará a continuación).
* Abrir el espacio para preguntas finales.
* Animar a los estudiantes a seguir practicando.
