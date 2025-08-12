# Ejercicio Autoguiado: Desarrollo del Backend para un Sistema de Gestión de Cursos Universitarios

## Objetivo del Ejercicio

- El objetivo de este ejercicio es desarrollar una **API RESTful** completa que sirva como backend para el sistema de gestión de cursos universitarios del Módulo 8. La API debe gestionar la persistencia de datos, la lógica de negocio y la seguridad, reemplazando la simulación local del frontend. Deberás implementar las entidades, relaciones y endpoints necesarios para soportar todas las funcionalidades descritas.
- El otro objetivo es que veas cómo un desarrollador podría interactuar con un agente de IA para construir cada pieza del backend de forma incremental.

## 1. Configuración de la Aplicación y Entidades

Comenzaremos por configurar la conexión a la base de datos y luego definiremos las entidades que mapean nuestras tablas.

### 1.1. Configurar la conexión a la base de datos

Un buen primer paso es indicarle a la IA que genere el archivo de configuración. Un prompt efectivo debe ser específico sobre el archivo, la base de datos y los parámetros de configuración.

**Prompt para el Agente IA**:

> Escribe el contenido para el archivo `application.properties`. Necesito configurar una conexión a una base de datos PostgreSQL local. La base de datos está en la misma maquina y su nombre es `university_db`, el usuario `postgres` y la contraseña password`. Además, configura Hibernate para que muestre las consultas SQL en la consola y para que actualice el esquema de la base de datos automáticamente al iniciar. El puerto del servidor debe ser 8080.

### 1.2. Crear las entidades `User` y `Student`

Las entidades son objetos Java que representan las tablas de tu base de datos. Un prompt para una entidad debe ser detallado, especificando los campos, tipos de datos, validaciones y relaciones. En este caso, usaremos herencia de JPA para `Student` que extiende de `User`.

**Prompt para el Agente IA**:

> Crea las clases de entidad para `User` y `Student` en el paquete `com.example.university.model`.
>
> 1. `User`: Debe ser una clase base (`@Entity`) con `@Table(name = "users")`. Los campos son: `id` (autogenerado), `name`, `lastName`, `email` (único), `password`, y `isTutor` (boolean). Usa `@NotBlank`, `@Email` y `@Size` para las validaciones. Usa Lombok para los constructores y getters/setters.
> 2. `Student`: Debe heredar de `User` y tener una relación `ManyToMany` con la entidad `Course` (que crearemos más tarde). Añade una lista de `Course` inscritos y usa las anotaciones de Lombok.

### 1.3. Crear la entidad `Course`

Ahora crearemos la entidad `Course` para completar nuestro modelo de datos.

**Prompt para el Agente IA**:

> Crea la clase de entidad `Course` en el paquete `com.example.university.model`. Debe tener los siguientes campos: `id` (Long, autogenerado), `name` (String), `description` (String), y una relación @`ManyToOne` con un `User` (el tutor). También debe tener una relación `@ManyToMany` con una lista de `Student` que se inscriben en el curso. Usa la anotación `@JoinTable` para la tabla de unión.

### 1.4. Crear los Repositorios

Los repositorios son interfaces que Spring Data JPA utiliza para generar automáticamente las consultas a la base de datos.

**Prompt para el Agente IA**:

> Genera las interfaces de repositorio para las entidades `User`, `Student` y `Course`. Estas interfaces deben extender de `JpaRepository` y estar en el paquete `com.example.university.repository`. Para el `UserRepository`, añade un método `findByEmail(String email)` que devuelva un `Optional<User>`.

## 2. Lógica de Negocio y API REST

Ahora que el modelo de datos está listo, construiremos la lógica de negocio y la capa de la API REST.

### 2.1. Crear los Servicios

La capa de servicio contiene la lógica de negocio. Un buen prompt para un servicio debe enumerar los métodos que necesitas y sus responsabilidades.

**Prompt para el Agente IA**:

> Genera una clase de servicio `UserService.java` en el paquete `com.example.university.service`. La clase debe estar anotada con `@Service` e inyectar `UserRepository`. Debe tener los siguientes métodos:
>
> - `getAllUsers()`: Para obtener todos los usuarios.
> - `getUserById(Long id)`: Para obtener un usuario por su ID.
> - `saveUser(User user)`: Para guardar o actualizar un usuario.
> - `deleteUser(Long id)`: Para eliminar un usuario.
> - `login(String email, String password)`: Para validar las credenciales de un usuario.
> - `register(User user)`: Para registrar un nuevo usuario.

### 2.2. Crear el Controlador REST

El controlador es la capa que expone nuestra API. El prompt debe especificar la URL base y el método HTTP para cada endpoint.

**Prompt para el Agente IA**:

> Genera la clase UserController.java en el paquete com.example.university.controller. Debe ser un @RestController con la URL base /api/users. Inyecta el UserService.
>
> - Crea un endpoint `GET /` para obtener todos los usuarios.
> - Crea un `GET /{id}` para obtener un usuario por su ID.
> - Crea un `POST /register` para registrar un nuevo usuario.
> - Crea un `POST /login` para autenticar a un usuario.
> - Crea un `PUT /{id}` para actualizar un usuario.
> - Crea un `DELETE /{id}` para eliminar un usuario.
> - Usa `@Valid` para la validación del cuerpo de la solicitud y maneja los errores con `ResponseEntity`.

---

**Manos a la obra!**

Este ejercicio te permitirá consolidar tus conocimientos de desarrollo backend y comprender cómo se integra con un frontend moderno, creando un sistema completo y funcional.
