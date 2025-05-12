# Clase 2: Variables, Tipos de Datos, Constantes y Operadores

En esta clase, vamos a sumergirnos en cómo Java maneja la información. Aprenderás a almacenar diferentes tipos de datos, a realizar cálculos y a asegurar que tu código sea flexible pero robusto.

## Objetivos de Aprendizaje de la Clase

Al finalizar esta clase, serás capaz de:

- Comprender y utilizar correctamente los tipos de datos primitivos en Java.
- Declarar variables, constantes y realizar conversiones de tipo, incluyendo la inferencia de tipo con `var`.
- Utilizar operadores aritméticos básicos en expresiones simples y compuestas.
- Implementar la entrada y salida de datos por consola de forma efectiva.

## 1. Fundamentos de Datos en Java

Para que un programa haga algo útil, necesita trabajar con información (datos). Las variables son como "cajas" con nombre donde guardamos esos datos.

![Variables](https://okhosting.com/wp-content/uploads/2019/01/variables-400x250.png)

### 1.1. ¿Qué son las Variables?

Una **variable** es un espacio en la memoria de la computadora que tiene un nombre, un tipo de dato y puede almacenar un valor que puede cambiar durante la ejecución del programa.

- **Declaración**: Le dices a Java qué tipo de dato va a almacenar tu variable y le das un nombre.
  
  ```Java
  int edad; // Declara una variable 'edad' de tipo entero
  double precio; // Declara una variable 'precio' de tipo decimal
  ```

- **Inicialización**: Le das un valor inicial a la variable.

  ```Java
  int edad = 30; // Declara e inicializa 'edad' con el valor 30
  double precio = 99.99; // Declara e inicializa 'precio' con el valor 99.99
  ```

- **Asignación**: Cambias el valor de una variable ya declarada.

  ```Java
  edad = 31; // Cambia el valor de 'edad' a 31
  precio = 85.50; // Cambia el valor de 'precio' a 85.50
  ```

- **Reglas para nombrar variables**:
  - Deben empezar con una letra, `_` (guion bajo) o `$`.
  - No pueden contener espacios.
  - Son sensibles a mayúsculas y minúsculas (`miVariable` es diferente de `mivariable`).
  - Se recomienda usar `camelCase` (la primera palabra en minúscula y las siguientes palabras empiezan con mayúscula, ej. `numeroDeEstudiantes`).
  - No pueden ser palabras clave de Java (ej. `int`, `class`, `public`).

### 1.2. Java es un Lenguaje Fuertemente Tipado

Java es un lenguaje **fuertemente tipado** (o de tipado estático). Esto significa que:

- Cada variable tiene un `tipo de dato definido` (ej. `int`, `double`, `String`).
- El tipo de una variable se verifica en **tiempo de compilación** (antes de que el programa se ejecute).
- Una vez que una variable es declarada con un tipo, **solo puede almacenar valores de ese tipo**. No puedes asignar un valor de un tipo incompatible sin realizar una conversión explícita (casting).

```Java
int contador = 10; // 'contador' es de tipo int

contador = 5.5; // ERROR de compilación: No puedes asignar un double a un int directamente
String nombre = 123; // ERROR de compilación: No puedes asignar un int a un String
```

Esta característica ayuda a detectar muchos errores comunes (como intentar operar con tipos incompatibles) **antes** de que el programa llegue a ejecutarse, lo que hace que Java sea más robusto y menos propenso a errores en tiempo de ejecución relacionados con tipos de datos.

### 1.3. Tipos de Datos Primitivos en Java

Java tiene 8 tipos de datos primitivos, que son los bloques de construcción fundamentales para almacenar valores. Ocupan un tamaño fijo en memoria.

| Tipo de Dato | Descripción | Rango (aproximado) | Tamaño (bits) | Ejemplo de Uso | Por defecto | Sufijo |
| --- | --- | --- | --: | --- | --- | --- |
| `byte` | Entero muy pequeño | -128 a 127 | 8 | Edad, conteo pequeño | 0 | |
| `short` | Entero corto | -32,768 a 32,767 | 16 | Cantidad de items | 0 | |
| `int` | **Entero (más común)** | -2.147.483.648 a 2.147.483.647 | 32 | Edad, cantidad de alumnos | 0 | |
| `long` | Entero largo | -9.223.372.036.854.775.808 a 9.223.372.036.854.775.807 | 64 | Población mundial, ID único | 0L | l / L |
| `float` | Flotante de precisión simple | -3.40282347e38 a 3.40282347e38 (hasta 7 decimales) | 32 | Coordenadas simples | 0.0f | f / F |
| `double` | **Flotante de precisión doble (más común)** | -1.79769313486231570E+308 a 1.79769313486231570E+308 (hasta 15-16 decimales) | 64 | Precios, cálculos científicos | 0.0 | d / D |
| `boolean` | Valor lógico | `true` o `false` | 1 | ¿Está activo? ¿Es verdadero? | `false` | |
| `char` | Carácter único | Un solo carácter (entre comillas simples) | 16 | 'a', 'Z', '5', '@' | '\u0000' | |

- **Ejemplos de uso de primitivos**:

  ```Java
  int cantidad = 100;
  double precioUnitario = 25.50;
  boolean estaActivo = true;
  char inicial = 'J';

  System.out.println("Cantidad: " + cantidad);
  System.out.println("Precio Unitario: " + precioUnitario);
  System.out.println("¿Está activo?: " + estaActivo);
  System.out.println("Inicial: " + inicial);
  ```

- **Los tipos de números enteros `byte`, `short`, `int`, `long`**
  - Estos representan los valores numéricos que **no** tienen parte decimal.
  - Por defecto los números enteros literales que usemos en el código, se entenderán que son de tipo `int`.
  - Para poder decir que un número entero literal es de tipo `long`, debemos agregar al final la letra `L` o `l`. Ej: `10L`
  - En los números enteros literales, podemos usar el **guion bajo** (`_`) como símbolo separador de miles para mejorar la legibilidad. Java lo ignorará al momento de operar on ellos. Ej: `10000000 -> 10_000_000`
- **Los tipos de números decimales `float`, `double`**
  - Estos representan los valores numéricos que **si** tienen parte decimal.
  - El símbolo que separa el valor entero del decimal es el **punto** (`.`)
  - Por defecto los números decimales literales que usemos en el código, se entenderán que son de tipo `double`. Ej: `12.5 = 12.5d`
  - Para poder decir que un número decimal literal es de tipo `float`, debemos agregar al final la letra `F` o `f`. Ej: `12.5f`
  - En los números decimales literales, también podemos usar el **guion bajo** (`_`) como símbolo separador de miles para mejorar la legibilidad. Java lo ignorará al momento de operar on ellos. Ej: `10000000 -> 10_000_000`
- **El tipo `char`**
  - En Java los caracteres no están restringidos a los [ASCII](https://elcodigoascii.com.ar/) sino que son [Unicode](https://symbl.cc/es/unicode-table/).
  - Un carácter está siempre rodeado de **comillas simples** (`'`) como `'A'`, `'9'`, `'ñ'`, etc.
  - Un tipo especial de carácter es la **secuencia de escape**, que se utilizan para representar caracteres de control o caracteres que no se imprimen.

      | Secuencia de escape | Descripción |
      | :-: | --- |
      | `\t` | Tabulación horizontal |
      | `\b` | Retroceso (backspace) |
      | `\n` | Salto de línea |
      | `\r` | Retorno de carro |
      | `\f` | Avance de página |
      | `\"` | Comilla doble |
      | `\'` | Comilla simple |
      | `\\` | Barra diagonal |
      | `\0` | Fin de archivo |

  - Para indicar un carácter Unicode que no puede ser representado en ASCII, como 'ö', se utilizado la secuencia de escape `'\udddd'` donde cada `d` en la secuencia de escape es un dígito hexadecimal. Ej: `'\u00F6' = 'ö'`
- **El tipo `String` (Importante: NO es primitivo)**:

  - Aunque `String` se usa muchísimo y parece un primitivo, es una **clase en Java** (un tipo de dato no primitivo u "objeto"). Representa secuencias de caracteres (texto). Se declara y usa de manera similar:

    ```Java
    String nombre = "Juan Pérez"; // Texto va entre comillas dobles
    String mensaje = "¡Hola, " + nombre + "!"; // Se puede concatenar con el operador +
    String emoji = "\uD83D\uDE00"; // 😀

    System.out.println(mensaje);
    System.out.println(emoji);
    ```
  
  - Se define como una colección de caracteres char encerradas en **comillas dobles** (`"`). Ej: `"Hola Mundo"`
  - En algunos otros lenguajes de programación, una cadena o string es una matriz o array de caracteres. Este no es el caso con Java. **Los Strings son objetos**.
  - **Cadena de caracteres multilínea**:
    - Las cadenas de caracteres multilínea, también conocidas como bloques de texto, se introdujeron como una característica en Java 15. Estas permiten definir cadenas que abarcan varias líneas de una manera más legible y conveniente.
    - Se define utilizando tres comillas dobles (`"""`) al principio y al final del bloque de texto.

      ```Java
      String cadenaMultilinea = """
          Este es un ejemplo
          de una cadena
          multilínea en Java.
          """;
      ```

    - **Características y ventajas**
      - **Legibilidad**: Permite escribir cadenas largas y complejas de manera más clara y organizada.
      - **Manejo de espacios en blanco**: La indentación inicial se determina por la posición del cierre de las comillas triples, lo que ayuda a mantener el formato deseado.
      - **Compatibilidad con caracteres especiales**: No es necesario escapar caracteres especiales como las comillas dobles dentro del bloque de texto.

        ```Java
        String consultaSQL = """
            SELECT *
            FROM usuarios
            WHERE edad > 18
            ORDER BY nombre;
            """;
        ```

      - E**spacios en blanco**: La indentación inicial se elimina automáticamente, pero cualquier espacio adicional dentro del bloque se mantiene.
      - **Errores comunes**: Asegúrate de que el bloque de texto comience con `"""` seguido de un salto de línea y termine con `"""` en una nueva línea para evitar errores de compilación.

        ```Java
        String mensaje = """
            Hola,
            Este es un mensaje
            que abarca varias líneas.
            """;
        System.out.println(mensaje);
        ```

### 1.4. Inferencia de Tipo con `var` (Java 10+)

A partir de Java 10, la palabra clave `var` te permite declarar variables locales sin especificar explícitamente su tipo, siempre y cuando el tipo pueda ser **inferido** por el compilador a partir de su valor inicial.

- **Concepto**: `var` no hace que Java sea un lenguaje de tipado dinámico. El tipo de la variable sigue siendo estático (se define en tiempo de compilación), solo que es el compilador quien lo deduce, en lugar de que tú lo escribas explícitamente.
- **Ventajas**:
  - **Concisión**: Reduce la verbosidad del código, especialmente con tipos complejos.
  - **Legibilidad**: Puede mejorar la legibilidad cuando el tipo es obvio a partir del inicializador.
- **Ejemplos**:

  ```Java
  // En lugar de:
  // String nombreCompleto = "Ana María López";
  // int edadUsuario = 25;
  // double temperatura = 36.7;

  // Puedes usar 'var':
  var nombreCompleto = "Ana María López"; // Java infiere String
  var edadUsuario = 25;               // Java infiere int
  var temperatura = 36.7;             // Java infiere double

  System.out.println("Nombre: " + nombreCompleto);
  System.out.println("Edad: " + edadUsuario);
  System.out.println("Temperatura: " + temperatura);

  var listaNumeros = new java.util.ArrayList<Integer>(); // Java infiere ArrayList<Integer>
  // Es lo mismo que:
  // ArrayList<Integer> listaNumeros = new ArrayList<Integer>();
  ```

- **Limitaciones Importantes (Cuándo NO usar `var`)**:
  - **Solo para Variables Locales**: No se puede usar para declarar campos de clase, parámetros de métodos, o tipos de retorno de métodos.
  - **Debe ser Inicializada**: La variable `var` debe ser inicializada en la misma línea en que se declara. El compilador necesita el valor inicial para inferir el tipo.

    ```Java
    var miVariable; // ERROR: No se puede usar 'var' sin inicializarla
    miVariable = 10;
    ```

  - **No puede inicializarse con `null`**: El compilador no puede inferir un tipo a partir de `null`.

    ```Java
    var miObjeto = null; // ERROR: Tipo no se puede inferir
    ```

  - **No es tipado dinámico**: Una vez que el compilador infiere un tipo, ese tipo es fijo. No puedes reasignar un valor de un tipo diferente.

    ```Java
    var contador = 0; // Infiere int
    // contador = "Hola"; // ERROR: No se puede asignar un String a un int
    ```

- **Buenas Prácticas con `var`**:
  - Úsala cuando el tipo sea obvio y la concisión mejore la legibilidad.
  - Evita usarla si el tipo no es evidente para el lector.

### 1.5. Declaración y Uso de Constantes (`final`)

Una **constante** es una variable cuyo valor, una vez asignado, no puede ser modificado durante la ejecución del programa. Se declaran usando la palabra clave `final`.

- **Sintaxis**:

  ```Java
  final tipoDato NOMBRE_CONSTANTE = valor;
  ```

- **Convención de Nombres**: Las constantes suelen nombrarse en `UPPER_SNAKE_CASE` (todo en mayúsculas, palabras separadas por guiones bajos) para distinguirlas fácilmente de las variables.
- **Ejemplo**:

  ```Java
  final double PI = 3.14159; // Una constante para el valor de Pi
  final int MAX_INTENTOS = 3; // Número máximo de intentos

  // PI = 3.14; // Esto causaría un error de compilación, ¡no se puede cambiar!

  double radio = 5.0;
  double areaCirculo = PI *radio* radio;
  System.out.println("El área del círculo es: " + areaCirculo);
  ```

  Las constantes mejoran la legibilidad y evitan errores al usar valores fijos en el código.

## 2. Conversión de Tipos (Type Casting)

A veces, necesitas convertir un valor de un tipo de dato a otro. Java maneja esto de dos formas:

### 2.1. Conversión Implícita (Widening Conversion - Ampliación)

- Ocurre automáticamente cuando conviertes un tipo de dato de "menor capacidad" a uno de "mayor capacidad".
- No hay riesgo de pérdida de datos.
- Ejemplo: Convertir un `int` a un `double`.

  ```Java
  int miEntero = 10;
  double miDoble = miEntero; // Conversión implícita: 10 se convierte en 10.0

  System.out.println("Entero: " + miEntero); // Salida: Entero: 10
  System.out.println("Doble: " + miDoble);   // Salida: Doble: 10.0
  ```

### 2.2. Conversión Explícita (Casting - Narrowing Conversion - Estrechamiento)

- Ocurre cuando conviertes un tipo de dato de "mayor capacidad" a uno de "menor capacidad".
- Requiere que le digas a Java explícitamente que estás de acuerdo con la posible pérdida de datos.
- Se usa el operador de casting `(tipoDato)`.
- **Advertencia**: Puede haber pérdida de información o truncamiento (decimales se pierden, valores fuera de rango se "envuelven").
- **Ejemplo**: Convertir un `double` a un `int`.

  ```Java
  double otroDoble = 9.75;
  int otroEntero = (int) otroDoble; // Conversión explícita: 9.75 se trunca a 9

  System.out.println("Doble: " + otroDoble);     // Salida: Doble: 9.75
  System.out.println("Entero (cast): " + otroEntero); // Salida: Entero (cast): 9

  // Otro ejemplo con posible pérdida de información
  int numGrande = 200;
  byte numByte = (byte) numGrande; // byte solo va hasta 127. Ocurrirá un desbordamiento.
  System.out.println("Int original: " + numGrande); // Salida: Int original: 200
  System.out.println("Byte (cast): " + numByte);     // Salida: Byte (cast): -56 (valor incorrecto)
  ```

## 3. Operadores en Java

Los operadores son símbolos que le dicen al compilador que realice distintas operaciones.

### 3.1. Operadores Aritméticos

Realizan operaciones matemáticas básicas.

| Operador | Descripción | Ejemplo | Resultado |
| :-: | --- | --- | --- |
| `+` | Suma | `5 + 3` | `8` |
| `-` | Resta | `10 - 4` | `6` |
| `*` | Multiplicación | `2 * 7` | `14` |
| `/` | División | `10.0 / 3.0` (double) | `3.333...` |
| `/` | División (entera) | `10 / 3` (int) | `3` |
| `%` | Módulo (Residuo) | `10 % 3` | `1` |

- **Consideraciones con la División (`/`)**:
  - Si ambos operandos son `int`, el resultado es un `int` (se trunca la parte decimal). `10 / 3` es `3`.
  - Si al menos uno de los operandos es `double` (o `float`), el resultado es un `double` (con decimales). `10.0 / 3` es `3.333...`.
- **Operador Módulo (`%`)**: Es muy útil para saber si un número es par/impar (ej. `numero % 2 == 0` para par) o para obtener el último dígito de un número.
  
  ```Java
  int resultado1 = 5 + 3 *2; // 5 + 6 = 11
  int resultado2 = (5 + 3)* 2; // 8 * 2 = 16
  int resultado3 = 17 / 5; // 3 (división entera)
  double resultado4 = 17.0 / 5; // 3.4 (división con decimales)
  int resultado5 = 17 % 5; // 2 (el residuo de 17/5 es 2)

  System.out.println("Resultado 1: " + resultado1); // Salida: 11
  System.out.println("Resultado 2: " + resultado2); // Salida: 16
  System.out.println("Resultado 3: " + resultado3); // Salida: 3
  System.out.println("Resultado 4: " + resultado4); // Salida: 3.4
  System.out.println("Resultado 5: " + resultado5); // Salida: 2
  ```

### 3.2 Operadores de acumulación (Incremento y Decremento)

| Operador | Descripción | Ejemplo | Resultado |
| :-: | --- | :-: | :-: |
| `++` | Suma y asigna | `a++` | `a = a + 1` |
| `--` | Resta y asigna | `a--` | `a = a - 1` |

- Los operadores `++` y `--` en Java se utilizan para incrementar o decrementar el valor de una variable en 1, respectivamente.
- Estos operadores pueden ser utilizados en dos formas: **prefijo** y **sufijo**, y cada forma tiene un comportamiento ligeramente diferente.

- **Operador de Incremento (`++`)**
  - **Prefijo** (`++variable`): Incrementa el valor de la variable antes de que se utilice en la expresión.
  - **Sufijo** (`variable++`): Incrementa el valor de la variable después de que se utilice en la expresión.
- **Operador de Decremento (`--`)**
  - **Prefijo** (`--variable`): Decrementa el valor de la variable antes de que se utilice en la expresión.
  - **Sufijo** (`variable--`): Decrementa el valor de la variable después de que se utilice en la expresión.

  ```Java
  int a = 5;
  int b = 5;

  // Incremento prefijo
  System.out.println("++a: " + ++a); // Output: 6
  // Incremento sufijo
  System.out.println("b++: " + b++); // Output: 5
  System.out.println("b después de b++: " + b); // Output: 6

  int c = 5;
  int d = 5;

  // Decremento prefijo
  System.out.println("--c: " + --c); // Output: 4
  // Decremento sufijo
  System.out.println("d--: " + d--); // Output: 5
  System.out.println("d después de d--: " + d); // Output: 4
  ```

### 3.3 Operadores de asignación compuesta

| Operador | Descripción | Ejemplo | Resultado |
| :-: | --- | :-: | :-: |
| `+=` | Suma y asigna | `a += b` | `a = a + b` |
| `-=` | Resta y asigna | `a -= b` | `a = a - b` |
| `*=` | Multiplica y asigna | `a *= b` | `a = a * b` |
| `/=` | Divide y asigna | `a /= b` | `a = a / b` |
| `%=` | Calcula el módulo y asigna | `a %= b` | `a = a % b` |

- Estos combinan una operación aritmética con una asignación.
- Estos operadores son útiles para simplificar el código y hacer las operaciones más concisas.

  ```java
  int x = 10;
  x += 5; // Ahora x es 15
  x -= 3; // Ahora x es 12
  x *= 2; // Ahora x es 24
  x /= 4; // Ahora x es 6
  x %= 5; // Ahora x es 1
  ```

### 3.4  Precedencia de operadores

- La precedencia de operadores en Java determina el orden en que se evalúan los operadores en una expresión. Esto es crucial para asegurar que las operaciones se realicen en el orden correcto, similar a cómo se manejan las operaciones matemáticas.

  | Prioridad | Operador |
  | :-: | --- |
  | 1 | Operadores Postfijos: `expr++`, `expr--` |
  | 2 | Operadores Unarios: `++expr`, `--expr`, `-expr` |
  | 3 | Multiplicación y División: `*`, `/`, `%` |
  | 4 | Suma y Resta: `+`, `-` |
  | 5 | Asignación: `=`, `+=`, `-=`, `*=`, `/=`, `%=` |

- Operadores de mayor precedencia (número de prioridad mas pequeño) se evalúan antes que los de menor precedencia. Operadores con la misma precedencia se evalúan de izquierda a derecha (asociatividad de izquierda) dependiendo del operador.

- **Uso de Paréntesis**: Los paréntesis se utilizan para alterar la precedencia natural de los operadores.

  ```text
  13 - 4 * ( 5 - 2 ) + 3 * ( 2 + 8 )
  13 - 4 *     3     + 3 *     10
  13 -   12          +   30
  31
  ```

  ```Java
  int a = 10;
  int b = 5;
  int c = 2;

  // Ejemplo de precedencia
  int resultado = a + b * c; // Multiplicación se realiza primero
  System.out.println("Resultado: " + resultado); // Output: 20

  // Uso de paréntesis para alterar la precedencia
  resultado = (a + b) * c; // Suma se realiza primero
  System.out.println("Resultado con paréntesis: " + resultado); // Output: 30
  ```
  
  En el ejemplo anterior, sin paréntesis, la multiplicación se realiza antes que la suma debido a su mayor precedencia. Al usar paréntesis, forzamos a que la suma se realice primero.

## 4. Entrada y Salida de Datos por Consola y Diferencia Primitivos vs. Objetos

### 4.1. Leyendo Entrada del Usuario con `Scanner`

Para que tu programa pueda interactuar con el usuario y leer lo que este escribe en la consola, usamos la clase Scanner.

- **Pasos**:
  1. **Importar `Scanner`**: Al principio de tu archivo Java, antes de la declaración de la clase:

      ```Java
      import java.util.Scanner;
      ```

  2. **Crear un objeto `Scanner`**: Dentro de tu método main:

      ```Java
      Scanner scanner = new Scanner(System.in); // 'System.in' representa la entrada estándar (teclado)
      ```

  3. **Leer diferentes tipos de datos**:

      ```Java
      System.out.print("Ingrese su edad: ");
      int edadUsuario = scanner.nextInt(); // Lee un entero

      System.out.print("Ingrese su salario: ");
      double salarioUsuario = scanner.nextDouble(); // Lee un double

      // ¡Importante! Limpiar el buffer después de nextInt/nextDouble
      // porque nextLine() solo lee el salto de línea restante.
      scanner.nextLine(); // Consume el salto de línea pendiente

      System.out.print("Ingrese su nombre completo: ");
      String nombreUsuario = scanner.nextLine(); // Lee una línea completa de texto

      System.out.println("Hola " + nombreUsuario + ", tienes " + edadUsuario + " años y ganas " + salarioUsuario);

      scanner.close(); // Es buena práctica cerrar el Scanner cuando ya no lo necesites
      ```

- **Nota sobre `scanner.nextLine()` después de `nextInt()`/`nextDouble()`**: Cuando usas `nextInt()` o `nextDouble()`, estos métodos leen solo el número, dejando el caracter de "salto de línea" (`\n`) en el buffer de entrada. Si llamas a `nextLine()` inmediatamente después, este leerá ese `\n` vacío y no esperará la entrada real del usuario. Por eso, se suele añadir un `scanner.nextLine()`; extra para "consumir" ese salto de línea pendiente.

### 4.2. Salida de Datos por Consola (`System.out`)

Ya usamos `System.out.println()` para imprimir mensajes. Java ofrece otras formas para controlar la salida en la consola:

- `System.out.println()`: Imprime el contenido y luego inserta un salto de línea al final. El cursor se mueve a la siguiente línea.

  ```Java
  System.out.println("Línea 1");
  System.out.println("Línea 2");

  // Salida:
  // Línea 1
  // Línea 2
  ```

- `System.out.print()`: Imprime el contenido sin añadir un salto de línea al final. El cursor permanece en la misma línea.

  ```Java
  System.out.print("Parte 1");
  System.out.print(" Parte 2");
  System.out.println(" Parte 3 (con salto de línea al final)");
  System.out.print("Esto está en una nueva línea después del println.");

  // Salida:
  // Parte 1 Parte 2 Parte 3 (con salto de línea al final)
  // Esto está en una nueva línea después del println.
  ```

- `System.out.printf()` **(Salida Formateada)**: Permite imprimir texto con formato, similar a la función `printf` en C. Es muy útil para controlar el número de decimales, alinear texto, etc. Utiliza **especificadores de formato**.
  - **Especificadores comunes**:
    - `%s`: Cadena de texto (String).
    - `%d`: Número entero (Decimal integer).
    - `%f`: Número de punto flotante (Float/Double).
    - `%.2f`: Número de punto flotante con 2 decimales.
    - `%n`: Salto de línea (equivalente a `\n`).
  - **Ejemplo**:

    ```Java
    String producto = "Laptop";
    double precio = 1250.758;
    int cantidad = 2;

    System.out.printf("El producto es %s, su precio es %.2f y la cantidad es %d.%n", producto, precio, cantidad);
    System.out.printf("Precio total: %.2f%n", precio * cantidad);

    // Salida:
    // El producto es Laptop, su precio es 1250.76 y la cantidad es 2.
    // Precio total: 2501.52
    ```

  - Observa cómo `%.2f` redondea el número y `%n` agrega un salto de línea.

### 4.3. Primitivos vs. Clases Envolventes (Wrapper Classes)

Mientras que los tipos primitivos (`int`, `double`, `boolean`, etc.) almacenan directamente el valor, las **clases envolventes (Wrapper Classes)** son clases que "envuelven" a estos primitivos para darles funcionalidades de objeto. Esto es útil cuando necesitas tratar un valor primitivo como un objeto (ej. en colecciones de Java o cuando necesitas que un valor pueda ser `null`).

- Correspondencia de Wrapper Classes:
  - `byte` -> `Byte`
  - `short` -> `Short`
  - `int` -> `Integer`
  - `long` -> `Long`
  - `float` -> `Float`
  - `double` -> `Double`
  - `char` -> `Character`
  - `boolean` -> `Boolean`
- **Autoboxing y Unboxing**: Java realiza automáticamente las conversiones entre primitivos y sus wrappers cuando es necesario (ej. `Integer numero = 10;` esto es `autoboxing`).

## Resumen de la Clase

- Has aprendido a declarar y usar **variables** (incluyendo la inferencia de tipo con `var`) y **constantes** para almacenar datos en tu programa.
- Comprendes que **Java es un lenguaje fuertemente tipado** y qué implica eso para el manejo de tipos de datos.
- Conoces los **tipos de datos primitivos** más comunes (`int`, `double`, `boolean`, `char`) y la diferencia con `String` (que es un objeto).
- Sabes cómo realizar **conversiones de tipo** (implícitas y explícitas) y las precauciones del casting.
- Dominas los **operadores aritméticos** (`+`, `-`, `*`, `/`, `%`) y su precedencia.
- Has tenido una primera aproximación a la **entrada de datos por consola** usando la clase `Scanner`, fundamental para crear programas interactivos.
- Ahora conoces las diferentes opciones para la **salida de datos** (`System.out.print`, `System.out.println`, `System.out.printf`) y cómo formatear tu información.

¡Estos son los bloques fundamentales de cualquier programa! Ahora, ¡a practicar lo aprendido!

## Ejercicio: Conversor de Medidas Simple

1. **Preparación**: Dentro de tu repositorio `java-course/module-01/`, crea una nueva carpeta llamada `class-02`. Dentro de `class-02`, crea un archivo Java llamado `ConversorMedidas.java`.
2. **Requisitos del Programa**: El programa `ConversorMedidas.java` debe:
    - Utilizar la clase `Scanner` para solicitar al usuario valores numéricos para las medidas.
    - Declarar variables (puedes usar `var` donde sea apropiado) para almacenar la entrada del usuario y los resultados de las conversiones.
    - Utilizar **constantes** (`final`) para los factores de conversión. Define las siguientes:
      - `CM_POR_METRO = 100.0`
      - `LIBRAS_POR_KG = 2.20462`
      - `FACTOR_CELSIUS_FAHRENHEIT = 1.8`
      - `OFFSET_FAHRENHEIT = 32.0`
    - Realizar las siguientes conversiones y mostrar los  resultados en la consola:
      - **Metros a Centímetros**:
        - Solicita al usuario una cantidad en metros.
        - Calcula la equivalencia en centímetros.
        - Imprime el resultado usando `System.out.println()`.
      - **Kilogramos a Libras**:
        - Solicita al usuario una cantidad en kilogramos.
        - Calcula la equivalencia en libras.
        - Imprime el resultado usando `System.out.printf()` para mostrar solo 2 decimales.
      - **Grados Celsius a Grados Fahrenheit**:
        - Solicita al usuario una temperatura en grados Celsius.
        - Calcula la equivalencia en grados Fahrenheit (`F = C × 1.8 + 32`).
        - Imprime el resultado usando `System.out.print()` para que el siguiente mensaje continúe en la misma línea (si lo deseas) o `printf()` con 1 decimal.
3. **Ejemplo de Interacción en Consola (Sugerido)**:

    ```text
    --- Bienvenido al Conversor de Medidas! ---

    Ingrese la cantidad en metros: 15
    15.0 metros equivalen a 1500.0 centímetros.

    Ingrese la cantidad en kilogramos: 70
    70.00 kilogramos equivalen a 154.32 libras.

    Ingrese la temperatura en grados Celsius: 25
    25.0 grados Celsius equivalen a 77.0 grados Fahrenheit.

    --- Conversiones completadas. Gracias! ---
    ```

4. **Uso de Git**:
    - Después de completar y probar tu código, asegúrate de que estás en la carpeta raíz `java-course`.
    - Añade los nuevos archivos al área de staging: `git add .`
    - Realiza un `commit` con un mensaje descriptivo, por ejemplo: `"feat: Implementado Conversor de Medidas usando variables, constantes, Scanner y var"`
    - Sube tus cambios a tu repositorio `java-course` en GitHub: `git push origin main` (o `master`).
5. Comentarios en el Código:

    Asegúrate de que tu código esté bien comentado, explicando la declaración de variables, las constantes utilizadas y la lógica de cada sección de conversión.

### Criterios de Evaluación (para el estudiante)

Tu ejercicio será evaluado en base a los siguientes puntos:

- El programa compila y ejecuta correctamente.
- Se utilizan variables (con o sin `var` según la conveniencia) para almacenar los datos de entrada y salida.
- Se utilizan constantes (`final`) para los factores de conversión (incluyendo los valores específicos).
`- Se aplican correctamente los operadores aritméticos para las conversiones.
- Se utiliza `Scanner` para la entrada de datos del usuario.
- La salida en consola es clara, muestra los resultados de las conversiones, y utiliza al menos una vez `System.out.printf()` para formatear la salida de decimales.
- El código está versionado en GitHub con un `commit` relevante.
- El código está comentado adecuadamente.

### Retos Adicionales (Opcional)

- Añade una nueva conversión a la aplicación, como Litros a Galones o Kilómetros a Millas.
- Experimenta con más especificadores de formato en `System.out.printf()` (ej. ancho de campo, alineación).
- Implementa un pequeño menú para que el usuario elija qué conversión desea realizar (esto puede requerir estructuras de control como `if/else` o `switch`, que se verán en la próxima clase, por lo que es un reto muy avanzado para esta etapa).
