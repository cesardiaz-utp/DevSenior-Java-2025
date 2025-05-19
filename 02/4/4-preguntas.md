# Preguntas de Entrevista Técnica: Módulo 2 - Programación Orientada a Objetos

Este documento contiene preguntas comunes que podrías encontrar en una entrevista técnica para un puesto junior o de entrada, basadas en los conceptos que aprendiste en el Módulo 2. Intenta responderlas por tu cuenta antes de ver las respuestas.

## Sección 1: Clases, Objetos, Constructores y Encapsulamiento

### 1. ¿Cuál es la diferencia fundamental entre una clase y un objeto en Programación Orientada a Objetos?

Una **clase** es una plantilla o plano para crear objetos. Define la estructura (atributos) y el comportamiento (métodos) que todos los objetos de ese tipo tendrán. Un **objeto** es una instancia concreta de una clase. Es una entidad real que existe en la memoria de la computadora, creada a partir de una clase, con su propio estado (valores específicos en sus atributos).

### 2. Explica el concepto de Encapsulamiento. ¿Por qué es importante y cómo se logra en Java?

El **Encapsulamiento** es un principio de POO que consiste en agrupar los datos (atributos) y los métodos que operan sobre esos datos dentro de una sola unidad (la clase), y **ocultar los detalles internos** del objeto, controlando el acceso a sus datos desde fuera.
Es importante porque:

- **Protege la integridad de los datos**: Evita que se modifiquen de forma inesperada.
- **Permite el control y la validación**: Podemos añadir lógica en los métodos de acceso/modificación (getters/setters).
- **Facilita la mantenibilidad**: Podemos cambiar la implementación interna sin afectar el código que usa la clase, siempre que la interfaz pública (los métodos) se mantenga. En Java, se logra principalmente haciendo los atributos privados (`private`) y proporcionando métodos públicos (`public`) para acceder a ellos y/o modificarlos, conocidos convencionalmente como **getters** y **setters**.

### 3. ¿Qué es un constructor en Java? ¿Puede una clase tener más de un constructor?

Un **constructor** es un método especial que se utiliza para crear objetos (instanciar una clase) e inicializar el estado de esos objetos. Tiene el mismo nombre que la clase y no tiene tipo de retorno (ni siquiera `void`).

Sí, una clase puede tener **múltiples constructores**, siempre y cuando cada uno tenga una **lista de parámetros diferente** (diferente número, tipo o orden de parámetros). Esto se conoce como **sobrecarga de constructores** y permite crear objetos de diferentes maneras.

### 4.¿Cuál es el propósito de la palabra clave `this` en Java?

La palabra clave `this` se refiere a la **instancia actual** del objeto. Dentro de un método o constructor de una clase, `this` te permite referirte al objeto específico sobre el cual estás operando en ese momento.

Su uso más común es para **diferenciar entre los atributos de la instancia y los parámetros o variables locales** que tienen el mismo nombre, especialmente en constructores y setters (ej. `this.nombreAtributo = nombreParametro;`).

## Sección 2: Herencia, Polimorfismo e Interfaces

### 5. Explica el concepto de Herencia en Java. ¿Para qué se usa la palabra clave `extends` y cuáles son sus beneficios?

La **Herencia** es un mecanismo que permite que una clase (subclase o clase hija) herede atributos y métodos de otra clase (superclase o clase padre). Esto modela la relación "es un tipo de" (ej. Un `Perro` es _un tipo de_ `Animal`).

La palabra clave `extends` se utiliza para indicar que una clase hereda de otra (ej. `class Perro extends Animal { ... }`).

Sus beneficios principales son:

- **Reutilización de código**: No tienes que redeclarar o reimplementar miembros que ya existen en la superclase.
- **Organización y modelado**: Crea jerarquías claras que reflejan relaciones del mundo real.
- **Facilita el polimorfismo**: Es la base para que objetos de subclases puedan ser tratados como objetos de la superclase.

### 6. ¿Cuál es la diferencia entre una superclase y una subclase?

La **Superclase** (o clase padre/base) es la clase de la cual se heredan miembros. La **Subclase** (o clase hija/derivada) es la clase que hereda miembros de la superclase y puede añadir sus propios miembros o modificar los heredados.

### 7. Explica el propósito de la llamada `super()` en un constructor de una subclase

La llamada `super(argumentos)` se utiliza dentro del constructor de una subclase para **llamar al constructor de la superclase**. Su propósito es inicializar la parte heredada del objeto que se está creando. **Debe ser la primera sentencia** en el constructor de la subclase. Si la superclase tiene un constructor con parámetros, la subclase debe llamar explícitamente a uno de los constructores de la superclase usando `super()`.

### 8. ¿Qué es Polimorfismo? Explica los dos tipos principales en Java (Sobrecarga y Sobrescritura) y la diferencia entre ellos

