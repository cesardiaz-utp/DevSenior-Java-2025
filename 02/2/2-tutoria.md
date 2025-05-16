# Tutoría: Reforzamiento de Jerarquías, Abstracción y Reutilización

## 1. Repaso y Sondeo Inicial

- **Objetivo**: Evaluar la comprensión de los conceptos de la Clase 2 (Herencia, Polimorfismo, Interfaces, Abstractas) y el ejercicio práctico.

- **Dinámica**:
  - Iniciar preguntando: "¿Cómo les fue con los conceptos de Herencia, Polimorfismo e Interfaces? ¿Qué les resultó más desafiante?"
  - Preguntas clave:
    - "¿Cuál es la diferencia fundamental entre herencia (`extends`) e implementación (`implements`)?"
    - "¿Qué problema resuelve la palabra clave `super()`?"
    - "¿Pueden explicar la diferencia entre sobrecarga y sobrescritura?"
    - "¿Cuándo usarían una clase abstracta en lugar de una interfaz?"
    - "¿Qué significa que una interfaz sea un 'contrato'?"
    - "¿Encontraron dificultades con el ejercicio práctico de Empleados/Trabajable?"
  - Anotar los puntos débiles para abordarlos durante la tutoría.

## 2. Análisis de Diagramas de Clases Simples

- **Objetivo**: Practicar la lectura de diagramas de clases y la identificación de relaciones de herencia e implementación.
- **Dinámica**:
  - Presentar 2-3 diagramas de clases simples (pueden ser dibujados a mano o usando herramientas online como draw.io). Ejemplos:
    - Una jerarquía de vehículos (Vehiculo -> Coche, Moto, Camion).
    - Una jerarquía de formas geométricas (Figura -> Circulo, Cuadrado, Triangulo).
    - Clases que implementan una interfaz (ej. Animal -> Perro, Gato; Interfaz SonidoHacible).
  - **Para cada diagrama**:
    - Preguntar: "¿Cuál es la superclase? ¿Cuáles son las subclases?"
    - "¿Qué atributos y métodos creen que tendría la superclase?"
    - "¿Qué atributos y métodos serían específicos de cada subclase?"
    - "¿Hay alguna interfaz? ¿Qué clases la implementan? ¿Qué significa eso?"
    - "¿Qué miembros de la superclase serían accesibles en las subclases?"

## 3. Ejercicio Colaborativo: Modelo Básico de Zoológico

- **Objetivo**: Aplicar Herencia, Polimorfismo y Abstracción de forma integrada en un modelo simple.
- **Dinámica**: Como grupo (o en parejas si es remoto), construir un pequeño modelo de zoológico en código Java.
  1. **Clase Base Abstracta `Animal`**:
      - Crear una clase `Animal` y declararla `abstracta`.
      - Atributos comunes privados: `nombre` (String), `edad` (int).
      - Constructor para inicializar estos atributos.
      - Getters para `nombre` y `edad`.
      - Método concreto `dormir()` que imprima un mensaje genérico.
      - Método abstracto `hacerSonido()` (sin implementación).
      - Método abstracto `mover()` (sin implementación).
  2. **Subclases Concretas**:
      - Crear clases `Leon`, `Aguila`, `Pez` que extiendan de `Animal`.
      - Implementar los constructores, usando `super()`.
      - Implementar los métodos abstractos `hacerSonido()` y `mover()` en cada subclase con lógica específica (ej. `Leon` ruge y camina, `Aguila` chilla y vuela, `Pez` no hace sonido y nada). Usar `@Override`.
  3. **Interfaces de Comportamiento**:
      - Crear interfaces `Volador` con método `volar()`.
      - Crear interfaz `Nadador` con método `nadar()`.
  4. **Implementar Interfaces**:
      - Modificar `Aguila` para que **implemente** `Volador` y proporcione la implementación de `volar()`.
      - Modificar `Pez` para que **implemente** `Nadador` y proporcione la implementación de `nadar()`.
  5. Clase Principal (`main`) para Demostración:
      - Crear una clase principal (ej. **ZooApp.java**) con un método `main`.
      - Crear **variables individuales** para objetos de `Leon`, `Aguila`, `Pez`.

        ```java
        Leon miLeon = new Leon("Simba", 5);
        Aguila miAguila = new Aguila("Hedwig", 3);
        Pez miPez = new Pez("Nemo", 1);
        ```

      - **Demostración de Polimorfismo (Abstracta)**: Crear variables de tipo `Animal` y asignarles los objetos concretos. Llamar a los métodos `dormir()`, `hacerSonido()`, `mover()` a través de estas referencias `Animal`.

        ```java
        System.out.println("\n--- Demostración de Polimorfismo (Animal) ---");
        Animal animal1 = miLeon;
        Animal animal2 = miAguila;
        Animal animal3 = miPez;

        animal1.dormir(); animal1.hacerSonido(); animal1.mover();
        animal2.dormir(); animal2.hacerSonido(); animal2.mover();
        animal3.dormir(); animal3.hacerSonido(); animal3.mover();
        ```

      - **Demostración de Polimorfismo (Interfaces):** Crear variables de tipo `Volador` y `Nadador` y asignarles los objetos correspondientes. Llamar a los métodos de las interfaces.

          ```java
        System.out.println("\n--- Demostración de Polimorfismo (Interfaces) ---");
        Volador volador = miAguila;
        Nadador nadador = miPez;

        volador.volar();
        nadador.nadar();
        ```

## 4. Casos de Mal Diseño y Buenas Prácticas

- **Objetivo**: Identificar y comprender errores comunes en el diseño de clases y jerarquías.
- **Dinámica**: Presentar [2-3 escenarios](assets/mal_diseno.md) o fragmentos de código que ilustren malas prácticas relacionadas con la herencia y la abstracción.
- **Casos a discutir**:
  - **Herencia Innecesaria**: Una clase `A` extiende de `B`, pero `A` no es un tipo de `B` en el sentido conceptual, o solo hereda un par de métodos sin una relación lógica fuerte. (Ej. `ClaseUtilidades` que hereda de `String` solo por un método).
- **Duplicación de Código a Pesar de Herencia Posible**: Mostrar dos clases con atributos y métodos muy similares que podrían haberse beneficiado de una superclase común.
- **Romper el Encapsulamiento en Subclases**: Acceder directamente a atributos `protected` de la superclase cuando sería mejor usar un getter (si existe lógica asociada al acceso).
- **Crear Clases Abstractas/Interfaces sin Propósito Claro**: Definir abstracciones que no se usan o no simplifican el diseño.

### Cierre y Próximos Pasos

- **Objetivo**: Recapitular los conceptos reforzados y presentar el próximo ejercicio autónomo.
- R**esumen de la tutoría**: Identificación de jerarquías, aplicación de polimorfismo con abstractas/interfaces, análisis de diseño, errores comunes.
- Asegurarse de que los estudiantes se sientan más cómodos modelando con herencia y abstracción.
- Introducir el ejercicio autónomo para que sigan practicando estos conceptos de forma independiente.
- Sesión de preguntas finales.
