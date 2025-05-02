# Proyecto Integrador del Módulo: Sistema Simplificado de Gestión de Biblioteca (API RESTful en Memoria)

¡Hola a todos!

Es momento de aplicar y consolidar todo lo aprendido en el módulo construyendo un proyecto completo. Este proyecto les permitirá integrar la estructura básica de Spring Boot, la arquitectura por capas, los principios SOLID, la validación de datos y la documentación automática en una sola aplicación funcional.

## Objetivo General del Proyecto

Construir una API RESTful completa y bien estructurada para la gestión básica de una biblioteca, **utilizando estructuras de datos en memoria para simular la persistencia**, aplicando todos los conceptos y herramientas vistos en el módulo: estructura de proyecto Spring Boot, arquitectura por capas, principios SOLID, validación de datos y documentación automática con OpenAPI, utilizando **Cursor AI** como herramienta principal durante todo el proceso.

## Descripción del Proyecto

Desarrollarán el backend de un sistema de biblioteca que permitirá gestionar información sobre libros. La interacción con el sistema se realizará completamente a través de una API RESTful. La información de los libros **se almacenará temporalmente en una colección en memoria (como `List` o `Map`) y, por lo tanto, se perderá al reiniciar la aplicación**.

## Requisitos Mínimos de Funcionalidad

1. **Gestión de Libros:**
    * **Entidad Principal: `Libro`**. Un libro debe tener al menos los siguientes atributos:
        * `ID` (identificador único). Deberán generar este ID manualmente en la capa de servicio o repositorio simulado (ej. usando un contador atómico como `AtomicLong`).
        * `Título` (texto).
        * `Autor` (texto).
        * `ISBN` (identificador único de libro, formato texto).
        * `Año de Publicación` (numérico).
        * `Género` (texto, opcional).
        * `Estado` (ej. "Disponible", "Prestado" - puede ser un `String` simple o un `Enum`).
    * **Operaciones CRUD a través de Endpoints REST:**
        * `POST /api/libros`: Crear un nuevo libro en el sistema.
        * `GET /api/libros`: Obtener un listado de todos los libros.
        * `GET /api/libros/{id}`: Obtener los detalles de un libro específico por su ID.
        * `PUT /api/libros/{id}`: Actualizar la información de un libro existente identificado por su ID.
        * `DELETE /api/libros/{id}`: Eliminar un libro por su ID.
    * **Operaciones Adicionales:**
        * `GET /api/libros/buscar?q={textoBusqueda}`: Buscar libros cuyo título o autor contengan el texto proporcionado.
        * **(Opcional, para ir más allá)** `POST /api/libros/{id}/prestar`: Marcar un libro específico como "Prestado".

## Aplicación de Conceptos del Módulo

Este proyecto está diseñado para que apliquen integradamente todos los conceptos y habilidades que han aprendido:

1. **Introducción a Spring Boot y Estructura de Proyectos:**
    * Configuren el proyecto usando [Spring Initializr](https://start.spring.io/) con las dependencias necesarias: `Spring Web`, `Validation`, `Springdoc OpenAPI UI`, `Lombok` (opcional).
    * Organicen su código en paquetes estándar y lógicos (ej. `com.suempresa.biblioteca.controller`, `.service`, `.repository`, `.model`).
2. **Arquitectura por Capas (Controlador, Servicio, Persistencia Simulada):**
    * Implementen la lógica de negocio para cada operación (CRUD, búsqueda, prestar opcional) en una **Capa de Servicio** (`@Service`).
    * Implementen una capa de **Persistencia Simulada** (`@Repository`). En lugar de interactuar con una base de datos real, esta clase manejará una colección en memoria (ej. `private List<Libro> libros = new ArrayList<>();` o `private Map<Long, Libro> libros = new HashMap<>();`). Implementen manualmente los métodos `save`, `findById`, `findAll`, `deleteById`, etc., manipulando esta colección en memoria. Asegúrense de gestionar la generación de IDs aquí (ej. con un `AtomicLong`).
    * Asegúrense de que los **Controladores** (`@RestController`) solo manejen las peticiones HTTP (recibir, delegar al servicio, formatear respuesta).
    * Utilicen la **Inyección de Dependencias** (`@Autowired` en el constructor) para conectar las capas Controlador -> Servicio -> Repositorio Simulado, dependiendo de interfaces siempre que sea posible.
3. **Principios SOLID y su Aplicación Práctica:**
    * **Conscientemente, busquen oportunidades para aplicar SOLID en su diseño.** No es necesario aplicar los 5 principios de forma forzada, pero al menos consideren:
        * **SRP (Responsabilidad Única):** ¿Su `LibroService` se enfoca solo en la lógica de negocio de libros? ¿Hay alguna otra responsabilidad que podría ir en otra clase dedicada?
        * **DIP (Inversión de Dependencia):** Asegúrense de que sus clases de "alto nivel" (Controlador, Servicio) dependan de *interfaces* (ej. `LibroService`, `LibroRepository`) y no de implementaciones concretas (ej. `InMemoryLibroRepository`).
        * **OCP (Abierto/Cerrado):** Si añaden lógica que podría variar en el futuro, piensen en usar interfaces y polimorfismo para que su código sea extensible sin modificar las clases existentes.
    * Utilicen las herramientas de análisis y refactorización de **Cursor AI** para identificar posibles violaciones de SOLID a medida que desarrollan y para aplicar refactorizaciones.
4. **Uso de Java Validation API y Documentación con OpenAPI:**
    * **Java Validation API:** Añadan anotaciones de validación (`@NotBlank`, `@Size`, `@Min`, `@FutureOrPresent`, etc.) a los atributos de su entidad `Libro` (o de un DTO si deciden usar uno para las peticiones POST/PUT) para validar los datos de entrada. Activen la validación en los métodos del controlador que reciben cuerpos de petición usando la anotación `@Valid`.
    * **OpenAPI (Springdoc):** Configuren la dependencia de Springdoc OpenAPI. Añadan anotaciones de Swagger (`@Tag`, `@Operation`, `@Parameter`, `@RequestBody`, `@ApiResponses`, `@Schema`) a su Controlador y Entidad `Libro` para generar documentación clara y completa que sea accesible a través de Swagger UI.

## Tecnología y Herramientas

* Java SDK (versión compatible con Spring Boot).
* Spring Boot (dependencias `Spring Web`, `Validation`, `Springdoc OpenAPI UI`).
* Maven o Gradle (para gestionar dependencias).
* Jakarta Validation API (implementada por Hibernate Validator, incluida con la dependencia `Validation`).
* Springdoc OpenAPI UI.
* Lombok (Opcional, pero recomendado para reducir código boilerplate).
* **Cursor AI** (Su editor de código principal y asistente durante todo el proyecto).
* Un cliente REST para probar la API (Postman, Insomnia, `curl`, o la interfaz de Swagger UI).
* Git y GitHub (para control de versiones y entrega del proyecto).

## Cómo Abordar el Proyecto (Sugerencia de Pasos)

1. **Configuración Inicial:** Creen el proyecto base en Spring Initializr con las dependencias correctas e impórtenlo en Cursor AI.
2. **Modelo:** Definan la clase `Libro` con sus atributos.
3. **Repositorio Simulado:** Creen la interfaz `LibroRepository` y la implementación `InMemoryLibroRepository` que use una colección en memoria. Implementen los métodos CRUD y búsqueda manipulando esta colección. Anoten la implementación con `@Repository` y gestionen la generación de IDs.
4. **Servicio:** Creen la interfaz `LibroService` y la implementación `LibroServiceImpl`. Inyecten el `LibroRepository` (la interfaz) en el servicio. Implementen los métodos de servicio llamando al repositorio.
5. **Controlador:** Creen `LibroController`. Inyecten el `LibroService` (la interfaz). Implementen los endpoints REST para cada operación (GET, POST, etc.), llamando a los métodos correspondientes en el servicio. Prueben cada endpoint a medida que lo implementan usando un cliente REST.
6. **Validación:** Añadan las anotaciones de validación a la clase `Libro`. Vayan a su `LibroController` y añadan `@Valid` a los parámetros `@RequestBody` en los métodos `POST` y `PUT`. Prueben enviando datos que violen las reglas de validación.
7. **Documentación:** Configuren Springdoc OpenAPI. Añadan anotaciones de Swagger (@Tag, @Operation, @Parameter, @RequestBody, @ApiResponses, @Schema) a su `LibroController` y a la clase `Libro` para documentar su API. Ejecuten la aplicación y revisen el Swagger UI (`http://localhost:8080/swagger-ui.html`). Asegúrense de que la documentación sea clara.
8. **Implementar Búsqueda y Opcionales:** Añadan la lógica de búsqueda en el repositorio/servicio y el endpoint `GET /api/libros/buscar` en el controlador. Si tienen tiempo, implementen la funcionalidad de "prestar".
9. **Refactorización y SOLID:** Revisen su código completo. Busquen oportunidades para aplicar SRP y DIP usando interfaces y refactorizaciones con la ayuda de **Cursor AI**.
10. **Documentación Final:** Asegúrense de que todas las partes de la API estén bien documentadas en Swagger UI y que los mensajes de validación sean claros.
11. **Control de Versiones y Entrega:** Suban su proyecto a un repositorio en GitHub. Asegúrense de que el archivo `.gitignore` esté bien configurado. Creen un archivo `README.md` profesional explicando el proyecto, cómo ejecutarlo, cómo probarlo y cómo acceder a la documentación.

## Entrega

Deberán entregar o estar listos para presentar:

* El código fuente completo del proyecto en un repositorio de GitHub público.
* Un archivo `README.md` profesional en la raíz del repositorio que explique el proyecto y su uso, mencionando explícitamente que la persistencia es en memoria.
* Ser capaces de ejecutar la aplicación localmente y mostrar la API funcionando (ej. interactuando con ella a través de Swagger UI o Postman).
* Ser capaces de explicar su diseño por capas (incluyendo la capa de persistencia simulada), cómo aplicaron las validaciones, cómo documentaron la API y cómo intentaron aplicar principios SOLID, utilizando **Cursor AI** para mostrar y explicar partes específicas del código si es necesario.

Este proyecto es su oportunidad para integrar y demostrar todas las habilidades clave de desarrollo backend con Spring Boot vistas en el módulo. ¡Será un excelente complemento para su portafolio profesional!

¡Mucho éxito con este proyecto integrador! Utilicen **Cursor AI** al máximo para asistirlos en cada etapa, desde la generación de código hasta la refactorización y la documentación. ¡A codificar!
