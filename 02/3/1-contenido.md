# Clase 3: Modelado Avanzado de Clases en POO

¡Bienvenido a la tercera clase del Módulo 2! Ya sabes crear clases, objetos, usar herencia y polimorfismo. Ahora, vamos a profundizar en cómo las clases interactúan y se relacionan entre sí para construir sistemas más complejos y realistas. Aprenderás a modelar relaciones como "tiene un" de diferentes maneras y a usar la abstracción de forma más estratégica. También introduciremos una estructura de datos fundamental: los Arrays, que nos ayudarán a manejar colecciones de objetos en nuestras relaciones.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- Utilizar **Arrays** para almacenar colecciones de datos del mismo tipo.
- Comprender y modelar diferentes tipos de relaciones entre clases: **Asociación**, **Composición** y **Agregación**, representando colecciones de objetos con Arrays.
- Implementar relaciones bidireccionales simples en tu código.
- Utilizar **clases abstractas** y **métodos abstractos** para definir plantillas con comportamiento incompleto.
- Consolidar la comprensión de las diferencias y usos de **clases abstractas** e **interfaces**.
- Aplicar estos conceptos de modelado avanzado en un ejercicio práctico de diseño de sistema.

## 1. Introducción a Arrays: Colecciones Simples y de Tamaño Fijo

Hasta ahora, hemos trabajado con variables individuales para almacenar datos u objetos. Pero, ¿qué pasa si necesitas almacenar varios valores del mismo tipo, como las notas de un estudiante o una lista de productos en un pedido? Para eso usamos estructuras de datos. La estructura de datos más básica en Java es el **Array**.

### 1.1. ¿Qué es un Array?

Un **Array** es un contenedor que almacena una secuencia ordenada de elementos del mismo tipo. Una vez que se crea un array, su **tamaño es fijo**; no puedes cambiar cuántos elementos puede contener.

![Estructura de dato Array](assets/arrays.png)

### 1.2. Declaración

Dices el tipo de dato de los elementos seguido de corchetes `[ ]` y el nombre del array.

```Java
int[] ages; // Declaración de un array de enteros
String[] names; // Declaración de un array de cadenas de texto
double[] prices; // Declaración de un array de números decimales
```

### 1.3. Inicialización (Creación)

Usas la palabra clave `new` y especificas el tamaño del array.

```Java
int[] ages = new int[5]; // Crea un array de 5 enteros. Inicializados a 0.
String[] names = new String[3]; // Crea un array de 3 Strings. Inicializados a null.
double[] prices = new double[10]; // Crea un array de 10 doubles. Inicializados a 0.0.
```

Cuando creas un array con `new` y un tamaño, los elementos se inicializan automáticamente con valores por defecto: `0` para tipos numéricos, `false` para booleanos, y `null` para tipos de referencia (objetos).

### 1.4. Inicialización Directa

Puedes declarar e inicializar un array con valores específicos en una sola línea. El tamaño del array se determina automáticamente por el número de valores proporcionados.

```Java
int[] scores = {85, 92, 78, 95}; // Crea un array de 4 enteros con estos valores
String[] colors = {"Red", "Green", "Blue"}; // Crea un array de 3 Strings

// El tamaño de 'scores' es 4, el tamaño de 'colors' es 3
```

### 1.5. Acceso a Elementos

Los elementos de un array se acceden utilizando un índice numérico encerrado entre corchetes `[]`. Los índices de un array siempre comienzan en `0`.

```Java
int[] numbers = {10, 20, 30, 40};

System.out.println("Primer elemento: " + numbers[0]); // Salida: Primer elemento: 10
System.out.println("Tercer elemento: " + numbers[2]); // Salida: Tercer elemento: 30

numbers[1] = 25; // Modifica el segundo elemento (índice 1)
System.out.println("Segundo elemento modificado: " + numbers[1]); // Salida: Segundo elemento modificado: 25
```

### 1.6. Longitud del Array

Puedes obtener el número de elementos en un array usando el atributo `.length`.

```Java
int[] data = {1, 2, 3, 4, 5};
System.out.println("Tamaño del array: " + data.length); // Salida: Tamaño del array: 5

// El último índice válido es length - 1
System.out.println("Último elemento: " + data[data.length - 1]); // Salida: Último elemento: 5
```

### 1.7. El error (excepción) `ArrayIndexOutOfBoundsException`

