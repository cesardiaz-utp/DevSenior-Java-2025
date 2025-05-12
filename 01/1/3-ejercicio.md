# Ejercicio Autónomo: Mi Primer Repositorio de Java y Programa Personalizado

## Objetivos del Ejercicio

- Configurar un repositorio Git localmente y vincularlo con GitHub.
- Aplicar una estructura de carpetas organizada para los proyectos del curso.
- Crear un programa Java simple que imprima un mensaje personalizado.
- Utilizar Git para versionar y subir tus cambios a GitHub.
- Documentar tu código con comentarios.

## Instrucciones Detalladas

### 1. Estructura del Proyecto (Local)

- En tu máquina local, crea una carpeta raíz para todos los proyectos del curso, llamada `java-course`.
- Dentro de `java-course`, crea una carpeta para este módulo: `module-01`.
- Dentro de `module-01`, crea una carpeta para esta clase: `class-01`.
- Tu estructura debería verse así:

  ```text
  java-course/
  └── module-01/
      └── class-01/
          └── (aquí irá tu código Java)
  ```

### 2. Crea tu Programa "Hola Mundo" Personalizado

- Dentro de la carpeta **class-01**, crea un archivo llamado `MiPrimerSaludo.java`.
- En este archivo, escribe un programa Java que imprima en la consola un mensaje personalizado que incluya tu nombre. Por ejemplo:

  ```Java
  // Este programa imprime un saludo personalizado
  public class MiPrimerSaludo {
      public static void main(String[] args) {
          System.out.println("¡Hola, soy [Tu Nombre] y este es mi primer programa en Java!");
          System.out.println("Estoy emocionado/a de aprender programación con Java.");
      }
  }
  ```

- ¡Importante! Reemplaza `[Tu Nombre]` con tu nombre real.
- Asegúrate de que el programa compile y se ejecute correctamente desde VSCode y/o la terminal.

### 3. Inicializa tu Repositorio Git

- Abre la terminal integrada de VSCode o tu terminal/símbolo del sistema.
- Navega hasta la carpeta raíz de tu curso, es decir, `java-course`.
- Inicializa un repositorio Git en esta carpeta:

  ```Bash
  git init
  ```

### 4. Primer Commit Local

- Añade todos los archivos del proyecto que acabas de crear al área de staging:

  ```Bash
  git add .
  ```

- Realiza tu primer commit con un mensaje descriptivo que siga las buenas prácticas (por ejemplo, en inglés para mensajes de commit):

  ```Bash
  git commit -m "feat: Initial setup and first personalized Java program"
  ```

### 5. Crea un Repositorio en GitHub

- Ve a [github.com](github.com) e inicia sesión en tu cuenta.
- Haz clic en el botón "New" (Nuevo) para crear un nuevo repositorio.
- Asígnale el nombre `java-course` (debe coincidir con tu carpeta local).
- Selecciona `Public` (Público).
- **¡Muy importante!** No marques las opciones para "Add a README file", "Add .gitignore" o "Choose a license". Queremos un repositorio completamente vacío.
- Haz clic en "Create repository".

### 6. Conecta y Sube a GitHub

- Una vez creado el repositorio en GitHub, te mostrará instrucciones para conectar un repositorio local existente. Copia la línea que se ve así:

  ```Bash
  git remote add origin https://github.com/tu_usuario/java-course.git
  # O si usas SSH
  git remote add origin git@github.com:tu_usuario/java-course.git
  ```

- Pégala y ejecuta en tu terminal (estando en la carpeta `java-course`).
- Finalmente, sube tus cambios al repositorio remoto:

  ```Bash
  git push -u origin main
  # O si tu rama principal se llama 'master'
  git push -u origin master
  ```

- Refresca la página de tu repositorio en GitHub. Deberías ver tus carpetas y el archivo `MiPrimerSaludo.java`.

### 7. Documentación del Código

Asegúrate de que tu archivo `MiPrimerSaludo.java` contenga comentarios relevantes que expliquen brevemente qué hace el programa, la clase principal y la línea que imprime el mensaje. (Ya están en el ejemplo proporcionado).

## Criterios de Evaluación (para el estudiante)

Tu ejercicio será evaluado en base a los siguientes puntos:

- **Estructura del Repositorio**: El repositorio en GitHub es accesible y sigue la estructura de carpetas `java-course/module-01/class-01/`.
- **Funcionalidad del Programa**: El archivo `MiPrimerSaludo.java` está presente en la ubicación correcta y ejecuta sin errores, mostrando el mensaje personalizado con tu nombre en la consola.
- **Uso de Git**: Se evidencia el uso de Git con al menos un `commit` descriptivo (`git log` mostrará el historial).
- **Conexión a GitHub**: El repositorio local está correctamente vinculado y los cambios han sido subidos al repositorio remoto en GitHub.
- **Documentación**: El código Java contiene comentarios claros y útiles.

## Retos Adicionales (Opcional)

- **Segundo Commit**: Haz un pequeño cambio en tu programa (ej. añade una tercera línea con otro mensaje), realiza un nuevo `commit` y sube los cambios a GitHub.
- **Archivo `.gitignore`**: Investiga y crea un archivo `.gitignore` en la raíz de tu repositorio `java-course` para ignorar los archivos compilados de Java (`.class`) y las carpetas de configuración de VSCode (`.vscode/`). Luego, realiza un `commit` y `push` de este nuevo archivo.
