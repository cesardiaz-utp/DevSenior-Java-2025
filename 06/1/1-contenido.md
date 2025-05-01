# Introducción a PostgreSQL y Conexión desde Java

¡Hola a todos!

Bienvenidos al emocionante mundo de la persistencia de datos. Guardar información de forma permanente es esencial para la mayoría de las aplicaciones que construimos. En esta clase, nos centraremos en las bases de datos relacionales, un pilar del desarrollo de software, y específicamente en **PostgreSQL**, un sistema de base de datos muy robusto y popular. También daremos los primeros pasos para conectar nuestras aplicaciones **Spring Boot** a una base de datos.

## ¿Qué es una Base de Datos Relacional? Características de PostgreSQL
### Concepto Fundamental: El Modelo Relacional

Una base de datos relacional organiza los datos en **tablas**. Piensen en cada tabla como una hoja de cálculo.

* Cada tabla tiene **columnas** (también llamadas campos o atributos) que definen el tipo de datos que se almacenará (ej. `nombre`, `edad`, `fecha_creacion`).
* Cada fila (también llamada registro o tupla) en la tabla contiene un conjunto de valores para esas columnas, representando una instancia individual (ej. una persona, un producto específico).
* **Lo más importante**: las bases de datos relacionales permiten definir relaciones entre tablas. Esto se hace típicamente usando claves primarias (que identifican únicamente una fila dentro de su tabla) y claves foráneas (que son columnas en una tabla que referencian la clave primaria de otra tabla).

### Rol en Aplicaciones Empresariales: Robustez y Estructura

Las bases de datos relacionales han sido el estándar por décadas por buenas razones:

* **Integridad de Datos**: Garantizan que los datos sean precisos y consistentes mediante esquemas (la definición de tablas y columnas) y restricciones (reglas que los datos deben cumplir).
* **Transacciones ACID**: Soportan transacciones que son Atómicas (todo o nada), Consistentes (mantienen la integridad), Aisladas (no interfieren entre sí) y Durables (los cambios persisten). Esto es vital para operaciones críticas (ej. transferencias bancarias).
* **Datos Estructurados**: Son ideales para manejar datos que tienen una estructura clara y predecible, y donde las relaciones entre diferentes tipos de datos son cruciales (ej. clientes que realizan pedidos, productos que pertenecen a categorías).
* **Lenguaje Estándar (SQL)**: Se interactúa con ellas usando SQL (Structured Query Language), un lenguaje potente y estandarizado para consultar y manipular datos.

### PostgreSQL: Un Gigante de Código Abierto

PostgreSQL es uno de los sistemas de gestión de bases de datos relacionales (RDBMS) más avanzados, potentes y respetados que existen.

* Es de **código abierto** y gratuito, pero con capacidades a la par o superiores a muchos sistemas comerciales.
* Alto cumplimiento del **estándar SQL**. Lo que aprendan de SQL aquí les servirá en otras bases de datos.
* **Extensibilidad**: Permite añadir nuevos tipos de datos, funciones, etc.
* Soporte para **tipos de datos avanzados**: Puede manejar texto plano, números, fechas, booleanos, pero también tipos más complejos como JSON, arrays, rangos, tipos geométricos, etc.
* **Concurrencia**: Maneja múltiples usuarios y transacciones de forma eficiente y segura.
* Es una elección sólida y fiable para casi cualquier tipo de aplicación.

## Instalación, Configuración y Uso de Herramientas (pgAdmin o DBeaver)

### Instalación de PostgreSQL:

El primer paso es instalar el servidor de base de datos en su computadora.

