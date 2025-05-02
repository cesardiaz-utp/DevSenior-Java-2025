# Ejercicio Autónomo: Profundizando en Spring Data JPA - Relaciones y Consultas Avanzadas

¡Hola!

Ya dieron un gran paso al conectar Spring Boot a PostgreSQL, crear tablas y mapear entidades básicas. En la Clase 2 y la tutoría, exploramos a fondo cómo manejar relaciones entre entidades y cómo usar Spring Data JPA para operaciones CRUD y consultas básicas. Este ejercicio es para que apliquen todo ese conocimiento extendiendo su proyecto existente de Clientes y Pedidos.

## Objetivo del Ejercicio

Extender su proyecto actual de API de Clientes y Pedidos para incluir una nueva entidad (`Producto`), implementar una relación Muchos a Muchos (`N:N`) entre Pedidos y Productos, y utilizar Spring Data JPA para realizar operaciones CRUD y consultas que involucren estas relaciones.

## Escenario del Ejercicio: Clientes, Pedidos y Productos

Continuaremos con su proyecto de Clientes y Pedidos. Ahora, cada Pedido estará compuesto por uno o varios Productos, y un Producto puede estar incluido en múltiples Pedidos a lo largo del tiempo.

### Requerimientos Adicionales

- Guardar información de **Productos**:
  - Cada producto debe tener un identificador único, un nombre, un precio y una descripción opcional.
- Relación Muchos a Muchos (`N:N`) entre **Pedidos** y **Productos**:
  - Un Pedido contiene varios Productos.
  - Un Producto puede estar en varios Pedidos.
- Añadir validaciones apropiadas a la nueva entidad `Producto`.
- Implementar operaciones para:
  - Crear y gestionar Productos (CRUD).
  - Obtener todos los Productos que están incluidos en un Pedido específico.
  - Buscar Productos dentro de un rango de precio.
  - *(Opcional / Desafío Adicional)*: Modelar la cantidad de cada producto dentro de un pedido específico. Esto requiere una tabla intermedia con un atributo extra.

## Pasos a Seguir para Extender su Proyecto

### Paso 1: Actualizar el Esquema de la Base de Datos en PostgreSQL

- **Tu Tarea**: Utilicen pgAdmin o DBeaver para modificar su base de datos `clientes_pedidos_db`.
  - Creen la tabla `productos`. Asegúrense de definir su clave primaria (`BIGSERIAL PRIMARY KEY`) y otras columnas (`nombre VARCHAR NOT NULL UNIQUE`, `precio NUMERIC NOT NULL CHECK (precio > 0)`, `descripcion TEXT`).
  - Creen la tabla de enlace (o tabla intermedia) para la relación N:N entre pedidos y productos. Nómbrala `pedido_producto` (o similar). Esta tabla típicamente tendrá dos columnas que son claves foráneas, referenciando la clave primaria de la tabla `pedidos` y la clave primaria de la tabla `productos`. La combinación de estas dos columnas foráneas a menudo actúa como la clave primaria compuesta de la tabla de enlace.
    - Ejemplo de columnas: `pedido_id` (BIGINT REFERENCES pedidos(id)), `producto_id` (BIGINT REFERENCES productos(id)).
  - *(Opcional / Desafío)*: Si van a modelar la cantidad, añadan una columna `cantidad INT NOT NULL CHECK (cantidad > 0)` a la tabla `pedido_producto`. Si hacen esto, la clave primaria de la tabla de enlace podría ser solo un ID `BIGSERIAL` propio, o la combinación `(pedido_id, producto_id)`.
  - **Importante**: Ejecuten las sentencias `CREATE TABLE` para crear las nuevas tablas y definir las claves foráneas.

- **Cómo Cursor AI puede ayudar**:
  - Descríbanle las tablas `productos` y `pedido_producto` con sus columnas y relaciones y pídanle las sentencias SQL `CREATE TABLE` correspondientes. Por ejemplo: "Generate the SQL CREATE TABLE statements for a 'products' table and a linking table 'order_product' for a Many-to-Many relationship between 'orders' and 'products'."
  - Si tienen dudas sobre cómo definir claves foráneas o una clave primaria compuesta en SQL, pregunten a la IA.

#### Paso 2: Actualizar las Entidades JPA en su Proyecto Spring Boot