Es un error común. Ocurre cuando intentas acceder a un elemento usando un índice que está fuera del rango válido (menor que `0` o mayor o igual a `length`).

```Java
int[] smallArray = new int[2]; // Índices válidos: 0 y 1
System.out.println(smallArray[2]); // Esto causaría un ArrayIndexOutOfBoundsException
```

### 1.8. Arrays de Objetos

Puedes crear arrays para almacenar referencias a objetos.

```Java
// Asumiendo que tienes una clase Dog
Dog[] dogs = new Dog[4]; // Crea un array para 4 referencias a objetos Dog (inicializadas a null)

dogs[0] = new Dog("Buddy"); // Crea un objeto Dog y lo asigna al primer elemento
dogs[1] = new Dog("Max");

System.out.println(dogs[0].getName()); // Accede a un método del objeto en el array
System.out.println(dogs[2]); // Salida: null (el tercer elemento no ha sido inicializado con un objeto)
```

Es importante recordar que el array solo almacena las _referencias_. Debes crear los objetos individualmente y asignarlos a las posiciones del array.

### 1.9. Arrays Multidimensionales

A veces necesitas representar datos en forma de tabla, matriz o cuadrícula (por ejemplo, un tablero de ajedrez, una hoja de cálculo o una matriz matemática). Para esto, Java permite crear **arrays multidimensionales**. El más común es el array de dos dimensiones (matriz).

![Array multidimencionales](assets/arrays_multidimencional.png)

#### Declaración e Inicialización

Un array de dos dimensiones se declara usando dos pares de corchetes:

```Java
int[][] matrix; // Declaración de un array de dos dimensiones de enteros
```

Para crearlo e inicializarlo con un tamaño específico:

```Java
int[][] matrix = new int[3][4]; // 3 filas, 4 columnas
```

Esto crea una “tabla” de 3 filas y 4 columnas, donde cada elemento es un int inicializado a 0.

#### Acceso a Elementos

Para acceder o modificar un elemento, usas dos índices: el primero para la fila y el segundo para la columna.

```Java
matrix[0][2] = 7; // Asigna el valor 7 a la fila 0, columna 2
System.out.println(matrix[0][2]); // Imprime: 7
```

#### Inicialización Directa

Puedes inicializar un array de dos dimensiones con valores específicos:

```Java
int[][] board = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
// board[1][2] es 6
```

#### Arrays de Más Dimensiones

Java permite arrays de más dimensiones (ejemplo: `int[][][]`), pero en la práctica, los de dos dimensiones son los más usados.

#### Ejemplo Práctico

Supón que quieres almacenar las calificaciones de 3 estudiantes en 4 materias:

```Java
int[][] grades = new int[3][4]; // 3 estudiantes, 4 materias cada uno

grades[0][0] = 85; // Primer estudiante, primera materia
grades[2][3] = 90; // Tercer estudiante, cuarta materia
```

#### Recorrido de un Array Bidimensional

Puedes usar bucles anidados para recorrer todos los elementos:

```Java
for (int i = 0; i < grades.length; i++) { // Filas
    for (int j = 0; j < grades[i].length; j++) { // Columnas
        System.out.println("Estudiante " + i + ", Materia " + j + ": " + grades[i][j]);
    }
}
```

> **Nota:** Los arrays multidimensionales en Java son realmente arrays de arrays, por lo que las filas pueden tener diferentes longitudes si lo deseas (arrays “irregulares” o “jagged”).

## 2. Relaciones Avanzadas entre Clases: Cómo los Objetos Interactúan

Ahora que conocemos los Arrays, podemos modelar relaciones "tiene un" donde un objeto "todo" tiene _múltiples_ objetos "parte".

### 2.1. Asociación

Una clase conoce o utiliza a otra. Ahora, una clase puede tener un Array de referencias a objetos de otra clase.

![Asociación](assets/asociacion.png)

```Java
public class Student {
    private String name;
    private Course[] enrolledCourses; // Student HAS Courses (Association using Array)

    public Student(String name, Course[] enrolledCourses) {
        this.name = name;
        this.enrolledCourses = enrolledCourses; // Asigna el array de cursos
    }

    public void displayCourses() {
        System.out.println("Cursos de " + this.name + ":");
        // Iterar sobre el array (veremos bucles en detalle en M1)
        for (int i = 0; i < enrolledCourses.length; i++) {
            System.out.println("- " + enrolledCourses[i].getName()); // Accede a cada objeto Course en el array
        }
    }
}
```

