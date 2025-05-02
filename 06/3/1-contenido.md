# Consultas Avanzadas con JPQL, Criteria API y Spring Queries

¡Hola a todos!

Ya sabemos cómo conectar Spring Boot a una base de datos relacional, definir nuestras entidades y usar los métodos básicos de JpaRepository para operaciones CRUD simples. Sin embargo, en la vida real, necesitamos consultar nuestros datos de formas mucho más complejas: buscar por múltiples criterios, traer datos relacionados, ordenar, paginar, etc. En esta clase, exploraremos las herramientas que nos ofrece Spring Data JPA para construir consultas avanzadas y cómo optimizar su rendimiento.

## 1. Introducción a las Consultas en Spring Data JPA

### 1.1 Recordando lo Básico

Ya conocemos métodos como `findById(id)` y `findAll()` del `JpaRepository`. Son muy útiles para casos simples.

### 1.2 Métodos Derivados (Derived Query Methods)

Vimos brevemente en la Clase 2 que podemos crear métodos de consulta definiendo la firma del método en nuestra interfaz Repository siguiendo convenciones de nombres (ej. `findByNombre`). Hoy profundizaremos en esto.

### 1.3 ¿Por Qué Otras Opciones?

Para consultas más complejas (varias condiciones WHERE, JOINs, funciones agregadas, proyecciones), los métodos derivados pueden volverse muy largos o simplemente no ser suficientes. Necesitamos formas más flexibles como JPQL o Criteria API.

## 2. Métodos Derivados (Derived Query Methods) - Detallado

### 2.1 El Concepto

Spring Data JPA "lee" el nombre de los métodos que ustedes declaran en su interfaz `JpaRepository` y, si sigue ciertas convenciones, genera la implementación de la consulta SQL automáticamente.

### 2.2 Convenciones de Nombres

Comienzan con palabras clave como `find`, `get`, `query`, `count`, `delete`, `exists`. Luego viene el predicado (las condiciones `WHERE`).

* `findBy...`: Encuentra entidades por ciertos atributos.
* `countBy...`: Cuenta entidades por ciertos atributos.
* `deleteBy...`: Elimina entidades por ciertos atributos.
* `findDistinctBy...`: Encuentra entidades únicas por ciertos atributos.

### 2.3 Palabras Clave Comunes en el Predicado

Se combinan usando `And` y `Or`.

* `findByAtributo`: Busca por un valor exacto del atributo.
* `findByAtributo1AndAtributo2`: Busca por dos condiciones unidas por `AND`.
* `findByAtributo1OrAtributo2`: Busca por dos condiciones unidas por `OR`.
* `Between`: Rango (ej. `findByPrecioBetween(min, max)`).
* `LessThan`, `LessThanEqual`, `GreaterThan`, `GreaterThanEqual`: Comparaciones (<, <=, >, >=).
* `After`, `Before`: Comparaciones para fechas/tiempos.
* `Like`, `Containing`, `StartingWith`, `EndingWith`: Búsquedas de texto con patrones (el signo `%` se añade automáticamente). `Containing` es `LIKE %valor%`.
* `IsNull`, `IsNotNull`: Chequea si el atributo es NULL o no NULL.
* `OrderByAtributoAsc`, `OrderByAtributoDesc`: Ordena los resultados.
* `TopN`, `FirstN`: Limita el número de resultados (ej. `findTop5BySalarioDesc()`).

### 2.4 Limitaciones

Los nombres pueden volverse extremadamente largos para consultas muy complejas. No soportan todas las funcionalidades de SQL/JPQL (ej. funciones agregadas complejas, subconsultas avanzadas).

### 2.5 Actividad/Demostración

