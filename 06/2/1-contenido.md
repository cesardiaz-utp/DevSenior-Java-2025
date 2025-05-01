# Spring Data JPA – Entidades, Repositorios y Relaciones

¡Hola a todos!

En la clase anterior, dimos los primeros pasos con bases de datos relacionales y PostgreSQL, y vimos cómo conectar nuestro proyecto Spring Boot a la base de datos. Hoy, vamos a construir el puente entre nuestra aplicación Java y la base de datos: Spring Data JPA. Aprenderemos cómo mapear nuestros objetos Java a tablas, realizar operaciones comunes (CRUD) de forma sencilla, y lo más importante, cómo manejar las relaciones entre nuestras entidades.

## Mapeando Objetos Java a Tablas (`@Entity`, `@Id`, `@GeneratedValue`, `@Column`, `@Table`)

El Rol de JPA (Java Persistence API): JPA es una especificación de Java que define cómo mapear objetos Java a bases de datos relacionales (el proceso ORM - Mapeo Objeto-Relacional). Hibernate es la implementación por defecto de JPA que usa Spring Boot.  Ustedes definirán sus clases Java como "entidades" y usarán anotaciones para indicarle a JPA/Hibernate cómo se corresponden con las tablas y columnas de su base de datos.

### Entidades JPA: Las clases Java que representan tablas.

* `@Entity`: Anotación a nivel de clase que marca una clase Java como una entidad JPA, lo que significa que mapeará a una tabla en la base de datos.
    ```java
    import jakarta.persistence.Entity;

    @Entity // Esta clase mapea a una tabla en la DB
    public class NombreDeMiTabla { // El nombre de la clase por defecto es el nombre de la tabla
        // ... atributos ...
    }
    ```
* `@Table(name = "nombre_tabla")`: Anotación opcional a nivel de clase para especificar el nombre de la tabla en la base de datos si es diferente al nombre de la clase.
    ```java
    import jakarta.persistence.Entity;
    import jakarta.persistence.Table;

    @Entity
    @Table(name = "usuarios") // Mapea esta clase a la tabla 'usuarios' en la DB
    public class User {
        // ...
    }
    ```
* `@Id`: Anotación en un atributo que indica que este campo es la clave primaria de la entidad (y, por lo tanto, de la tabla).
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
* `@GeneratedValue(strategy = GenerationType.STRATEGY)`: Anotación en el atributo `@Id` para configurar cómo se genera el valor de la clave primaria.
    * `GenerationType.IDENTITY`: La base de datos asigna un valor de identidad automáticamente (común con `SERIAL`/`BIGSERIAL` en PostgreSQL). Es la estrategia más usada con bases de datos autogenerables.
    * `GenerationType.SEQUENCE`: Usa una secuencia de base de datos.
    * `GenerationType.TABLE`: Usa una tabla especial para generar IDs.
    * `GenerationType.AUTO`: El proveedor de JPA (Hibernate) elige la mejor estrategia según la DB.
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
* `@Column(name = "nombre_columna", nullable = boolean, unique = boolean, length = int)`: Anotación en un atributo para mapearlo a una columna específica de la tabla.
    * `name`: Especifica el nombre de la columna en la DB si es diferente al nombre del atributo Java.
    * `nullable`: Si es `false`, indica que la columna en la DB no permite `NULL` (Hibernate añadirá la restricción `NOT NULL` si usa `ddl-auto=update`). Por defecto es `true`. Corresponde a la restricción `NOT NULL` en SQL.
    * `unique`: Si es `true`, indica que los valores en esta columna deben ser únicos en la tabla. Corresponde a la restricción `UNIQUE` en SQL.
    * `length`: Para columnas de texto (`String`), especifica la longitud máxima en la DB (`VARCHAR(length)`).
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
* `@Transient`: Anotación en un atributo que indica que este campo no debe ser persistido en la base de datos. Es solo para uso temporal o calculado en la aplicación.
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

### Mapeo de Tipos de Datos:

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

### Actividad/Demostración:

Revisaremos una clase `Curso` o `Cliente` completa con estas anotaciones (`@Entity`, `@Table`, `@Id`, `@GeneratedValue(strategy=IDENTITY)`, `@Column` con name, nullable, unique).

#### Asistencia de Cursor AI:

Pueden pedirle a Cursor AI que les genere una entidad JPA para una tabla existente (dándole la sentencia `CREATE TABLE`) o que añada anotaciones `@Column` con atributos específicos a los campos de una entidad. Pídanle que explique el propósito de `@Transient` o las diferentes estrategias de `@GeneratedValue`.

