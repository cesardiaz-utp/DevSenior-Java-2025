# Spring Data JPA – Entidades, Repositorios y Relaciones

¡Hola a todos!

En la clase anterior, dimos los primeros pasos con bases de datos relacionales y PostgreSQL, y vimos cómo conectar nuestro proyecto Spring Boot a la base de datos. Hoy, vamos a construir el puente entre nuestra aplicación Java y la base de datos: Spring Data JPA. Aprenderemos cómo mapear nuestros objetos Java a tablas, realizar operaciones comunes (CRUD) de forma sencilla, y lo más importante, cómo manejar las relaciones entre nuestras entidades.

## 1. Mapeando Objetos Java a Tablas (`@Entity`, `@Id`, `@GeneratedValue`, `@Column`, `@Table`)

### 1.1 El Rol de JPA (Java Persistence API)

JPA es una especificación de Java que define cómo mapear objetos Java a bases de datos relacionales (el proceso ORM - Mapeo Objeto-Relacional). Hibernate es la implementación por defecto de JPA que usa Spring Boot. Ustedes definirán sus clases Java como "entidades" y usarán anotaciones para indicarle a JPA/Hibernate cómo se corresponden con las tablas y columnas de su base de datos.

### 1.2 Entidades JPA: Las clases Java que representan tablas

#### `@Entity`

Anotación a nivel de clase que marca una clase Java como una entidad JPA, lo que significa que mapeará a una tabla en la base de datos.

```java
import jakarta.persistence.Entity;

@Entity // Esta clase mapea a una tabla en la DB
public class NombreDeMiTabla { // El nombre de la clase por defecto es el nombre de la tabla
    // ... atributos ...
}
```

#### `@Table(name = "nombre_tabla")`

Anotación opcional a nivel de clase para especificar el nombre de la tabla en la base de datos si es diferente al nombre de la clase.

```java
import jakarta.persistence.Entity;
import jakarta.persistence.Table;

@Entity
@Table(name = "usuarios") // Mapea esta clase a la tabla 'usuarios' en la DB
public class User {
    // ...
}
```

#### `@Id`

Anotación en un atributo que indica que este campo es la clave primaria de la entidad (y, por lo tanto, de la tabla).

```java
import jakarta.persistence.Id;
import jakarta.persistence.Entity;

@Entity
public class Product {
    @Id // 'id' es la clave primaria de esta entidad
    private Long id;
    // ...
}
```

#### `@GeneratedValue(strategy = GenerationType.STRATEGY)`

Anotación en el atributo `@Id` para configurar cómo se genera el valor de la clave primaria.

- `GenerationType.IDENTITY`: La base de datos asigna un valor de identidad automáticamente (común con `SERIAL`/`BIGSERIAL` en PostgreSQL). Es la estrategia más usada con bases de datos autogenerables.
- `GenerationType.SEQUENCE`: Usa una secuencia de base de datos.
- `GenerationType.TABLE`: Usa una tabla especial para generar IDs.
- `GenerationType.AUTO`: El proveedor de JPA (Hibernate) elige la mejor estrategia según la DB.

```java
import jakarta.persistence.Id;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Entity;

@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // ID generado automáticamente por la DB
    private Long id;
    // ...
}
```

#### `@Column(name = "nombre_columna", nullable = boolean, unique = boolean, length = int)`

Anotación en un atributo para mapearlo a una columna específica de la tabla.

- `name`: Especifica el nombre de la columna en la DB si es diferente al nombre del atributo Java.
- `nullable`: Si es `false`, indica que la columna en la DB no permite `NULL` (Hibernate añadirá la restricción `NOT NULL` si usa `ddl-auto=update`). Por defecto es `true`. Corresponde a la restricción `NOT NULL` en SQL.
- `unique`: Si es `true`, indica que los valores en esta columna deben ser únicos en la tabla. Corresponde a la restricción `UNIQUE` en SQL.
- `length`: Para columnas de texto (`String`), especifica la longitud máxima en la DB (`VARCHAR(length)`).

```java
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
// ...

@Entity
public class User {
    // ... @Id y @GeneratedValue ...

    private String username; // Por defecto mapea a columna 'username'

    @Column(name = "email_address", nullable = false, unique = true, length = 255)
    private String email; // Mapea a columna 'email_address', no puede ser null, debe ser único, máx 255 chars

    // ...
}
```

#### `@Transient`

Anotación en un atributo que indica que este campo no debe ser persistido en la base de datos. Es solo para uso temporal o calculado en la aplicación.

