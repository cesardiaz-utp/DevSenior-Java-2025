# Ejercicio Autónomo: Modelando una Flota de Vehículos

Este ejercicio te desafiará a aplicar los conceptos de Herencia, Polimorfismo (sobrecarga y sobrescritura) e Interfaces, modelando diferentes tipos de vehículos y sus comportamientos.

## Objetivos de Aprendizaje del Ejercicio

- Diseñar y crear una jerarquía de clases utilizando herencia (`extends`).
- Implementar constructores en subclases y usar `super()` para inicializar la parte heredada.
- Aplicar polimorfismo de sobrescritura (`@Override`) para adaptar métodos en subclases.
- Definir e implementar una interfaz (`interface`, `implements`) para definir un contrato de comportamiento común.
- Demostrar polimorfismo utilizando referencias de la superclase y de la interfaz en tu programa principal, sin usar arrays o colecciones.
- Opcional: Aplicar polimorfismo de sobrecarga en alguna clase.

## Instrucciones Detalladas

### 1. Crear la Clase Base `Vehiculo`

- Crea una clase llamada `Vehiculo`.
- Atributos privados: `marca` (String), `modelo` (String), `año` (int).
- Constructor para inicializar estos atributos.
- Getters para los atributos.
- Método público `mostrarInformacion()` que imprima la información básica del vehículo (marca, modelo, año).
- Método público mover() que imprima un mensaje genérico como "El vehículo se está moviendo."

### 2. Crear Subclases que Hereden de `Vehiculo`

- Crea al menos tres subclases que hereden de Vehiculo:
  - Coche
  - Motocicleta
  - Camion
- Cada subclase debe tener un constructor que use `super()` para inicializar los atributos heredados y, opcionalmente, puede añadir atributos propios (ej. `numeroPuertas` en `Coche`, `cilindrada` en `Motocicleta`, `capacidadCarga` en `Camion`).
- Implementa getters para los atributos propios de cada subclase.
- **Sobrescribe** (`@Override`) el método `mover()` en cada subclase para imprimir un mensaje específico (ej. "El coche se mueve por la carretera.", "La motocicleta acelera.", "El camión avanza lentamente.").
- **Sobrescribe** (`@Override`) el método `mostrarInformacion()` en cada subclase para que primero llame a `super.mostrarInformacion()` y luego añada los detalles específicos de esa subclase.

### 3. Crear una Interfaz de Comportamiento `Arrancable`

- Crea una interfaz llamada `Arrancable`.
- Declara un método abstracto `void arrancar()`.

### 4. Implementar la Interfaz

- Modifica las clases `Coche` y `Motocicleta` para que **implementen** la interfaz `Arrancable`.
- En cada una de estas clases, proporciona una implementación concreta para el método `arrancar()`. La implementación debe imprimir un mensaje que describa cómo arranca ese tipo específico de vehículo.

### 5. Clase Principal (`main`) para Demostración

- Crea una clase principal (ej. `FlotaApp.java`) con un método `main`.
- Dentro de `main`:
  - Crea **variables individuales** para al menos un objeto de `Coche`, un objeto de `Motocicleta` y un objeto de `Camion`.

    ```Java
    Coche miCoche = new Coche("Toyota", "Corolla", 2022, 4);
    Motocicleta miMoto = new Motocicleta("Honda", "CBR500R", 2023, 500);
    Camion miCamion = new Camion("Volvo", "FH16", 2021, 25000.0);
    ```

  - Llama a los métodos `mostrarInformacion()` y `mover()` de cada uno de estos objetos para ver su comportamiento específico.
  - **Demostración de Polimorfismo (Sobrescritura por Herencia)**: Crea variables usando el tipo de la superclase `Vehiculo` y asígnales los objetos concretos. Llama a los métodos `mostrarInformacion()` y `mover()` a través de estas referencias `Vehiculo`. Observa cómo se ejecutan las versiones sobrescritas.

    ```Java
    System.out.println("\n--- Demostración de Polimorfismo (Vehiculo) ---");
    Vehiculo vehiculo1 = miCoche;
    Vehiculo vehiculo2 = miMoto;
    Vehiculo vehiculo3 = miCamion;

    vehiculo1.mostrarInformacion();
    vehiculo1.mover();
    System.out.println("---");
    vehiculo2.mostrarInformacion();
    vehiculo2.mover();
    System.out.println("---");
    vehiculo3.mostrarInformacion();
    vehiculo3.mover();
    ```

- **Demostración de Polimorfismo (Interfaces)**: Crea variables usando el tipo de la interfaz `Arrancable` y asígnales los objetos que la implementan. Llama al método `arrancar()`.

    ```Java
    System.out.println("\n--- Demostración de Polimorfismo (Arrancable) ---");
    Arrancable arrancable1 = miCoche;
    Arrancable arrancable2 = miMoto;

    arrancable1.arrancar();
    arrancable2.arrancar();
    ```

- **Opcional: Demostración de Sobrecarga**: Si añadiste algún método sobrecargado en alguna clase, demuéstralo en el `main`.

## Criterios de Evaluación (Autónoma)

Al completar este ejercicio, revisa tu propio código y ejecución considerando:

- ¿Creaste la clase base `Vehiculo` y las subclases `Coche`, `Motocicleta`, `Camion` utilizando `extends`?
- ¿Los atributos en todas las clases son `private` y accedes a ellos usando getters/setters (si aplica) o desde métodos dentro de la clase?
- ¿Implementaste constructores en las subclases usando `super()` para inicializar los atributos heredados?
- ¿Sobrescribiste (`@Override`) los métodos `mostrarInformacion()` y `mover()` en las subclases con lógica específica?
- ¿Creaste la interfaz `Arrancable` con el método `arrancar()`?
- ¿Implementaste la interfaz `Arrancable` en `Coche` y `Motocicleta` y proporcionaste la implementación del método `arrancar()`?
- ¿Creaste objetos de cada tipo de vehículo en el `main`?
- ¿Demostraste polimorfismo de sobrescritura usando referencias de tipo `Vehiculo`?
- ¿Demostraste polimorfismo de interfaz usando referencias de tipo `Arrancable`?
- ¿Tu código es legible, está bien indentado y comentado?

Este ejercicio te dará una práctica sólida en la aplicación de Herencia, Polimorfismo e Interfaces para modelar y manipular objetos en Java.

¡Mucho éxito!
