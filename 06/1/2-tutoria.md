# Tutoría: Modelado, Conexión y Validación de Entidades (Bases de Datos Relacionales)

¡Hola a todos!

En la clase anterior dimos los primeros pasos con bases de datos relacionales y PostgreSQL. En esta tutoría, vamos a profundizar en cómo diseñar correctamente nuestras estructuras de datos relacionales y asegurar que estas estructuras sean robustas y funcionales. Nos enfocaremos en la práctica del modelado, la identificación de errores comunes y la verificación de los esquemas diseñados con la ayuda de la IA.

## Objetivos de la sesión

* Asegurar que comprenden el proceso de diseño de tablas y cómo se relacionan correctamente en un esquema de base de datos relacional.
* Reforzar el uso y la comprensión de modelos de datos relacionales reales y sus componentes clave.

## Contenidos y Actividades

### 1. Ejercicios de Modelado: De Requerimientos a Esquemas Relacionales (40 minutos)

#### 1.1 Propósito

Practicar cómo traducir necesidades de información del mundo real en un diseño estructurado de base de datos relacional. Esto implica identificar qué "cosas" (entidades) guardar, qué información guardar de cada una (atributos) y cómo se relacionan entre sí.

#### 1.2 Descripción de la Actividad

1. Presentaremos algunos escenarios o requerimientos sencillos (ej. "Necesitamos guardar información de clientes y los pedidos que realizan", "Queremos un sistema para registrar empleados y los departamentos a los que pertenecen").
2. Trabajando individualmente o en pequeños grupos, ustedes deberán:
    * Identificar las **entidades** (las "cosas" principales).
    * Definir los **atributos** clave para cada entidad (las columnas de la tabla).
    * Identificar las **relaciones** entre las entidades (ej. un cliente puede tener muchos pedidos).
    * Determinar las **claves primarias** (el identificador único para cada fila en una tabla).
    * Determinar las **claves foráneas** necesarias para establecer las relaciones entre tablas.
    * Elegir **tipos de datos** adecuados para cada atributo (ej. `VARCHAR`, `INT`, `DATE`, `NUMERIC`).
    * Definir restricciones importantes (`NOT NULL`, `UNIQUE`, etc.).
    * Plasmar el diseño, ya sea dibujando un diagrama sencillo (Entidad-Relación) o escribiendo las sentencias SQL `CREATE TABLE` para definir las tablas.
3. Compartiremos y discutiremos algunos de los diseños propuestos, analizando diferentes enfoques.

#### 1.3 Cómo Cursor AI ayuda en el Modelado

* **Sugerencias de Esquemas**: Pueden describirle a Cursor AI un escenario o entidad y pedirle sugerencias para el diseño de tablas. Por ejemplo: "Dado el requerimiento de un blog simple (publicaciones, comentarios, usuarios), sugiere las tablas de base de datos, columnas y relaciones".
* **Generar Sentencias SQL**: Una vez que tengan clara la estructura, pueden pedirle a Cursor AI que les ayude a escribir las sentencias `CREATE TABLE` completas con tipos de datos y restricciones. Por ejemplo: "Generate the CREATE TABLE statement for a 'posts' table with columns for id (primary key), title (not null), content (text), creation_date (timestamp not null), and author_id (foreign key referencing users table)".
* **Explicar Relaciones**: Si no están seguros de cómo representar una relación particular (1:N, N:M), pueden preguntarle a la IA por ejemplos de diseño de tablas para ese tipo de relación.

### 2. Análisis de Errores Comunes en Modelado y SQL (30 minutos)

#### 2.1 Propósito

Familiarizarse con los errores más frecuentes que se cometen al diseñar bases de datos o escribir SQL, para poder identificarlos y evitarlos.

#### 2.2 Descripción de la Actividad

1. Presentaremos ejemplos de sentencias SQL `CREATE TABLE` o descripciones de estructuras de tablas que contienen errores comunes o problemas de diseño.
2. Discutiremos como grupo (o en parejas):
    * ¿Qué error hay en esta definición de tabla?
    * ¿Qué problema causaría este error (ej. no poder identificar filas únicas, perder datos, no poder relacionar tablas, inconsistencia de datos)?
    * ¿Cómo se corregiría la sentencia SQL o el diseño?
3. Ejemplos de errores a analizar:
    * Faltar la `PRIMARY KEY`.
    * Usar un tipo de dato incorrecto para una clave primaria (ej. un `VARCHAR` no único o no estable).
    * Faltar `NOT NULL` en columnas esenciales (ej. nombre de producto, fecha de pedido).
    * Definir incorrectamente una `FOREIGN KEY` o no definirla cuando debería existir una relación.
    * No usar la restricción `UNIQUE` en columnas que deben tener valores únicos (ej. nombre de usuario, código de producto).
    * Guardar múltiples valores en una sola columna (violación de 1FN, ej. lista de tags en un campo de texto).