```java
import jakarta.persistence.Transient;
import jakarta.persistence.Entity;
// ...

@Entity
public class Product {
    // ... @Id y otros campos ...

    private double price; // Se persiste por defecto

    @Transient // Este campo no se guardará ni leerá de la base de datos
    private double discountedPrice; // Campo calculado en Java

    // ... constructor, getters, setters ...

    // Ejemplo de uso: calcular descuento después de cargar la entidad
    @PostLoad // Anotación JPA que se ejecuta después de cargar la entidad
    public void calculateDiscount() {
        this.discountedPrice = this.price * 0.9; // Ejemplo simple
    }
}
```

### 1.3 Mapeo de Tipos de Datos

Generalmente, JPA/Hibernate mapea automáticamente los tipos de datos comunes de Java a los tipos SQL correspondientes (ej. String a VARCHAR/TEXT, int/Integer a INT, boolean/Boolean a BOOLEAN, java.time.LocalDate a DATE, java.time.LocalDateTime a TIMESTAMP).

#### Mapeo entre Java y PostgreSQL

| Tipo de Dato en Java | Tipo de Dato Común en PostgreSQL | Notas |
| --- | --- | --- |
| `boolean` / `Boolean` | `BOOLEAN` | Mapeo directo |
| `byte` / `Byte` | `SMALLINT` | |
| `short` / `Short` | `SMALLINT` | |
| `int` / `Integer` | `INT` / `INTEGER` | Mapeo directo |
| `long` / `Long` | `BIGINT` | Ideal para IDs |
| `float` / `Float` | `REAL` / `FLOAT4` | Precisión simple |
| `double` / `Double` | `DOUBLE PRECISION` / `FLOAT8` | Mayor precisión |
| `BigDecimal` | `NUMERIC` / `DECIMAL` | Ideal para dinero, precisión exacta. Ejemplo: `@Column(precision = 10, scale = 2)` |
| `String` | `VARCHAR(n)` / `TEXT` | `VARCHAR` con `@Column(length=n)`, `TEXT` por defecto si no se especifica longitud o si es muy larga. |
| `byte[]` / `Byte[]` | `BYTEA` | Datos binarios |
| `char[]` / `Character[]` | `VARCHAR` / `TEXT` | Menos común, preferir `String` |
| `java.time.LocalDate` | `DATE` | Recomendado para fechas sin hora. |
| `java.time.LocalTime` | `TIME` | Recomendado para horas sin fecha. |
| `java.time.LocalDateTime` | `TIMESTAMP` | Recomendado para fecha y hora. |
| `java.time.Instant` | `TIMESTAMP` | Fecha y hora con zona horaria (UTC). |
| `java.util.UUID` | `UUID` | Si el tipo UUID está habilitado en PostgreSQL. Alternativa: `VARCHAR`. Ejemplo: `@Column(columnDefinition = "UUID")` |
| `Enum (en Java)` | `VARCHAR` / `INT` | Por defecto a `VARCHAR` (guarda el nombre del enum). Puede configurarse para guardar el ordinal (`INT`) con `@Enumerated`. |
| Tipos LOB (`Clob`, `Blob`) | `TEXT` / `BYTEA` | Para datos grandes de texto o binarios. |

### 1.4 Actividad/Demostración

Revisaremos una clase `Curso` o `Cliente` completa con estas anotaciones (`@Entity`, `@Table`, `@Id`, `@GeneratedValue(strategy=IDENTITY)`, `@Column` con name, nullable, unique).

#### 1.5 Asistencia de Cursor AI

Pueden pedirle a Cursor AI que les genere una entidad JPA para una tabla existente (dándole la sentencia `CREATE TABLE`) o que añada anotaciones `@Column` con atributos específicos a los campos de una entidad. Pídanle que explique el propósito de `@Transient` o las diferentes estrategias de `@GeneratedValue`.

## 2. Repositorios con Spring Data JPA (`JpaRepository`, `CrudRepository`)

### 2.1 El Patrón Repository

Es una forma de separar la lógica de acceso a datos (interacción con la base de datos) del resto del código (lógica de negocio en el servicio). Actúa como una colección de objetos del dominio en memoria.

### 2.2 Spring Data JPA Simplifica Repositorios

En lugar de escribir la implementación de cada método de repositorio (ej. `guardar(Producto p)` que haría un `INSERT` o `UPDATE`), Spring Data JPA lo hace por ustedes. Solo necesitan definir una interfaz.

#### `CrudRepository<T, ID>`

