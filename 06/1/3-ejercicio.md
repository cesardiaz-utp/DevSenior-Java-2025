# Ejercicio Autónomo: Modelado, Creación de Tablas y Normalización de Datos

¡Hola!

Es hora de poner manos a la obra y practicar lo que vimos en la clase y la tutoría sobre bases de datos relacionales y PostgreSQL. Este ejercicio les permitirá consolidar el modelado, la creación de tablas, la normalización de datos y la creación del esquema de base de datos.

## Objetivo del Ejercicio:

Diseñar un esquema de base de datos relacional más complejo, aplicando principios de normalización, y crearlo físicamente en PostgreSQL.

## Escenario del Ejercicio: Sistema de Gestión de Clientes, Pedidos y Productos

Imaginen que necesitan una base de datos para registrar clientes, los pedidos que realizan y los productos que se venden.

### Requerimientos:

* Queremos guardar información de **Clientes**. Cada cliente debe tener un identificador único, nombre completo, dirección de correo electrónico (que debe ser única), número de teléfono y la fecha en que se registró en el sistema.
* Queremos guardar información de **Pedidos**. Cada pedido debe tener un identificador único, la fecha en que se realizó, el monto total y el estado del pedido (por ejemplo: "pendiente", "enviado", "entregado").
* Queremos guardar información de **Productos**. Cada producto debe tener un identificador único, nombre, descripción, precio unitario y cantidad en inventario.
* Un **Cliente puede tener muchos Pedidos**, pero cada **Pedido pertenece a un único Cliente**.
* Un **Pedido puede incluir múltiples Productos**, y un **Producto puede estar en múltiples Pedidos**. Además, cada producto en un pedido debe registrar la cantidad solicitada.

## Pasos a Seguir:

Este ejercicio es para que lo desarrollen de forma autónoma, aplicando lo que aprendieron. ¡Utilicen Cursor AI como su asistente en cada paso!

### Paso 1: Diseñar el Esquema Relacional (Clientes, Pedidos y Productos)

* **Tu Tarea**: Basándose en los requerimientos, identifiquen las entidades principales, sus atributos y las relaciones entre ellas.
    * **Entidades**: ¿Cuáles son las "cosas" sobre las que guardaremos información?
    * **Atributos**: ¿Qué datos guardaremos de cada Cliente, Pedido y Producto?
    * **Relaciones**: ¿Qué tipo de relaciones existen entre Clientes, Pedidos y Productos (1 a 1, 1 a N, N a N)? ¿Quién "tiene" al otro en términos de base de datos?
    * **Claves**: Definan la clave primaria para cada tabla. ¿Necesitan claves foráneas para las relaciones? ¿En qué tabla deben ir?
    * **Tipos de Datos y Restricciones**: Para cada atributo, elijan un tipo de dato adecuado en PostgreSQL (`VARCHAR`, `INT`, `BIGINT`, `DATE`, `NUMERIC`, etc.). Definan restricciones importantes como `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, `UNIQUE`.
* Cómo Cursor AI puede ayudar:
    * Pueden describirle a Cursor AI el escenario y pedirle una sugerencia de diseño de tablas: "Suggest a relational database schema (tables, columns, relationships) for a customer, order, and product management system".
    * Si tienen dudas sobre qué tipo de dato usar o qué restricción es apropiada para un campo, pregúntenle a la IA.

### Paso 2: Aplicar Normalización de Datos

* **Tu Tarea**: Asegúrense de que el diseño de su base de datos cumple con las tres primeras formas normales (1FN, 2FN, 3FN):
    * **Primera Forma Normal (1FN)**: Cada columna debe contener valores atómicos y cada fila debe ser única.
    * **Segunda Forma Normal (2FN)**: Todos los atributos no clave deben depender completamente de la clave primaria.
    * **Tercera Forma Normal (3FN)**: No debe haber dependencias transitivas entre atributos no clave.
* Cómo Cursor AI puede ayudar:
    * Describan su diseño a Cursor AI y pídanle que revise si cumple con las formas normales: "Does this database schema comply with 1NF, 2NF, and 3NF?".
    * Si encuentran problemas de normalización, pidan sugerencias para corregirlos.

### Paso 3: Crear la Base de Datos y Tablas en PostgreSQL

* **Tu Tarea**: Utilicen pgAdmin o DBeaver para conectarse a su servidor local de PostgreSQL.
    * Creen una nueva base de datos (ej. `gestion_clientes_pedidos_productos_db`).
    * Escriban las sentencias SQL `CREATE TABLE` para crear las tablas `clientes`, `pedidos`, `productos` y una tabla intermedia para la relación muchos a muchos entre `pedidos` y `productos` (por ejemplo, `detalle_pedidos`).
    * Asegúrense de definir correctamente las **claves primarias** y las **claves foráneas** que establecen las relaciones entre las tablas.
    * Ejecuten las sentencias SQL para crear las tablas. Verifiquen en el árbol de objetos de pgAdmin/DBeaver que las tablas se crearon correctamente con sus columnas y restricciones.

* Cómo Cursor AI puede ayudar:
    * Una vez que tengan su diseño claro (nombre de tablas, columnas, tipos, restricciones), pídanle a Cursor AI que les genere las sentencias `CREATE TABLE` correspondientes. Por ejemplo: "Generate the SQL CREATE TABLE statement for a 'products' table with id (primary key autogenerated), name (not null), description, price (not null), and stock_quantity (not null)". Y luego para las demás tablas, asegurándose de incluir las claves foráneas.
    * Si les da un error al ejecutar el SQL, péguenlo en el chat de Cursor AI y pidan ayuda para corregirlo: "I'm getting this error when executing my CREATE TABLE statement: [pegar mensaje de error]. What is wrong?".

### Paso 4: Verificar el Diseño y la Implementación

* **Tu Tarea**: Antes de terminar, hagan una revisión.
    * Comparen sus sentencias `CREATE TABLE` con los requerimientos iniciales. ¿Cada columna tiene un propósito claro? ¿Las claves primarias y foráneas están correctamente definidas? ¿Los tipos de datos son apropiados?
    * Reflexionen sobre su diseño: ¿Cumple con los requerimientos? ¿Las tablas están bien definidas y normalizadas?
* Cómo Cursor AI puede ayudar:
    * Pueden usar el chat de AI para hacer una comparación cruzada. Peguen su sentencia `CREATE TABLE` y pregunten: "Does this database schema meet the requirements and follow best practices?".

## Entrega (Opcional, según las indicaciones del tutor):
* Pueden subir las sentencias SQL `CREATE TABLE` a un archivo en un repositorio en GitHub (ej. `schema.sql`).
* Asegúrense de que las tablas se crearon correctamente en PostgreSQL y que cumplen con los requerimientos del ejercicio.

Este ejercicio les dará una base sólida en el diseño y creación de bases de datos relacionales, aplicando principios de normalización y buenas prácticas.

¡Mucho éxito y disfruten practicando con su asistente de IA!