## Repositorios con Spring Data JPA (`JpaRepository`, `CrudRepository`)

### El Patrón Repository

Es una forma de separar la lógica de acceso a datos (interacción con la base de datos) del resto del código (lógica de negocio en el servicio). Actúa como una colección de objetos del dominio en memoria.

### Spring Data JPA Simplifica Repositorios

En lugar de escribir la implementación de cada método de repositorio (ej. `guardar(Producto p)` que haría un `INSERT` o `UPDATE`), Spring Data JPA lo hace por ustedes. Solo necesitan definir una interfaz.

* `CrudRepository<T, ID>`: 
    * Es la interfaz base en Spring Data para proporcionar operaciones CRUD estándar.
    * `T`: El tipo de la entidad (ej. `Curso`, `Cliente`).
    * `ID`: El tipo de la clave primaria de la entidad (ej. `Long`, `String`).
    * Métodos que proporciona (ej. para `CrudRepository<Curso, Long>`):
        * `save(Curso entity)`: Guarda (crea o actualiza) una entidad.
        * `findById(Long id)`: Busca una entidad por su ID, devuelve un `Optional<Curso>`.
        * `findAll()`: Devuelve una lista de todas las entidades.
        * `deleteById(Long id)`: Elimina la entidad con el ID especificado.
        * `count()`: Retorna el número total de entidades.
        * `existsById(Long id)`: Retorna true si existe una entidad con el ID especificado.
* `JpaRepository<T, ID>`:
    * Es una interfaz que extiende `PagingAndSortingRepository` (para paginación y ordenación) y `CrudRepository`.
    * Añade métodos específicos de JPA (ej. `flush()`, `saveAndFlush()`, `findAll(Sort sort)`).
    * Es la interfaz **más comúnmente utilizada** cuando se trabaja con JPA en Spring Data. Recomendamos usar `JpaRepository` en la mayoría de los casos.

### Actividad/Demostración:

1. Crear una interfaz en un paquete `repository` (ej. `com.suempresa.biblioteca.repository`).
1. Hacer que extienda `JpaRepository` (o `CrudRepository`).
1. Especificar la Entidad y el tipo de su ID entre `< >`.
1. Anotarla con `@Repository` (aunque a menudo es opcional si extiende de las interfaces de Spring Data, es buena práctica para claridad).
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

### Asistencia de Cursor AI

Pídanle a Cursor AI que les genere una interfaz Spring Data JPA Repository para una entidad específica, indicando la entidad y el tipo de ID. Por ejemplo: "Generate a Spring Data JPA Repository interface for the 'Producto' entity, where the ID is of type Long".

## Realizando Operaciones CRUD de manera Profesional con Spring Data JPA

### Inyectando el Repositorio:

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

### Usando los Métodos Estándar de `JpaRepository`:

* **Crear/Actualizar**: Usen el método `save(entity)`. Si la entidad tiene un ID (no nulo y existente en DB), `save` actualizará. Si el ID es `null` o no existe, `save` creará un nuevo registro y asignará el ID autogenerado a la entidad.
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
* **Leer por ID**: Usen `findById(id)`. Devuelve un `Optional<Entity>` para manejar casos donde el ID no existe.
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
* **Leer Todos**: Usen `findAll()`. Devuelve una `List<Entity>`.
    * `List<Curso> todosLosCursos = cursoRepository.findAll();`
    ```java
    // Método en CursoServiceImpl
    @Override
    public List<Curso> findAll() {
        // Aquí iría lógica de negocio
        List<Curso> todosLosCursos = cursoRepository.findAll(); // Llama al método del repositorio
        return todosLosCursos;
    }
    ```
* **Eliminar por ID**: Usen `deleteById(id)`.
    * `cursoRepository.deleteById(id);`
    ```java
    // Método en CursoServiceImpl
    @Override
    public void deleteById(Long id) {
        // Aquí iría lógica de negocio (ej. verificar si existe antes de borrar)
        cursoRepository.deleteById(id); // Llama al método del repositorio
    }
    ```

### Métodos de Consulta Derivados del Nombre (Derived Query Methods):

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


### Actividad/Demostración:

* En su proyecto, creen una interfaz Repository para una entidad (ej. `ProductoRepository`).
* En una clase de servicio que use este repositorio, inyecten el repositorio.
* Implementen métodos de servicio simples para las operaciones CRUD básicas utilizando los métodos del repositorio inyectado.
* Añadan algunos métodos de consulta derivados del nombre en su interfaz Repository y usenlos en su servicio.

### Asistencia de Cursor AI:

Pídanle a Cursor AI que les ayude a escribir métodos de servicio que usan operaciones del repositorio. Pídanle que les genere la firma de un método de consulta personalizado en el repositorio (ej. "Add a method to search orders placed after a specific date in OrderRepository").

## Relaciones entre Entidades

El poder de las bases de datos relacionales radica en la conexión de tablas a través de relaciones. JPA nos permite modelar estas relaciones directamente en nuestras clases de entidad Java.

### Tipos de Relaciones y Cómo Mapearlas con Anotaciones:

* **Uno a Uno (1:1)**: Ej. Usuario y Perfil de Usuario.
* **Uno a Muchos (1:N)**: Ej. Departamento y Empleados (Departamento 1 -> N Empleados). Pedido y Items de Pedido.
* **Muchos a Uno (N:1)**: Es la otra vista de 1:N. Ej. Empleado pertenece a UN Departamento (Empleado N -> 1 Departamento). Item de Pedido pertenece a UN Pedido. Esta es la vista donde está la clave foránea en la base de datos.
* **Muchos a Muchos (N:N)**: Ej. Estudiantes toman Cursos (Un estudiante muchos cursos, un curso muchos estudiantes). Requiere una **tabla intermedia** en la base de datos.

#### Uno a Uno (`@OneToOne`):

Una instancia de Entidad A se relaciona con como máximo una de Entidad B, y viceversa.

* **Mapeo**: 
    * Se usa `@OneToOne` en un atributo en _ambas_ clases de entidad.
    * Un lado es el **propietario (owning side)**, donde se define la clave foránea (generalmente con `@JoinColumn`).
    * El otro es el **inverso (inverse side)**, donde se usa `mappedBy` para referenciar el atributo en el lado propietario.
* **Ejemplo**: `Usuario` y `PerfilUsuario`. Si la FK está en la tabla `perfiles_usuario`, `PerfilUsuario` es el propietario.
    ```Java
    // En la clase Usuario.java (Lado Inverso)
    import jakarta.persistence.*;

    @Entity
    public class Usuario {
        @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        // ... otros campos de Usuario ...

        @OneToOne(mappedBy = "usuario", cascade = CascadeType.ALL) // mappedBy = "atributo en la clase PerfilUsuario"
        private PerfilUsuario perfil; // No hay FK en la tabla 'usuarios' para esta relación aquí

        // ... constructor, getters, setters ...
    }
    ```

    ```Java
    // En la clase PerfilUsuario.java (Lado Propietario)
    import jakarta.persistence.*;

    @Entity
    public class PerfilUsuario {
        @Id // El ID podría ser el mismo que el del Usuario relacionado, o autogenerado
        private Long id;
        // ... otros campos de Perfil ...

        @OneToOne // Un PerfilUsuario pertenece a UN Usuario
        @JoinColumn(name = "usuario_id") // Especifica la columna FK en la tabla 'perfiles_usuario'
        private Usuario usuario; // Campo que referencia al Usuario relacionado

        // ... constructor, getters, setters ...
    }
    ```

#### Uno a Muchos (`@OneToMany`) / Muchos a Uno (`@ManyToOne`):

Es la relación más común. Una instancia de Entidad A tiene _cero o muchos_ de Entidad B, pero cada Entidad B pertenece a como máximo _una_ de Entidad A.
* **Mapeo**: 
    * Se usa `@ManyToOne` en el lado "Muchos" de una relación 1:N o N:1 (ej. `Pedido` tiene `@ManyToOne` Cliente) y representa la columna que tiene la **clave foránea** (propietario de la relación).
    * Se usa `@OneToMany` en el lado "Uno" de una relación 1:N o N:1 (ej. `Cliente` tiene `@OneToMany` colección de `Pedido`) y generalmente es el lado **inverso**, usando `mappedBy` para referenciar el atributo en el lado `@ManyToOne`. Generalmente se mapea a una colección (ej. `List`, `Set`).