- Es la interfaz base en Spring Data para proporcionar operaciones CRUD estándar.
- `T`: El tipo de la entidad (ej. `Curso`, `Cliente`).
- `ID`: El tipo de la clave primaria de la entidad (ej. `Long`, `String`).
- Métodos que proporciona (ej. para `CrudRepository<Curso, Long>`):
  - `save(Curso entity)`: Guarda (crea o actualiza) una entidad.
  - `findById(Long id)`: Busca una entidad por su ID, devuelve un `Optional<Curso>`.
  - `findAll()`: Devuelve una lista de todas las entidades.
  - `deleteById(Long id)`: Elimina la entidad con el ID especificado.
  - `count()`: Retorna el número total de entidades.
  - `existsById(Long id)`: Retorna true si existe una entidad con el ID especificado.

#### `JpaRepository<T, ID>`

- Es una interfaz que extiende `PagingAndSortingRepository` (para paginación y ordenación) y `CrudRepository`.
- Añade métodos específicos de JPA (ej. `flush()`, `saveAndFlush()`, `findAll(Sort sort)`).
- Es la interfaz **más comúnmente utilizada** cuando se trabaja con JPA en Spring Data. Recomendamos usar `JpaRepository` en la mayoría de los casos.

### 2.3 Actividad/Demostración

1. Crear una interfaz en un paquete `repository` (ej. `com.suempresa.biblioteca.repository`).
2. Hacer que extienda `JpaRepository` (o `CrudRepository`).
3. Especificar la Entidad y el tipo de su ID entre `< >`.
4. Anotarla con `@Repository` (aunque a menudo es opcional si extiende de las interfaces de Spring Data, es buena práctica para claridad).

```Java
package com.example.apicursos.repository;

import com.example.apicursos.model.Curso;
import org.springframework.data.jpa.repository.JpaRepository;

// @Repository // Opcional si extiendes JpaRepository
public interface CursoRepository extends JpaRepository<Curso, Long> {
    // Spring Data JPA genera la implementación por nosotros
    // ¡Podemos añadir métodos de consulta personalizados aquí!
}
```

Spring Data JPA creará una implementación concreta de esta interfaz en tiempo de ejecución. Podemos inyectarla donde la necesitemos (ej. en un `@Service`).

#### 2.4 Asistencia de Cursor AI

Pídanle a Cursor AI que les genere una interfaz Spring Data JPA Repository para una entidad específica, indicando la entidad y el tipo de ID. Por ejemplo: "Generate a Spring Data JPA Repository interface for the 'Producto' entity, where the ID is of type Long".

## 3. Realizando Operaciones CRUD de manera Profesional con Spring Data JPA

### 3.1 Inyectando el Repositorio

En su clase de servicio (`@Service`), declaren un campo privado final del tipo de la interfaz Repository y usen inyección por constructor con `@Autowired`.  
Ejemplo:

```Java
package com.example.apicursos.service;

import com.example.apicursos.repository.CursoRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
// ... otros imports ...

@Service
public class CursoServiceImpl implements CursoService { // Su clase de servicio

    private final CursoRepository cursoRepository; // Inyectamos la INTERFAZ

    @Autowired // Spring inyectará la implementación generada por Spring Data JPA
    public CursoServiceImpl(CursoRepository cursoRepository) {
        this.cursoRepository = cursoRepository;
    }

    // ... métodos de servicio ...
}
```

### 3.2 Usando los Métodos Estándar de `JpaRepository`

#### Crear/Actualizar

Usen el método `save(entity)`. Si la entidad tiene un ID (no nulo y existente en DB), `save` actualizará. Si el ID es `null` o no existe, `save` creará un nuevo registro y asignará el ID autogenerado a la entidad.

```java
// Método en CursoServiceImpl
@Override
public Curso save(Curso curso) {
    // Aquí iría lógica de negocio ANTES de guardar (ej. validación compleja)
    Curso cursoGuardado = cursoRepository.save(curso); // Llama al método del repositorio
    // Aquí iría lógica de negocio DESPUÉS de guardar (ej. enviar notificación)
    return cursoGuardado;
}
```

#### Leer por ID

Usen `findById(id)`. Devuelve un `Optional<Entity>` para manejar casos donde el ID no existe.

```java
// Método en CursoServiceImpl
@Override
public Optional<Curso> findById(Long id) {
    // Aquí iría lógica de negocio ANTES de buscar (ej. validar ID, permisos)
    Optional<Curso> cursoOpt = cursoRepository.findById(id); // Llama al método del repositorio
    // Aquí iría lógica de negocio DESPUÉS de buscar (si se encuentra)
    return cursoOpt;
}
```