```Java
public class Course {
    private String name;

    public Course(String name) {
        this.name = name;
    }

    public String getName() { return this.name; }
}
```

### 2.2. Composición (Relación "Parte-Todo Fuerte")

- La "parte" no existe lógicamente sin el "todo". Dependencia fuerte en ciclo de vida.
- **Analogía**: Un `Order` (Pedido) compone `OrderItem[]` (Array de Ítems de Pedido). Los ítems específicos de un pedido solo existen como parte de ese pedido.
- **Implementación en Java**: El Array de objetos "parte" a menudo **se crea e inicializa dentro del constructor o de un método del objeto "todo"**. El "todo" es responsable de la creación y gestión del ciclo de vida de las "partes" contenidas en el array.

![Composición](assets/composicion.png)

```Java
public class OrderItem {
    private String itemName;
    private int quantity;
    private double unitPrice;

    public OrderItem(String itemName, int quantity, double unitPrice) {
        this.itemName = itemName;
        this.quantity = quantity;
        this.unitPrice = unitPrice;
    }

    public double calculateSubtotal() {
        return this.quantity * this.unitPrice;
    }

    public String getItemName() { return this.itemName; }
    public int getQuantity() { return this.quantity; }
    public double getUnitPrice() { return this.unitPrice; }
}
```

```Java
public class Order {
    private int orderNumber;
    private OrderItem[] items; // Order HAS OrderItems (Composition using Array)

    // The Order creates its own array of OrderItems
    public Order(int orderNumber, int numberOfItems) {
        this.orderNumber = orderNumber;
        // Composition: The Order creates the array of its parts
        this.items = new OrderItem[numberOfItems];
        // The individual OrderItem objects still need to be created and added later
         System.out.println("Pedido número " + this.orderNumber + " creado con espacio para " + numberOfItems + " ítems.");
    }

    // Method to add an item (simplified)
    public void addItem(int index, OrderItem item) {
        if (index >= 0 && index < this.items.length) {
            this.items[index] = item;
             System.out.println("Ítem '" + item.getItemName() + "' agregado al pedido en posición " + index);
        } else {
             System.out.println("Índice inválido para agregar ítem.");
        }
    }

    public double calculateTotal() {
        double total = 0;
        // Iterate through the array to sum subtotals
        for (int i = 0; i < this.items.length; i++) {
            if (this.items[i] != null) { // Check if the element is not null
                total += this.items[i].calculateSubtotal();
            }
        }
        return total;
    }

    public int getOrderNumber() { return this.orderNumber; }
    public OrderItem[] getItems() { return this.items; } // Be careful returning internal arrays!
}
```

### 2.3. Agregación (Relación "Parte-Todo Débil")

- La "parte" puede existir independientemente del "todo". Dependencia débil en ciclo de vida.
- **Analogía**: Un `Department` (Departamento) agrega `Employee[]` (Array de Empleados). Los empleados existen fuera del departamento y podrían trabajar en otro departamento.
- **Implementación en Java**: El Array de objetos "parte" a menudo se **crea _fuera_ del objeto "todo" y se le pasa** (ej. al constructor o setter del "todo"). El "todo" simplemente mantiene referencias a estas "partes" que fueron creadas externamente.

![Agregación](assets/agregacion.png)

```Java
public class Employee {
    private String name;
    private String id;

    public Employee(String name, String id) {
        this.name = name;
        this.id = id;
    }

    public String getName() { return this.name; }
    public String getId() { return this.id; }
}
```

```Java
public class Department {
    private String name;
    private Employee[] employees; // Department HAS Employees (Aggregation using Array)

    // The array of Employees is passed FROM OUTSIDE
    public Department(String name, Employee[] employees) {
        this.name = name;
        // Aggregation: The Department receives an array of externally created Employees
        this.employees = employees;
        System.out.println("Departamento '" + this.name + "' creado.");
    }

    public void displayEmployees() {
        System.out.println("Empleados en " + this.name + ":");
        // Iterate through the array
        for (int i = 0; i < this.employees.length; i++) {
            if (this.employees[i] != null) {
                System.out.println("- " + this.employees[i].getName() + " (ID: " + this.employees[i].getId() + ")");
            }
        }
    }

    public String getName() { return this.name; }
    public Employee[] getEmployees() { return this.employees; } // Be careful returning internal arrays!
}
```