- **Tu Tarea**: Vuelvan a su proyecto Spring Boot en Cursor AI.
  - Creen la clase `Producto.java` en su paquete `model`. Anótenla con `@Entity` y mapeen sus atributos (`@Id`, `@GeneratedValue`, `@Column`) a las columnas de la tabla `productos` que crearon en el Paso 1.
  - **Añadan validaciones** a la clase `Producto` usando anotaciones de `jakarta.validation` (`@NotBlank`, `@Min`, etc.) para los campos requeridos.
  - **Mapeen la relación N:N**: Modifiquen las clases `Pedido.java` y `Producto.java`.
    - En la clase `Pedido`, añadan un atributo que sea una colección (`List` o `Set`) de `Producto`s. Anótenlo con `@ManyToMany`.
    - En la clase `Producto`, añadan un atributo que sea una colección de `Pedido`s. Anótenlo con `@ManyToMany` y usen el atributo `mappedBy` para referenciar el nombre del atributo en la clase `Pedido` que mapea esta relación.
    - En el lado "propietario" de la relación (generalmente el lado donde les resulte más natural gestionar la asociación, o el lado sin `mappedBy` si la relación es bidireccional), usen `@JoinTable` para especificar el nombre de la tabla de enlace (`pedido_producto`) y los nombres de las columnas de claves foráneas en esa tabla de enlace que apuntan a esta entidad (`joinColumns`) y a la otra entidad (`inverseJoinColumns`).
    - Consideren qué `CascadeType` es apropiado (ej. si eliminan un Pedido, ¿quieren que se eliminen los productos asociados en la tabla de enlace? `REMOVE` o `ALL`).
    - Añadan `@Valid` a la colección de `Producto`s en la entidad `Pedido` (si usan cascade para `PERSIST` o `MERGE`) para validar los productos al guardar el pedido.
  - *(Opcional / Desafío)*: Si modelaron la cantidad en la tabla de enlace, necesitarán crear una entidad separada (ej. `PedidoProducto`) que mapee la tabla de enlace, con una clave primaria compuesta (usando `@Embeddable` y `@EmbeddedId`) y los atributos para las claves foráneas y la cantidad. Luego, la relación entre `Pedido` y `PedidoProducto` será 1:N, y entre `Producto` y `PedidoProducto` también será 1:N.

- **Cómo Cursor AI puede ayudar**:
  - Pídanle que les genere la clase `@Entity` para la tabla `productos` con sus anotaciones de validación.
  - Descríbanle la relación N:N entre `Pedido` y `Producto` (incluyendo el nombre de la tabla de enlace y los nombres de las columnas FK) y pídanle que añada las anotaciones `@ManyToMany` y `@JoinTable` correctas en ambas entidades. Por ejemplo: "Add a Many-to-Many relationship between the Pedido and Producto entities, using the 'pedido_producto' linking table with 'pedido_id' and 'producto_id' foreign key columns."
  - Si implementan el desafío de la cantidad, pídanle ayuda para crear la entidad intermedia con clave primaria compuesta (`@Embeddable`, `@EmbeddedId`).
- Pídanle que explique el significado de cualquier atributo de relación (`mappedBy`, `cascade`, `fetch`, `@JoinTable`, `joinColumns`, `inverseJoinColumns`) en el contexto de su código.

#### Paso 3: Actualizar Repositorios

- **Tu Tarea**:
  - Creen una interfaz `ProductoRepository` en su paquete `repository` extendiendo `JpaRepository<Producto, Long>`.
  - Añadan métodos de consulta derivados del nombre a `ProductoRepository` para buscar productos (ej. `findByNombreContainingIgnoreCase`, `findByPrecioBetween`).
  - *(Opcional)* Si su diseño de N:N lo requiere (especialmente con el desafío de la cantidad), podrían necesitar un Repository para la entidad de enlace (`PedidoProductoRepository`).

- Cómo Cursor AI puede ayudar:
  - Pídanle que les genere la interfaz `ProductoRepository`.
  - Descríbanle las consultas que necesitan (ej. buscar productos por rango de precio) y pídanle que les genere la firma del método correspondiente en ProductoRepository usando convenciones de Spring Data JPA.

#### Paso 4: Actualizar la Capa de Servicio

- **Tu Tarea**:
  - Inyecten el nuevo `ProductoRepository` en su `PedidoService` o en un nuevo `ProductoService` (si deciden crear uno).
  - Implementen la lógica de negocio para las nuevas funcionalidades:
  - Métodos CRUD para `Producto`s (delegando al `ProductoRepository`).
  - Un método en `PedidoService` para asociar productos a un pedido existente (esto implicará recuperar el pedido y los productos, añadirlos a las colecciones en ambos lados de la relación, y guardar el pedido).
  - Un método en `PedidoService` o un nuevo servicio que, dado un ID de pedido, recupere el pedido con sus productos asociados (tengan en cuenta `fetch`).
  - Un método en su servicio (o un nuevo servicio) para buscar productos por rango de precio (delegando al `ProductoRepository`).
  - (Opcional / Desafío): Implementen la lógica para calcular el total del pedido si modelaron la cantidad.

