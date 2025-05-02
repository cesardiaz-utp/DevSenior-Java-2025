# Tutoría: Práctica NoSQL y Comparativa Relacional vs. Documental

¡Hola a todos!

En la clase anterior, nos introdujimos al fascinante mundo de las bases de datos NoSQL con MongoDB, aprendiendo su estructura flexible y cómo conectarla a Spring Boot. En esta tutoría, vamos a consolidar esos conocimientos con práctica intensiva en MongoDB y, fundamentalmente, a comparar lo aprendido sobre bases de datos relacionales y documentales para entender cuándo y por qué elegir una sobre la otra en diferentes escenarios de aplicaciones.

## Objetivos de la sesión

- Consolidar el conocimiento práctico en el modelado, operaciones y consultas con bases de datos NoSQL, enfocándonos en MongoDB y Spring Data.
- Evaluar de manera informada en qué escenarios de aplicación es más adecuado utilizar una base de datos relacional (como PostgreSQL) frente a una base de datos documental (como MongoDB), basándose en sus características.

## Contenidos y Actividades

### 1. Taller Práctico: Construir una Aplicación de Mensajería Simple Usando MongoDB (60 minutos)

#### 1.1 Propósito

Aplicar de forma integrada los conceptos de MongoDB y Spring Data MongoDB en un escenario realista que se beneficia de la flexibilidad del modelo documental.

#### 1.2 Descripción de la Actividad

1. Trabajarán en un mini-proyecto (pueden usar el de usuarios de la clase anterior y extenderlo, o crear uno nuevo). El escenario será el backend de una aplicación de mensajería simple.
2. Deberán modelar e implementar las siguientes entidades/documentos en MongoDB:
    - `User`: Pueden usar el de la clase anterior. Añádanle campos que muestren la flexibilidad, como una lista de `intereses` (array de strings) o un campo `perfil_extra` que sea un subdocumento que pueda variar entre usuarios (ej. algunos tienen `compania`, otros tienen `escuela`).
    - `Message`: Representa un mensaje enviado. Campos:
      - `id` (@Id, String).
      - `senderId` (String, referencia al ID del usuario emisor).
      - `receiverId` (String, referencia al ID del usuario receptor).
      - `content` (String, el texto del mensaje).
      - `timestamp` (java.time.LocalDateTime o Instant).
      - `type` (String, ej. "text", "image", "link" - muestren cómo diferentes mensajes pueden tener campos distintos según el tipo, aunque no los implementen todos).
      - `status` (String, ej. "sent", "read").
    - **(Opcional / Desafío)**: `Conversation`: Un documento que represente una conversación entre dos usuarios. Campos: `id`, `participantIds` (array de IDs de usuario), y una forma de relacionar los mensajes. Discutan si sería mejor em beber un resumen de los últimos mensajes (ej. los 10 últimos) o solo guardar referencias a los IDs de los mensajes (`messageIds`: array de Strings). Analicen los pros y contras de cada opción para este caso.
3. Implementarán el backend de estas entidades usando Spring Data MongoDB:
    - Crear/ajustar las clases `@Document` con sus anotaciones (`@Id`, `@Field`). Incluyan campos con arrays y subdocumentos simples.
    - Crear las interfaces `MongoRepository` correspondientes (`UserRepository`, `MessageRepository`, `ConversationRepository` opcional).
    - Implementar métodos básicos en la capa de servicio (`@Service`) para:
      - Enviar un mensaje (guardar un nuevo documento `Message`).
      - (Desafío de consulta, se verá más a fondo después, pero intenten): Obtener todos los mensajes entre dos usuarios específicos, ordenados por fecha.
4. (Opcional) Implementar endpoints REST básicos para enviar mensajes o listar mensajes entre usuarios.

#### 1.3 Cómo Cursor AI ayuda en este Taller

- **Diseñar Documentos**: Descríbanle a la IA los datos que necesitan guardar para `User` o `Message` (incluyendo arrays o subdocumentos opcionales) y pídanle que les genere la clase `@Document` con las anotaciones correctas. Por ejemplo: "Generate a MongoDB @Document class for a 'Message' with fields for senderId, receiverId, text content, timestamp, and a list of 'tags' (array of strings)".
- **Modelar Relaciones NoSQL**: Pregúntenle a la IA cómo modelarían la relación entre `User` y `Message` (referencia por ID) o entre `Conversation` y `Message` (embebido vs referencia). "How should I model the relationship between a 'Conversation' document and 'Message' documents in MongoDB? Should I embed messages or use references?".
- **Generar Repositorios y Métodos**: Pídanle a la IA que les genere las interfaces `MongoRepository` y que añada métodos de servicio/controlador para las operaciones básicas.

### 2. Creación de Consultas con Filtros Anidados y Expresiones Regulares (Regex) (30 minutos)

#### 2.1 Propósito

