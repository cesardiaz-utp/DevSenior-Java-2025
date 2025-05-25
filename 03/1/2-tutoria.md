# Tutoría : Maven y Excepciones

## 1. Bienvenida y Recopilación de Dudas Rápidas

- Abrir el espacio para preguntas muy puntuales sobre la clase anterior o el ejercicio propuesto. No profundizamos en el repaso, ya que el mini-proyecto servirá como tal.
- Animar a los estudiantes a compartir cualquier problema de configuración inicial de Maven que hayan tenido.

## 2. Mini-Proyecto: Sistema de Gestión de Biblioteca

- **Escenario**: Están construyendo un sistema de gestión para una pequeña biblioteca. Necesitarán registrar libros y usuarios, permitir el préstamo de libros y gestionar devoluciones. El sistema debe ser robusto y manejar errores como intentar prestar un libro no disponible o a un usuario que no existe.
- **Entidades Clave**:
  - `-book`: `isbn` (único, String), `title` (String), `author` (String), `available` (boolean).
  - `User`: `idUser` (único, String), `name` (String), `email` (String).
- **Operaciones Clave**:
  - Registrar un nuevo libro.
  - Registrar un nuevo usuario.
  - Prestar un libro a un usuario.
  - Devolver un libro.
  - Buscar libro por ISBN.
  - Buscar usuario por ID.
  - Listar todos los libros (indicando disponibilidad).
  - Listar todos los usuarios.
- **Excepciones a Implementar**:
  - `BookNotFoundException` (checked)
  - `UserNotFoundException` (checked)
  - `BookNotAvailableException` (checked)
  - `InvalidDataException` (unchecked - para datos de entrada nulos/vacíos/inválidos)
  - `DuplicatedIdException` (unchecked - para ISBN de libro o ID de usuario duplicado)

### Paso 1: Configuración del Proyecto Maven

**Guiar a los estudiantes** en la creación de un **nuevo proyecto Maven** en VS Code.

- `groupId: com.devsenior.library`
- `artifactId: library-management`
- Ajustar `maven.compiler.source` y `target` a 21 (o su versión de JDK).

**Discusión**: ¿Por qué crear un nuevo proyecto Maven en lugar de reutilizar el anterior? (Estandarización, modularidad, buenas prácticas).

### Paso 2: Definición de Entidades y Excepciones

- **Crear paquetes**: `com.devsenior.library.model` (para `Book` y `User`) y `com.devsenior.library.exceptions` (para las excepciones personalizadas).
- **Implementar `Book.java` y `User.java`**: Recordar `equals()`, `hashCode()` y `toString()`.
- **Implementar las clases de excepción**: `BookNotFoundException`, `UserNotFoundException`, `BookNotAvailableException` (todas extends `Exception`) y `InvalidDataException`, `DuplicatedIdException` (ambas extends `RuntimeException`).

**Discusión**: ¿Qué implicaciones tiene extends `Exception` vs extends `RuntimeException`? ¿Cuándo usar cada una?

### Paso 3: Lógica de Negocio (LibrarySystem) con Excepciones

- Crear `LibrarySystem.java` en `com.devsenior.library.system`.
- Implementa los métodos para `registerBook`, `registerUser`, `borrowBook`, `returnBook`, `findBook`, `findUser`, `listAllBooks`, `listAllUsers`.
- Implementar métodos para registrarLibro, registrarUsuario, prestarLibro, devolverLibro, buscarLibro, buscarUsuario, listarLibros, listarUsuarios.
- **Enfocarse en los `throw new ...Exception()`**: Guiarlos para que lancen las excepciones en los puntos correctos (ej. `BookNotAvailableException` al intentar prestar un libro ya prestado, `InvalidDataException` si un título es nulo, `DuplicatedIdException` si el ISBN ya existe, etc.).
- **Soporte**: Circular por el aula (o compartir pantalla) para resolver dudas de sintaxis y lógica en tiempo real.

## 3. Prueba y Manejo de Excepciones en `App.java`

- Modificar `App.java` en `com.devsenior.library`.
- Crear una instancia de `LibrarySystem`.
- Guiar la implementación de `try-catch`:
  - Demostrar el uso de `try-catch` para manejar las excepciones verificadas ( `BookNotFoundException`, `UserNotFoundException`, `BookNotAvailableException`).
  - Demostrar cómo las excepciones no verificadas (`InvalidDataException`, `DuplicatedIdException`) no obligan a `try-catch` pero es buena práctica manejarlas en ciertos puntos para dar feedback al usuario.
  - Probar casos exitosos y fallidos (ej. prestar un libro dos veces, buscar un libro/usuario inexistente, registrar un libro con datos inválidos).
  - **Discusión**: ¿Cómo haría el código más robusto y amigable para el usuario final? (Uso de e.getMessage()).
- **Ejecución con Maven**: Recordar y guiar cómo ejecutar `mvn clean install` y luego `java -jar target/library-management-1.0.0-SNAPSHOT.jar` (o ejecutar directamente desde VS Code).

## Cierre y Próximos Pasos

- ¿Qué aprendieron de nuevo o qué se les aclaró?
- Responder preguntas finales.
- Presentar el ejercicio propuesto para práctica autónoma.
- Adelantar el tema de la próxima clase.
