# Ejercicio Autónomo: Catálogo de Productos Flexible con MongoDB

¡Hola!

Ya se introdujeron a las bases de datos NoSQL con MongoDB y aprendieron a conectarla a Spring Boot y realizar operaciones básicas. También exploramos cómo realizar consultas en campos anidados, arrays y usar expresiones regulares. Ahora es su turno de aplicar todo esto construyendo una API que aproveche la flexibilidad de MongoDB: un Catálogo de Productos donde cada producto puede tener especificaciones técnicas diferentes.

## Objetivo del Ejercicio

Construir una API RESTful utilizando Spring Boot y MongoDB para gestionar un catálogo de productos, enfocándose en el modelado de datos con esquema flexible y la implementación de consultas que busquen dentro de campos anidados y arrays, así como búsquedas de texto.

## Escenario del Ejercicio: Un Catálogo de Productos con Especificaciones Variables

Imagina que necesitas un sistema para gestionar productos que no tienen una estructura fija de atributos. Por ejemplo, un libro tiene ISBN, un laptop tiene RAM y procesador, una camiseta tiene talla y color. MongoDB es ideal para esto debido a su esquema flexible.

### Requerimientos

#### Queremos guardar información de Productos en una colección de MongoDB

Cada producto debe tener:

- Un identificador único (gestionado por MongoDB).
- Un nombre.
- Un precio.
- Una descripción general.
- **Especificaciones Flexibles**: Un campo que permita almacenar diferentes atributos dependiendo del tipo de producto (ej. para un libro: autor, ISBN, editorial; para un laptop: RAM, CPU, tamaño pantalla; para una camiseta: talla, color, material). Este campo debe ser capaz de almacenar diferentes subdocumentos o pares clave-valor para cada producto.
- Una lista de tags (etiquetas) asociados al producto (ej. ["electrónica", "laptop"], ["libro", "ficción"]).

#### Implementar una API RESTful con los siguientes endpoints

- `POST /api/productos`: Crear un nuevo producto. Al enviar los datos, deben incluir las especificaciones flexibles.
- `GET /api/productos`: Obtener la lista de todos los productos.
- `GET /api/productos/{id}`: Obtener un producto específico por su ID.
- `PUT /api/productos/{id}`: Actualizar la información de un producto existente, incluyendo la modificación de sus especificaciones flexibles.
- `DELETE /api/productos/{id}`: Eliminar un producto.
Implementar Endpoints de Consulta:
- `GET /api/productos/buscar?q={textoBusqueda}`: Buscar productos cuyo nombre o descripción contenga el texto proporcionado (búsqueda parcial, ignorando mayúsculas/minúsculas, usando Regex).
- `GET /api/productos/por-tag?tag={tag}`: Encontrar todos los productos que tengan una etiqueta específica dentro de su lista de tags.
- `GET /api/productos/especificacion?key={clave}&value={valor}`: Encontrar productos que en su campo de especificaciones flexibles tengan un campo específico (ej. "RAM") con un valor dado (ej. "16GB"). Esto requiere consultar dentro del campo de especificaciones anidado.
- (Opcional): `GET /api/productos/precio-rango?minPrice=...&maxPrice=...`: Encontrar productos cuyo precio esté dentro de un rango dado.

## Pasos a Seguir

Construyan este proyecto desde cero o reutilicen un proyecto Spring Boot limpio. ¡Utilicen Cursor AI intensivamente en cada paso!

### Paso 1: Configurar el Proyecto Spring Boot para MongoDB

#### 1.1 Tu Tarea

- Creen un nuevo proyecto Spring Boot con las dependencias: `Spring Web`, `Spring Boot Starter Data MongoDB`, `Validation`, `Springdoc OpenAPI UI`, `Lombok` (opcional).
- Configuren la conexión a su servidor de MongoDB (local o Atlas) en `application.properties` o `application.yml`, especificando la URL/host, puerto, base de datos, usuario y contraseña.

#### 1.2 Cómo Cursor AI puede ayudar

- Pídanle ayuda para crear el proyecto base en Spring Initializr.
- Pídanle que les genere el bloque de configuración de MongoDB para `application.properties` basándose en sus datos de conexión.

### Paso 2: Diseñar el Documento Producto y Crear el Repositorio

