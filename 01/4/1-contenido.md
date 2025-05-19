# Clase 4: Funciones, Modularidad y Proyecto Práctico Integrador

¡Felicidades por llegar a la última clase del Módulo 1! Hasta ahora, sabes almacenar datos, realizar cálculos, tomar decisiones y repetir acciones. En esta clase, aprenderás una técnica esencial para construir programas más grandes y organizados: las **funciones** (también llamadas **métodos** en Java). Además, consolidarás todo lo aprendido en un emocionante proyecto.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:
    - Dividir tu código en bloques reutilizables usando **funciones** (**métodos**).
    - Crear funciones que realicen tareas específicas, con o sin **retorno** de valores.
    - Pasar información a tus funciones mediante **parámetros**.
    - Comprender el **alcance** (**scope**) de las variables en tu programa.
    - Aplicar los principios básicos de **diseño modular** para organizar tu código.
    - Integrar todos los conceptos del Módulo 1 en un **proyecto práctico**.

## 1. Funciones (Métodos): Los Bloques de Construcción

Una **función** o **método** es un bloque de código que tiene un nombre y realiza una tarea específica. Piensa en ellos como herramientas que puedes usar varias veces en diferentes partes de tu programa sin tener que reescribir el mismo código.

**¿Por qué usar funciones?**

- **Reutilización**: Escribes el código una vez y lo llamas (**invocas**) cada vez que lo necesitas.
- **Organización**: Dividen un programa grande en partes más pequeñas y manejables.
- **Legibilidad**: El código es más fácil de leer y entender cuando las tareas están separadas en funciones con nombres descriptivos.
- **Mantenimiento**: Es más fácil encontrar y corregir errores o modificar una tarea si está encapsulada en una función.

Ya has usado un método: el método `main`. Es el punto de entrada de tu programa.

```Java
public class MiPrograma {
    // Este es el método main
    public static void main(String[] args) {
        // Código principal
    }
}
```

### 1.1. Declaración de Métodos (void)

Para crear tus propias funciones, necesitas declararlas. Aquí vemos un método simple que no devuelve ningún valor (usa la palabra clave `void`) y no recibe información (no tiene **parámetros**).

```Java
public class EjemploFunciones {

    // Declaración del método saludar
    // public static: Por ahora, siempre usaremos esto para los métodos que creamos en Módulo 1
    // void: Indica que este método NO devuelve ningún valor
    // saludar: Es el nombre del método
    // (): Indica que no recibe parámetros
    public static void saludar() {
        System.out.println("¡Hola desde una función!");
    }

    public static void main(String[] args) {
        System.out.println("Antes de llamar a la función.");

        saludar(); // Llamada o invocación al método saludar()

        System.out.println("Después de llamar a la función.");
    }
}
// Salida:
// Antes de llamar a la función.
// ¡Hola desde una función!
// Después de llamar a la función.
```

### 1.2. Parámetros y Argumentos

Los métodos a menudo necesitan información para realizar su tarea. Esta información se pasa a través de **parámetros**. Cuando llamas a un método, los valores que le envías se llaman **argumentos**.

- **Parámetros**: Variables declaradas en la firma del método que reciben los valores.
- **Argumentos**: Los valores reales que pasas cuando llamas al método.

  ```Java
  public class EjemploFuncionesConParametros {

      // Método que recibe un parámetro de tipo String llamado 'nombre'
      public static void saludarNombre(String nombre) {
          System.out.println("¡Hola, " + nombre + "!");
      }

      // Método que recibe dos parámetros: un String y un int
      public static void imprimirMensajeRepetido(String mensaje, int veces) {
          for (int i = 0; i < veces; i++) {
              System.out.println(mensaje);
          }
      }

      public static void main(String[] args) {
          saludarNombre("Ana"); // "Ana" es el argumento para el parámetro 'nombre'
          saludarNombre("Juan"); // "Juan" es otro argumento

          imprimirMensajeRepetido("Repetir esto", 3); // "Repetir esto" y 3 son argumentos
      }
  }
  // Salida:
  // ¡Hola, Ana!
  // ¡Hola, Juan!
  // Repetir esto
  // Repetir esto
  // Repetir esto
  ```

