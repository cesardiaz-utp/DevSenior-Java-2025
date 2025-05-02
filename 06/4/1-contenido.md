# MongoDB – Introducción a NoSQL y Conexión con Spring Boot

¡Hola a todos!

Hemos pasado varias clases explorando el mundo de las bases de datos relacionales con PostgreSQL y Spring Data JPA. Hoy, vamos a abrir la puerta a un paradigma diferente: las bases de datos NoSQL. Conoceremos sus fundamentos, sus diferencias con las relacionales, y nos enfocaremos en MongoDB, una base de datos NoSQL muy popular y versátil, aprendiendo a integrarla en nuestras aplicaciones Spring Boot.

## 1. ¿Qué es MongoDB? Estructura Flexible, Documentos y Colecciones

### 1.1 Concepto de Bases de Datos NoSQL (Not Only SQL)

- Son bases de datos que se alejan del modelo relacional tradicional. No usan tablas con esquemas fijos, relaciones estrictas y SQL como lenguaje de consulta principal (aunque algunas tienen lenguajes similares).
- Surgieron para manejar escenarios que las bases de datos relacionales a veces no gestionan bien: grandes volúmenes de datos (Big Data), datos no estructurados o semi-estructurados, cambios frecuentes en la estructura de los datos, requisitos de escalabilidad horizontal masiva y alta disponibilidad.
- Tipos principales de Bases de Datos NoSQL:
  - **Clave-Valor** (Redis, DynamoDB): Datos almacenados como pares clave-valor simples.
  - **Orientadas a Documentos** (MongoDB, Couchbase): Almacenan datos en documentos, típicamente en formato JSON o BSON (JSON Binario). Los documentos pueden tener estructuras anidadas complejas.
  - **Columnares** (Cassandra, HBase): Almacenan datos en familias de columnas.
  - **Grafo** (Neo4j, ArangoDB): Optimizadas para almacenar y navegar relaciones entre entidades (nodos y aristas).

### 1.2 Introducción a MongoDB

- Es el sistema de gestión de bases de datos orientado a documentos más popular.
- **Estructura Flexible (Schema-less / Schema-on-read)**: Los documentos en una misma colección no tienen que tener la misma estructura o campos. Pueden añadir o eliminar campos a documentos individuales en cualquier momento. La estructura se "interpreta" al leer los datos.
- **Documentos**: Son la unidad básica de almacenamiento. Son similares a objetos JSON. Contienen campos y valores, que pueden ser tipos de datos básicos, arrays, u **otros documentos anidados** (permite representar datos jerárquicos o relaciones "embebidas").
- **Colecciones**: Son agrupaciones de documentos. Son el equivalente aproximado de las tablas en bases de datos relacionales, pero sin el esquema fijo.
- Ejemplo de Documento (JSON):

  ```JSON
  {
    "_id": ObjectId("..."), // ID único generado por MongoDB
    "nombre": "Laptop X1",
    "precio": 1200.50,
    "tags": ["electronica", "computadora", "portatil"],
    "detalles": { // Documento anidado
      "marca": "Lenovo",
      "procesador": "Intel i7"
    },
    "stock": 50
  }
  ```

  ```JSON
  {
    "_id": ObjectId("..."),
    "nombre": "Monitor Ultra",
    "precio": 350.00,
    "tags": ["electronica", "pantalla"],
    "stock": 20,
    "dimensiones": { "alto": 40, "ancho": 60 } // Este documento tiene un campo 'dimensiones' que el anterior no tiene
  }
  ```

## 2. Diferencias entre SQL y NoSQL (Modelo, Consultas, Escalabilidad)

Comprender estas diferencias es clave para saber cuándo usar cada tipo de base de datos.