* En su `ProductoRepository` (o una entidad similar con varios campos), añadiremos los siguientes métodos:

  ```java
  // Buscar productos por nombre (ignorando mayúsculas/minúsculas)
  List<Producto> findByNombreContainingIgnoreCase(String nombre);

  // Buscar productos con precio mayor a un valor y con stock > 0
  List<Producto> findByPrecioGreaterThanAndStockGreaterThan(Double precio, Integer stock);

  // Contar productos por categoría
  // (Asumiendo una relación ManyToOne a Categoria con atributo 'categoria')
  long countByCategoriaNombre(String nombreCategoria);

  // Eliminar productos con stock cero
  int deleteByStock(Integer stock); // Spring Data puede ejecutar la eliminación

  // Buscar los 5 productos más caros
  List<Producto> findTop5ByPrecioDesc();
  ```

* Implementaremos el uso de estos métodos en una clase de servicio.
* **Asistencia de Cursor AI**: Descríbanle a Cursor AI la consulta que necesitan en lenguaje natural (ej. "Add a method to find all books published after a specific year and sort them by title ascending"). Cursor AI les sugerirá la firma correcta del método derivado en su interfaz Repository.

## 3. Introducción a JPQL (Java Persistence Query Language)

### 3.1 ¿Qué es JPQL?

Es un lenguaje de consulta orientado a objetos definido por la especificación JPA. A diferencia de SQL que opera en tablas y columnas, JPQL opera en las entidades y sus atributos.

  ```sql
  SELECT o FROM Order o WHERE o.customer.id = :customerId 
  ```

Aquí `Order` es el nombre de la clase entidad, `o` es un alias, `customer` es un atributo de relación en `Order`, `id` es un atributo de `customer`, `:customerId` es un parámetro.

### 3.2 Uso de @Query para Consultas JPQL Personalizadas

Para usar JPQL, se escribe la consulta como una cadena de texto en la anotación @Query encima de un método en la interfaz Repository.

  ```Java
  package com.example.apicursos.repository;

  import com.example.apicursos.model.Pedido;
  import org.springframework.data.jpa.repository.JpaRepository;
  import org.springframework.data.jpa.repository.Query;
  import java.util.List;

  public interface PedidoRepository extends JpaRepository<Pedido, Long> {

      // Consulta JPQL para encontrar pedidos de un cliente específico
      @Query("SELECT p FROM Pedido p WHERE p.cliente.id = :clienteId ORDER BY p.fechaPedido DESC")
      List<Pedido> findPedidosByClienteId(@Param("clienteId") Long clienteId); // Usar @Param para parámetros con nombre

      // Consulta JPQL con JOIN implícito para encontrar pedidos con total mayor a un valor
      @Query("SELECT p FROM Pedido p WHERE p.total > :minTotal")
      List<Pedido> findPedidosWithTotalGreaterThan(@Param("minTotal") Double minTotal);

      // JPQL para obtener solo ciertos campos (Proyección) o construir DTOs
      // Esto es más avanzado y requiere definir interfaces o constructores DTO
      // @Query("SELECT p.id, p.fechaPedido FROM Pedido p WHERE ...") // Ejemplo de selección parcial
  }
  ```

* `@Query("SELECT p FROM Pedido p ...")`: La consulta JPQL como una cadena de texto.
* `@Param("nombreParametro")`: Se usa en los parámetros del método para vincularlos a los parámetros con nombre en la consulta JPQL (ej. `:clienteId`). Si los parámetros en JPQL son por posición (`?1`, `?2`), se usan los parámetros del método sin `@Param`.

### 3.3 Ventajas de JPQL

* Más flexible que los métodos derivados para consultas complejas, joins, subconsultas, etc.
* Aún opera a nivel de objetos JPA, lo que es más seguro que SQL nativo al refactorizar nombres de clases/atributos.

### 3.4 Desventajas de JPQL

* La consulta es una cadena de texto. Errores tipográficos en nombres de entidades o atributos no se detectan hasta que la aplicación se ejecuta.

### 3.5 Actividad/Demostración

* En su `PedidoRepository` o similar, añadiremos métodos usando `@Query` con consultas JPQL para:
  * Encontrar pedidos de un cliente específico (usando la relación `@ManyToOne` cliente).
  * Encontrar pedidos con un total mayor a un valor.