**Polimorfismo** significa "muchas formas". Se refiere a la capacidad de un objeto o método de tomar diferentes formas o comportarse de diferentes maneras.

Los dos tipos principales en Java son:

- **Polimorfismo de Sobrecarga (Overloading)**: Ocurre en **tiempo de compilación**. Permite tener múltiples métodos en la _misma_ clase con el _mismo nombre pero diferentes listas de parámetros_ (número, tipo u orden de parámetros). El compilador elige qué método llamar basándose en los argumentos que se le pasan.

    ```Java
    public class Example {
        public void display(int num) {
            System.out.println("Mostrar entero: " + num);
        }
        public void display(String text) { // Sobrecarga - Overload
            System.out.println("Mostrar cadena: " + text);
        }
    }
    ```

- **Polimorfismo de Sobrescritura (Overriding)**: Ocurre en **tiempo de ejecución**. Permite que una _subclase_ proporcione su propia implementación para un método que ya está definido en su superclase. La versión del método que se ejecuta se determina por el tipo _real_ del objeto al que se hace referencia, no por el tipo de la variable de referencia. Requiere que la firma del método sea idéntica a la de la superclase.

  ```Java
  public class Animal {
      public void makeSound() { System.out.println("Sonido generico"); }
  }
  public class Dog extends Animal {
      @Override // Sobrescritura - Override
      public void makeSound() { System.out.println("Guau guau"); }
  }
  // En main: Animal a = new Dog(); a.makeSound(); // Ejecuta la version de Dog
  ```

La diferencia clave es que la **sobrecarga** es una característica dentro de una sola clase y se resuelve en **compilación** (basado en la firma del método), mientras que la **sobrescritura** es una característica de la herencia entre clases y se resuelve en ejecución (basado en el tipo real del objeto).

### 9. ¿Qué es un método sobrescrito? ¿Para qué se utiliza la anotación `@Override`?

Un método **sobrescrito** es un método en una subclase que tiene la misma firma (nombre y lista de parámetros) y tipo de retorno que un método en su superclase. La subclase proporciona su propia implementación para ese método.

La anotación `@Override` es opcional, pero altamente recomendada. Le dice al compilador que intentas sobrescribir un método de la superclase. Si cometes un error en la firma del método (ej. nombre mal escrito, parámetros incorrectos), el compilador te avisará, evitando que crees un nuevo método sobrecargado por accidente en lugar de sobrescribir el deseado.

### 10. ¿Qué es una Interfaz en Java? ¿Para qué se usa la palabra clave implements y cuál es su propósito principal?

Una **Interfaz** es un contrato. Define un conjunto de métodos abstractos (sin implementación, antes de Java 8) y constantes que una clase que la implemente **debe** proporcionar. Se declara con la palabra clave `interface`.

La palabra clave `implements` se usa para indicar que una clase se compromete a proporcionar implementaciones para todos los métodos abstractos declarados en una o varias interfaces (ej. `class MyClass implements MyInterface, AnotherInterface { ... }`).

Su propósito principal es:

- Definir un **contrato de comportamiento** que clases diferentes (incluso no relacionadas por herencia) pueden cumplir.
- Lograr **abstracción**, ocultando los detalles de implementación.
- Permitir **polimorfismo**, tratando objetos de diferentes clases de manera uniforme a través de la referencia de la interfaz.
- Simular **herencia múltiple de comportamiento** (una clase puede implementar varias interfaces).

### 11. ¿Qué es una Clase Abstracta en Java? ¿Para qué se usa la palabra clave `abstract`?

Una **Clase Abstracta** es una clase que no puede ser instanciada directamente (no puedes usar `new NombreClaseAbstracta()`). Se declara usando la palabra clave `abstract`. Están diseñadas para ser superclases en jerarquías de herencia.

La palabra clave `abstract` se usa para declarar la clase como abstracta (`public abstract class MyAbstractClass { ... }`) o para declarar métodos abstractos dentro de ella (`public abstract void myAbstractMethod();`). Un método abstracto no tiene implementación (solo la firma seguida de ;) y fuerza a las subclases concretas a proporcionar su propia implementación.

Las clases abstractas pueden tener atributos, constructores, métodos concretos (con implementación) y métodos abstractos.

### 12. Compara Clases Abstractas e Interfaces. ¿Cuándo usarías una sobre la otra?

- **Clase Abstracta**: Se usa cuando quieres compartir **implementación común, estado (atributos)** y definir algunos métodos que deben ser implementados por las subclases. Representa una relación "es un tipo de" y permite herencia simple.
- **Interfaz**: Se usa cuando quieres definir solo un **contrato de comportamiento** (qué hacer, no cómo hacerlo) que clases potencialmente no relacionadas pueden cumplir. No puede tener estado de instancia (solo constantes). Permite implementación múltiple. Usarías una **clase abstracta** si tienes una base común con código que quieres compartir y/o necesitas definir atributos que representen un estado base para la jerarquía. Usarías una interfaz si solo quieres definir un conjunto de capacidades que diferentes clases deben tener, sin importar su estructura interna o jerarquía de herencia.