| Característica | Bases de Datos Relacionales (SQL) | Bases de Datos NoSQL (Ej: MongoDB) |
| --- | --- | --- |
| **Modelo de Datos** | Tablas con esquema fijo (columnas predefinidas). Relaciones definidas por Claves Foráneas. | Documentos (JSON/BSON) con esquema flexible. Relaciones embebidas o referenciadas por ID. |
| **Integridad Datos** | Alta (ACID, esquemas, restricciones, FK). | Varía según el tipo. A menudo usan propiedades BASE (Básicamente Consistente). La consistencia se maneja más en la aplicación. |
| **Consultas** | SQL (lenguaje potente y estándar con JOINs complejos). | Lenguajes de consulta varían. Consultas por documento/campo. Agregación. JOINs entre colecciones menos comunes o diferentes. |
| **Escalabilidad** | Tradicionalmente Vertical (servidor más potente). Escalabilidad Horizontal (Sharding) compleja. | Diseñadas para Escalabilidad Horizontal (agregar más servidores). Distribución de datos (Sharding) más simple. |
| **Flexibilidad Esquema** | Esquema rígido. Cambios requieren ALTER TABLE (costoso). | Esquema flexible. Fácil añadir/eliminar campos en documentos individuales. |
| **Casos de Uso Típicos** | Datos muy estructurados, transacciones complejas, reportes analíticos, donde la integridad es crítica (finanzas, inventarios). | Datos semi-estructurados/no estructurados, Big Data, perfiles de usuario, catálogos, CMS, caches, IoT, donde la agilidad y escala son clave. |

## 3. Instalación de MongoDB Local o en Atlas (Cloud)

Necesitan acceso a un servidor de MongoDB para practicar.

### 3.1 Instalación Local (MongoDB Community Server)