```Java
public class CompanyApp {
    public static void main(String[] args) {
        // Employees are created independently
        Employee emp1 = new Employee("Juan", "E001");
        Employee emp2 = new Employee("Ana", "E002");
        Employee emp3 = new Employee("Pedro", "E003");

        // Create an array of Employees
        Employee[] itEmployees = {emp1, emp2}; // This array exists here

        // Create a Department and pass the existing array of Employees
        Department itDept = new Department("IT", itEmployees); // Aggregation

        // Create another array for a different department
        Employee[] salesEmployees = {emp3};
        Department salesDept = new Department("Sales", salesEmployees); // Aggregation

        itDept.displayEmployees();
        salesDept.displayEmployees();

        // If itDept object is removed, emp1 and emp2 still exist in memory
    }
}
```

### 2.4. Relaciones Bidireccionales y Restricciones

Ambas clases se conocen. Implementación con atributos de referencia en ambas clases. Gestión cuidadosa en constructores/setters. Las restricciones se implementan con lógica.

## 3. Abstracción con Clases Abstractas y Interfaces

Ya introdujimos las clases abstractas y las interfaces como herramientas para la abstracción. Ahora, veamos cómo se usan estratégicamente en el diseño de sistemas con relaciones.

### 3.1. Clases Abstractas

- Se usan para definir una base común para clases relacionadas en una jerarquía "es un tipo de", cuando la clase base no tiene sentido ser instanciada por sí sola, o cuando tiene métodos que deben ser implementados de forma específica por las subclases.
- Pueden tener atributos (estado), métodos concretos (con implementación) y **métodos abstractos** (sin implementación).
- Las subclases **concretas** (no abstractas) que extienden una clase abstracta **deben** implementar todos sus métodos abstractos.
- Son útiles cuando tienes una implementación parcial que quieres compartir.
- **Uso con Arrays**: Puedes tener un array de referencias a una clase abstracta para almacenar objetos de cualquiera de sus subclases concretas, aprovechando el polimorfismo.

```Java
// Abstract class: Represents a generic Shape
public abstract class Shape {
    private String color;

    public Shape(String color) {
        this.color = color;
    }

    public String getColor() { return this.color; }

    // Abstract methods: Must be implemented by concrete subclasses
    public abstract double calculateArea();
    public abstract double calculatePerimeter();
}
```

```Java
// Concrete subclass
public class Circle extends Shape {
    private double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * this.radius * this.radius;
    }

    @Override
    public double calculatePerimeter() {
        return 2 * Math.PI * this.radius;
    }
}
```

```Java
// Class that holds an array of abstract class references
public class Drawing {
    private Shape[] shapes; // Drawing HAS Shapes (Composition or Aggregation, depending on creation)

    public Drawing(int maxShapes) {
        // Composition if drawing creates the array
        this.shapes = new Shape[maxShapes];
        System.out.println("Dibujo creado con espacio para " + maxShapes + " figuras.");
    }

    // Method to add a shape (accepts any object that is a Shape)
    public void addShape(int index, Shape shape) {
         if (index >= 0 && index < this.shapes.length) {
            this.shapes[index] = shape;
             System.out.println("Figura agregada al dibujo en posición " + index);
        } else {
             System.out.println("Índice inválido para agregar figura.");
        }
    }

    public void displayAreas() {
        System.out.println("Áreas de las figuras en el dibujo:");
        // Iterate and call calculateArea() polymorphically
        for (int i = 0; i < this.shapes.length; i++) {
            if (this.shapes[i] != null) {
                // The correct calculateArea() method is called based on the actual object type (Circle, etc.)
                System.out.printf("- Área: %.2f%n", this.shapes[i].calculateArea());
            }
        }
    }
}
```

En el ejemplo anterior, `Drawing` tiene un array de `Shape`. Puedes poner cualquier subclase concreta de `Shape` (como `Circle`) en ese array, y al llamar a `calculateArea()`, Java ejecutará el método correcto para el objeto específico en esa posición del array.

### 3.2. Interfaces

