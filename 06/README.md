# Java Senior con IA

## Modulo 06 - Bases de Datos Relacionales y NoSQL con Java y Spring Boot

Este módulo tiene como objetivo desarrolar competencias sólidas en el diseño,  conexión y manipulación de bases de datos relacionales y NoSQL desde  aplicaciones Java. Se trabajará con PostgreSQL y MongoDB, y se aprenderá a realizar operaciones CRUD, modelado de entidades, relaciones y optimización de consultas utilizando Spring Data JPA. Se incorporará el uso de Cursor AI para sugerencias de mapeo, creación de consultas y diseño de esquemas eficientes.

### [Clase 01](1) - Introducción a PostgreSQL y conexión desde Java

#### 1.1 Objetivos de aprendizaje

* Entender el rol de las bases de datos relacionales en aplicaciones empresariales.
* Modelar estructuras de datos eficientes y escalables.
* Conectar y consultar una base de datos PostgreSQL desde Java.

#### 1.2 Contenido

* Qué es una base de datos relacional? Características de PostgreSQL.
* Instalación, configuración y uso de herramientas como pgAdmin o DBeaver.
* Creación de esquemas, tablas, tipos de datos y restricciones (`PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, etc.).
* Conceptos de normalización y modelado lógico de datos.
* Conexión desde Java usando Spring Boot + Spring Data JPA.
* Configuración del archivo application.properties y dependencias necesarias.
* Prueba de conexión con Cursor AI: sugerencias para verificación automática.

### [Clase 02](2) - Spring Data JPA – Entidades, Repositorios y Relaciones

#### 2.1 Objetivos de aprendizaje

* Mapear objetos Java a tablas de base de datos usando JPA.
* Implementar relaciones entre entidades de forma correcta.
* Realizar operaciones CRUD de manera profesional con Spring Data JPA.

#### 2.2 Contenido

* Uso de anotaciones `@Entity`, `@Id`,`@GeneratedValue`, `@Column`, `@Table`.
* Creación de clases Repository y uso de interfaces `JpaRepository` y `CrudRepository`.
* Relaciones entre entidades:
  * `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`.
  * Atributos de relación: `cascade`, `fetch`, `mappedBy`.
* Validación de relaciones complejas con ayuda de Cursor AI.

### [Clase 03](3) - Consultas Avanzadas con JPQL, Criteria API y Spring Queries

#### 3.1 Objetivos de aprendizaje

* Crear consultas avanzadas en base a las necesidades del negocio.
* Optimizar consultas y estructuras para eficiencia en tiempo real.

#### 3.2 Contenido

* Introducción a JPQL (Java Persistence Query Language): queries basadas en clases, no en tablas.
* Uso de anotaciones @Query para crear consultas personalizadas.
* Consultas dinámicas con Criteria API.
* Métodos derivados (`findByNombre`, `findByPrecioGreaterThan`).
* Paginación y ordenamiento con Pageable, Sort.
* Cursor AI como generador automático de métodos de consulta y optimizador de filtros.

### [Clase 04](4) - MongoDB – Introducción a NoSQL y conexión con Spring Boot

#### 4.1 Objetivos de aprendizaje

* Comprender los fundamentos de las bases de datos NoSQL.
* Implementar una aplicación con MongoDB y Spring Boot.

#### 4.2 Contenido

* ¿Qué es MongoDB? Estructura flexible, documentos y colecciones.
* Diferencias entre SQL y NoSQL (modelo, consultas, escalabilidad).
* Instalación de MongoDB local o en Atlas (cloud).
* Uso de `spring-boot-starter-data-mongodb` en un proyecto.
* Anotaciones `@Document`, `@Id`, `@Field`, repositorios `MongoRepository`.
* Operaciones CRUD básicas y consultas por campos.
* Uso de IA para transformar consultas relacionales a NoSQL.