- **Paso por Valor (para tipos primitivos)**: Cuando pasas un tipo de dato primitivo (`int`, `double`, `boolean`, `char`, etc.) como argumento a un método, Java crea una **copia** de ese valor y la pasa al parámetro del método. Si modificas el valor del parámetro dentro del método, la variable original fuera del método **no se ve afectada**.

  ```Java
  public class PasoPorValorEjemplo {

      public static void modificarNumero(int num) {
          System.out.println("Dentro del método - Antes de modificar: " + num); // Muestra la copia
          num = num * 2; // Modifica la copia local
          System.out.println("Dentro del método - Después de modificar: " + num); // Muestra la copia modificada
      }

      public static void main(String[] args) {
          int miNumero = 10;
          System.out.println("Antes de llamar al método: " + miNumero); // Salida: 10

          modificarNumero(miNumero); // Pasamos una copia de miNumero (el valor 10)

          System.out.println("Después de llamar al método: " + miNumero); // Salida: 10 (la variable original no cambió)
      }
  }
  ```

### 1.3. Métodos con Retorno

A menudo, quieres que una función no solo realice una tarea, sino que también te dé un **resultado**. Para esto, usas métodos que **devuelven un valor**. En lugar de `void`, especificas el **tipo de dato** que el método devolverá, y usas la palabra clave `return`.

```Java
public class EjemploFuncionesConRetorno {

    // Método que suma dos números enteros y devuelve el resultado (un int)
    public static int sumar(int a, int b) {
        int resultado = a + b;
        return resultado; // Devuelve el valor de 'resultado'
        // Cualquier código después de 'return' en este método NO se ejecutará
    }

    // Método que verifica si un número es par y devuelve un boolean
    public static boolean esPar(int numero) {
        return numero % 2 == 0; // Devuelve directamente el resultado de la expresión booleana
    }

    // Método que concatena dos cadenas y devuelve un String
    public static String unirCadenas(String cad1, String cad2) {
        return cad1 + " " + cad2;
    }


    public static void main(String[] args) {
        // Llamamos a sumar() y usamos el valor retornado
        int sumaTotal = sumar(5, 3);
        System.out.println("La suma es: " + sumaTotal); // Salida: La suma es: 8

        // Llamamos a esPar() y usamos el valor retornado en un condicional
        int miNumero = 4;
        if (esPar(miNumero)) {
            System.out.println(miNumero + " es un número par."); // Salida: 4 es un número par.
        }

        // Llamamos a unirCadenas() y mostramos el resultado directamente
        System.out.println(unirCadenas("Hola", "Mundo")); // Salida: Hola Mundo
    }
}
```

El tipo de dato que especificas antes del nombre del método (`int`, `boolean`, `String`, etc.) indica el tipo de valor que debes devolver usando `return`. Un método declarado como `void` no puede usar `return` para devolver un valor (solo `return;` para salir anticipadamente).

## 2. Alcance de Variables (Scope)

El alcance de una variable se refiere a la parte del código donde esa variable es accesible.

- **Alcance Local**: Las variables declaradas _dentro de un método_ o _dentro de un bloque_ de código específico (como el cuerpo de un `if`, `for`, `while`, etc.) tienen alcance local. Solo son visibles y utilizables desde el punto donde se declaran hasta el final del bloque en el que se encuentran.

  ```Java
  public class AlcanceLocalEjemplo {

      public static void metodo1() {
          int variableLocalMetodo1 = 10; // Alcance: solo dentro de metodo1()
          System.out.println("Dentro de metodo1: " + variableLocalMetodo1);
          // System.out.println(variableLocalMetodo2); // ERROR: variableLocalMetodo2 no es visible aquí
      }

      public static void metodo2() {
          int variableLocalMetodo2 = 20; // Alcance: solo dentro de metodo2()
          System.out.println("Dentro de metodo2: " + variableLocalMetodo2);
          // System.out.println(variableLocalMetodo1); // ERROR: variableLocalMetodo1 no es visible aquí
      }

      public static void main(String[] args) {
          int variableLocalMain = 30; // Alcance: solo dentro de main()
          System.out.println("Dentro de main: " + variableLocalMain);

          metodo1();
          metodo2();

          // System.out.println(variableLocalMetodo1); // ERROR: Fuera de alcance
          // System.out.println(variableLocalMetodo2); // ERROR: Fuera de alcance

          if (variableLocalMain > 0) {
              String mensaje = "Es positivo"; // Alcance: solo dentro de este bloque if
              System.out.println(mensaje);
          }
          // System.out.println(mensaje); // ERROR: 'mensaje' está fuera de alcance aquí
      }
  }
  ```

  Las variables locales solo existen mientras se ejecuta el método o bloque donde están declaradas.