* **Ejemplo**: `Cliente` y `Pedido` (un Cliente tiene muchos Pedidos, un Pedido tiene un Cliente).
    ```Java
    // En la clase Pedido.java (Lado Propietario)
    import jakarta.persistence.*;

    @Entity
    public class Pedido {
        @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        // ... otros campos de Pedido ...

        @ManyToOne // Un Pedido pertenece a UN Cliente
        @JoinColumn(name = "cliente_id") // Especifica la columna FK en la tabla 'pedidos'
        private Cliente cliente; // Campo que referencia al Cliente relacionado

        // ... constructor, getters, setters ...
    }
    ```
    La anotación `@JoinColumn(name = "nombre_columna_fk")` es crucial para especificar el nombre de la columna de clave foránea en esta tabla (`pedidos`).

    ```Java
    // En la clase Cliente.java (Lado Inverso)
    import jakarta.persistence.*;
    import java.util.List; // O Set

    @Entity
    public class Cliente {
        @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        // ... otros campos de Cliente ...

        // mappedBy = "atributo en la clase Pedido que referencia al Cliente"
        @OneToMany(mappedBy = "cliente", cascade = CascadeType.ALL, orphanRemoval = true)
        private List<Pedido> pedidos; // Colección de Pedidos de este Cliente

        // Método de conveniencia para mantener ambos lados de la relación sincronizados
        public void addPedido(Pedido pedido) {
            pedidos.add(pedido);
            pedido.setCliente(this); // ¡Muy Importante! Establecer el lado ManyToOne
        }
        public void removePedido(Pedido pedido) {
            pedidos.remove(pedido);
            pedido.setCliente(null); // ¡Muy Importante! Eliminar el vínculo en el lado ManyToOne
        }

        // ... constructor, getters, setters ...
    }
    ```
    `mappedBy = "cliente"`: El campo `cliente` en la entidad `Pedido` es el que "manda" en esta relación a nivel de base de datos (contiene la clave foránea).


#### Muchos a Muchos (`@ManyToMany`):

Una instancia de Entidad A se relaciona con _muchas_ de Entidad B, y una instancia de Entidad B se relaciona con _muchas_ de Entidad A.

* **Mapeo**:
    * Se usa en ambos lados de una relación N:N.
    * Requiere una **tabla de unión/enlace** en la base de datos (ej. `estudiante_curso`). Esta tabla tiene claves foráneas a las claves primarias de ambas tablas (`estudiantes` y `cursos`). 
    * Se usa `@ManyToMany` en un atributo en ambas clases de entidad. 
    * Uno de los lados es el **propietario**, usando `@JoinTable` para definir el nombre de la tabla de unión y los nombres de las columnas de claves foráneas en esa tabla.
    * El otro lado es el `inverso`, usando `mappedBy` para referenciar el atributo en el lado propietario.
* **Ejemplo**: `Estudiante` y `Curso` (un Estudiante toma muchos Cursos, un Curso tiene muchos Estudiantes).
    ```Java
    // En la clase Estudiante.java (Lado Propietario, ejemplo)
    import jakarta.persistence.*;
    import java.util.Set; // O List

    @Entity
    public class Estudiante {
        @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        // ... otros campos ...

        @ManyToMany // Muchos Estudiantes a Muchos Cursos
        @JoinTable( // Define la tabla de unión y sus columnas FKs
            name = "estudiante_curso", // Nombre de la tabla de unión
            joinColumns = @JoinColumn(name = "estudiante_id"), // Columna FK en tabla unión que apunta a ESTA entidad (Estudiante)
            inverseJoinColumns = @JoinColumn(name = "curso_id") // Columna FK en tabla unión que apunta a la OTRA entidad (Curso)
        )
        private Set<Curso> cursos = new HashSet<>(); // Colección de Cursos de este Estudiante

        // ... constructor, getters, setters, métodos addCurso/removeCurso ...
    }
    ```

    ```Java
    // En la clase Curso.java (Lado Inverso)
    import jakarta.persistence.*;
    import java.util.Set; // O List

    @Entity
    public class Curso {
        @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        // ... otros campos ...

        @ManyToMany(mappedBy = "cursos") // mappedBy = "atributo en la clase Estudiante que mapea esta relación"
        private Set<Estudiante> estudiantes = new HashSet<>(); // Colección de Estudiantes en este Curso

        // ... constructor, getters, setters ...
    }
    ```

#### Atributos de Relación Comunes:

* `mappedBy = "nombre_atributo"`: Se usa en el lado inverso de la relación (`@OneToOne`, `@OneToMany`, `@ManyToMany`) para indicar cuál es el atributo en el lado propietario que define la relación (donde está la clave foránea o la anotación `@JoinTable`). El lado inverso no gestiona la clave foránea. **¡Es muy común olvidarlo o confundirlo!**
* `cascade = CascadeType.TYPE`: Determina qué operaciones (persistir, eliminar, etc.) deben "cascadear" del propietario a la entidad relacionada.
    * `CascadeType.PERSIST`: Si persistes el propietario, también persiste la entidad relacionada.
    * `CascadeType.REMOVE`: Si eliminas el propietario, también elimina la entidad relacionada.
    * `CascadeType.ALL`: Todas las operaciones (`PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, `DETACH`). Úsenlo con cuidado.
* `fetch = FetchType.TYPE`: Controla cuándo se cargan los datos de la relación desde la base de datos.
    * `FetchType.LAZY`: La relación se carga solo cuando se accede a ella (por defecto para `@OneToMany`, `@ManyToMany`). Evita cargar datos innecesariamente, mejor rendimiento para relaciones grandes.
    * `FetchType.EAGER`: La relación se carga inmediatamente cuando se carga la entidad propietaria (por defecto para `@ManyToOne`, `@OneToOne`). Puede ser ineficiente si se cargan muchas relaciones a la vez.

#### Actividad/Demostración:

* En su proyecto, modificaremos las entidades `Cliente` y `Pedido`.
* Añadiremos las anotaciones `@ManyToOne` en `Pedido` y `@OneToMany` en `Cliente` para mapear la relación 1:N.
* Configuraremos `@JoinColumn` en el lado propietario (`@ManyToOne`).
* Configuraremos `mappedBy` en el lado inverso (`@OneToMany`).
* Discutiremos y añadiremos un `CascadeType` apropiado (ej. `PERSIST`, `REMOVE`) a la relación.
* Mostraremos cómo guardar un `Pedido` asociado a un `Cliente` existente y cómo guardar un `Cliente` con una nueva lista de` Pedidos` (si `cascade` está configurado).
* (Si da tiempo) Mostraremos el mapeo básico de una relación N:N con `@ManyToMany` y `@JoinTable`.

#### Asistencia de Cursor AI: 

Pídanle a Cursor AI que les ayude a añadir una relación entre dos entidades existentes (ej. "Add a One-to-Many relationship from Cliente to Pedido in these JPA entities"). Pídanle que explique el significado de mappedBy, cascade o fetch en el contexto de su código.

## Validación de Relaciones Complejas con Ayuda de Cursor AI

### Validación de Entidades Individuales (`@Valid`): 

Vimos en la clase anterior que `jakarta.validation` permite validar los campos de una entidad usando anotaciones como `@NotBlank`, `@Size`, etc., y que `@Valid` en el controlador activa esta validación.

### Validación de Entidades Relacionadas: 

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
    @Valid // <--- Si se valida el Cliente, también se validan las Direcciones en la lista
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
Cuando llamen al controlador con `@Valid Cliente cliente`, si la lista `direcciones` de ese `cliente` contiene objetos `Direccion`, se validarán sus campos automáticamente gracias al `@Valid` en la lista.

### Validación de Integridad Referencial (Existencia de FK en DB): 

Es importante entender que las anotaciones de validación de Bean Validation (`@NotNull`, `@Valid`, etc.) **no** verifican automáticamente si un ID de clave foránea que ustedes proporcionan realmente existe en la tabla referenciada en la base de datos.

 Esa verificación es responsabilidad de la restricción `FOREIGN KEY` en la base de datos o de la lógica que implementen en su capa de servicio (ej. buscar la entidad relacionada por ID antes de asignarla). 
 
 Si intentan guardar una entidad con un ID de clave foránea inválido, la base de datos lanzará un error de integridad referencial, que JPA/Spring convertirá en una excepción en su aplicación (típicamente `DataIntegrityViolationException`).

### Validación de Lógica de Negocio en la capa de Servicio: 

A menudo, la validación más compleja que involucra el estado de la base de datos (ej. ¿El libro existe? ¿Está disponible para prestar?) se realiza en la capa de servicio antes de guardar o modificar la entidad, no solo con anotaciones de validación.

### Actividad con Cursor AI:

* Pregúntenle a Cursor AI cómo validar entidades relacionadas usando @Valid. Por ejemplo: "How can I use @Valid to ensure that all Direccion entities within a Cliente entity are also validated when saving the Cliente?".
* Pregúntenle a Cursor AI sobre cómo manejar errores de integridad referencial cuando intentan guardar una entidad con una clave foránea que apunta a un registro que no existe. Por ejemplo: "What happens in Spring Boot/JPA if I try to save a Pedido with a client_id that does not exist in the database? How can I handle this error professionally?".

## Conclusión:

Hoy dimos un gran paso al aprender a mapear nuestras clases Java a estructuras de base de datos usando JPA. Cubrimos cómo usar Spring Data JPA para las operaciones CRUD básicas de forma elegante y, lo más importante, aprendimos a representar y mapear las relaciones entre nuestras entidades, que es fundamental para bases de datos relacionales.

¡Practiquen mucho el mapeo de entidades y relaciones en sus proyectos! En la próxima clase, exploraremos consultas más avanzadas y manejo de transacciones.