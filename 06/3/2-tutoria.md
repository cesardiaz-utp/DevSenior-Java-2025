# Tutoría: Diagnóstico, Rendimiento y Consultas Avanzadas con IA

¡Hola a todos!

En la clase anterior, desbloqueamos el potencial de consulta con Spring Data JPA, aprendiendo a usar métodos derivados, JPQL, SQL nativo, Criteria API y a manejar paginación/ordenamiento. En esta tutoría, pondremos a prueba esos conocimientos en escenarios más realistas, enfocándonos en entender el rendimiento de nuestras consultas y utilizando herramientas de base de datos e Inteligencia Artificial para diagnosticar y optimizar nuestro código.

## Objetivos de la sesión

- Comprender y aplicar técnicas avanzadas de consultas (JPQL, Criteria API, métodos derivados, paginación, ordenamiento).
- Analizar el rendimiento de consultas en la base de datos y aplicar optimizaciones basadas en análisis y recomendaciones de IA.

## Contenidos y Actividades

### 1. Simulación de Escenarios Reales y Diseño de Consultas (40 minutos)

#### 1.1 Propósito

Aplicar lo aprendido para diseñar consultas que respondan a necesidades de negocio complejas, identificando la mejor herramienta (método derivado, JPQL, Criteria API) para cada caso.

#### 1.2 Descripción de la Actividad

1. Presentaremos escenarios de consulta complejos, como:
    - Encontrar **clientes activos** en una **ciudad específica** con **pedidos mayores a X** en los **últimos N meses**, ordenados por **fecha de registro**.
    - Obtener una lista **paginada** de **productos en stock** de una **categoría dada**, permitiendo buscar por **nombre parcial**, ordenados por precio.
2. Para cada escenario:
    - Analicen los requerimientos: entidades, relaciones, filtros, ordenamientos, agregaciones.
    - Decidan la técnica de consulta más adecuada (método derivado, JPQL, Criteria API).
    - Diseñen la consulta y consideren la integración de paginación y ordenamiento (`Pageable`).
3. Discutiremos las propuestas, analizando por qué una técnica es mejor que otra.

#### 1.3 Cómo Cursor AI ayuda en el Diseño de Consultas

- **Sugerir Técnica**: Describan el requisito de consulta a Cursor AI para obtener recomendaciones.
- **Generar Consultas**: Soliciten ayuda para escribir consultas (firma de método derivado, JPQL, Criteria API).
- **Resolver Problemas Específicos**: Pidan asistencia con cláusulas complejas o predicados.

### 2. Taller: Consultas con Combinaciones Complejas (Criteria API y Paginación) (40 minutos)

#### 2.1 Propósito

Implementar consultas desafiantes usando Criteria API, asegurando la integración con paginación y ordenamiento.

#### 2.2 Descripción de la Actividad

1. Implementen un método de servicio que reciba filtros opcionales y un objeto `Pageable`.
2. Usen Criteria API para construir consultas dinámicas:
    - Añadan predicados a una lista solo si el filtro correspondiente no es `null`.
    - Combinen predicados con `cb.and()`.
    - Apliquen el objeto `Pageable` correctamente.
3. Implementen un endpoint REST que reciba filtros opcionales como `@RequestParam` y paginación/ordenamiento como `Pageable`.
4. Prueben el endpoint con diferentes combinaciones de filtros y paginación.

#### 2.3 Cómo Cursor AI ayuda en este Taller de Codificación

- **Generar Código**: Soliciten el esqueleto del código para consultas dinámicas con Criteria API.
- **Resolver Problemas**: Pidan ayuda con predicados, joins o integración de paginación.
- **Escribir el Controlador**: Soliciten asistencia para crear métodos en el controlador.

### 3. Análisis de Rendimiento con EXPLAIN ANALYZE y Revisión con IA (30 minutos)

#### 3.1 Propósito

Usar `EXPLAIN ANALYZE` para entender el rendimiento de consultas y optimizarlas con ayuda de IA.

#### 3.2 Descripción de la Actividad