#### 2.1 Tu Tarea

- Creen la clase `Producto.java` en un paquete `model`.
- Anótenla con `@Document(collection = "productos")`.
- Añadan atributos para `id` (@Id, String), `nombre`, `precio`, `descripcion`, `tags` (usen `List<String>` o `Set<String>`).
- Para las **especificaciones flexibles**, añadan un atributo que pueda almacenar pares clave-valor heterogéneos. `Map<String, Object>` es una opción común en Java, o pueden usar la clase `Document` del driver de MongoDB (aunque `Map` suele ser más simple con Spring Data). Anoten este campo con `@Field("especificaciones")`.
- Añadan constructores, getters, setters (Lombok o generación de código).
- Añadan anotaciones de validación (`@NotBlank`, `@Min`, etc.) a los campos requeridos (`nombre`, `precio`).

#### 2.2 Cómo Cursor AI puede ayudar

- Pídanle que les genere la clase `Producto` con los campos mencionados, incluyendo el campo de especificaciones flexibles como `Map<String, Object>`. Asegúrense de que añada las anotaciones `@Document`, `@Id`, `@Field`.
- Pídanle que añada anotaciones de validación a los campos obligatorios.

#### 2.3 Tu Tarea

- Creen la interfaz `ProductoRepository` en un paquete `repository`.
- Debe extender MongoRepository<Producto, String>.

#### 2.4 Cómo Cursor AI puede ayudar

- Pídanle que genere la interfaz `ProductoRepository` para la entidad `Producto`.

### Paso 3: Implementar Operaciones CRUD Básicas

#### 3.1 Tu Tarea

- Creen una clase de servicio `ProductoService` (`@Service`) que inyecte `ProductoRepository`.
  - Implementen métodos de servicio para las operaciones CRUD: `guardarProducto(Producto p)`, `obtenerTodos()`, `obtenerPorId(String id)`, `eliminarPorId(String id)`.
- Creen una clase ProductoController (`@RestController`, `@RequestMapping("/api/productos")`) que inyecte `ProductoService`.
  - Implementen los endpoints REST para CRUD (`POST /api/productos`, `GET /api/productos`, `GET /{id}`, `PUT /{id}`, `DELETE /{id}`).
  - Usen `@RequestBody` para `POST` y `PUT`. Usen `@Valid` para activar la validación de los campos básicos en `POST` y `PUT`.
  - Usen `ResponseEntity` para manejar las respuestas HTTP (ej. 201 Created para POST, 404 Not Found si no se encuentra un producto en GET/PUT/DELETE).

#### 3.2 Cómo Cursor AI puede ayudar

- Pídanle que les ayude a inyectar el repositorio en el servicio y el servicio en el controlador.
- Pídanle que les genere el esqueleto de los métodos de servicio y los endpoints REST correspondientes para las operaciones CRUD.

### Paso 4: Implementar Endpoints y Consultas de Búsqueda Avanzada

#### 4.1 Tu Tarea

- Añadan los métodos de consulta necesarios en su `ProductoRepository` y la lógica en su `ProductoService` para implementar los endpoints de búsqueda:
  - **Buscar por Nombre/Descripción (Regex)**: Añadan un método derivado a `ProductoRepository` como `List<Producto> findByNombreContainingIgnoreCaseOrDescripcionContainingIgnoreCase(String textoBusqueda);`.
  - **Buscar por Tag (Array)**: Añadan un método derivado a `ProductoRepository` como `List<Producto> findByTagsContaining(String tag);`.
  - `Buscar por Especificación Flexible (Anidado con Clave/Valor Dinámica)`: Esta es más compleja con métodos derivados puros. Probablemente necesitarán usar @Query con sintaxis de consulta de MongoDB.
    - Ejemplo de `@Query`: `@Query("{ 'especificaciones.?0' : ?1 }") List<Producto> findByEspecificacionKeyAndValue(String key, Object value);`. Aquí `?0` es el nombre del campo de especificación (la clave en el mapa) y `?1` es su valor.
  - Implementen los métodos de servicio correspondientes para cada consulta.
- Añadan los nuevos endpoints a su `ProductoController` para exponer estas búsquedas (`GET /api/productos/buscar`, `GET /api/productos/por-tag`, `GET /api/productos/especificacion`). Usen `@RequestPara`m para recibir los parámetros de búsqueda.