- **Alcance de Clase (Miembros de Clase)**: Las variables declaradas directamente dentro de la clase, pero fuera de cualquier método, se llaman **variables de instancia** o **variables estáticas** (si usan `static`). Tienen un alcance más amplio y pueden ser accedidas por todos los métodos de la clase (dependiendo de su nivel de acceso, que veremos más adelante).  
Por ahora, nos enfocaremos principalmente en el alcance local.

Comprender el alcance es crucial para evitar errores y escribir código que funcione correctamente.

## 3. Diseño Modular: Dividiendo el Problema

La **modularidad** es la práctica de dividir un programa grande en partes más pequeñas, independientes y manejables (módulos o funciones). Cada módulo/función debe tener una responsabilidad clara.

En este módulo, tus funciones son tus "módulos". En lugar de tener todo el código en el `main`, divides las tareas en funciones: una función para leer datos, otra para calcular algo, otra para imprimir resultados, otra para validar, etc.

Esto no solo hace que tu código sea más organizado, sino que también te prepara para problemas del mundo real, donde los sistemas son complejos y necesitan ser divididos para ser manejados.

## 4. Ejercicio Práctico (Autoguiado): Calculadora de Operaciones Básicas Modular

Crear un programa simple que realice algunas operaciones matemáticas básicas, pero dividiendo la lógica en métodos separados para demostrar la modularidad y explorar el alcance de las variables.

### 4.1 Configuración Inicial

- Crea un nuevo archivo Java llamado CalculadoraModular.java.
- Define la estructura básica de la clase con el método main.

  ```Java
  public class CalculadoraModular {

      public static void main(String[] args) {
          // Aquí llamaremos a nuestras funciones
      }

      // Aquí declararemos nuestras funciones
  }
  ```

### 4.2 Método Simple sin Parámetros ni Retorno (void) - Demostrando Modularidad Básica

- Crea un método llamado `mostrarBienvenida` que simplemente imprima un mensaje de bienvenida.
- Llama a este método desde `main`.

```Java
// Dentro de la clase CalculadoraModular, fuera del main
public static void mostrarBienvenida() {
    System.out.println("--- Bienvenido a la Calculadora Modular ---");
}

// Dentro del main
public static void main(String[] args) {
    mostrarBienvenida(); // Llamada al método
    // ... resto del main ...
}
```

- **Discusión**: Explica que hemos "modularizado" la tarea de mostrar la bienvenida en un método separado.

### 4.3 Método con Parámetros (void) - Pasando Información

- Crea un método llamado `imprimirResultado` que reciba dos parámetros: un `String` para la descripción de la operación y un `double` para el resultado.
- Este método debe imprimir la descripción y el resultado formateado (ej. con 2 decimales).
- Llama a este método desde `main` con algunos ejemplos fijos (ej. `imprimirResultado("Suma:", 10.5);`).

```Java
// Dentro de la clase CalculadoraModular
public static void imprimirResultado(String operacion, double resultado) {
    System.out.printf("%s %.2f%n", operacion, resultado);
}

// Dentro del main
public static void main(String[] args) {
    mostrarBienvenida();
    imprimirResultado("Resultado fijo:", 123.456); // Llamada con argumentos
    // ... resto del main ...
}
```

- **Discusión**: Explica cómo los parámetros permiten que los métodos trabajen con datos específicos que les enviamos.

### 4.4 Método con Parámetros y Retorno - Realizando Cálculos

- Crea un método llamado `sumar` que reciba dos parámetros `double` (`num1`, `num2`) y **retorne** la suma (`double`).
- Llama a este método desde `main`, almacena el resultado en una variable y luego usa `imprimirResultado` para mostrarlo.

```Java
// Dentro de la clase CalculadoraModular
public static double sumar(double num1, double num2) {
    double resultadoSuma = num1 + num2; // Variable local dentro de sumar
    return resultadoSuma; // Retorna el resultado
}

// Dentro del main
public static void main(String[] args) {
    mostrarBienvenida();
    double numeroA = 5.0; // Variable local dentro de main
    double numeroB = 3.0; // Variable local dentro de main

    double resultadoDeSuma = sumar(numeroA, numeroB); // Llama a sumar y guarda el retorno
    imprimirResultado("Suma:", resultadoDeSuma);

    // ... resto del main ...
}
```

- **Discusión**: Explica la palabra clave `return` y cómo el valor devuelto puede ser usado en el lugar donde se llamó al método.

