# Tutoría: Refuerzo Práctico de Modelado de Clases y Encapsulamiento

## 1. Repaso y Sondeo Inicial sobre POO

- **Objetivo**: Evaluar la comprensión de los conceptos de la Clase 1 (Clases, Objetos, Constructores, Encapsulamiento) y el ejercicio práctico (`Libro`).
- **Dinámica**:
  - **Iniciar preguntando**: "¿Qué les pareció la introducción a la POO? ¿Entienden la diferencia entre una clase y un objeto? ¿Qué parte les generó más dudas?"
  - Preguntas clave:
    - "¿Qué es un constructor y cuál es su propósito principal?"
    - "¿Por qué hacemos los atributos `private`? ¿Qué problema resuelve el encapsulamiento?"
    - "¿Cuál es la convención para nombrar **getters** y **setters**?"
    - "¿Pudieron completar el ejercicio del `Libro`? ¿Encontraron alguna dificultad?"
  - Anotar los puntos débiles o confusiones comunes para abordarlos durante la tutoría.

## 2. Taller de Modelado Orientado a Objetos

- **Objetivo**: Practicar la identificación de atributos y comportamientos para modelar entidades como clases Java.
- **Dinámica**:
  - Presentar las entidades a modelar: `Producto`, `Estudiante`, `Curso`.
  - **Para cada entidad**:
    - **Preguntar al grupo**: "¿Qué características (atributos) tiene un [Producto/Estudiante/Curso]? ¿De qué tipo de dato serían?". Anotar las ideas en la pizarra/pantalla.
    - **Preguntar al grupo**: "¿Qué acciones (métodos) puede realizar un [Producto/Estudiante/Curso], o qué podemos hacer con él?". Anotar las ideas.
    - Discutir brevemente la visibilidad inicial de estos miembros (atributos `private`, métodos `public`).
  - **Práctica de Codificación (Guiada)**: Elegir una de las clases (ej. `Producto`) y, como grupo, escribir el código básico en VSCode:
    - Declarar los atributos como `private`.
    - Escribir un constructor parametrizado.
    - Generar o escribir manualmente **getters** y **setters** básicos para un par de atributos.
    - Escribir un método de comportamiento simple (ej. `vender` en `Producto`, que reduzca el stock y devuelva el monto total).

## 3. Revisión de Errores Comunes y Buenas Prácticas

- **Objetivo**: Discutir los errores típicos que surgen al iniciar con POO y cómo evitarlos.
- **Dinámica**:
  - Presentar fragmentos de código con errores o malas prácticas comunes (o pedir a los estudiantes si encontraron alguno similar en su ejercicio del `Libro`).
- **Errores a discutir**:
  - **Atributos Públicos**: Mostrar un ejemplo donde un atributo es `public` y se modifica directamente desde `main`. Explicar por qué esto rompe el encapsulamiento y es riesgoso.
  - **Falta de Encapsulación**: Código donde la lógica de acceso/modificación de un atributo está dispersa en lugar de centralizada en un **setter**.
  - **Duplicación de Código**: Mostrar un escenario simple donde la misma lógica se repite en varios lugares en lugar de ponerla en un método de la clase.
  - **Nombres de Métodos/Variables Confusos**: Código difícil de leer por nombres poco descriptivos.
  - **Problemas con Constructores**: Olvidar `this`., llamar al constructor incorrecto, no inicializar todos los atributos importantes.
- **Buenas Prácticas a reforzar**:
  - **"Private by Default"**: Siempre empezar declarando atributos como `private` a menos que haya una razón muy específica para no hacerlo (raro al inicio).
  - **Uso Consistente de Getters/Setters**: Acceder y modificar atributos privados siempre a través de sus **getters** y **setters** públicos desde fuera de la clase.
  - **Validación en Setters**: Implementar lógica de validación en los setters para asegurar la integridad de los datos.
  - **Nombres Descriptivos**: Clases en `PascalCase`, atributos/métodos en `camelCase`, constantes en `UPPER_SNAKE_CASE`.
  - **Métodos con Responsabilidad Única**: Cada método debe hacer una cosa bien definida.

## 4. Mini Proyecto: Sistema de Gestión de Productos

- **Objetivo**: Aplicar de forma integrada los conceptos de la clase en un pequeño sistema funcional.
- **Dinámica**: Guiar a los estudiantes en la creación de un programa simple que utilice la clase Producto (creada o revisada en el taller) para simular operaciones básicas.
- **Pasos a seguir (guiados por el tutor)**:

  1. Asegurarse de que la clase `Producto` tenga atributos privados (`nombre`, `precio`, `stock`), un constructor y **getters**/**setters**. Añadir un método `vender(int cantidadVendida)` que reduzca el stock si hay suficiente y retorne el monto total de la venta, o 0 si no hay stock.
  2. Crear una clase principal con `main` (ej. `TiendaApp.java`).
  3. Dentro del `main`, crear al menos dos objetos `Producto` usando el constructor.
  4. Mostrar la información inicial de los productos usando sus `getters`.
  5. Simular una venta:
      - Preguntar al usuario cuántas unidades de un producto específico desea comprar (usar `Scanner`).
      - Llamar al método `vender()` del objeto `Producto` correspondiente con la cantidad ingresada.
      - Mostrar el resultado de la venta (monto total o mensaje de stock insuficiente) usando el valor retornado por `vender`.
      - Mostrar el stock actualizado del producto usando el getter después de la venta.
  6. (Opcional, si hay tiempo) Simular una reposición: Crear un método `reponer(int cantidadRepuesta)` en la clase `Producto` y llamarlo desde `main`.

## Cierre y Próximos Pasos

- Recapitular los conceptos reforzados y presentar el próximo ejercicio autónomo.
- Resumen de la tutoría: Modelado, Clases/Objetos, Encapsulamiento, Getters/Setters, errores comunes, aplicación en el mini-proyecto.
- Asegurarse de que los estudiantes se sientan más cómodos con la creación y uso de clases y objetos encapsulados.
- Introducir el ejercicio autónomo para que sigan practicando estos conceptos de forma independiente.
- Sesión de preguntas finales.