- Cómo Cursor AI puede ayudar:
  - Pídanle ayuda para inyectar `ProductoRepository` en su servicio.
  - Descríbanle la funcionalidad que necesitan implementar (ej. "Write the service method to add a list of Products to an existing Order") y pídanle el código. Recuérdenle la relación N:N y cómo manejar ambos lados de la relación en el código Java.
  - Pídanle que les explique cómo manejar la carga de relaciones (`fetch`) cuando recuperan entidades.

#### Paso 5: Actualizar la Capa de Controlador

- **Tu Tarea**:
  - Añadan nuevos endpoints a su `PedidoController` o creen un nuevo `ProductoController` para exponer las nuevas funcionalidades.
  - Ejemplos: `POST /api/productos` (para crear productos), `GET /api/productos/{id}`, `GET /api/productos/buscar` (para buscar por precio), `GET /api/pedidos/{id}/productos` (para obtener productos de un pedido).
- Asegúrense de usar `@Valid` en los parámetros `@RequestBody` para las entidades `Producto` y `Pedido` (la validación en cascada de los productos en el pedido se activará si configuraron `@Valid` en la colección en la entidad `Pedido`).

- Cómo Cursor AI puede ayudar:
  - Pídanle que les genere el esqueleto de nuevos métodos de controlador para las rutas que necesitan (ej. "Generate a Spring Boot RestController method for a GET request to /api/productos/buscar that accepts minPrice and maxPrice as query parameters").
  - Asegúrense de usar `@Valid` en los métodos `POST`/`PUT` y pídanle a la IA que lo revise si no están seguros.

#### Paso 6: Actualizar la Documentación (OpenAPI/Swagger)

- **Tu Tarea**:
  - Añadan o actualicen las anotaciones de Swagger (`@Tag`, `@Operation`, `@Parameter`, `@RequestBody`, `@ApiResponses`, `@Schema`) en la nueva entidad Producto y en los nuevos endpoints del controlador.
  - Asegúrense de que las relaciones (la lista de productos en un pedido, o la lista de pedidos de un producto, dependiendo de cómo la expongan) se muestren correctamente en el Swagger UI.

- Cómo Cursor AI puede ayudar:
  - Pídanle que les ayude a añadir anotaciones de Swagger a la clase `Producto`.
  - Pídanle que les sugiera anotaciones `@Operation`, `@Parameter`, `@RequestBody`, `@ApiResponses` para los nuevos métodos del controlador, basándose en lo que hacen.

#### Paso 7: Probar a Fondo

- **Tu Tarea**: ¡La prueba es clave! Utilicen su cliente REST (o Swagger UI) para probar todos los endpoints nuevos y modificados.
  - Prueben crear Clientes, Pedidos y Productos.
  - Prueben asociar Productos a Pedidos (esto implica crear los objetos en Java y guardarlos, manejando la colección relacionada).
  - Prueben recuperar Pedidos y verificar que los Clientes y Productos relacionados se cargan (observen los logs si tienen `show-sql` activado para ver las consultas generadas por JPA).
  - Prueben recuperar Clientes y verificar sus Pedidos.
  - Prueben la búsqueda de Productos por precio.
  - Prueben las validaciones: Intenten crear/actualizar Clientes, Pedidos o Productos con datos inválidos (vacíos, negativos, etc.) y vean si reciben el error 400 de validación esperado.
  - Prueben la integridad referencial de la base de datos: Intenten crear un Pedido con un `cliente_id` que no existe en la tabla `clientes` (directamente en SQL o intentando guardar en Java si no manejan la relación por objetos). Intenten eliminar un Cliente que tiene Pedidos asociados. Observen los errores que da la base de datos (y cómo se manifiestan en su aplicación).

#### Paso 8: Refactorizar y Pulir

- **Tu Tarea**: Revisen su código completo. ¿Está limpio? ¿Los nombres son claros? ¿La separación de capas es correcta? ¿Siguen aplicando SOLID?
- Cómo Cursor AI puede ayudar:
  - Pídanle a Cursor AI un análisis general de sus clases y métodos.
  - Pídanle sugerencias de refactorización.
  - Pregúntenle si su diseño de entidades y relaciones aplica bien los principios SOLID, especialmente SRP y DIP.

## Entrega (Opcional, según las indicaciones del tutor)

Deberán actualizar su repositorio de GitHub con el código del proyecto extendido. Asegúrense de que el README refleje las nuevas funcionalidades y cómo probarlas. Estén listos para demostrar su API extendida y explicar cómo implementaron las relaciones y consultas.

¡Este ejercicio les desafiará a integrar muchos conceptos importantes! Enfóquense en entender cómo las relaciones se mapean y gestionan en JPA. ¡Utilicen Cursor AI como su guía!