## Sección 3: Modelado Avanzado de Clases (Relaciones) y Arrays

### 13. Explica los diferentes tipos de relaciones "tiene un" entre clases (Asociación, Agregación, Composición). ¿Cuál es la diferencia clave entre Agregación y Composición?

Son formas de modelar la relación "tiene un" entre clases:

- **Asociación**: La relación más general. Una clase conoce o usa a otra. Se representa con un atributo de un tipo en otra clase.
- **Agregación**: Una relación "parte-todo" **débil**. El objeto "parte" (el contenido) puede existir independientemente del objeto "todo" (el contenedor). Si el todo desaparece, la parte puede seguir existiendo.
- **Composición**: Una relación "parte-todo" **fuerte**. El objeto "parte" no puede existir lógicamente sin el objeto "todo" en ese contexto. Si el todo desaparece, la parte asociada también pierde sentido o se considera destruida. La **diferencia clave** es la **dependencia del ciclo de vida**. En **Composición**, el ciclo de vida de la parte está ligado al del todo (el todo crea o gestiona exclusivamente la parte); en **Agregación**, la parte tiene un ciclo de vida independiente del todo.

### 14. ¿Cómo representarías una relación "uno a muchos" o "muchos a muchos" simple en Java?

Se representan utilizando un **Array** como atributo en la clase que representa el lado "uno" o un lado del "muchos a muchos".

- **Uno a Muchos**: La clase "uno" tendría un atributo que es un Array de referencias a objetos de la clase "muchos" (ej. `Department` tiene `Employee[]`).
- **Muchos a Muchos (simplificado)**: Ambas clases tendrían un Array de referencias a objetos de la otra clase, y la relación se gestionaría estableciendo las referencias en ambos arrays cuando se crea el vínculo (ej. `Student` tiene `Course[]`, y `Course` tiene `Student[]` - aunque la gestión en arrays es compleja). A menudo, se introduce una tercera clase de "enlace" para muchos a muchos, pero usando solo arrays puros, es más directo poner arrays en ambas clases.

### 15. ¿Qué es un Array en Java? ¿Cuáles son sus características principales?

Un **Array** es una estructura de datos básica en Java que almacena una colección **ordenada** de elementos del **mismo tipo**.

Sus características principales son:

- **Tamaño fijo**: Una vez creado, su tamaño no puede cambiar.
- **Acceso por índice**: Los elementos se acceden utilizando un índice numérico que comienza en 0.
- **Tipo homogéneo**: Todos los elementos deben ser del mismo tipo de dato (primitivo o de referencia).

## Sección 4: Proyecto Integrador (Temas de Organización y Diseño)

### 16. Explica el concepto de Paquetes (`package`) en Java. ¿Por qué son importantes y cómo los utilizas?

Los **Paquetes** son un mecanismo para organizar clases relacionadas en espacios de nombres jerárquicos (que corresponden a estructuras de carpetas).

Son importantes para:

- **Organización**: Mantener el código ordenado y modular.
- **Evitar conflictos de nombres**: Dos clases con el mismo nombre pueden coexistir si están en paquetes diferentes.
- **Control de acceso**: Los modificadores de acceso `default` y `protected` se basan en paquetes. Se utilizan declarando el paquete al que pertenece una clase (`package com.mycompany.myapp;` como primera línea del archivo Java) e importando clases de otros paquetes que necesitas usar (`import com.otherpackage.OtherClass;`).

### 17. Describe brevemente la Arquitectura por Capas en el contexto de organizar una aplicación Java. ¿Cuáles son las capas típicas?

La **Arquitectura por Capas** es un patrón de diseño que divide una aplicación en divisiones lógicas (capas) con responsabilidades específicas. Cada capa solo interactúa con las capas adyacentes (típicamente hacia abajo), creando una separación de preocupaciones.

Las capas típicas en una aplicación Java básica son:

- **Capa de Presentación / UI (User Interface)**: Maneja la interacción con el usuario (ej. entrada/salida por consola, interfaz gráfica).
- **Capa de Lógica de Negocio / Servicio (Service)**: Contiene la lógica principal de la aplicación y las reglas de negocio.
- **Capa de Modelo / Datos (Model/DAO - Data Access Object)**: Representa las entidades del negocio (clases de datos) y maneja la interacción con la fuente de datos (base de datos, archivos, etc.). Organizar tus clases en paquetes que reflejen estas capas es una buena práctica (ej. `com.myapp.ui`, `com.myapp.service`, `com.myapp.model`).

