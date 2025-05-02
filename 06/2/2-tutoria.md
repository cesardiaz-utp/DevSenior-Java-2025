# Tutoría: Refuerzo en Entidades, Relaciones y Pruebas de Integridad

¡Hola a todos!

En la clase anterior nos sumergimos de lleno en Spring Data JPA, aprendiendo a mapear nuestras entidades Java a tablas, usar repositorios para CRUD y, crucialmente, a definir relaciones entre entidades. En esta tutoría, vamos a consolidar esos conocimientos. Nos enfocaremos en verificar que la implementación de relaciones y operaciones básicas sea correcta, afianzar el concepto de un modelo de dominio bien diseñado y utilizar herramientas (¡incluida la IA!) para probar la integridad de nuestros datos y detectar posibles errores.

## Objetivos de la sesión

* Verificar que han implementado correctamente las relaciones entre sus entidades JPA y las operaciones básicas de persistencia usando Spring Data JPA.
* Afianzar el concepto de cómo diseñar un modelo de dominio (sus entidades y relaciones) que sea coherente con la base de datos y escalable para su aplicación.

## Contenidos y Actividades

### 1. Taller de Creación y Ajuste de Relaciones entre Entidades (Usando Diagramas de Clases) (40 minutos)

#### 1.1 Propósito

Practicar el proceso completo de diseñar relaciones a nivel conceptual y traducirlas a código JPA, y aprender a ajustar ese diseño si cambian los requisitos.

#### 1.2 Descripción de la Actividad

1. Presentaremos un par de escenarios que requieran modelar relaciones entre entidades. Podrían ser escenarios nuevos o extensiones de los que ya hemos visto (ej. añadir autores a los libros, añadir detalles de perfil a los usuarios, añadir categorías a los productos).
2. Trabajando individualmente o en pequeños grupos, ustedes deberán:
    * Dibujar un diagrama simple (a mano alzada, en una pizarra o herramienta básica) que muestre las entidades involucradas y el tipo de relación entre ellas (1:1, 1:N, N:N). Indiquen la cardinalidad (ej. 1..* en el lado "Muchos").
    * Discutir quién es el "dueño" de la relación a nivel de base de datos (¿dónde va la clave foránea?) en el caso de 1:1 y 1:N. Para N:N, recuerden la tabla intermedia.
    * Traducir este diseño a código Java, añadiendo o modificando las entidades JPA correspondientes (`@Entity`) con las anotaciones de relación correctas (`@OneToOne`, `@ManyToOne`, `@OneToMany`, `@ManyToMany`) y atributos clave (`@JoinColumn`, `mappedBy`, `cascade`, `fetch`).
3. Presentaremos un "giro" o un requisito adicional al escenario inicial (ej. "Ahora, un libro puede tener varios autores", cambiando una relación 1:N a N:N). Ustedes deberán discutir cómo ajustarían su diseño de base de datos y el código JPA para acomodar este cambio.
4. Compartiremos y discutiremos algunos de los diseños y ajustes propuestos.

#### 1.3 Cómo Cursor AI ayuda en este Taller

* **Generar Código de Relación**: Describan la relación que quieren modelar (ej. "Generate the JPA entity code for a One-to-Many relationship from 'Departamento' to 'Empleado'"). Cursor AI les proporcionará el código con las anotaciones en ambos lados.
* **Explicar Atributos**: Si no entienden por qué necesitan mappedBy o qué CascadeType usar, seleccionen la anotación y pídanle a Cursor AI que se los explique en el contexto de su código.
* **Refactorizar Relaciones**: Si necesitan cambiar el tipo de una relación existente, pueden pedirle a Cursor AI que les ayude a refactorizar las entidades correspondientes. Por ejemplo: "Refactor these two entities to change the relationship between User and Address from One-to-One to One-to-Many".

### 2. Verificación de Consistencia entre Clases Java y Tablas Reales (30 minutos)

#### 2.1 Propósito

Asegurar que las definiciones de sus entidades JPA en el código Java coinciden exactamente con la estructura de las tablas en su base de datos PostgreSQL. Las inconsistencias son una fuente común de errores en tiempo de ejecución.

#### 2.2 Descripción de la Actividad