Practicar cómo realizar consultas en MongoDB que van más allá de buscar por campos de primer nivel, utilizando las capacidades de Spring Data.

#### 2.2 Descripción de la Actividad

1. Basándose en los documentos `User` y `Message` de la Actividad 1:
2. **Consultar Campos Anidados**: Añadan un método a su `UserRepository` o `MessageRepository` para buscar documentos basándose en el valor de un campo dentro de un subdocumento anidado (si lo modelaron).
    - **Ejemplo**: Si `User` tiene un subdocumento `address` con un campo `city`, añadan un método como `List<User> findByAddress_City(String city);`. Spring Data MongoDB traducirá `address_City` correctamente.
3. **Consultar Arrays**: Añadan un método para buscar documentos donde un campo de array contenga un valor específico.
    - **Ejemplo**: Si `Message` tiene un array `tags`, añadan un método como `List<Message> findByTagsContaining(String tag);`.
4. **Consultar con Expresiones Regulares (Regex)**: Añadan un método para realizar búsquedas parciales de texto ignorando mayúsculas/minúsculas.
    - **Ejemplo**: Añadan a `UserRepository` un método para buscar usuarios por nombre de usuario que contenga cierto texto. Pueden intentar `List<User> findByUsernameContainingIgnoreCase(String usernamePart);` (Spring Data a menudo lo mapea a regex), o usar `@Query` con sintaxis de MongoDB: `@Query("{ 'username' : { $regex: ?0, $options: 'i' } }") List<User> findByUsernameRegex(String regex);`.
5. Implementen el uso de estos nuevos métodos de consulta en su capa de servicio.

#### 2.3 Cómo Cursor AI ayuda en las Consultas

- **Métodos Derivados**: Pídanle a la IA que genere firmas de métodos derivados para buscar en campos anidados o arrays.
- **Consultas `@Query` con Sintaxis MongoDB**: Si necesitan una consulta más compleja, descríbanle la consulta en lenguaje natural y pídanle la sintaxis de consulta de MongoDB y cómo ponerla en una anotación `@Query`. Por ejemplo: "Write a Spring Data MongoDB @Query annotation using MongoDB syntax to find all products where the 'tags' array contains both 'electronics' and 'discounted'".
- **Regex**: Pídanle ayuda para escribir la expresión regular adecuada o la firma del método para una búsqueda regex.

### 3. Comparación de Rendimiento (Conceptual y Simple) y Análisis con IA (30 minutos)

#### 3.1 Propósito

Entender las implicaciones de rendimiento de las decisiones de modelado en MongoDB (embebido vs referencia) y comparar conceptualmente cómo se realizarían ciertas operaciones en MongoDB vs una base de datos relacional.

#### 3.2 Descripción de la Actividad

1. **Discusión Conceptual (Embebido vs Referencia)**:
    - Basándose en su diseño de `Message` (si embebieron o referenciaron remitente/receptor) y su decisión en el desafío opcional de `Conversation` (em beber mensajes vs referenciar), discutan:
      - ¿Qué ventajas tiene el diseño elegido para las operaciones de lectura (ej. obtener un mensaje, obtener una conversación)? (Menos o más consultas).
      - ¿Qué ventajas tiene para las operaciones de escritura (ej. actualizar el nombre de usuario del remitente, añadir un nuevo mensaje a una conversación)? (Menos o más actualizaciones, tamaño del documento).
      - ¿Cuándo sería mejor em beber datos y cuándo mejor usar referencias? (Regla general: Em beber datos si cambian juntos, se consultan juntos y no son colecciones muy grandes. Usar referencias si son datos que cambian independientemente, colecciones grandes o si se consultan mucho por separado).
2. **Comparación Conceptual Relacional vs. Documental (Consultas)**:
    - Tomen un ejemplo de consulta de su proyecto relacional anterior que involucraba un JOIN (ej. encontrar pedidos de un cliente). Discutan cómo se resolvería esa misma consulta en MongoDB (probablemente usando el Aggregation Framework con `$lookup`, que es similar a un LEFT OUTER JOIN). Analicen las diferencias en cómo se "unen" los datos en ambos modelos.
    - Tomen una consulta donde MongoDB brilla (ej. encontrar usuarios con un campo opcional `linkedinUrl` no nulo, o buscar en un array `tags`). Discutan cómo sería esa consulta en SQL (más compleja, tal vez con tablas de unión auxiliares o JSON functions si la DB lo soporta).
3. **Análisis con Cursor AI**:
    - Péguenle a Cursor AI una consulta (JPQL o SQL nativo) de su proyecto relacional anterior que use JOINs. Pregúntenle: "How would I perform an equivalent query in Spring Data MongoDB, assuming a document model where [describir cómo mapearon las entidades en MongoDB, ej. Pedido con referencia a Cliente, Producto con referencia a Pedido/PedidoProducto]?. What MongoDB query approach would you recommend (derived, @Query, Aggregation)?".
    - Describanle una consulta compleja de MongoDB que les interese (ej. buscar en un array y un campo anidado a la vez). Pregúntenle sobre cómo MongoDB ejecutaría esa consulta y qué índices podrían ayudar.