### 4.5 Explorando el Alcance Local

- **Demostración**: Intenta acceder a `resultadoSuma` (declarada en sumar) desde `main` o desde `imprimirResultado`. Muestra el error de compilación.
- **Explicación**: Refuerza que `resultadoSuma` solo existe y es accesible dentro del método `sumar`. `numeroA` y `numeroB` solo existen en main.

### 4.6 Método con Bucle y Condicionales (Integración)

- Crea un método llamado `contarPares` que reciba dos parámetros `int` (`inicio`, `fin`).
- Dentro de este método, usa un **bucle** `for` para iterar desde inicio hasta fin.
- Dentro del bucle, usa una **estructura condicional** `if` y el operador módulo (`%)` para verificar si cada número es par.
- Si el número es par, imprímelo.
- Este método no necesita retornar nada (`void`).
- Llama a este método desde `main`.

```Java
// Dentro de la clase CalculadoraModular
public static void contarPares(int inicio, int fin) {
    System.out.println("Números pares entre " + inicio + " y " + fin + ":");
    for (int i = inicio; i <= fin; i++) {
        if (i % 2 == 0) {
            System.out.print(i + " "); // Usamos print para que estén en la misma línea
        }
    }
    System.out.println(); // Salto de línea al final
}

// Dentro del main
public static void main(String[] args) {
    mostrarBienvenida();
    double numeroA = 5.0;
    double numeroB = 3.0;
    double resultadoDeSuma = sumar(numeroA, numeroB);
    imprimirResultado("Suma:", resultadoDeSuma);

    contarPares(1, 20); // Llama al método con bucle y condicionales

    // ... resto del main ...
}
```

- **Discusión**: Muestra cómo las estructuras de control aprendidas en la Clase 3 se usan dentro de los métodos.

### 4.7 Introducción a Variables Estáticas (Alcance de Clase)

- **Concepto**: Explica que las variables declaradas con la palabra clave `static` directamente dentro de la clase (fuera de cualquier método) pertenecen a la clase misma, no a una instancia específica (objeto). Son compartidas por todos los métodos `static` de la clase.
- **Demostración**: Declara una variable estática para contar cuántas veces se ha llamado a un método (aunque no es el mejor caso de uso, ilustra el alcance). O declara una constante estática (aunque las constantes ya las vimos, refuerza el `static`). **Mejor demostración**: Declara una variable estática simple y modifica su valor en un método, y muéstrala en otro método.

```Java
public class AlcanceEstaticoEjemplo {

    // Variable estática (alcance de clase)
    static int contadorLlamadas = 0;

    public static void incrementarContador() {
        contadorLlamadas++; // Accede y modifica la variable estática
        System.out.println("Contador incrementado. Valor actual: " + contadorLlamadas);
    }

    public static void mostrarContador() {
        System.out.println("El contador actual es: " + contadorLlamadas); // Accede a la variable estática
    }

    public static void main(String[] args) {
        mostrarContador(); // Salida: El contador actual es: 0
        incrementarContador(); // Salida: Contador incrementado. Valor actual: 1
        incrementarContador(); // Salida: Contador incrementado. Valor actual: 2
        mostrarContador(); // Salida: El contador actual es: 2
    }
}
```

- **Discusión**: Compara el alcance de `contadorLlamadas` (estático, visible en ambos métodos) con el alcance de las variables locales (como `num` en `incrementarContador`, que solo existe dentro de ese método). Explica que las variables estáticas son útiles para datos que son compartidos por toda la clase `static`.

### 4.8 Integración con Scanner (Opcional, si el tiempo lo permite)

Si el tiempo permite, modifica `main` para usar `Scanner` para leer los números para la suma o el rango para contar pares, llamando a los métodos creados. Esto integra los conceptos de entrada/salida con la modularidad.

## Resumen de la Clase

- Aprendiste a crear tus propios bloques de código reutilizables: **funciones** (**métodos**).
- Sabes cómo declarar métodos que no devuelven valor (`void`) y métodos que sí.
- Comprendes cómo pasar información a las funciones usando **parámetros** y **argumentos**, y el concepto de **paso por valor** para primitivos.
- Entiendes el **alcance local** de las variables y por qué es importante.
- Tienes una idea básica de la **modularidad** y cómo las funciones ayudan a organizar el código.

¡Estás listo para aplicar todos los conceptos del Módulo 1 en un [proyecto integrador](3-proyecto.md)!