* Mostraremos cómo usar `@Param` para pasar parámetros a la consulta.
* **Asistencia de Cursor AI**: Pídanle a Cursor AI que les ayude a escribir consultas JPQL para un requisito específico. Por ejemplo: "Write a JPQL query to find all products in a specific order, given the order ID. Assume a Many-to-Many relationship between Order and Product". La IA generará la consulta JPQL y la firma del método `@Query`.

## 4. Consultas Nativas con @Query

### 4.1 ¿Cuándo usar SQL Nativo?

A veces, JPQL no es suficiente. Necesitan usar características muy específicas de la base de datos (ej. funciones geoespaciales de PostgreSQL), consultas muy complejas que son más fáciles en SQL nativo, o necesitan optimizar manualmente una consulta crítica usando SQL exacto.

### 4.2 Uso de @Query(nativeQuery = true)

Pueden escribir consultas SQL nativas directamente en la anotación `@Query` añadiendo el atributo `nativeQuery = true`.

  ```Java
  package com.example.apicursos.repository;

  import com.example.apicursos.model.Producto;
  import org.springframework.data.jpa.repository.JpaRepository;
  import org.springframework.data.jpa.repository.Query;
  import java.util.List;

  public interface ProductoRepository extends JpaRepository<Producto, Long> {

      // Consulta SQL Nativa para encontrar productos con precio exacto usando función DB específica (ejemplo)
      // (Nota: Este ejemplo es simple, nativeQuery es más útil para cosas complejas)
      @Query(value = "SELECT * FROM productos WHERE precio = ?1", nativeQuery = true)
      List<Producto> findByPrecioNativo(Double precio);

      // Consulta nativa que podría usar alguna función específica de PostgreSQL
      // @Query(value = "SELECT p.* FROM productos p WHERE p.nombre LIKE :patronEscapado", nativeQuery = true)
      // List<Producto> findByNombreLikeNativo(@Param("patronEscapado") String patron);
  }
  ```

* `@Query(value = "...", nativeQuery = true)`: La consulta SQL nativa.
* Pueden usar parámetros posicionales (?1) o con nombre (:nombre).

### 4.3 Desventajas de SQL Nativo

* No es `type-safe`. Errores en nombres de tablas o columnas solo se detectan en tiempo de ejecución.
* Menos portable entre diferentes bases de datos (el SQL puede variar).

### 4.4 Asistencia de Cursor AI

Pídanle a Cursor AI que les ayude a escribir consultas SQL nativas para requisitos específicos. Por ejemplo: "Write a native SQL query for PostgreSQL to find the top 10 most expensive products".

## 5. Paginación y Ordenamiento con `Pageable`, `Sort`

### 5.1 El Problema de Listas Largas

`findAll()` trae todos los registros. Si hay miles o millones, esto es ineficiente y consume mucha memoria. Necesitamos traer los datos en "páginas" y ordenarlos.

### 5.2 `Sort`

Permite especificar criterios de ordenamiento (por qué atributo(s), ascendente/descendente).

* Se crea una instancia de `Sort`.
* Se puede añadir como parámetro a métodos Repository existentes (`findAll(Sort sort)`, `findBy... (..., Sort sort)`).

### 5.3 `Pageable`

Combina información de paginación (número de página, tamaño de página) y ordenamiento.

* Es una interfaz. Spring Data la implementa.
* Se añade como parámetro a métodos Repository (`findAll(Pageable pageable)`, `findBy... (..., Pageable pageable)`).
* Los métodos que aceptan `Pageable` típicamente devuelven un objeto `Page<Entity>`, que contiene la lista de entidades para esa página más información útil (número total de páginas, número total de elementos, si es la primera/última página, etc.).

  ```Java
  package com.example.apicursos.repository;

  import com.example.apicursos.model.Curso;
  import org.springframework.data.domain.Page; // Importar Page
  import org.springframework.data.domain.Pageable; // Importar Pageable
  import org.springframework.data.jpa.repository.JpaRepository;

  public interface CursoRepository extends JpaRepository<Curso, Long> {

      // Encuentra cursos por nombre (puede ser derivado o @Query) con paginación y ordenamiento
      Page<Curso> findByNombreContainingIgnoreCase(String nombre, Pageable pageable);

      // JPQL con paginación
      @Query("SELECT c FROM Curso c WHERE c.creditos > :minCreditos")
      Page<Curso> findByCreditosGreaterThan(@Param("minCreditos") Integer minCreditos, Pageable pageable);

      // findAll con paginación
      // Page<Curso> findAll(Pageable pageable); // Este método ya existe en JpaRepository
  }
  ```