#### 3.3 Cómo Cursor AI ayuda en el Análisis de Rendimiento y Comparación

- **Traducción de Consultas**: Ayuda a "traducir" la lógica de consulta entre paradigmas (SQL a MongoDB Query).
- **Explicación de Modelos**: Ayuda a entender los pros y contras de las decisiones de modelado (embebido vs referencia) para escenarios específicos.
- **Análisis de Ejecución MongoDB**: Aunque no es `EXPLAIN ANALYZE`, puede explicar cómo MongoDB típicamente resuelve ciertos tipos de consultas (ej. consultas en arrays, consultas en subdocumentos) y qué factores influyen en el rendimiento (índices, tamaño del documento).

### 4. Discusión Grupal: ¿Cuándo Preferir SQL? ¿Cuándo es Mejor Usar NoSQL? (30 minutos)

#### 4.1 Propósito

Sintetizar todo lo aprendido en el módulo para poder tomar decisiones informadas sobre qué tipo de base de datos es la más adecuada para diferentes tipos de proyectos.

#### 4.2 Descripción de la Actividad

1. Dirigirán una discusión abierta basada en las diferencias y casos de uso de bases de datos relacionales (PostgreSQL) y documentales (MongoDB) vistas a lo largo del módulo.
2. Temas a Cubrir:
    - Recapitulación de las **fortalezas de las Bases de Datos Relacionales**: Integridad ACID, estructura rígida (buena cuando el modelo es estable), SQL potente para JOINs y reportes complejos, madurez. ¿Cuándo un proyecto necesita esto? (Ej. finanzas, sistemas de inventario estricto, aplicaciones con reportes complejos y relaciones bien definidas).
    - Recapitulación de las **fortalezas de las Bases de Datos NoSQL (MongoDB)**: Esquema flexible, escalabilidad horizontal (más fácil), manejo de datos no estructurados/semi-estructurados, agilidad en el desarrollo inicial si el esquema cambia mucho. ¿Cuándo un proyecto se beneficia más de esto? (Ej. perfiles de usuario con campos variados, catálogos flexibles, IoT, datos de log, cachés a gran escala, CMS).
    - **Errores Comunes**: ¿Cuándo no usar MongoDB? (Ej. cuando tienen relaciones N:N muy complejas con muchas consultas que involucren navegarlas frecuentemente, o cuando necesitan transacciones multi-documento complejas que ACID garantiza fácilmente en relacionales). ¿Cuándo no usar Relacionales? (Ej. cuando el esquema cambia constantemente, cuando necesitan escalar masivamente de forma horizontal con datos semi-estructurados).
    - **Enfoques Híbridos**: ¿Es posible usar ambos? ¿Cuándo tendría sentido usar una base de datos relacional para una parte de la aplicación y una NoSQL para otra? (Ej. relacional para la información de pedidos y clientes principales, NoSQL para perfiles de usuario o datos de log).
    - **Principios de Diseño**: ¿Cómo influye la elección de la base de datos en el diseño de la capa de persistencia y el servicio? (Relacionen con DIP: su servicio no debería estar atado a JPA o MongoRepository concreto, sino a interfaces que abstraigan la persistencia, permitiendo cambiar la tecnología de base de datos más fácilmente).
3. Fomenten la participación. Pidan a los estudiantes que compartan sus experiencias o ideas sobre cuándo usarían una u otra basándose en proyectos anteriores (aunque no fueran con Spring Boot).

### 4.3 Cómo Cursor AI ayuda en la Discusión

- Los estudiantes pueden usar la IA para pedirle que resuma las diferencias, casos de uso, pros y contras de MongoDB vs PostgreSQL para refrescar su memoria o obtener otra perspectiva durante la discusión.
- Pueden pedirle que justifique la elección de una base de datos para un escenario hipotético.

## Conclusión

Hemos llegado al final de nuestro módulo de bases de datos. Hemos explorado a fondo el mundo relacional con Spring Data JPA y el mundo documental con Spring Data MongoDB. Lo más importante es que ahora comprenden que no hay una solución única para todos los problemas de persistencia. La elección de la base de datos correcta depende de las características específicas de los datos, los requisitos de consulta, la escalabilidad y la tolerancia a fallos. Saber cuándo usar PostgreSQL y cuándo usar MongoDB es una habilidad valiosa. Y recuerden, la IA es una herramienta poderosa para ayudarlos a navegar por estas decisiones y a implementar sus soluciones de persistencia.

¡Felicidades por completar este módulo! Sigan practicando con ambos tipos de bases de datos y apliquen su conocimiento para elegir la herramienta adecuada en sus futuros proyectos.