- Pueden descargar el instalador para su sistema operativo desde el sitio oficial de [MongoDB](https://www.mongodb.com/try/download/community).
- Sigan las instrucciones para instalar el servidor.
- También es útil instalar [MongoDB Compass](https://www.mongodb.com/products/compass), una herramienta GUI oficial para interactuar con MongoDB (similar a pgAdmin para PostgreSQL).

### 3.2 MongoDB Atlas (Servicio en la Nube)

- Es la opción oficial de MongoDB como servicio gestionado en la nube (AWS, Azure, GCP).
- Es una forma más sencilla de empezar sin instalar nada localmente.
- Ofrecen un **nivel gratuito (free tier)** perfecto para aprender y desarrollar proyectos pequeños.
- **Actividad**: Vayan a [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) y creen una cuenta. Creen un _Free Cluster_. Configuren la seguridad (permitir conexiones desde su IP actual) y creen un usuario de base de datos. Obtendrán una cadena de conexión.

### 3.3 Actividad Práctica 1: Acceso a MongoDB

- **Su Tarea**: Decidan si instalan MongoDB Community Server localmente + Compass, O si crean un Free Cluster en MongoDB Atlas.
- **Su Tarea**: Una vez tengan el servidor/cluster listo, usen MongoDB Compass para conectarse a él. Creen una nueva base de datos (ej. `mi_base_nosql`). Dentro de esa base de datos, creen una colección (ej. `usuarios`). Inserten manualmente un documento simple de ejemplo en la colección `usuarios` usando Compass.
- **Verificación**: Confirmen que pueden conectarse a la base de datos y crear/ver documentos usando Compass.

## 4. Uso de spring-boot-starter-data-mongodb en un Proyecto

- **Spring Data MongoDB**: Al igual que Spring Data JPA simplificó el acceso a bases de datos relacionales, Spring Data MongoDB simplifica la interacción con MongoDB desde Spring Boot. Proporciona mapeo de objetos a documentos y la funcionalidad de Repositorios.

- **Dependencia Necesaria**: Para usar Spring Data MongoDB en su proyecto Spring Boot, añadan la siguiente dependencia:

  **Maven** (`pom.xml`):

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-mongodb</artifactId>
  </dependency>
  ```

  **Gradle** (`build.gradle`):

  ```gradle
  implementation 'org.springframework.boot:spring-boot-starter-data-mongodb'
  ```

- **Actividad**: Creen un nuevo proyecto Spring Boot (o usen uno existente limpio) y añadan esta dependencia. Recarguen el proyecto en Cursor AI.

## 5. Configuración en application.properties para MongoDB

Spring Boot necesita saber cómo conectar a su instancia de MongoDB.

### 5.1 Propiedades Clave

- Para conexión local (si instalaron el servidor):

```Properties
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017 # Puerto por defecto de MongoDB
spring.data.mongodb.database=mi_base_nosql # El nombre de la DB que crearon
# Si no tienen autenticación configurada localmente, esto es suficiente
```

- Para conexión usando una cadena de conexión (recomendado, especialmente con Atlas):

```Properties
spring.data.mongodb.uri=mongodb://usuario:contraseña@host:puerto/mi_base_nosql?opciones_extra # La cadena proporcionada por Atlas o su config local
# Ejemplo de Atlas: spring.data.mongodb.uri=mongodb+srv://su_usuario_atlas:su_contraseña_atlas@su_cluster.mongodb.net/mi_base_nosql?retryWrites=true&w=majority
spring.data.mongodb.database=mi_base_nosql # Nombre de la base de datos
```

- Autenticación (si no usan URI con credenciales):

```Properties
spring.data.mongodb.username=su_usuario
spring.data.mongodb.password=su_contraseña
# spring.data.mongodb.authentication-database=admin # Usualmente 'admin' para autenticación
```

### 5.2 Actividad

Añadan estas propiedades al archivo `src/main/resources/application.properties` (o `application.yml`) de su proyecto Spring Boot, adaptándolas a su configuración (local o Atlas).

## 6. Anotaciones `@Document`, `@Id`, `@Field`, `@MongoRepository`

Al igual que JPA tenía `@Entity` para mapear a tablas, Spring Data MongoDB tiene anotaciones para mapear objetos Java a documentos.

### 6.1 Anotaciones para Mapear Documentos

- `@Document(collection = "nombre_coleccion")`: Anotación a nivel de clase. Marca la clase como un documento que se almacenará en la colección especificada. Si omiten `collection`, Spring Data usará el nombre de la clase en minúsculas.

  ```Java
  import org.springframework.data.mongodb.core.mapping.Document;

  @Document(collection = "usuarios") // Mapea esta clase a la colección 'usuarios'
  public class User {
      // ...
  }
  ```

- `@Id`: Anotación en un atributo para marcarlo como el identificador único del documento. MongoDB genera un `ObjectId` por defecto. Pueden mapearlo a un campo `String` o `BigInteger` en Java. Si guardan un documento con el campo `@Id` como `null`, Spring Data MongoDB le pedirá a MongoDB que genere uno automáticamente.

  ```Java
  import org.springframework.data.annotation.Id; // Importante: es de spring.data.annotation, no persistence

  @Document(collection = "usuarios")
  public class User {
      @Id // Este campo será el '_id' del documento en MongoDB
      private String id; // Mapeo común para ObjectId

      // ...
  }
  ```

- `@Field(name = "nombre_campo")`: Anotación opcional en un atributo para especificar el nombre del campo en el documento de MongoDB si es diferente al nombre del atributo Java.

  ```Java
  import org.springframework.data.mongodb.core.mapping.Field;
  import org.springframework.data.annotation.Id;
  import org.springframework.data.mongodb.core.mapping.Document;

  @Document(collection = "productos")
  public class Product {
      @Id
      private String id;

      @Field(name = "product_name") // El atributo Java 'name' mapea al campo 'product_name' en el documento
      private String name;

      private Double price; // Mapea a campo 'price' (nombre por defecto)

      // ...
  }
  ```Java

- Los objetos anidados y las colecciones (List, Set) en su clase Java se mapean automáticamente a documentos anidados y arrays en el documento de MongoDB. ¡Aquí es donde brilla la flexibilidad de NoSQL!

  ```Java
  // En la clase User.java
  private Address address; // Mapeará a un campo 'address' que contiene un subdocumento
  private List<String> roles; // Mapeará a un campo 'roles' que es un array de strings
  ```

  (Asumiendo que `Address` es otra clase simple de Java).

### 6.2 Repositorios con Spring Data MongoDB (`MongoRepository`)

- Similar a JPA, solo definen una interfaz que extiende `MongoRepository<T, ID>`.
- `T`: El tipo de la entidad/documento (ej. `User`).
- `ID`: El tipo del campo `@Id` (ej. `String`).
- `MongoRepository` proporciona automáticamente métodos CRUD básicos y la capacidad de crear métodos de consulta derivados del nombre.

Ejemplo:

```Java
package com.example.midatabase.repository;

import com.example.midatabase.model.User;
import org.springframework.data.mongodb.repository.MongoRepository;

public interface UserRepository extends MongoRepository<User, String> {
    // Spring Data MongoDB proporciona CRUD (save, findById, findAll, deleteById, etc.)
    // Podemos añadir métodos de consulta derivados del nombre aquí...
}
```

### 6.3 Actividad

- Creen una clase Java para representar un documento (ej. `User`, `Product`, `DocumentoGenerico`). Anótenla con `@Document`, `@Id`, y `@Field` si es necesario. Incluyan atributos con tipos básicos, arrays o incluso sub-objetos (otras clases simples Java sin `@Entity` o `@Document` si las quieren embebidas).
- Creen una interfaz Repository para su documento extendiendo `MongoRepository`.
- **Asistencia de Cursor AI**: Pídanle a Cursor AI que les genere una clase `@Document` para una colección, incluyendo atributos con tipos anidados o arrays. Pídanle que les genere una interfaz `MongoRepository` para esa clase. Pídanle que explique cualquier anotación de Spring Data MongoDB.

## 7. Operaciones CRUD Básicas y Consultas por Campos

### 7.1 Usando el Repositorio en el Servicio

Inyecten su interfaz `MongoRepository` en una clase de servicio (`@Service`) usando inyección por constructor `@Autowired`.

### 7.2 Operaciones CRUD con `MongoRepository`

- `save(document)`: Guarda (inserta si es nuevo, actualiza si ya tiene un ID) un documento.
- `findById(id)`: Busca un documento por su ID (`Optional<T>`).
- `findAll()`: Devuelve una lista de todos los documentos en la colección.
- `deleteById(id)`: Elimina un documento por su ID.
- `count()`, `existsById()`, etc.

### 7.3 Consultas por Campos (Métodos Derivados)

Al igual que en JPA, pueden definir métodos en su `MongoRepository` usando convenciones de nombres para buscar documentos basándose en el valor de sus campos.

- Ejemplo en UserRepository:

  ```Java
  // Buscar usuario por username
  User findByUsername(String username);

  // Buscar usuarios con edad mayor a un valor
  List<User> findByAgeGreaterThan(int age);

  // Buscar usuarios cuyo email contenga un texto (regex case-insensitive en MongoDB)
  List<User> findByEmailRegex(String emailRegex); // O findByEmailLike (Spring Data puede usar regex)

  // Buscar documentos anidados: Buscar usuarios por ciudad en su dirección
  // Asumiendo que User tiene un campo 'address' que es un objeto Address con campo 'city'
  List<User> findByAddress_City(String city); // Usar '_' para navegar campos anidados
  ```

- Spring Data MongoDB traduce estos nombres de métodos a las consultas correspondientes de MongoDB.

### 7.4 Actividad

- Creen una clase de servicio que inyecte su `MongoRepository`.
- Implementen métodos simples en el servicio para realizar operaciones CRUD básicas utilizando los métodos del repositorio.
- Añadan algunos métodos de consulta derivados del nombre en su `MongoRepository` y úsenlos en el servicio.
- **Asistencia de Cursor AI**: Pídanle a Cursor AI que les ayude a escribir métodos de servicio que usan operaciones del repositorio. Pídanle que les genere la firma de un método de consulta derivado del nombre en su `MongoRepository` para buscar por un campo o por un campo dentro de un documento anidado.

## 8. Uso de IA para Transformar Consultas Relacionales a NoSQL

- **El Desafío del Cambio de Paradigma**: Pasar de SQL con JOINs a un mundo basado en documentos requiere pensar diferente. Las relaciones no siempre se modelan igual (embebidas vs referenciadas por ID).
- Cómo la IA Ayuda en la Transformación Conceptual y de Consulta: Cursor AI puede ser invaluable para ayudar a "traducir" su pensamiento relacional a un modelo NoSQL.
  - **Modelado de Relaciones en NoSQL**: Descríbanle a la IA una relación relacional (ej. "Tengo una relación 1:N entre Clientes y Pedidos en SQL, donde la FK está en la tabla Pedidos"). Pregúntenle: "¿Cómo representaría esta relación en un modelo de documentos de MongoDB? ¿Debería embeber los Pedidos dentro del Cliente, o solo guardar el ID del Cliente en el Pedido?". La IA les explicará las opciones (embebido, referenciado) y sus pros y contras en MongoDB.
  - **Equivalencia de Consultas SQL**: Si tienen una consulta SQL que resuelve una necesidad (ej. un JOIN para encontrar pedidos de un cliente), péguenle la consulta SQL a la IA y pregunten: "¿Cómo haría una consulta equivalente en Spring Data MongoDB para mi modelo de documentos?". La IA les sugerirá cómo usar métodos derivados o quizás el Aggregation Framework de MongoDB.
  - **Generación de Código de Consulta NoSQL**: Una vez que tengan clara la lógica de consulta en MongoDB, pídanle a la IA que les ayude a escribir el código de Spring Data MongoDB para esa consulta (método derivado, `@Query` con JSON query, o código de Aggregation).
- **Actividad**:
  - Discutan un par de escenarios relacionales de su proyecto anterior (ej. Cliente-Pedido 1:N, Pedido-Producto N:N).
  - Usen Cursor AI para explorar cómo representarían esas relaciones en un modelo de documentos de MongoDB (ej. ¿embeberían los pedidos en el cliente? ¿los productos en el pedido? ¿o solo usarían referencias por ID?). Analicen las implicaciones de cada opción.
  - Si tienen una consulta SQL del proyecto anterior que involucra un JOIN simple (ej. encontrar pedidos de un cliente), peguen el SQL a Cursor AI y pídanle cómo hacer la consulta equivalente en Spring Data MongoDB.

## 9. Proyecto en Clase: API RESTful Básica de Usuarios con MongoDB

### 9.1 Propósito

Aplicar inmediatamente lo aprendido sobre conexión, mapeo, repositorios y operaciones CRUD básicas con MongoDB en un mini-proyecto práctico durante la clase.

### 9.2 Descripción del Proyecto

- Crear una colección de `usuarios` en MongoDB.
- Cada documento de usuario debe tener un ID, un nombre de usuario (`username`), un correo electrónico (`email`), y algunos otros campos que muestren la flexibilidad del esquema (ej. una lista de `roles`, un campo `configuracion` que sea un subdocumento con diferentes campos opcionales para cada usuario).
- Configurar una aplicación Spring Boot con la dependencia y propiedades de MongoDB.
- Crear una clase Java `User` anotada como `@Document`, con `@Id` y `@Field` si es necesario, y atributos para los campos.
- Crear una interfaz `UserRepository` extendiendo `MongoRepository`.
- Implementar un Controlador REST (`/api/users`) con endpoints para:
  - `POST /api/users`: Registrar un nuevo usuario (`save`).
  - `GET /api/users`: Obtener la lista de todos los usuarios (`findAll`).
  - `GET /api/users/{id}`: Obtener un usuario por su ID (`findById`).
  - `DELETE /api/users/{id}`: Eliminar un usuario por su ID (`deleteById`).
  - (Opcional) `GET /api/users/buscar?username=...`: Buscar usuario por nombre de usuario (usando un método derivado).
- **Actividad Principal de la Clase**: Desarrollo guiado de este mini-proyecto de usuarios en MongoDB, utilizando Cursor AI en cada paso (creación de clase `User`, `UserRepository`, configuración, métodos de servicio/controlador, métodos derivados).

## 10. Conclusión

Hoy dimos un paso importante al introducirnos al mundo de las bases de datos NoSQL con MongoDB. Entendimos su estructura flexible de documentos y colecciones, las diferencias clave con las bases de datos relacionales, y cómo configurar Spring Boot para conectar a MongoDB. Aprendimos a mapear objetos a documentos con `@Document` y a usar `MongoRepository` para operaciones CRUD y consultas básicas. La IA es una herramienta muy útil para navegar por el cambio de paradigma y para generar código.

¡Practiquen la instalación de MongoDB o el uso de Atlas, completen el proyecto de usuarios y empiecen a pensar cómo modelarían los datos de su proyecto integrador en un modelo de documentos de MongoDB!