* Cómo usar Pageable en el Servicio:

  ```Java
  // En CursoServiceImpl
  import org.springframework.data.domain.Page;
  import org.springframework.data.domain.Pageable;
  import org.springframework.data.domain.PageRequest; // Implementación concreta de Pageable
  import org.springframework.data.domain.Sort; // Para ordenar

  // ... inyección de repositorio ...

  public Page<Curso> getCursosPaginados(int page, int size, String sortBy, String sortDirection) {
      // Crear un objeto Sort
      Sort sort = Sort.by(sortDirection.equalsIgnoreCase("asc") ? Sort.Direction.ASC : Sort.Direction.DESC, sortBy);

      // Crear un objeto Pageable
      Pageable pageable = PageRequest.of(page, size, sort);

      // Llamar al método del repositorio que acepta Pageable
      // Si usan un método findBy, le pasan Pageable al final:
      // return cursoRepository.findByNombreContainingIgnoreCase("...", pageable);

      // Si es findAll con paginación:
      return cursoRepository.findAll(pageable);
  }
  ```

### 5.4 Actividad/Demostración

* Modificaremos algunos métodos Repository (`findAll`, `findByNombreContainingIgnoreCase`) para que acepten un parámetro `Pageable` y devuelvan un `Page<Entity>`.
* En la clase de servicio, mostraremos cómo construir un objeto `Pageable` (especificando número de página, tamaño y criterios de ordenamiento) y pasarlo al método Repository.
* **Asistencia de Cursor AI**: Pídanle a Cursor AI que añada parámetros Pageable y Sort a métodos Repository existentes. Pídanle ayuda para construir un objeto Pageable o Sort en su código de servicio. Pídanle que explique el objeto Page y la información que contiene.

## 6. Consultas Dinámicas con Criteria API

### 6.1 El Problema

* ¿Qué pasa si necesitamos construir una consulta donde las condiciones `WHERE` dependen de muchos parámetros de entrada opcionales (filtros)? (Ej. Buscar productos filtrando por nombre, descripción, precio mínimo, precio máximo, categoría, stock... ¡pero solo si el usuario especifica esos filtros!).
* JPQL/SQL Nativo con muchas condiciones `if / else` es complicado y propenso a errores.

### 6.2 Criteria API

Es una API de Java para construir consultas de forma programática, utilizando el modelo de objetos (type-safe). Es más segura al refactorizar que las cadenas de texto.

#### 6.2.1 Componentes Principales

* `CriteriaBuilder`: Fábrica para crear elementos de consulta (FROM, WHERE, SELECT, etc.).
* `CriteriaQuery`: El objeto que representa la consulta que están construyendo.
* `Root<Entity>`: Representa la entidad base desde la que consultan (el FROM).
* `Predicate`: Representa una condición `WHERE` (ej. `cb.equal(root.get("nombre"), "Laptop")`). Se pueden combinar con `cb.and()`, `cb.or()`.