- Se usan para definir un contrato de comportamiento. Especifican _qué_ debe hacer una clase, pero no _cómo_.
- Todos los métodos abstractos son implícitamente `public abstract`. Las variables son `public static final`.
- Una clase que implementa una interfaz **debe** proporcionar implementación para todos sus métodos abstractos.
- Son útiles para definir capacidades que clases potencialmente no relacionadas pueden compartir (ej. `Flyable`, `Swimmable`). Permiten la implementación múltiple.
- **Uso con Arrays**: Similar a las clases abstractas, puedes tener un array de referencias a una interfaz para almacenar objetos de cualquiera de las clases que implementan esa interfaz, aprovechando el polimorfismo.

```Java
// Interface: Defines a contract for anything that can make a sound
public interface SoundEmitter {
    void makeSound(); // Implícitamente public abstract
}
```

```Java
// Class implementing the interface
public class Dog implements SoundEmitter {
    private String name;

    public Dog(String name) {
        this.name = name; 
    }

    @Override public void makeSound() { System.out.println(this.name + " dice: ¡Guau guau!"); }
}
```

```Java
// Another class implementing the same interface (not related by inheritance to Dog)
public class CarAlarm implements SoundEmitter {
    @Override 
    public void makeSound() {
        System.out.println("La alarma del coche suena: ¡Bip bip bip!");
    }
}
```

```Java
// Class that holds an array of interface references
public class SoundSystem {
    private SoundEmitter[] emitters; // SoundSystem HAS SoundEmitters (Aggregation or Composition)

    public SoundSystem(int maxEmitters) {
        this.emitters = new SoundEmitter[maxEmitters];
         System.out.println("Sistema de sonido creado con espacio para " + maxEmitters + " emisores.");
    }

    // Method to add an emitter (accepts any object implementing SoundEmitter)
    public void addEmitter(int index, SoundEmitter emitter) {
         if (index >= 0 && index < this.emitters.length) {
            this.emitters[index] = emitter;
             System.out.println("Emisor de sonido agregado en posición " + index);
        } else {
             System.out.println("Índice inválido para agregar emisor.");
        }
    }

    public void activateAll() {
        System.out.println("Activando todos los emisores de sonido:");
        // Iterate and call makeSound() polymorphically
        for (int i = 0; i < this.emitters.length; i++) {
            if (this.emitters[i] != null) {
                // The correct makeSound() method is called based on the actual object type (Dog, CarAlarm, etc.)
                this.emitters[i].makeSound();
            }
        }
    }
}
```

En este caso, `SoundSystem` tiene un array de `SoundEmitter`. Puedes poner cualquier objeto que implemente `SoundEmitter` (como `Dog` o `CarAlarm`) en ese array, y al llamar a `makeSound()`, se ejecutará la implementación específica de cada objeto.

### 4. Ejercicio Práctico en Clase: Modelando un Sistema Básico de Restaurante

Este ejercicio se realizará de forma guiada en clase para aplicar los conceptos de modelado de clases, relaciones (Asociación, Composición, Agregación), encapsulamiento, constructores, abstracción y **Arrays**.

**Objetivo del Ejercicio**: Diseñar y codificar las clases principales para un sistema simplificado de gestión de un restaurante, enfocándose en las relaciones entre ellas y utilizando Arrays para representar colecciones.

**Reglas de Codificación**: Escribe el código en **inglés**. Los comentarios dentro del código y los mensajes de salida en consola (`System.out.println`, `printf`) deben estar en **español**.

### Identificar las Entidades Principales

Discutir con los estudiantes qué "cosas" o conceptos son importantes en un sistema de restaurante (Restaurante, Mesa, Pedido, Ítem del Menú, Ítem del Pedido, Empleado/Mesero).

### Crear la Clase `Restaurant`

- **Atributos privados**: `name` (String), `address` (String).
- **Relación con Mesas (Agregación)**: Añade un atributo `private Table[] tables;`. El restaurante _agrega_ mesas que existen independientemente. Pasa un Array de objetos `Table` al constructor del `Restaurant`.
- **Relación con Empleados (Agregación)**: Añade un atributo `private Employee[] employees;`. Pasa un Array de objetos `Employee` al constructor.
- **Relación con Menú (Composición o Agregación)**: Crea una clase `Menu`. Decide si el `Menu` _compone_ el `Restaurant` o es _agregado_. Implementa la relación con un atributo `private Menu menu;`. Si es composición, crea el `Menu` dentro del constructor del `Restaurant`. Si es agregación, pásalo al constructor.
- Constructor que inicialice los atributos y reciba los Arrays de `Table` y `Employee` y el objeto `Menu`.
- Getters para todos los atributos.
- Método `displayMenu()`: Llama al método `displayItems()` del objeto `Menu`.
- Método `findTable(int tableNumber)`: Recorre el array `tables` y retorna la `Table` con el número especificado, o `null` si no se encuentra.

