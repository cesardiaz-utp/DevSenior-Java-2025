# Tutoría : Reforzamiento de Modelado Avanzado y Relaciones

## 1. Repaso y Sondeo Inicial

- **Objetivo**: Evaluar la comprensión de los conceptos de la Clase 3 (Arrays, Composición, Agregación, Relaciones Bidireccionales, Abstracción en Modelado) y el ejercicio práctico del Restaurante.
- **Dinámica**:
  - Iniciar preguntando: "¿Qué les pareció la introducción a los Arrays y el modelado de relaciones? ¿Entienden la diferencia entre Composición y Agregación? ¿Qué parte les generó más dudas?"
  - Preguntas clave:
    - "¿Cómo se declara un Array en Java? ¿Cómo se accede a sus elementos?"
    - "¿Cuál es la principal limitación de un Array?"
    - "¿Cuándo usarían Composición en lugar de Agregación?" (Pedir analogías o ejemplos).
    - "¿Cómo implementarían una relación 'un Pedido tiene varios Ítems de Pedido' usando Composición y Arrays?"
    - "¿Cómo implementarían una relación 'un Departamento tiene varios Empleados' usando Agregación y Arrays?"
    - "¿Qué significa que una relación sea bidireccional?"
    - "¿Cómo encajan las clases abstractas e interfaces en el diseño de sistemas con relaciones?"
    - "¿Encontraron dificultades con el ejercicio práctico del Restaurante?"
  - Anotar los puntos débiles para abordarlos durante la tutoría.

## 2. Taller de Modelado y Relaciones con Arrays

- **Objetivo**: Practicar la identificación e implementación de relaciones con múltiples "partes" utilizando Arrays.
- **Dinámica**: Presentar 2-3 escenarios simples y guiar a los estudiantes en el diseño y codificación de las clases y sus relaciones.
- **Escenarios a modelar** (elegir 2 o 3):
  - **Sistema de Pedidos Online (Refuerzo de Composición)**: Clases `Customer` (Cliente), `Order` (Pedido), `Product` (Producto), `OrderItem` (Ítem de Pedido). `Order` compone `OrderItem[]`. `OrderItem` asocia `Product`. `Customer` asocia `Order[]` (si se quiere modelar que un cliente tiene varios pedidos, aunque el foco es Order/OrderItem/Product).
  - **Gestión de Biblioteca (Refuerzo de Agregación)**: Clases `Library` (Biblioteca), `Book` (Libro), `Author` (Autor). `Library` agrega `Book[]`. `Book` asocia `Author` (o `Author[]` si un libro puede tener varios autores). Los libros y autores existen fuera de la biblioteca.
  - **Estructura de Empresa (Mezcla)**: Clases `Company` (Empresa), `Department` (Departamento), `Employee` (Empleado). `Company` agrega `Department[]`. `Department` agrega `Employee[]`. `Employee` asocia `Department` (relación bidireccional).
- **Para cada escenario**:
  - Dibujar el diagrama de clases con relaciones (Asociación, Composición, Agregación) y multiplicidad (usando `[]` para "muchos").
  - Discutir si la relación es Composición o Agregación y por qué.
  - Discutir si es bidireccional y cómo implementarlo.
- **Práctica de Codificación (Guiada)**: Escribir el código básico para las clases y sus relaciones en VSCode (en inglés, comentarios/salida en español). Enfocarse en:
  - Declarar los atributos Array (`private Tipo[] nombreArray;`).
  - Implementar los constructores: crear el array internamente (Composición) o recibirlo como parámetro (Agregación).
  - Escribir métodos para añadir elementos al Array (simplificado para Arrays de tamaño fijo, ej. `addEmployee(int index, Employee emp)` o buscando la primera posición null).
  - Escribir métodos para mostrar los elementos en el Array (iterando con un bucle for).
  - Si es bidireccional, mostrar cómo establecer la referencia en ambos sentidos.

## 3. Revisión de Errores Comunes y Buenas Prácticas

- **Objetivo**: Discutir los errores típicos al modelar relaciones y usar Arrays en POO.
- **Dinámica**: Presentar fragmentos de código con errores o malas prácticas comunes (o pedir a los estudiantes si encontraron alguno similar en su ejercicio del Restaurante).
- **Errores a discutir**:
  - **Confundir Composición y Agregación**: Implementar una relación como agregación cuando lógicamente debería ser composición (o viceversa).
  - **Manejo Incorrecto de Arrays**: `ArrayIndexOutOfBoundsException`, olvidar inicializar los elementos del array (quedan nul`l`), iterar incorrectamente, no verificar `null` al acceder a elementos de arrays de objetos.
  - **Romper Encapsulamiento con Arrays**: Devolver el array interno directamente desde un getter (`public Tipo[] getItems()`). Esto permite que código externo modifique el array directamente. **Mejor Práctica**: Devolver una copia del array o un elemento específico por índice.
  - **Gestión Incorrecta de Relaciones Bidireccionales**: Establecer la relación en un solo sentido.
  - **Duplicación de Lógica de Iteración**: Repetir el bucle `for` para recorrer un array en múltiples métodos de la misma clase en lugar de crear un método auxiliar.
- **Buenas Prácticas a reforzar**:
  - **Diagramación**: Visualizar las relaciones antes de codificar.
  - **"Private by Default" para Arrays**: El array atributo siempre debe ser privado.
  - **Getters Seguros para Arrays**: Devolver copias o elementos individuales, no la referencia directa al array interno.
  - **Validación de Índices**: Siempre verificar que los índices estén dentro del rango válido al acceder/modificar elementos del array.
  - **Manejo de `null`**: Al iterar sobre arrays de objetos, verificar si cada elemento no es `null` antes de intentar usarlo.
  - **Lógica de Relación Centralizada**: Implementar la lógica para establecer/romper relaciones bidireccionales en métodos específicos de una o ambas clases involucradas.

## 4. Aplicación de Abstracción en Relaciones

- **Objetivo**: Ver cómo las clases abstractas e interfaces se usan en conjuntos de objetos relacionados.
- **Dinámica**: Tomar uno de los escenarios modelados (ej. Biblioteca o Empresa) y modificarlo para usar abstracción.
- **Ejemplo**: En el sistema de Empresa, si tuvieras diferentes tipos de empleados que comparten un comportamiento común pero con implementación específica (ej. interfaz `Reportable` con `generarReporte()`), el `Department` podría tener un `Reportable[]` en lugar de `Employee[]`, permitiendo agregar cualquier objeto que sea `Reportable` (no solo `Employee`s) y llamar a `generarReporte()` polimórficamente.

## Cierre y Próximos Pasos

- **Objetivo**: Recapitular los conceptos reforzados y presentar el próximo ejercicio autónomo.
- **Resumen de la tutoría**: Modelado de relaciones (Composición/Agregación con Arrays), Relaciones Bidireccionales, Abstracción en diseño, errores comunes, buenas prácticas.
- Asegurarse de que los estudiantes se sientan más cómodos modelando sistemas con múltiples objetos relacionados.
- Introducir el ejercicio autónomo para que sigan practicando estos conceptos de forma independiente.
- Sesión de preguntas finales.