1. Trabajarán con su propio proyecto Spring Boot y su base de datos PostgreSQL (usando pgAdmin o DBeaver).
2. Seleccionen algunas de sus entidades clave (especialmente las que participan en relaciones).
3. Para cada entidad/tabla seleccionada:
    * Abran la clase JPA `@Entity` correspondiente en Cursor AI.
    * Abran la definición de la tabla en pgAdmin o DBeaver (pueden ver las columnas, sus tipos de datos, si permiten `NULL`, si son únicas, cuáles son las claves primarias y foráneas).
    * **Realicen una comparación manual columna por columna, atributo por atributo**:
        * ¿Cada atributo en la clase Java tiene una columna correspondiente en la tabla? (y viceversa, si esperan mapear todas las columnas).
        * ¿Los tipos de datos coinciden (ej. `String` en Java vs `VARCHAR`/`TEXT`, `Long` vs `BIGINT`, `LocalDate` vs `DATE`)?
        * ¿La anotación `@Column(nullable = false)` en Java coincide con la restricción `NOT NULL` en la columna de la DB?
        * ¿La anotación `@Column(unique = true)` coincide con una restricción `UNIQUE` en la columna de la DB?
        * Si es una relación, ¿la columna de clave foránea existe en la tabla correcta en la DB? ¿El `@JoinColumn(name = "nombre_columna_fk")` en su entidad JPA usa el nombre correcto de la columna FK en la tabla? ¿Referencia la tabla correcta?
4. Identifiquen cualquier inconsistencia.

#### 2.3 Cómo Cursor AI ayuda en la Verificación de Consistencia

* **Comparación Cruzada**: Pueden pegar en el chat de AI la sentencia `CREATE TABLE` de una de sus tablas y el código de la clase @Entity correspondiente y pedirle a la IA que los compare y señale posibles inconsistencias en el mapeo. Por ejemplo: "Review these two code snippets (CREATE TABLE and JPA Entity). Do the JPA annotations seem to correctly map to the table definition?".
* **Generar Mapeo Basado en DB**: Si tienen una tabla existente, pueden pedirle a Cursor AI que les genere la clase `@Entity` correcta basándose en la definición de la tabla. Luego pueden comparar esta generación con su propia entidad.
* **Explicar Errores de Mapeo**: Si se encuentran con errores de mapeo en tiempo de ejecución (ej. "Unknown column", "Data type mismatch"), péguenle el error a Cursor AI y pídanle que les explique la causa probable relacionada con el mapeo incorrecto.

### 3. Pruebas Cruzadas y Diagnóstico con Cursor AI (Relaciones, Métodos, Configuración) (30 minutos)

#### 3.1 Propósito

Usar a Cursor AI como una herramienta de revisión automatizada para detectar posibles problemas en la implementación de relaciones, la definición de métodos de repositorio y la configuración JPA general, complementando la revisión manual y entre compañeros.

#### 3.2 Descripción de la Actividad

1. Se asignarán (o elegirán) el código de un compañero (o se usará un proyecto de ejemplo con problemas deliberados).
2. Utilizarán activamente las funciones de análisis de Cursor AI sobre ese código:
    * **Revisión de Mapeo de Relaciones**: Seleccionen las clases de dos entidades relacionadas (ej. `Cliente.java` y `Pedido.java` del compañero) y pidan a Cursor AI que revise la implementación de la relación. Por ejemplo: "Review the JPA relationship mapping between the Cliente and Pedido entities in this code. Are the @OneToMany and @ManyToOne annotations and their attributes correct?".
    * **Revisión de Métodos de Repositorio**: Abran una interfaz Repository del compañero y pídanle a Cursor AI que revise si los métodos de consulta personalizados siguen las convenciones de Spring Data JPA. Por ejemplo: "Review the custom methods in this ProductRepository interface. Do their names seem correct for Spring Data JPA derived queries?".
    * **Revisión de Configuración JPA**: Abran el archivo `application.properties` (o `application.yml`) del proyecto y pídanle a la IA que revise la configuración relacionada con JPA. Por ejemplo: "Review the Spring JPA configuration in this application.properties file. Are there any potential issues or missing recommended settings?".
    * **Análisis General con Foco en JPA**: Pidan un análisis más amplio: "Review this Service class (`ProductoService`) and its interaction with the Repository. Are there any code smells related to data access?".
3. Analicen las sugerencias y advertencias de la IA. Discutan como grupo si las sugerencias son válidas y por qué.