### Crear la Clase `Table`

- **Atributos privados**: `tableNumber` (int), `capacity` (int), `isOccupied` (boolean - inicialmente false).
- **Relación con Pedido (Asociación)**: Añade un atributo `private Order currentOrder;`. Una mesa _tiene un_ pedido actual. Implementa la relación.
- Constructor.
- Getters.
- Setter para `isOccupied`.
- Método `assignOrder(Order order)`: Asigna un `Order` a `currentOrder` y setea `isOccupied` a `true`.
- Método `clearTable()`: Setea `isOccupied` a `false` y `currentOrder` a `null`.
- Método `getCurrentOrder()`: Retorna el `currentOrder`.

### Crear la Clase `MenuItem`

- **Atributos privados**: `name` (String), `price` (double).
- Constructor.
- Getters.

### Crear la Clase `OrderItem`

- **Atributos privados**: `menuItem` (una referencia a un objeto `MenuItem`), `quantity` (int).
- Constructor que reciba un `MenuItem` y una `quantity`.
- Getters.
- Método `calculateSubtotal()` que retorne el precio del `MenuItem` por la `quantity`.

### Crear la Clase `Order`

- **Atributos privados**: `orderNumber` (int), `status` (String - ej. "Pending", "In Progress", "Delivered").
- `Relación con Ítems del Pedido (Composición)`: Añade un atributo `private OrderItem[] items;`. El pedido _compone_ ítems. El Array `items` debe ser **creado dentro del constructor** de `Order`, recibiendo el tamaño máximo de ítems.
- Constructor que inicialice el número y estado, y cree el array `items` con un tamaño dado.
- Método `addItem(OrderItem item)`: Añade un `OrderItem` al array `items` en la primera posición `null` disponible. Debe manejar el caso de que el array esté lleno.
- Getters.
- Método `calculateTotal()`: Itera sobre el array `items`, suma el subtotal de cada `OrderItem` (verificando que no sea `null`) y retorna el total.
- Método `displayDetails()`: Imprime el número del pedido, estado y los detalles de cada ítem en el array `items`.

### Crear la Clase `Menu`

- **Atributos privados**: name (String).
- **Relación con Ítems del Menú (Agregación)**: Añade un atributo `private MenuItem[] items;`. El menú _agrega_ ítems. El Array `items` debe ser **pasado al constructor** de `Menu`.
- Constructor que inicialice el nombre y reciba un Array de `MenuItem`s.
- Getters.
- Método `displayItems()`: Itera sobre el array `items` e imprime los detalles de cada `MenuItem`.
- Método `findItem(String itemName)`: Recorre el array `items` y retorna el `MenuItem` con el nombre especificado, o `null` si no se encuentra.

### Crear la Clase `Employee`

- **Atributos privados**: `name` (String).
- Constructor.
- Getter.

### Clase Principal (`main`)

- Crea una clase principal (ej. `RestaurantApp.java`) con un método `main`.
- Dentro de `main`:
  - Crea algunos objetos `MenuItem`.
  - Crea un Array de `MenuItem`s y úsalo para crear un objeto `Menu`.
  - Crea un Array de objetos `Table` y úsalo para crear un objeto `Restaurant`.
  - Crea un Array de objetos `Employee` y úsalo para crear el mismo objeto `Restaurant`.
  - Llama a `restaurant.displayMenu()`.
  - Crea un objeto `Order`, especificando un tamaño máximo de ítems (ej. 5).
  - Busca `MenuItem`s en el `Menu` (usando `menu.findItem()`) y úsalos para crear objetos `OrderItem`.
  - Añade los `OrderItem`s al `Order` (usando `order.addItem()`).
  - Busca una `Table` en el `Restaurant` (usando `restaurant.findTable()`) y asigna el `Order` a esa `Table`.
  - Llama a `order.displayDetails()` y `order.calculateTotal()`.
  - Demuestra la relación bidireccional si la implementaste (ej. desde el pedido, acceder a la mesa si es posible).