#### 6.2.2 Ejemplo (Concepto - Es más detallado en código)

  ```Java
  // Código dentro de un método personalizado en un Repository o un servicio
  // Usualmente requiere inyectar EntityManagerFactory o EntityManager
  // ...

  CriteriaBuilder cb = entityManager.getCriteriaBuilder();
  CriteriaQuery<Producto> cq = cb.createQuery(Producto.class);
  Root<Producto> producto = cq.from(Producto.class);

  List<Predicate> predicates = new ArrayList<>();

  // Añadir condiciones si los filtros están presentes
  if (nombreFiltro != null) {
      predicates.add(cb.like(producto.get("nombre"), "%" + nombreFiltro + "%"));
  }
  if (precioMinFiltro != null) {
      predicates.add(cb.greaterThanOrEqualTo(producto.get("precio"), precioMinFiltro));
  }
  // ... más condiciones ...

  // Combinar las condiciones con AND
  cq.where(cb.and(predicates.toArray(new Predicate[0])));

  // Opcional: añadir ordenamiento, paginación

  // Obtener resultados
  TypedQuery<Producto> query = entityManager.createQuery(cq);
  List<Producto> resultados = query.getResultList();
  ```

#### 6.2.3 Ventajas de Criteria API

* **Type-safe**: Errores en nombres de atributos se detectan en tiempo de compilación.
* Ideal para construir consultas dinámicas con muchos filtros opcionales.
* **Programática**: Permite lógica compleja para construir la consulta.

#### 6.2.4 Desventajas de Criteria API

* Mucho más **verbosa** que los métodos derivados o JPQL para consultas simples.
* Requiere un entendimiento más profundo de la API.

#### 6.2.5 Spring Data JPA y Criteria API

Spring Data JPA se integra con Criteria API, a menudo a través de Specifications o simplemente obteniendo el EntityManager para construir la consulta directamente.

### 6.3 Actividad/Demostración

* Mostraremos un ejemplo básico de cómo construir una consulta simple usando Criteria API para buscar por un campo.
* **Asistencia de Cursor AI**: Dado que Criteria API es verbosa, Cursor AI es un excelente asistente aquí. Descríbanle la consulta dinámica que necesitan (ej. "Write Java code using JPA Criteria API to search for products by name (optional) and/or price range (optional)"). Cursor AI puede generar el código Criteria API necesario.

## 7. Cursor AI como Generador Automático y Optimizador de Consultas/Filtros

### 7.1 Resumen de cómo Cursor AI ayuda con las consultas

* **Métodos Derivados**: Genera la firma correcta basándose en descripciones en lenguaje natural.
* **JPQL/SQL Nativo**: Ayuda a escribir la cadena de consulta, explica partes de la consulta, sugiere correcciones, genera la firma del método `@Query`.
* **Criteria API**: Genera el código Java verbose para construir la consulta.
* **Paginación/Ordenamiento**: Ayuda a añadir parámetros `Pageable`/`Sort` y a usar el objeto `Page`.
* **Optimización (Diagnóstico)**: Si tienen `spring.jpa.show-sql=true` activado, pueden copiar el SQL que ven en la consola y preguntarle a Cursor AI si parece eficiente o si hay un problema (ej. el problema N+1).
* **Sugerencias de Indexación**: Aunque la IA no modifica la DB por ustedes, si ven consultas lentas, pueden preguntarle: "Given this query [pegar SQL], what database indexes might improve its performance?".
* **Ayuda con `Specifications`**: Spring Data JPA tiene un patrón llamado `Specifications` (basado en Criteria API) para crear filtros dinámicos y reutilizables. Cursor AI puede ayudar a generarlos.

### 7.2 Actividad Continua

A medida que implementan sus consultas (métodos derivados, JPQL, Criteria), utilicen activamente Cursor AI para:

* Generar el código inicial.
* Explicar partes que no entienden.
* Sugerir alternativas.
* Revisar su código de consulta en busca de errores o ineficiencias obvias.

## 8. Optimización de Consultas y Estructuras para Eficiencia en Tiempo Real

### 8.1 La Importancia de la Eficiencia

Consultas lentas en una aplicación con muchos usuarios pueden afectar gravemente el rendimiento general y la experiencia del usuario.

### 8.2 Factores Clave de Optimización (Más allá del Código JPA)

#### 8.2.1 Índices de Base de Datos