#### 3.3 Cómo Cursor AI actúa como Herramienta de Diagnóstico

* **Conocimiento de Patrones**: La IA ha sido entrenada en millones de proyectos y conoce los patrones comunes de Spring Data JPA y los errores típicos.
* **Análisis Rápido**: Puede analizar grandes bloques de código o archivos de configuración rápidamente, señalando áreas problemáticas.
* **Sugerencias Específicas**: No solo detecta problemas, sino que a menudo sugiere la corrección o una forma de mejorarlo.

### 4. Validación de Datos: Campos Obligatorios, Formatos, Referencias (25 minutos)

#### 4.1 Propósito

Consolidar cómo la validación a nivel de aplicación (Bean Validation) y las restricciones a nivel de base de datos trabajan juntas para mantener la integridad de los datos, enfocándose en cómo se aplican a campos individuales y relaciones.

#### 4.2 Descripción de la Actividad

1. **Validación en la Aplicación (Bean Validation)**:
    * Revisen las anotaciones de validación (`@NotNull`, `@NotBlank`, `@Size`, `@Email`, `@Min`, `@Max`, `@Valid` en colecciones) en sus entidades o DTOs.
    * Discutan ejemplos de escenarios donde cada una es esencial (ej. un nombre de usuario no puede ser nulo ni vacío; una dirección de correo debe tener formato de email; una cantidad de producto debe ser positiva).
    * Recuerden la importancia de `@Valid` en el controlador para activar esto.
2. **Restricciones en la Base de Datos (SQL)**:
    * Revisen las restricciones SQL correspondientes: `NOT NULL`, `UNIQUE`, `CHECK`, `PRIMARY KEY`, `FOREIGN KEY`.
    * Enfatizen que estas son la última línea de defensa para la integridad de los datos, impidiendo que datos inválidos (o datos que pasaron por alto la validación en la aplicación) lleguen a la base de datos.
3. **Integridad Referencial (FOREIGN KEY)**:
    * Este es un punto clave. Discutan qué sucede en la base de datos si intentan realizar operaciones que violan una `FOREIGN KEY`:
        * Intentar insertar un `Pedido` con un `id_cliente` que no existe en la tabla clientes. (La DB rechazará la operación).
        * Intentar eliminar un `Cliente` que tiene pedidos asociados. (La DB lo impedirá por defecto - restricción `RESTRICT`, o si se configuró, podría eliminar los pedidos en cascada - `ON DELETE CASCADE`).
    * Discutan cómo Spring/JPA maneja estas violaciones de la DB (lanzan excepciones como `DataIntegrityViolationException`).
4. **Combinando Ambos**: La estrategia recomendada es usar **Bean Validation** en la capa web/servicio para dar feedback rápido y claro al usuario/cliente de la API, y tener **Restricciones de Base de Datos** (`NOT NULL`, `UNIQUE`, `FOREIGN KEY`, `CHECK`) como garantía final a nivel de almacenamiento.

#### 4.3 Cómo Cursor AI ayuda con la Validación y la Integridad

* **Explicar Restricciones DB**: Pídanle a Cursor AI que les explique en detalle cómo funciona una restricción `FOREIGN KEY` en PostgreSQL, incluyendo las opciones `ON DELETE` (`CASCADE`, `RESTRICT`, `SET NULL`).
* **Relación Java/SQL Validation**: Pregúntenle a la IA cómo se relacionan `@NotNull` en Java con `NOT NULL` en SQL, o `@Valid` con `FOREIGN KEY` constraints.
* **Manejo de Excepciones de Integridad**: Pídanle a Cursor AI ejemplos de cómo capturar y manejar una excepción `DataIntegrityViolationException` en Spring Boot (ej. usando `@ControllerAdvice`) para dar una respuesta más amigable al cliente de la API.

## Conclusión

En esta tutoría hemos reforzado conceptos cruciales: el modelado efectivo de relaciones, la verificación de que nuestro código JPA coincida con el esquema real de la base de datos, el uso de la IA para un diagnóstico técnico y la importancia combinada de la validación en la aplicación y las restricciones en la base de datos. Dominar estos aspectos es fundamental para construir aplicaciones robustas y fiables que interactúen con bases de datos relacionales.

¡Sigan practicando el modelado, la implementación de relaciones y la verificación de su código contra la base de datos real!