1. Seleccionen una consulta implementada en la Actividad 2.
2. Obtengan el SQL generado activando `spring.jpa.show-sql=true` en `application.properties`.
3. Analicen con `EXPLAIN ANALYZE`:
    - Abran pgAdmin o DBeaver y conéctense a su base de datos.
    - Abran una ventana de consulta.
    - Pegen la sentencia SQL que copiaron, precedida por la frase `EXPLAIN ANALYZE`.
    - Ejecuten la consulta. El resultado no serán los datos, sino el _plan de ejecución_ y los tiempos reales.
    - Observen el resultado: Busquen líneas que indiquen "Sequential Scan" en tablas grandes donde esperaban usar un índice. Busquen costos o tiempos altos en ciertas etapas del plan.
4. Revisen con Cursor AI:
    - Copien la sentencia SQL original y todo el resultado del `EXPLAIN ANALYZE`.
    - Péguenlo en el chat de Cursor AI y pregunten: "Analyze this SQL query and its EXPLAIN ANALYZE output. Why might this query be slow based on the plan? What are the bottlenecks? Suggest specific database indexes that could improve its performance."
    - Discutan la explicación de la IA y sus sugerencias de indexación.

#### 3.3 Cómo Cursor AI ayuda en el Análisis de Rendimiento

- **Interpretar Planes**: Leer `EXPLAIN ANALYZE` puede ser complicado. Cursor AI puede resumir los puntos clave y señalar las operaciones más costosas.
- **Identificar Bottlenecks**: La IA puede ayudar a identificar qué parte de la consulta o qué operación de la base de datos está tomando más tiempo.
- **Sugerir Índices**: Basándose en la cláusula `WHERE`, `JOIN` y `ORDER BY` de la consulta y el plan de ejecución, la IA puede sugerir qué columnas en qué tablas serían buenas candidatas para crear índices.

### 4. Refactorización y Limpieza de Consultas (Usando Recomendaciones) (20 minutos)

#### 4.1 Propósito

Aplicar sugerencias de optimización para mejorar consultas y la estructura de la base de datos.

#### 4.2 Descripción de la Actividad

1. Basándose en el análisis de rendimiento de la Actividad 3 y las sugerencias de Cursor AI:
    - **Si la IA sugirió refactorizar la consulta JPA**: Modifiquen su método Repository (JPQL, Criteria API) en su proyecto para implementar las sugerencias de la IA (ej. usar `JOIN FETCH`, cambiar la lógica de los predicados).
    - **Si la IA sugirió crear índices de base de datos**: En pgAdmin o DBeaver, escriban y ejecuten las sentencias SQL `CREATE INDEX` para crear los índices sugeridos en las tablas relevantes. (Recuerden que este es un cambio en la base de datos, no en el código Java, pero su código JPA se beneficiará de ellos).
    - **Limpieza General**: Aprovechen para aplicar cualquier otra recomendación de limpieza de código (nombres, comentarios) que Cursor AI haya sugerido en análisis previos sobre estas consultas.
2. (Opcional) Después de crear los índices o modificar la consulta, pueden volver a ejecutar `EXPLAIN ANALYZE` en pgAdmin para ver si el plan de ejecución ha mejorado (ej. ahora usa un "Index Scan" en lugar de un "Sequential Scan").

#### 4.3 Cómo Cursor AI ayuda en la Refactorización

- **Generar Código Refactorizado**: Si pidieron a la IA que les sugiriera cómo reescribir una consulta JPQL o Criteria, pueden usar el código que les proporcionó e integrarlo en su método Repository.
- **Generar Sentencias CREATE INDEX**: Si la IA sugirió un índice, pueden pedirle la sentencia SQL exacta: "Generate the SQL CREATE INDEX statement for the 'precio' column in the 'productos' table".
- **Asistir en Refactorizaciones de Código**: Si el cambio implica reestructurar un método o clase, Cursor AI puede asistirles en el proceso de refactorización dentro del editor.

## Conclusión

En esta tutoría, hemos llevado nuestras habilidades de consulta al siguiente nivel, enfrentando escenarios complejos y aprendiendo a analizar el rendimiento de consultas con `EXPLAIN ANALYZE`. La IA es un aliado poderoso para escribir, analizar y optimizar consultas, ayudándonos a construir aplicaciones más eficientes. ¡Continúen practicando y optimizando!