#### Leer Todos

Usen `findAll()`. Devuelve una `List<Entity>`.

```java
// Método en CursoServiceImpl
@Override
public List<Curso> findAll() {
    // Aquí iría lógica de negocio
    List<Curso> todosLosCursos = cursoRepository.findAll(); // Llama al método del repositorio
    return todosLosCursos;
}
```

#### Eliminar por ID

Usen `deleteById(id)`.

```java
// Método en CursoServiceImpl
@Override
public void deleteById(Long id) {
    // Aquí iría lógica de negocio (ej. verificar si existe antes de borrar)
    cursoRepository.deleteById(id); // Llama al método del repositorio
}
```

### 3.3 Métodos de Consulta Derivados del Nombre (Derived Query Methods)

¡Una característica muy útil de Spring Data JPA! Pueden definir métodos de consulta en su interfaz Repository simplemente nombrando el método de forma que Spring Data JPA entienda qué consulta SQL debe generar.

```java
// Buscar cursos por nombre (exacto)
List<Curso> findByNombre(String nombre);

// Buscar cursos cuyo nombre contenga un texto (ignorando mayúsculas/minúsculas)
List<Curso> findByNombreContainingIgnoreCase(String nombre);

// Buscar cursos por rango de créditos
List<Curso> findByCreditosBetween(Integer minCreditos, Integer maxCreditos);

// Buscar cursos que inicien después de una fecha
List<Curso> findByFechaInicioAfter(java.time.LocalDate fecha);
```

Spring Data JPA lee el nombre del método (`findBy...`, `countBy...`, `deleteBy...`, `findTopNBy...`) y los predicados (`...Nombre`, `...CreditosBetween`, `...FechaInicioAfter`) y genera la consulta SQL correspondiente.

### 3.4 Optimización de Consultas

#### Problema N+1

El problema N+1 ocurre cuando se realizan múltiples consultas para cargar datos relacionados. Por ejemplo, al cargar una lista de entidades y luego cargar sus relaciones una por una.

#### Solución con `JOIN FETCH`

Usen `@Query` con `JOIN FETCH` para cargar relaciones de manera eficiente.

```java
@Query("SELECT c FROM Curso c JOIN FETCH c.estudiantes WHERE c.id = :id")
Curso findCursoConEstudiantes(@Param("id") Long id);
```

### 3.5 Actividad/Demostración

- En su proyecto, creen una interfaz Repository para una entidad (ej. `ProductoRepository`).
- En una clase de servicio que use este repositorio, inyecten el repositorio.
- Implementen métodos de servicio simples para las operaciones CRUD básicas utilizando los métodos del repositorio inyectado.
- Añadan algunos métodos de consulta derivados del nombre en su interfaz Repository y usenlos en su servicio.

#### 3.6 Asistencia de Cursor AI

Pídanle a Cursor AI que les ayude a escribir métodos de servicio que usan operaciones del repositorio. Pídanle que les genere la firma de un método de consulta personalizado en el repositorio (ej. "Add a method to search orders placed after a specific date in OrderRepository").

## 4. Relaciones entre Entidades

El poder de las bases de datos relacionales radica en la conexión de tablas a través de relaciones. JPA nos permite modelar estas relaciones directamente en nuestras clases de entidad Java.

### 4.1 Tipos de Relaciones y Cómo Mapearlas con Anotaciones

#### Uno a Uno (`@OneToOne`)

Una instancia de Entidad A se relaciona con como máximo una de Entidad B, y viceversa.

#### Uno a Muchos (`@OneToMany`) / Muchos a Uno (`@ManyToOne`)

Es la relación más común. Una instancia de Entidad A tiene _cero o muchos_ de Entidad B, pero cada Entidad B pertenece a como máximo _una_ de Entidad A.

#### Muchos a Muchos (`@ManyToMany`)

Una instancia de Entidad A se relaciona con _muchas_ de Entidad B, y una instancia de Entidad B se relaciona con _muchas_ de Entidad A.

### 4.2 Actividad/Demostración

- En su proyecto, modificaremos las entidades `Cliente` y `Pedido`.
- Añadiremos las anotaciones `@ManyToOne` en `Pedido` y `@OneToMany` en `Cliente` para mapear la relación 1:N.
- Configuraremos `@JoinColumn` en el lado propietario (`@ManyToOne`).
- Configuraremos `mappedBy` en el lado inverso (`@OneToMany`).
- Discutiremos y añadiremos un `CascadeType` apropiado (ej. `PERSIST`, `REMOVE`) a la relación.
- Mostraremos cómo guardar un `Pedido` asociado a un `Cliente` existente y cómo guardar un `Cliente` con una nueva lista de`Pedidos` (si `cascade` está configurado).
- (Si da tiempo) Mostraremos el mapeo básico de una relación N:N con `@ManyToMany` y `@JoinTable`.