### 18. ¿Qué es un Enum en Java? ¿Cuándo lo usarías? Proporciona un ejemplo

Un **Enum** (enumeración) es un tipo de dato especial que define un conjunto fijo y limitado de constantes con nombre.

Lo usarías para representar un conjunto cerrado de valores que no cambian, como estados, tipos, roles, días de la semana, puntos cardinales, etc. Es más seguro y legible que usar `int` o `String` para estos propósitos.

**Ejemplo**:

```Java
/**
 * Posibles estados de una tarea.
 */
public enum TaskStatus {
    PENDING("Pendiente"),
    IN_PROGRESS("En Progreso"),
    COMPLETED("Completada"),
    CANCELLED("Cancelada");

    private String displayName;

    TaskStatus(String displayName) {
        this.displayName = displayName;
    }

    public String getDisplayName() {
        return this.displayName;
    }
}
```

### 19. ¿Para qué se utiliza Javadoc? ¿Cómo escribirías un comentario Javadoc básico para un método?

**Javadoc** es una herramienta de documentación estándar de Java que genera documentación HTML a partir de comentarios especiales en el código fuente. Se utiliza para explicar el propósito de clases, interfaces, constructores, métodos y atributos (especialmente los públicos/protegidos), cómo usarlos, sus parámetros, valores de retorno, etc.
Permite generar documentación de la API de tu código de forma automática.

Un comentario Javadoc básico para un método:

```Java
public class Calculator {
    /**
     * Suma dos números enteros.
     *
     * @param a El primer numero. // Describe el primer parametro
     * @param b El segundo numero. // Describe el segundo parametro
     * @return *La suma de a y b. // Describe el valor que retorna
     */
    public int add(int a, int b) {
        return a + b;
    }
}
```

### 20. Menciona algunas buenas prácticas de codificación que hayas aprendido en este módulo de POO

- **Encapsulamiento**: Siempre hacer los atributos `private` y usar getters/setters.
- **Nombres Descriptivos**: Usar nombres claros y estándar (`camelCase` para variables/métodos, `PascalCase` para clases, `UPPER_SNAKE_CASE` para constantes).
- **Principio de Responsabilidad Única**: Diseñar clases y métodos que tengan un único propósito bien definido.
- **Usar Enums**: Para conjuntos fijos de constantes con significado.
- **Usar `final`**: Para variables que no cambian de valor.
- **Documentar**: Usar Javadoc para explicar la API pública.
- **Código Legible**: Mantener la identación y formateo consistentes.
- **Manejo de Arrays**: Verificar límites de índices y null al trabajar con arrays.

### 21. Describe cómo abordarías el diseño de un sistema simple (por ejemplo, un sistema básico de gestión de una biblioteca donde los usuarios pueden tomar prestados libros) utilizando los conceptos de POO aprendidos en este módulo. Menciona algunas clases, sus posibles relaciones y qué principios de POO aplicarías

Abordaría el diseño identificando las entidades principales: `Book` (Libro), `Member` (Miembro/Usuario de la biblioteca), `Library` (la biblioteca como sistema).

- **Clases**: Book, Member, Library.
- **Atributos**: Book podría tener title, author, ISBN, isBorrowed (boolean). Member podría tener name, memberId, borrowedBooks (un Array de Books). Library podría tener name, availableBooks (un Array de Books), members (un Array de Members).
- **Relaciones**:
  - `Library` agrega `Book[]` (los libros existen independientemente de la biblioteca).
  - `Library` agrega `Member[]` (los miembros existen independientemente de la biblioteca).
  - `Member` asocia `Book[]` (o quizás modelar esto como una relación gestionada por la biblioteca).
  - Aplicaría **Encapsulamiento** a todos los atributos.
  - Utilizaría **Constructores** para crear instancias de cada entidad.
  - Modelaría la relación "Biblioteca tiene Libros/Miembros" y "Miembro tiene Libros Prestados" usando **Arrays**. La relación Biblioteca-Libro/Miembro sería **Agregación**. La relación Miembro-Libros Prestados podría ser **Composición** si asumimos que esos libros prestados solo pertenecen a la colección de prestados de ese miembro en ese momento.
  - Implementaría métodos como `addBook(Book)`, `addMember(Member)` en `Library`, `borrowBook(Book, Member)` en `Library`, `returnBook(Book, Member)` en `Library`, `displayBorrowedBooks()` en `Member`.
  - Aplicaría **Polimorfismo** si hubiera diferentes tipos de miembros (ej. `StudentMember`, `FacultyMember` heredando de `Member`) o diferentes tipos de libros.
  - Podría usar **Enums** para el estado de un libro (Available, Borrowed)
  - Organizaría en **paquetes** (ej. `model`, `service`, `app`).