¡Lo más importante para muchas consultas! Los índices son estructuras en la base de datos que aceleran la búsqueda de filas. Deben crear índices en las columnas que usan frecuentemente en sus cláusulas `WHERE`, `JOIN`, y `ORDER BY`. Se definen en el esquema de la base de datos (`CREATE INDEX ...`), no en el código JPA (aunque algunas anotaciones como `@Column(index=true)` pueden sugerirlo a Hibernate). **Asegúrense de tener índices en las columnas usadas en sus consultas principales.**

#### 8.2.2 Tipo de Carga (Fetch Type)

`EAGER` (`@ManyToOne`, `@OneToOne` por defecto) carga inmediatamente las relaciones. Si cargan una lista de 100 entidades y cada una tiene una relación @`ManyToOne` `EAGER`, JPA podría ejecutar 101 consultas (N+1 problem).

`LAZY` (`@OneToMany`, `@ManyToMany` por defecto) carga la relación solo cuando acceden a ella.

#### 8.2.3 Problema N+1

Es cuando al cargar una lista de entidades, JPA ejecuta una consulta extra por cada entidad cargada para traer una relación `EAGER` o una relación `LAZY` a la que se accede en un bucle.

**Ejemplo del Problema N+1 y Solución con `JOIN FETCH`:**

```java
// Problema N+1
@Query("SELECT p FROM Producto p")
List<Producto> findAllProductos(); // Esto puede generar múltiples consultas para relaciones EAGER

// Solución con JOIN FETCH
@Query("SELECT p FROM Producto p JOIN FETCH p.categoria")
List<Producto> findAllProductosConCategoria(); // Carga productos y sus categorías en una sola consulta
```

#### 8.2.4 Seleccionar Solo lo Necesario

Si solo necesitan el nombre y el precio de un producto en una consulta, es más eficiente seleccionar solo esas columnas en lugar de cargar toda la entidad Producto con todos sus atributos y relaciones. Pueden hacer esto con JPQL seleccionando atributos específicos (`SELECT p.nombre, p.precio FROM Producto p ...`) y mapeando el resultado a un DTO o una interfaz.

#### 8.2.5 Elegir el Método de Consulta Correcto

Usen métodos derivados para lo simple, JPQL para consultas complejas pero orientadas a objetos, Criteria API para consultas muy dinámicas, y SQL nativo solo cuando sea realmente necesario.

#### 8.2.6 Observar el SQL Generado

Usen `spring.jpa.show-sql=true` para ver qué SQL está ejecutando JPA/Hibernate y si parece eficiente. Busquen múltiples consultas inesperadas (problema N+1).

### 8.3 Asistencia de Cursor AI para Optimización

* Si activan `show-sql`, copien el SQL que les parece sospechoso y pregunten a Cursor AI si es eficiente y por qué. Pídanle sugerencias para optimizarlo (ej. "This SQL query seems slow. Can you suggest ways to optimize it?").
* Si ven múltiples consultas para cargar relaciones (problema N+1), descríbanle el escenario y pídanle cómo usar `JOIN FETCH` en JPQL o Criteria API para cargarlas en una sola consulta.
* Pídanle ayuda para escribir consultas JPQL o Criteria que seleccionen solo campos específicos o que usen `JOIN FETCH`.
* Pregúntenle sobre la indexación de base de datos y qué columnas en sus tablas deberían tener índices basándose en sus consultas.

## 9. Conclusión

Hoy expandimos enormemente nuestras capacidades de consulta con Spring Data JPA. Aprendimos a usar los métodos derivados de forma más potente, a escribir consultas personalizadas con JPQL y SQL nativo, a manejar paginación y ordenamiento con `Pageable`, y a construir consultas dinámicas con Criteria API. Más importante aún, comenzamos a pensar en la eficiencia de nuestras consultas, considerando índices, tipos de carga y cómo evitar el problema N+1. Spring Data JPA nos da las herramientas, y saber cuándo usar cada una, combinado con la asistencia de Cursor AI, es clave para aplicaciones eficientes.

¡Practiquen creando diferentes tipos de consultas en sus proyectos y observen el SQL generado! Utilicen Cursor AI para experimentar con JPQL, Criteria y la optimización.