#### 4.2 Cómo Cursor AI puede ayudar

- **Métodos Derivados**: Descríbanle las búsquedas simples por nombre/descripción/tag y pídanle las firmas de los métodos derivados en `ProductoRepository`.
- **Consultas `@Query` (Anidadas/Flexibles)**: Esta es una excelente oportunidad para usar la IA. Descríbanle el requisito de buscar productos por una clave y valor arbitrarios dentro del campo de especificaciones flexibles (`Map<String, Object>`) y pídanle la anotación `@Query` con la sintaxis de consulta de MongoDB y la firma del método Repository. Por ejemplo: "Write a Spring Data MongoDB @Query method to find products where the 'specifications' map contains a specific key with a specific value".
- **Lógica de Servicio y Controlador**: Pídanle que les genere el código para los métodos de servicio y controlador que utilizan estos nuevos métodos de repositorio.

### Paso 5: Documentar con OpenAPI (Swagger)

#### 5.1 Tu Tarea

- Añadan la dependencia springdoc-openapi-ui si aún no lo han hecho.
- Añadan anotaciones de Swagger (`@Tag`, `@Operation`, `@Parameter`, `@RequestBody`, @`ApiResponses`, `@Schema`) a la clase `Producto` y a todos los endpoints en `ProductoController` para documentar su API. Presten especial atención a documentar el campo de `especificaciones` flexible (tal vez con `@Schema` y `additionalProperties`).

#### 5.2 Cómo Cursor AI puede ayudar

- Pídanle que les ayude a añadir las anotaciones de Swagger a la clase `Producto` y a cada método del controlador.
- Pregúntenle cómo documentar un campo de tipo `Map<String, Object>` o una lista de strings (`List<String>`) en Swagger usando `@Schema`.

### Paso 6: Probar a Fondo y Ver Documentos

#### 6.1 Tu Tarea

- Utilicen un cliente REST (Postman, Swagger UI) para probar todos los endpoints:
  - Creen productos con **diferentes conjuntos de especificaciones flexibles** (ej. un POST con datos de laptop, otro con datos de libro).
  - Obtengan productos por ID y verifiquen que las especificaciones se guardaron y cargaron correctamente.
  - **Prueben las búsquedas**: por texto en nombre/descripción, por tags, y especialmente por especificaciones clave/valor (ej. buscar `key="RAM"`, `value="16GB"` o `key="ISBN"`, `value="978-..."`).
  - Prueben la validación.

- Utilicen **MongoDB Compass** para conectarse a su base de datos y ver los documentos que se han creado en la colección `productos`. Observen cómo MongoDB almacenó los diferentes conjuntos de especificaciones flexibles.

#### 6.2 Cómo Cursor AI puede ayudar

- Si una consulta no funciona como esperan, descríbanle a Cursor AI el método Repository, la sintaxis de la consulta (`@Query` o derivada) y lo que esperaban obtener. Pídanle que depure la consulta.
- Si ven documentos en Compass que no tienen la estructura esperada, revisen su clase `@Document` y pídanle a la IA que la revise.

### Paso 7: Refactorizar y Pulir

#### 7.1 Tu Tarea

Revisen su código. Asegúrense de que el servicio contiene la lógica y el controlador solo maneja las peticiones. Apliquen limpieza general.

#### 7.2 Cómo Cursor AI puede ayudar

Pídanle un análisis general de sus clases ProductoService y ProductoController en busca de olores de código o sugerencias de mejora.

## Entrega

Deberán subir su proyecto completo a un repositorio de GitHub. Asegúrense de que el README.md explique cómo ejecutar el proyecto y cómo usar los diferentes endpoints, incluyendo ejemplos para crear productos con especificaciones flexibles y cómo usar los endpoints de búsqueda avanzada. Estén listos para demostrar su API y explicar cómo modelaron y consultaron los datos flexibles.

---

Este ejercicio es clave para entender la flexibilidad de MongoDB y cómo Spring Data se adapta a ella. ¡El manejo de las especificaciones flexibles y las consultas sobre ellas es el desafío central! Utilicen Cursor AI sin miedo para experimentar con diferentes sintaxis de consulta y modelado.

¡Mucho éxito!