* **Windows/macOS**: La forma más sencilla es descargar el instalador oficial desde el sitio web de [PostgreSQL](https://www.postgresql.org/download/). El instalador los guiará en el proceso. Durante la instalación, les pedirá configurar la contraseña para el usuario `postgres` (el superusuario). ¡Guarden bien esta contraseña!
* **Linux**: Generalmente se instala a través del gestor de paquetes de su distribución (ej. sudo apt update && sudo apt install postgresql postgresql-contrib en sistemas basados en Debian/Ubuntu).
* **Actividad**: Asegúrense de tener PostgreSQL instalado y corriendo localmente antes o al inicio de la clase práctica.

### Herramientas Gráficas de Administración:

Interactuar directamente con la línea de comandos puede ser tedioso al principio. Las herramientas gráficas facilitan mucho la administración y consulta.

* **pgAdmin**: Es la herramienta gráfica oficial y más común para PostgreSQL. Permite crear bases de datos, diseñar tablas visualmente o mediante código SQL, ejecutar consultas, ver datos, monitorear el servidor, etc. Descarguen la versión 4 o superior.
* **DBeaver**: Es una herramienta universal que sirve para muchas bases de datos, incluyendo PostgreSQL. Si ya la usan para otra base de datos, pueden seguir usándola. Es muy potente y rica en funcionalidades.
* **Actividad**: Instalen pgAdmin o DBeaver y practiquen conectando a su servidor local de PostgreSQL (generalmente `localhost` o `127.0.0.1`, puerto `5432`, usuario `postgres`, con la contraseña que configuraron). Asegúrense de abrir la herramienta, crear una nueva conexión y verificar que pueden acceder a la base de datos.

## Creación de Esquemas, Tablas, Tipos de Datos y Restricciones (SQL)

Vamos a aprender el SQL básico para definir la estructura de nuestra base de datos.

### Concepto de Esquema:

* Piensen en una base de datos como un gran contenedor. Dentro de ese contenedor, pueden tener múltiples "esquemas".
* Un esquema es como una carpeta o un namespace que agrupa tablas, vistas, etc. El esquema por defecto, donde usualmente trabajamos al inicio, se llama `public`.
* Puedes crear esquemas para organizar objetos lógicos de tu aplicación. `CREATE SCHEMA nombre_esquema;`

### Creación de Tablas: La Sentencia `CREATE TABLE`

Así definimos la estructura de una tabla usando SQL. Si tienen dudas sobre la sintaxis, pueden usar Cursor AI para generar ejemplos personalizados o resolver dudas específicas.

```sql
CREATE TABLE nombre_tabla (
    nombre_columna1 TipoDeDato [Restricciones],
    nombre_columna2 TipoDeDato [Restricciones],
    -- ... más columnas ...
    [Restricciones a nivel de tabla]
);
```

### Tipos de Datos Comunes en PostgreSQL:

Al definir una columna, deben especificar qué tipo de datos contendrá:

* `INT`, `INTEGER`: Números enteros.
* `BIGINT`: Números enteros grandes.
* `VARCHAR(n)`: Texto con un límite máximo de n caracteres (ej. `VARCHAR(100)`).
* `TEXT`: Texto sin un límite de longitud práctico.
* `BOOLEAN`: `TRUE` o `FALSE`.
* `DATE`: Solo fecha (ej. '2023-10-27').
* `TIME`: Solo hora.
* `TIMESTAMP`: Fecha y hora.
* `NUMERIC(p, s)`, `DECIMAL(p, s)`: Números decimales con precisión exacta. `p` es la precisión total (número de dígitos), `s` es la escala (número de dígitos después del punto decimal).
* `SERIAL`: Un pseudotipo que crea una secuencia automáticamente y usa su próximo valor como valor por defecto para la columna. Ideal para claves primarias autogeneradas. Es equivalente a crear una secuencia y usar `DEFAULT nextval('nombre_secuencia')`.
    * `SMALLSERIAL`, `BIGSERIAL`: Versiones para números más pequeños o más grandes.

### Restricciones (Constraints): Reglas para la Integridad de Datos

Las restricciones son vitales para asegurar que los datos en sus tablas sean válidos y se relacionen correctamente.

* `PRIMARY KEY`:
    * Identifica de forma única cada fila en la tabla.
    * Solo puede haber una clave primaria por tabla.
    * No permite valores `NULL`.
    * A menudo se usa en una columna `SERIAL` o `BIGSERIAL`.
    * Ejemplo: `id BIGSERIAL PRIMARY KEY`
* `FOREIGN KEY`:
    * Establece y mantiene vínculos entre tablas.
    * Una columna (o grupo de columnas) en una tabla referencia la clave primaria (o única) en otra tabla.
    * Asegura la **integridad referencial** (no puedes tener un pedido para un cliente que no existe).
    * Ejemplo: `id_cliente BIGINT REFERENCES clientes(id)`
* `NOT NULL`:
    * Asegura que una columna debe tener un valor. No puede ser `NULL`.
    * Ejemplo: `nombre VARCHAR(100) NOT NULL`
* `UNIQUE`:
    * Asegura que todos los valores en una columna o grupo de columnas son únicos dentro de la tabla.
    * A diferencia de `PRIMARY KEY`, puede permitir valores `NULL` (si la columna no es también `NOT NULL`).
    * Ejemplo: `email VARCHAR(255) UNIQUE` (para asegurar que cada usuario tiene un email único).
* `CHECK`:
    * Permite definir una condición que cada fila debe cumplir para poder ser insertada o actualizada.
    * Ejemplo: `precio NUMERIC(10, 2) CHECK (precio > 0)`

### Actividad/Demostración:

* Abran pgAdmin o DBeaver y conéctense a su base de datos `clase_db`.
* Abran una ventana de consulta (Query Tool).
* Escriban y ejecuten las sentencias `CREATE TABLE` para crear una tabla simple `productos` con al menos:
    * `id` (BIGSERIAL PRIMARY KEY)
    * `nombre` (VARCHAR(255) NOT NULL)
    * `precio` (NUMERIC(10, 2) CHECK (precio > 0))
* Si tienen dudas sobre cómo escribir estas sentencias, pueden usar Cursor AI para generar ejemplos específicos. Por ejemplo: "Generate SQL to create a table 'orders' with id (primary key), order_date (datetime not null), and customer_id (foreign key to customers table)".
* (Si tienen tiempo) Creen otra tabla `categorias` con `id` (SERIAL PRIMARY KEY) y `nombre` (VARCHAR NOT NULL UNIQUE), y luego modifiquen la tabla `productos` para añadir una clave foránea `id_categoria` referenciando `categorias(id)`.
* **Nota**: Si están usando pgAdmin, asegúrense de guardar los cambios y verificar que las tablas aparecen en el esquema `public`. Si están usando DBeaver, pueden refrescar la vista del esquema para confirmar que las tablas se han creado correctamente.

## Conceptos de Normalización y Modelado Lógico de Datos
Antes de saltar a crear tablas, es bueno pensar en el diseño.

### Modelado Lógico de Datos:

* Es el proceso de diseñar la estructura de los datos sin pensar en la base de datos específica que usarán (PostgreSQL, MySQL, etc.).
* Identifican las **entidades** (cosas importantes: Clientes, Productos, Pedidos).
* Definen los **atributos** de cada entidad (Cliente tiene nombre, dirección, email).
* Identifican las **relaciones** entre entidades (Un Cliente puede tener muchos Pedidos; Un Pedido contiene muchos Productos).
* Pueden representar esto con diagramas (Diagramas Entidad-Relación - ER).

### Normalización: Organizando los Datos Correctamente

La normalización es un conjunto de reglas para estructurar las tablas y columnas para:

* Reducir la **redundancia de datos** (evitar guardar la misma información múltiples veces).
* Mejorar la **integridad de los datos** (asegurar que los datos son consistentes).
* Facilitar la **modificación** de los datos (evitar que actualizar un dato en un lugar requiera actualizarlo en muchos otros).
* Evitar **anomalías** al insertar, actualizar o eliminar datos.

Las formas normales son niveles de normalización (1FN, 2FN, 3FN son las más comunes para empezar).

* **1ª Forma Normal (1FN)**: Asegura que cada columna contenga valores atómicos (indivisibles) y no haya grupos repetidos de columnas en la misma tabla. (Ej: En lugar de tener `Telefono1`, `Telefono2`, `Telefono3` en la tabla `clientes`, se crea una tabla separada `telefonos` relacionada con `clientes`).
* **2ª Forma Normal (2FN)**: Si la clave primaria es compuesta (varias columnas), las columnas no clave deben depender de toda la clave primaria, no solo de una parte.
* **3ª Forma Normal (3FN)**: Las columnas no clave no deben depender de otras columnas no clave (evitar dependencias transitivas). (Ej: Si en la tabla `pedidos` tienes `id_producto`, `nombre_producto` y `precio_producto`, `nombre_producto` y `precio_producto` dependen de `id_producto`, no de `id_pedido`. Esto sugiere que `nombre_producto` y `precio_producto` deben estar en la tabla `productos`, no en `pedidos`).

La normalización es un proceso para refinar su diseño de tablas. Apunten a la 3FN en la mayoría de los casos.

## Conexión desde Java usando Spring Boot + Spring Data JPA

Ahora, ¿cómo hacemos que nuestra aplicación Java "hable" con la base de datos que acabamos de aprender a configurar?

### El Rol de Spring Boot y Spring Data JPA:
* **Spring Boot**: Simplifica radicalmente la configuración de la conexión a bases de datos gracias a su autoconfiguración. Solo necesitan poner las propiedades correctas.
* **Spring Data JPA**: Es una parte del gran proyecto Spring Data. Proporciona una capa de abstracción sobre el estándar JPA (Java Persistence API), que a su vez es una especificación para el mapeo Objeto-Relacional (ORM). JPA/Hibernate se encargan de traducir las operaciones con objetos Java a sentencias SQL y viceversa. Spring Data JPA simplifica aún más JPA, permitiéndoles hacer muchas operaciones de base de datos sin escribir código de implementación para los repositorios.

## Configuración del archivo `application.properties` y Dependencias Necesarias

### Dependencias en su Proyecto Spring Boot:

* Para usar Spring Data JPA con PostgreSQL, necesitan añadir las siguientes dependencias a su archivo de configuración de dependencias (`pom.xml` para Maven o `build.gradle` para Gradle):
    * **Spring Boot Starter Data JPA**: Proporciona todas las clases y configuraciones necesarias para usar Spring Data JPA, incluyendo una implementación de JPA (Hibernate por defecto).
        * Maven:
        ```XML
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        ```
        * Gradle:
        ```Gradle
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        ```
    * **Driver JDBC de PostgreSQL**: El software necesario para que Java se comunique específicamente con bases de datos PostgreSQL.
        * Maven:
        ```XML
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>
        ```
        * Gradle:
        ```Gradle
        runtimeOnly 'org.postgresql:postgresql'
        ```
* **Actividad**: Asegúrense de añadir estas dependencias a su proyecto Spring Boot y recarguen su proyecto en Cursor AI para que se descarguen.

### Configuración en `src/main/resources/application.properties`:

* Este archivo es el lugar central para configurar propiedades de Spring Boot. Aquí le decimos dónde está la base de datos, cómo conectarse, etc.
* Propiedades clave para la conexión:
    * `spring.datasource.url`: La URL de conexión JDBC. Sigue un formato `jdbc:nombre_db://host:puerto/nombre_base_datos`.
        * Ejemplo (para base de datos local `clase_db`): `spring.datasource.url=jdbc:postgresql://localhost:5432/clase_db`
    * `spring.datasource.username`: El nombre de usuario para autenticarse en la base de datos.
        * Ejemplo: `spring.datasource.username=postgres`
    * `spring.datasource.password`: La contraseña para autenticarse en la base de datos.
        * Ejemplo: `spring.datasource.password=su_contraseña_segura`
* Propiedades de JPA/Hibernate (opcionales pero útiles):
    * `spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect`: Le dice a Hibernate que use el dialecto específico de PostgreSQL para generar SQL optimizado.
    * `spring.jpa.hibernate.ddl-auto=opción`: **¡Importante!** Controla cómo Hibernate interactúa con el esquema de la base de datos al iniciar la aplicación.
        * `none`: No hace nada. No modifica el esquema. Lo más seguro para producción.
        * `update`: Intenta actualizar el esquema de la base de datos para que coincida con las entidades JPA definidas en su código. Puede añadir columnas, tablas, pero rara vez elimina algo. Útil en desarrollo, pero úsenlo con precaución.
        * `create-drop`: Elimina el esquema de la base de datos (si existe), lo crea de nuevo basándose en sus entidades y lo elimina al detener la aplicación. Muy útil para pruebas o prototipos rápidos donde no les importa perder los datos.
        * **Para esta clase y práctica inicial, pueden usar `update` o `create-drop` si quieren que Spring cree las tablas por ustedes. Para proyectos más serios donde controlan la DB por separado, usarían `none`.**
        * Ejemplo: `spring.jpa.hibernate.ddl-auto=update`
    * `spring.jpa.show-sql=true`: Si se establece a `true`, Hibernate imprimirá en la consola todas las sentencias SQL que ejecuta. ¡Es muy útil para entender qué está pasando y depurar!
    * `spring.jpa.properties.hibernate.format_sql=true`: Si `show-sql` es `true`, esto formatea el SQL para que sea más fácil de leer.
* **Actividad/Demostración**: Abren el archivo `src/main/resources/application.properties` en su proyecto Spring Boot y añaden estas propiedades, adaptando la URL, usuario, contraseña y la opción de `ddl-auto` según necesiten.
* **Asistencia de Cursor AI**: Pueden pedirle a Cursor AI que les genere el bloque de configuración para `application.properties` indicándole el tipo de base de datos, host, puerto, nombre de DB, usuario, contraseña y la opción deseada para `ddl-auto`.

## Prueba de Conexión con Cursor AI

Una vez que tienen las dependencias y la configuración en `application.properties`, ¿cómo saben si la conexión funciona?

### Verificación Automática al Iniciar Spring Boot:

* La forma más común y simple es simplemente ejecutar su aplicación Spring Boot. Spring Boot, al detectar la configuración de base de datos y las dependencias JPA, intentará automáticamente establecer una conexión al inicio.
* **Si la conexión tiene éxito**: Verán mensajes en la consola de inicio indicando que se está configurando el `DataSource` (HikariCP por defecto) y que Hibernate se está inicializando. La aplicación iniciará normalmente.
* **Si hay un error de conexión**: La aplicación probablemente no iniciará completamente y verán mensajes de error claros en la consola indicando el problema (ej. "Connection refused", "FATAL: password authentication failed", "database 'nombre_db' does not exist").

### Leyendo los Logs de Inicio:

* Acostúmbrense a leer los logs de inicio. Son la primera señal de problemas de configuración. Busquen líneas que mencionen `DataSource`, `HikariCP`, `Hibernate`, `JPA`, `EntityManagerFactory`.

### Prueba Manual o Diagnóstico de Errores con IA:

* Si ven errores y no entienden por qué, o si quieren estar seguros de que la conexión está activa después del inicio, pueden usar Cursor AI.
* **Actividad con Cursor AI (Diagnóstico de Errores)**: Si la aplicación no inicia o ven errores en la consola relacionados con la base de datos, copien los mensajes de error relevantes y péguenlos en el chat de Cursor AI. Pregúntenle: "Explain these Spring Boot logs related to the database connection error. What does this error mean and how can I fix it?". La IA a menudo les dará la causa probable y los pasos para solucionarlo (ej. revisar URL, usuario, contraseña, si el servidor de DB está corriendo).
* **Actividad con Cursor AI (Prueba Manual - Opcional)**: Si la aplicación inicia sin errores, pero quieren verificar programáticamente que el pool de conexiones está funcionando, pueden pedirle a Cursor AI que les ayude a escribir un pequeño componente o método simple para probar la conexión. Por ejemplo: "In Spring Boot, how can I write a simple piece of code to check if the database DataSource is properly configured and connected?".

### Actividad Final de Clase: 
Ejecutar su aplicación Spring Boot después de configurar `application.properties`. Observar la consola de inicio. Confirmar que no hay errores relacionados con la conexión a la base de datos, indicando que Spring Boot pudo conectar exitosamente a PostgreSQL.

## Conclusión

¡Excelente primer paso en la persistencia! Hoy entendimos el rol fundamental de las bases de datos relacionales y conocimos a PostgreSQL, aprendiendo a crear estructuras de datos básicas usando SQL. Lo más importante es que configuramos exitosamente nuestra aplicación Spring Boot para establecer una conexión con PostgreSQL utilizando las dependencias y propiedades adecuadas, y aprendimos a verificar esta conexión. En la próxima clase, veremos cómo podemos interactuar con estas tablas usando Spring Data JPA para guardar y recuperar datos sin escribir SQL directamente.

¡Sigan explorando PostgreSQL, practiquen la creación de tablas y asegúrense de que su conexión desde Spring Boot esté lista!