4. (Opcional) Pueden usar sus herramientas (pgAdmin/DBeaver) para intentar ejecutar algunas de las sentencias SQL incorrectas y ver los mensajes de error de la base de datos.

#### 2.3 Cómo Cursor AI ayuda en el Análisis de Errores

* **Detectar Errores SQL**: Si escriben una sentencia SQL incorrecta, Cursor AI (en el editor) a menudo la marcará. Si no, pueden copiarla en el chat y preguntar: "What is wrong with this SQL CREATE TABLE statement?".
* **Sugerir Restricciones Faltantes**: Pueden pedirle a Cursor AI que revise una definición de tabla y sugiera restricciones que podrían ser importantes: "Review this table definition. What essential constraints might be missing?".
* **Explicar Errores de Diseño**: Si describen un problema de diseño de base de datos ("Tengo los teléfonos 1, 2 y 3 en la tabla de clientes"), pueden pedirle a la IA que les explique por qué eso es un problema y cómo solucionarlo (ej. aplicando normalización).

#### 2.4 Resultado Esperado

Serán más hábiles para identificar y corregir errores comunes en el diseño y las sentencias SQL para crear tablas, mejorando la calidad de sus esquemas.

### 3. Refuerzo del Concepto de Relaciones (1:1, 1:N, N:N) en Casos Prácticos (30 minutos)

#### 3.1 Propósito

Comprender a fondo cómo se representan los diferentes tipos de relaciones entre entidades en el diseño de bases de datos relacionales utilizando claves foráneas.

#### 3.2 Descripción de la Actividad

1. Revisaremos los tres tipos principales de relaciones relacionales y cómo se modelan a nivel de base de datos:
    * **Relación Uno a Uno (1:1)**: Una instancia de Entidad A se relaciona con máximo una instancia de Entidad B, y viceversa. (Ej: Una Persona puede tener un Pasaporte; Un Pasaporte pertenece a una Persona). Representación en DB: La clave foránea en una tabla apunta a la clave primaria en la otra.
    * **Relación Uno a Muchos (1:N)**: Una instancia de Entidad A se relaciona con cero o muchas instancias de Entidad B, pero una instancia de Entidad B se relaciona con máximo una instancia de Entidad A. (Ej: Un Departamento tiene muchos Empleados; Un Empleado pertenece a un Departamento). Representación en DB: La clave foránea se coloca en el lado "Muchos".
    * **Relación Muchos a Muchos (N:N)**: Una instancia de Entidad A se relaciona con cero o muchas instancias de Entidad B, y una instancia de Entidad B se relaciona con cero o muchas instancias de Entidad A. (Ej: Un Estudiante puede tomar muchos Cursos; Un Curso puede tener muchos Estudiantes). Representación en DB: Requiere una tabla intermedia (o tabla de enlace/unión) que tiene claves foráneas a las claves primarias de ambas tablas originales.
2. Presentaremos escenarios prácticos que requieren cada tipo de relación.
3. Para cada escenario, ustedes deberán:
    * Identificar las entidades.
    * Determinar el tipo de relación.
    * Diseñar las tablas necesarias, incluyendo las claves primarias y foráneas correctas.
    * (Opcional) Escribir las sentencias SQL `CREATE TABLE` correspondientes con las restricciones `FOREIGN KEY`.

#### 3.3 Cómo Cursor AI ayuda con las Relaciones

* **Explicar Tipos de Relación**: Pidan a Cursor AI que les explique un tipo de relación específica (1:1, 1:N, N:N) con ejemplos del mundo real y cómo se representa típicamente en un esquema de base de datos.
* **Sugerir Modelos de Tablas para Relaciones**: Describan un escenario con una relación (ej. "Model a 1 to N relationship between authors and books") y pidan a la IA que les sugiera las estructuras de tablas necesarias con claves primarias y foráneas.
* **Ayudar con SQL de Claves Foráneas**: Si están escribiendo la sentencia `CREATE TABLE` para una tabla con una clave foránea, pueden pedirle a Cursor AI que les ayude con la sintaxis correcta para definir la `FOREIGN KEY`.

#### 3.4 Resultado Esperado

Comprenderán la lógica detrás de cada tipo de relación relacional y cómo se implementan correctamente a nivel de esquema de base de datos usando claves foráneas.

## Conclusión

En esta tutoría, hemos reforzado las bases del modelado de datos relacionales, practicando la traducción de requerimientos a esquemas y analizando errores comunes. La comprensión sólida del diseño de bases de datos es crucial antes de empezar a manipular los datos con código.

¡Continúen practicando el modelado y revisando sus esquemas! En la próxima clase, exploraremos cómo integrar estos esquemas con aplicaciones Java.
