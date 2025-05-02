# Ejercicio Autónomo: Consultas Avanzadas en su API de Biblioteca

¡Hola!

Ya exploramos a fondo las diferentes formas de consultar datos con Spring Data JPA: métodos derivados, JPQL, Criteria API, paginación y ordenamiento. También vimos cómo empezar a analizar el rendimiento de esas consultas. Ahora es momento de poner todo eso en práctica y llevar las capacidades de su API de Biblioteca a un nuevo nivel.

## Objetivo del Ejercicio

Implementar una serie de consultas avanzadas en su proyecto de API de Clientes, Pedidos y Productos, utilizando las diferentes técnicas de Spring Data JPA aprendidas, y realizar un análisis básico de rendimiento de al menos una de esas consultas con la ayuda de EXPLAIN ANALYZE y Cursor AI.

## Escenario del Ejercicio: Consulta y Análisis en la Biblioteca

Continuarán trabajando en su proyecto existente de API de Biblioteca con las entidades Cliente, Pedido y Producto, incluyendo la relación N:N entre Pedido y Producto y la relación 1:N entre Cliente y Pedido.

Implementarán las siguientes consultas, eligiendo la técnica de Spring Data JPA más adecuada para cada una:

### 1. Consulta por Método Derivado + Ordenamiento

#### 1.1 Requerimiento

Encontrar todos los **Productos** publicados en un **año específico o posterior**, ordenados por **precio** (ascendente o descendente, según lo indique un parámetro).

#### 1.2 Tu Tarea

Añadir un método a `ProductoRepository` que use la convención de nombres de Spring Data JPA para este filtro y que acepte un objeto `Sort` como parámetro. Implementar el método de servicio correspondiente.

### 2. Consulta con JPQL + JOIN + Parámetros + Filtro Opcional + Paginación

#### 2.1 Requerimiento

Encontrar todos los **Pedidos** realizados por un **Cliente** cuyo **nombre** contenga un texto dado (ignorando mayúsculas/minúsculas). Opcionalmente, filtrar solo los pedidos realizados **después de una fecha determinada**. Los resultados deben estar **paginados**.

#### 2.2 Tu Tarea

Añadir un método a `PedidoRepository` usando la anotación `@Query` con una consulta JPQL que involucre un `JOIN` implícito o explícito a la entidad `Cliente`. La consulta debe aceptar el texto de búsqueda del cliente y la fecha (esta última opcional, manejando la condición en el JPQL si es posible o en el servicio/Criteria API). Debe aceptar un parámetro `Pageable` y devolver `Page<Pedido>`. Implementar el método de servicio que construya los parámetros y llame al repositorio.

### 3. Consulta Dinámica con Criteria API + Múltiples Filtros Opcionales + Paginación + Ordenamiento

#### 3.1 Requerimiento

- Implementar un endpoint de búsqueda avanzada para **Clientes** que permita filtrar dinámicamente por:
  - Nombre (búsqueda parcial, opcional).
  - Correo electrónico (búsqueda parcial, opcional).
  - Rango de fecha de registro (fecha de inicio y fecha de fin, ambos opcionales).
- Los resultados deben estar paginados y ordenables por cualquier campo relevante (ej. nombre, fecha de registro).

#### 3.2 Tu Tarea

Implementar un método de servicio que reciba los filtros opcionales (pueden ser `String` o `LocalDate` que pueden ser `null`) y un objeto `Pageable`. Utilizar Criteria API para construir la cláusula `WHERE` de la consulta combinando los predicados solo para los filtros que no son `null`.

### 4. Consulta con Agregación y Agrupación (JPQL o SQL Nativo)

#### 4.1 Requerimiento

Obtener el **conteo total de Pedidos por cada Cliente**, pero **solo incluir a los Clientes que tengan más de X pedidos** (siendo X un valor dado, ej. 5). El resultado podría ser una lista de objetos que contengan el nombre del cliente y el conteo de pedidos.

#### 4.2 Tu Tarea

Añadir un método a su `ClienteRepository` o `PedidoRepository` usando la anotación `@Query`. Escribir una consulta JPQL o SQL nativo que use `COUNT()`, `GROUP BY` y `HAVING` para lograr este resultado. (JPQL es preferible si es posible). Implementar el método de servicio correspondiente.

### 5. Análisis de Rendimiento de una Consulta con EXPLAIN ANALYZE y Cursor AI

#### 5.1 Requerimiento

Seleccionar al menos una de las consultas implementadas en los puntos 2, 3, o 4 (idealmente una que involucre un `JOIN` o un filtro en una columna que aún no tiene índice).

#### 5.2 Tu Tarea

- Asegurarse de que `spring.jpa.show-sql=true` está activado y ejecutar la consulta seleccionada desde su aplicación para obtener el SQL generado por Hibernate.
- En pgAdmin o DBeaver, ejecutar el SQL generado precedido por `EXPLAIN ANALYZE`.
- Copiar la sentencia SQL generada y todo el resultado del `EXPLAIN ANALYZE`.
- Pegar ambos en el chat de **Cursor AI** y pedirle un análisis detallado: "Analyze this SQL query and its EXPLAIN ANALYZE output. Explain why it might be slow, identify the bottlenecks, and suggest potential database indexes (CREATE INDEX statements) that could improve its performance for this query."

#### 5.3 Tu Tarea

Analizar la respuesta de la IA. Entender qué parte de la consulta o qué operación es costosa y qué índice sugiere la IA. (No es necesario crear el índice en la base de datos para este ejercicio, solo identificarlo y entender por qué se sugiere).

## Pasos Generales a Seguir para Cada Requerimiento (1 a 4)

1. **Analizar el Requerimiento**: Decidir la mejor técnica (Método Derivado, JPQL @Query, Criteria API).
2. **Ir al Repositorio**: Añadir la firma del método con la anotación adecuada (`@Query` con JPQL/nativeQuery, o simplemente la firma para método derivado).
3. **Ir al Servicio**: Inyectar el Repositorio. Crear el método de servicio que implemente la lógica (construir parámetros, construir `Pageable`, llamar al repositorio). Si usan Criteria API, la mayor parte de la implementación va aquí.
4. **Ir al Controlador**: Añadir el endpoint REST (`@GetMapping`, etc.) que reciba los parámetros y el Pageable. Llamar al método de servicio.
5. **Actualizar Documentación**: Añadir anotaciones de Swagger (`@Operation`, etc.) al nuevo endpoint y, si es necesario, a las entidades afectadas.
6. **Probar**: Utilizar un cliente REST o Swagger UI para probar el nuevo endpoint con diferentes parámetros.

## Entrega

Deberán actualizar su repositorio de GitHub con el código implementado para estas consultas avanzadas. Asegúrense de que el README.md incluya una sección explicando cada una de las consultas implementadas, cómo llamarlas (URL, parámetros) y cuál fue el hallazgo principal del análisis de rendimiento (Punto 5) y qué índice sugirió la IA para esa consulta. Estén listos para demostrar estas consultas funcionando y explicar su implementación.

---

Este ejercicio es el corazón de la consulta de datos en Spring Data JPA. Les permitirá dominar las diferentes herramientas y empezar a pensar en la eficiencia. ¡Utilicen Cursor AI al máximo para navegar por la complejidad!

¡Mucho éxito!
