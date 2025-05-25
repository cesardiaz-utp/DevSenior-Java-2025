# Ejercicios adicionales

**Objetivos de cada Ejercicio**:

- **Aplicar Maven**: Crear y configurar un proyecto Maven, incluyendo la gestión de dependencias.
- **Diseñar POO con Relaciones**: Modelar entidades con una relación uno-a-muchos (un almacén contiene muchos productos).
- **Manejar Arrays**: Implementar lógica para agregar, buscar, actualizar y manipular elementos en arrays de tamaño fijo, gestionando la capacidad.
- **Implementar Excepciones Personalizadas**: Crear y utilizar excepciones personalizadas (`checked` y `unchecked`) para manejar situaciones de error específicas del dominio de la aplicación.
- **Control de Flujo con Excepciones**: Aplicar `try-catch` y `throws` para manejar y propagar excepciones de manera adecuada en la jerarquía de llamadas de métodos.
- **Desarrollar una Aplicación de Consola**: Crear una aplicación de consola funcional para interactuar con el sistema de inventario.

## Ejercicio 1: Sistema de Gestión de Inventario para una Pequeña Tienda

Estás desarrollando un sistema de gestión de inventario básico para una pequeña tienda que tiene varios almacenes. Cada almacén contiene diferentes productos. Necesitas poder añadir productos a almacenes específicos, actualizar las existencias, y realizar búsquedas.

**Requisitos Funcionales**:

El sistema debe permitir las siguientes operaciones, con sus respectivas validaciones y manejo de errores:

- De cada producto en el inventario se debe guardar el identificador único dentro de un almacén, nombre, precio, y cantidad disponible en inventario.
- De cada almacén se debe guardar identificador único del almacén), ubicación, y un mecanismo para almacenar múltiples productos.

**Operaciones Clave del Sistema**:

- **Gestionar productos (dentro de un almacen)**:
  - Agregar un nuevo producto al almacén. Debe validar que los datos del producto sean válidos, que el almacén no esté lleno y que el identificador no esté duplicado dentro de ese almacén.
  - Actualizar la cantidad de existencias de un producto específico en el almacén. Debe validar los datos de entrada, y que el producto exista y la nueva cantidad no sea negativo.
  - Buscar y retornar un producto por su identificador dentro del almacén.
  - Mostrar todos los productos actualmente almacenados en el almacén.
- **Gestionar almacenes (en el sistema general)**:
  - Agregar un nuevo almacén al sistema. Debe validar que los datos del almacén sean válidos, que el sistema no esté lleno de almacenes y que el identificador no esté duplicado.
  - Buscar y retornar un almacén por su identificador.
  - Agregar un producto a un almacén específico. Debe encontrar el almacén primero y luego intentar añadir el producto.
  - Actualizar la cantidad de inventario de un producto en un almacén específico. Debe encontrar el almacén y luego actualizar el producto.
  - Mostrar todos los almacenes registrados en el sistema y, para cada uno, sus productos asociados.

## Ejercicio 2: Sistema de Gestión de Citas Médicas

Estás desarrollando un sistema simplificado para gestionar las citas médicas de una clínica. El sistema debe permitir registrar pacientes, agendar citas para pacientes existentes y gestionar el estado de estas citas (por ejemplo, "PENDING", "CONFIRMED", "COMPLETED", "CANCELLED").

**Requisitos Funcionales**:

El sistema debe permitir las siguientes operaciones, con sus respectivas validaciones y manejo de errores:

- De un paciente se debe guardar su identificador único, nombre, fecha de nacimiento y un mecanismo para almacenar múltiples citas.
- De una cita médica se debe guardar su identificador único dentro de un paciente,  una referencia al ID del paciente), fecha, hora, causa o razon de la cita y estado (e.g., "PENDING", "CONFIRMED", "COMPLETED", "CANCELLED").

**Operaciones Clave del Sistema**:

- Agregar una nueva cita al paciente. Debe validar que los datos de la cita sean válidos, que el paciente no tenga su array de citas lleno y que el identificador no esté duplicado para ese paciente.
- Actualizar el estado de una cita específica del paciente. Debe validar los datos de entrada, que la cita exista y que el nuevo estado sea un estado válido.
- Buscar y retornar una cita por su identificador dentro del paciente.
- Mostrar todas las citas programadas para este paciente.
- Registrar un nuevo paciente en el sistema. Debe validar los datos del paciente, que el sistema no esté lleno de pacientes y que el identificador no esté duplicado.
- Buscar y retornar un paciente por su identificador.
- Agendar una cita para un paciente específico. Debe encontrar el paciente primero y luego intentar añadir la cita a ese paciente.
- Actualiza el estado de una cita para un paciente específico. Debe encontrar el paciente y luego actualizar el estado de la cita.
- Muestra todos los pacientes registrados en el sistema y, para cada uno, sus citas programadas.