#### 4.3 Asistencia de Cursor AI

Pídanle a Cursor AI que les ayude a añadir una relación entre dos entidades existentes (ej. "Add a One-to-Many relationship from Cliente to Pedido in these JPA entities"). Pídanle que explique el significado de mappedBy, cascade o fetch en el contexto de su código.

## 5. Validación de Relaciones Complejas con Ayuda de Cursor AI

### 5.1 Validación de Entidades Individuales (`@Valid`)

Vimos en la clase anterior que `jakarta.validation` permite validar los campos de una entidad usando anotaciones como `@NotBlank`, `@Size`, etc., y que `@Valid` en el controlador activa esta validación.

### 5.2 Validación de Entidades Relacionadas

Si tienen una relación donde una entidad "contiene" o referencia a otras entidades relacionadas en un atributo (ej. un `Cliente` tiene una `List<Direccion>` con `@OneToMany`) y configuran un tipo de `cascade` que incluya `PERSIST` o `MERGE`, pueden usar `@Valid` en el atributo de la relación para que las reglas de validación definidas en la entidad _relacionada_ (`Direccion`) se apliquen automáticamente cuando validan la entidad propietaria (`Cliente`).

```java
// En la clase Cliente.java
import jakarta.persistence.*;
import jakarta.validation.Valid; // Importar @Valid
import java.util.List;

@Entity
public class Cliente {
    // ... otros campos ...

    @OneToMany(mappedBy = "cliente", cascade = CascadeType.ALL, orphanRemoval = true)
    @Valid // <-- Si se valida el Cliente, también se validan las Direcciones en la lista
    private List<Direccion> direcciones;

    // ...
}

// En la clase Direccion.java (asumiendo que tiene anotaciones de validación como @NotBlank en sus campos)
import jakarta.persistence.*;
import jakarta.validation.constraints.NotBlank;

@Entity
public class Direccion {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "La calle no puede estar vacía")
    private String calle;
    // ... otros campos con validaciones ...

    @ManyToOne // Relación ManyToOne de Direccion a Cliente (lado propietario de FK)
    @JoinColumn(name = "cliente_id")
    private Cliente cliente;

    // ...
}
```

### 5.3 Validación de Integridad Referencial (Existencia de FK en DB)

Es importante entender que las anotaciones de validación de Bean Validation (`@NotNull`, `@Valid`, etc.) **no** verifican automáticamente si un ID de clave foránea que ustedes proporcionan realmente existe en la tabla referenciada en la base de datos.

Esa verificación es responsabilidad de la restricción `FOREIGN KEY` en la base de datos o de la lógica que implementen en su capa de servicio (ej. buscar la entidad relacionada por ID antes de asignarla).

### 5.4 Validación de Lógica de Negocio en la capa de Servicio

A menudo, la validación más compleja que involucra el estado de la base de datos (ej. ¿El libro existe? ¿Está disponible para prestar?) se realiza en la capa de servicio antes de guardar o modificar la entidad, no solo con anotaciones de validación.

### 5.5 Actividad con Cursor AI

- Pregúntenle a Cursor AI cómo validar entidades relacionadas usando @Valid. Por ejemplo: "How can I use @Valid to ensure that all Direccion entities within a Cliente entity are also validated when saving the Cliente?".
- Pregúntenle a Cursor AI sobre cómo manejar errores de integridad referencial cuando intentan guardar una entidad con una clave foránea que apunta a un registro que no existe. Por ejemplo: "What happens in Spring Boot/JPA if I try to save a Pedido with a client_id that does not exist in the database? How can I handle this error professionally?".

## 6. Conclusión

Hoy dimos un gran paso al aprender a mapear nuestras clases Java a estructuras de base de datos usando JPA. Cubrimos cómo usar Spring Data JPA para las operaciones CRUD básicas de forma elegante y, lo más importante, aprendimos a representar y mapear las relaciones entre nuestras entidades, que es fundamental para bases de datos relacionales.

¡Practiquen mucho el mapeo de entidades y relaciones en sus proyectos! En la próxima clase, exploraremos consultas más avanzadas y manejo de transacciones.
