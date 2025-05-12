# Clase 3: Estructuras de Control de Flujo (Condicionales y Bucles)

¡Hola! En las clases anteriores, aprendiste a almacenar datos en variables y a realizar cálculos básicos. Ahora, daremos un paso gigante: aprenderás a hacer que tus programas tomen **decisiones** y **repitan acciones**. Esto se logra con las **estructuras de control de flujo**.

Imagina que tu programa es como un camino. Normalmente, el camino va en línea recta (ejecución secuencial). Las estructuras de control de flujo son como **desvíos** (condicionales) o **círculos** (bucles) en ese camino, que te permiten cambiar la dirección o recorrer un tramo varias veces.

## Objetivos de Aprendizaje de la Clase

Al finalizar esta clase, serás capaz de:

- Controlar la ejecución de tu programa usando **condicionales** (`if`, `else if`, `else`, `switch`).
- Repetir bloques de código utilizando **bucles** (`while`, `do-while`, `for`).
- Modificar el comportamiento de los bucles con `break` y `continue`.
- Combinar condicionales y bucles para crear **lógica de flujo** más compleja, como menús interactivos.

## 1. Tomando Decisiones: Estructuras Condicionales

Las estructuras condicionales te permiten ejecutar un bloque de código _solo si_ una condición específica es verdadera (`true`).

### 1.1. Expresiones Booleanas: La Base de las Decisiones

Una condición es una expresión que se evalúa a un valor booleano: `true` o `false`. Para construir condiciones, usamos:

- **Operadores de Comparación**: Comparan dos valores y devuelven `true` o `false`.

  | Operador | Descripción | Ejemplo | Resultado |
  | :-: | --- | :-: | :-: |
  | `==` | Igual a | `5 == 5` | `true` |
  | `!=` | Diferente de | `5 != 3` | `true` |
  | `>` | Mayor que | `7 > 3` | `true` |
  | `<` | Menor que | `2 < 5` | `true` |
  | `>=` | Mayor o igual que | `5 >= 5` | `true` |
  | `<=` | Menor o igual que | `3 <= 4` | `true` |

  ```Java
  int a = 10;
  int b = 5;
  boolean esIgual = (a == b); // false
  boolean esMayor = (a > b);  // true
  ```

- **Operadores Lógicos**: Combinan o modifican expresiones booleanas. Son esenciales para crear condiciones más complejas.

  - `&&` **(AND Lógico)**: El resultado es `true` si ambas condiciones son `true`. Si alguna es `false`, el resultado es `false`.

    | `Condición 1` | `Condición 2` | `Condición 1 && Condición 2` |
    | :-: | :-: | :-: |
    | true | true | true |
    | true | false | false |
    | false | true | false |
    | false | false | false |

    ```Java
    int edad = 20;
    double salario = 1500.0;

    // ¿Es mayor de 18 Y gana más de 1000?
    boolean cumpleRequisitos = (edad >= 18) && (salario > 1000); // true && true -> true
    ```

  - `||` **(OR Lógico)**: El resultado es `true` si al menos una de las condiciones es `true`. Solo es `false` si ambas condiciones son `false`.

    | `Condición 1` | `Condición 2` | `Condición 1 \|\| Condición 2` |
    | :-: | :-: | :-: |
    | true | true | true |
    | true | false | true |
    | false | true | true |
    | false | false | false |

    ```Java
    boolean tieneDescuento = false;
    boolean esClienteVIP = true;

    // ¿Tiene descuento O es cliente VIP?
    boolean aplicaBeneficio = tieneDescuento || esClienteVIP; // false || true -> true
    ```

  - `!` **(NOT Lógico)**: Invierte el valor booleano de una condición. Si la condición es `true`, `!condicion` es `false`, y viceversa.

    | `Condición` | `! Condición` |
    | :-: | :-: |
    | true | false |
    | false | true |

    ```Java
    boolean estaActivo = true;
    boolean estaInactivo = !estaActivo; // !(true) -> false
    ```

Combinando operadores de comparación y lógicos, puedes crear condiciones muy específicas:

```Java
int temperatura = 25;
boolean estaLloviendo = false;

// ¿La temperatura está entre 20 y 30 grados (inclusive) Y NO está lloviendo?
boolean climaAgradable = (temperatura >= 20 && temperatura <= 30) && !estaLloviendo; // (true && true) && !(false) -> true && true -> true
```

### 1.2. La sentencia `if`

Ejecuta un bloque de código si la condición dentro de los paréntesis es `true`.

```Java
int numero = 10;

if (numero > 0) {
    // Este código se ejecuta SOLO si 'numero' es mayor que 0
    System.out.println("El número es positivo.");
}
// Si numero fuera -5, la línea anterior no se ejecutaría, y el programa continuaría aquí.
```

Si solo hay una línea de código dentro del `if`, las llaves `{ }` son opcionales, pero es una buena práctica usarlas siempre para evitar errores y mejorar la legibilidad.

### 1.3. La sentencia `if-else`

Permite ejecutar un bloque de código si la condición es `true`, y un bloque alternativo si la condición es `false`.

```Java
int numero = 7;

if (numero % 2 == 0) { // Si el residuo de la división por 2 es 0, es par
    System.out.println("El número es par.");
} else { // Si la condición del if es false...
    System.out.println("El número es impar.");
}
```

### 1.4. El Operador Ternario (`? :`)

Es una forma concisa de escribir un `if-else` simple que **devuelve un valor**. Su sintaxis es: `condicion ? valor_si_true : valor_si_false;`. Es útil para asignaciones simples basadas en una condición.

```Java
int edad = 20;
String mensaje;

// Usando if-else:
if (edad >= 18) {
    mensaje = "Mayor de edad";
} else {
    mensaje = "Menor de edad";
}
System.out.println(mensaje); // Salida: Mayor de edad

// Usando el operador ternario (equivalente al if-else anterior):
String mensajeTernario = (edad >= 18) ? "Mayor de edad" : "Menor de edad";
System.out.println(mensajeTernario); // Salida: Mayor de edad

// Otro ejemplo:
int num = 10;
String tipoNumero = (num % 2 == 0) ? "Par" : "Impar";
System.out.println("El número es: " + tipoNumero); // Salida: El número es: Par
```

Úsalo solo para lógicas simples que devuelven un valor; para lógicas más complejas o que no devuelven un valor, `if-else` es más legible.

### 1.5. La sentencia `if-else if-else`

Es útil cuando necesitas evaluar `múltiples posibles condiciones` en secuencia. Java evalúa las condiciones de arriba hacia abajo. Ejecuta el primer bloque cuyo `if` o `else if` sea `true` y luego salta al final de toda la estructura `if-else if-else`. Si ninguna condición es `true`, ejecuta el bloque `else` final (si existe).

```Java
int calificacion = 85;

if (calificacion >= 90) {
    System.out.println("Excelente");
} else if (calificacion >= 80) { // Se evalúa SOLO si la primera condición (calificacion >= 90) fue false
    System.out.println("Muy Bien");
} else if (calificacion >= 70) { // Se evalúa SOLO si las condiciones anteriores fueron false
    System.out.println("Bien");
} else { // Se ejecuta si ninguna de las condiciones anteriores fue true
    System.out.println("Necesita mejorar");
}
```

### 1.6. La sentencia `switch-case`

Es una alternativa más limpia y legible al `if-else if` cuando necesitas seleccionar entre varias opciones basadas en el valor exacto de una variable. Funciona con tipos de datos como `int`, `byte`, `short`, `char`, `String`, y `enum` (veremos `enum` más adelante).

```Java
int diaSemana = 3; // 1=Lunes, 2=Martes, etc.
String nombreDia;

switch (diaSemana) { // La variable a evaluar
    case 1: // Si diaSemana es 1...
        nombreDia = "Lunes";
        break; // Importante: sale del switch después de ejecutar el código del case
    case 2: // Si diaSemana es 2...
        nombreDia = "Martes";
        break;
    case 3: // Si diaSemana es 3...
        nombreDia = "Miércoles";
        break;
    case 4:
        nombreDia = "Jueves";
        break;
    case 5:
        nombreDia = "Viernes";
        break;
    case 6: // Múltiples casos pueden compartir el mismo bloque de código
    case 7:
        nombreDia = "Fin de semana";
        break;
    default: // Opcional: se ejecuta si el valor de diaSemana no coincide con ningún 'case'
        nombreDia = "Día inválido";
        break; // Es buena práctica poner break incluso en el default
}
System.out.println("El día es: " + nombreDia); // Salida: El día es: Miércoles
```

**¡Cuidado con el "fall-through"!** Si omites la palabra clave `break` al final de un `case`, Java continuará ejecutando el código del siguiente `case` (y los subsiguientes) hasta que encuentre un `break` o llegue al final del `switch`. Esto rara vez es deseado y es una fuente común de errores.

### 1.7. La expresión `switch` (Java 12+)

A partir de Java 12 (estándar en Java 14), puedes usar el `switch` como una expresión que **devuelve un valor**. Esto elimina la necesidad de `break` para evitar el "fall-through" y hace el código más conciso. Usa la flecha `->` en lugar de dos puntos `:` y no requiere `break`.

```Java
int diaSemana = 3; // 1=Lunes, 2=Martes, etc.

// Usando switch expression para asignar un valor a una variable
String nombreDiaModerno = switch (diaSemana) {
    case 1 -> "Lunes";
    case 2 -> "Martes";
    case 3 -> "Miércoles";
    case 4 -> "Jueves";
    case 5 -> "Viernes";
    case 6, 7 -> "Fin de semana"; // Múltiples casos separados por coma
    default -> "Día inválido";
}; // Nota el punto y coma al final de la expresión switch

System.out.println("El día (moderno) es: " + nombreDiaModerno); // Salida: El día (moderno) es: Miércoles
```

```Java
// También puedes usar bloques con 'yield' si necesitas más de una línea de código en un case
int mes = 4;
int diasEnMes = switch (mes) {
    case 1, 3, 5, 7, 8, 10, 12 -> 31;
    case 4, 6, 9, 11 -> 30;
    case 2 -> {
        // Lógica más compleja si es necesario
        System.out.println("Considerando Febrero...");
        yield 28; // 'yield' devuelve el valor para este case
    }
    default -> {
        System.out.println("Mes inválido.");
        yield -1; // Devuelve -1 para un mes inválido
    }
};
System.out.println("Días en el mes " + mes + ": " + diasEnMes);
```

El `switch expression` es más seguro (no hay "fall-through" accidental) y más legible para asignaciones de valor basadas en múltiples casos.

## 2. Repitiendo Acciones: Estructuras de Bucle

Los bucles te permiten ejecutar un bloque de código varias veces sin tener que escribirlo repetidamente.

### 2.1. El ciclo `while`

Repite un bloque de código mientras una condición sea `true`. La condición se verifica antes de cada posible iteración. Si la condición es `false` desde el principio, el código dentro del bucle nunca se ejecuta.

```Java
int contador = 0; // Inicialización de una variable de control del bucle

while (contador < 5) { // Condición: el bucle se repite mientras 'contador' sea menor que 5
    System.out.println("Iteración: " + contador);
    contador++; // Actualización: incrementa 'contador' en 1. ¡Esto es CRUCIAL!
}
// Salida:
// Iteración: 0
// Iteración: 1
// Iteración: 2
// Iteración: 3
// Iteración: 4
// Cuando contador llega a 5, la condición (5 < 5) es false, y el bucle termina.
```

**¡Advertencia!** Si la condición del `while` nunca se vuelve `false`, tendrás un bucle infinito, y tu programa se quedará "colgado". Siempre asegúrate de que algo dentro del bucle modifique las variables involucradas en la condición para que eventualmente se vuelva `false`.

### 2.2. El ciclo `do-while`

Similar a `while`, pero la condición se verifica **después** de cada iteración. Esto significa que el bloque de código dentro del `do-while` se ejecutará al menos una vez, incluso si la condición es `false` desde el principio.

```Java
int numero;
Scanner scanner = new Scanner(System.in);

do {
    System.out.print("Ingrese un número positivo: ");
    numero = scanner.nextInt();
} while (numero <= 0); // La condición se evalúa después de la primera ejecución y en cada iteración subsiguiente

System.out.println("Número ingresado: " + numero);
scanner.close();
// Este bucle siempre pedirá un número al menos una vez. Si el primer número es positivo, la condición es false y el bucle termina. Si no, seguirá pidiendo hasta que se ingrese uno positivo.
```

Nota el punto y coma `;` al final de la línea `while (condicion);` en el `do-while`.

### 2.3. El ciclo `for`

Es la estructura de bucle más común cuando sabes de antemano cuántas veces quieres que se repita un bloque de código, o cuando necesitas iterar sobre un rango de valores numéricos. Su sintaxis es muy compacta, agrupando la inicialización, la condición y la actualización en una sola línea.

```Java
// Sintaxis: for (inicialización; condición; actualización) { // código }

for (int i = 1; i <= 10; i++) { // i empieza en 1; el bucle continúa mientras i <= 10; i se incrementa en 1 después de cada iteración
    System.out.println("Contando: " + i);
}
// Salida:
// Contando: 1
// Contando: 2
// ...
// Contando: 10
```

- **Inicialización**: Se ejecuta una sola vez al inicio del bucle. Aquí se suelen declarar e inicializar variables de control (ej. `int i = 1;`).
- **Condición**: Se evalúa antes de cada iteración. Si es `true`, el bucle continúa; si es `false`, el bucle termina.
- **Actualización**: Se ejecuta después de cada iteración (normalmente para modificar la variable de control, ej. `i++`).

Puedes omitir cualquiera de las tres partes del for (aunque la condición es necesaria para evitar un bucle infinito a menos que uses `break`), pero los puntos y comas `;` son obligatorios.

```Java
// Ejemplo de for con decremento
for (int j = 5; j > 0; j--) { // j empieza en 5; se repite mientras j > 0; j se decrementa en 1
    System.out.println("Cuenta regresiva: " + j);
}
// Salida: Cuenta regresiva: 5 ... Cuenta regresiva: 1
```

## 3. Controlando el Flujo Dentro de los Bucles: `break` y `continue`

Estas palabras clave te dan un control más fino sobre cómo se ejecutan los bucles.

### 3.1. `break`

Sale _inmediatamente_ del bucle más interno en el que se encuentra. La ejecución del programa continúa en la primera línea de código que sigue al bucle.

```Java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        System.out.println("Encontrado el número 5. Saliendo del bucle.");
        break; // Sale del bucle for
    }
    System.out.println("Procesando: " + i);
}
System.out.println("Después del bucle.");
// Salida:
// Procesando: 1
// Procesando: 2
// Procesando: 3
// Procesando: 4
// Encontrado el número 5. Saliendo del bucle.
// Después del bucle.
```

### 3.2. `continue`

Salta el resto del código dentro del bucle para la _iteración actual_ y pasa directamente a la _siguiente iteración_. En un bucle `for`, salta a la sección de actualización; en un `while` o `do-while`, salta a la evaluación de la condición.

```Java
for (int i = 1; i <= 10; i++) {
    if (i % 2 != 0) { // Si i es impar...
        System.out.println("Saltando número impar: " + i);
        continue; // Salta el resto del código en esta iteración (el println de abajo)
    }
    // Este código solo se ejecuta si i es par
    System.out.println("Procesando número par: " + i);
}
// Salida:
// Saltando número impar: 1
// Procesando número par: 2
// Saltando número impar: 3
// Procesando número par: 4
// ... y así sucesivamente.
```

## 4. Construyendo Lógica con Condicionales y Bucles

La verdadera potencia de las estructuras de control de flujo se ve cuando las combinas. Puedes crear lógica compleja y programas interactivos.

- **Menús Interactivos**: Un bucle `while` o `do-while` puede mantener el programa ejecutándose y mostrando opciones al usuario repetidamente. Dentro de ese bucle, una estructura condicional (`switch` o `if-else if`) puede manejar la acción a realizar según la elección del usuario. La opción "Salir" en el menú puede usar `break` o cambiar una variable que controle la condición del bucle principal.
- **Validación de Entrada**: Un bucle `while` o `do-while` es perfecto para seguir pidiendo al usuario que ingrese datos hasta que cumplan con ciertos criterios (ej. un número positivo, una opción válida del menú).

Estas combinaciones son fundamentales para crear programas que respondan de manera inteligente a la interacción del usuario.

## Resumen de la Clase

- Has aprendido a usar condicionales (`if`, `else if`, `else`, `switch`) para que tu programa tome decisiones basadas en condiciones booleanas, incluyendo el uso de operadores lógicos (`&&`, `||`, `!`) y sus tablas de verdad.
- Exploraste los bucles (`while`, `do-while`, `for`) para repetir bloques de código.
- Viste cómo `break` y `continue` te dan un control más fino sobre la ejecución de los bucles.
- Comprendes cómo combinar estas estructuras para construir flujos de lógica más complejos, lo cual aplicarás en el proyecto de clase.

¡Estás listo para aplicar estos conceptos en un proyecto práctico!

## Ejercicio: Aplicación de Utilidades en Consola

Crear un programa en Java que presente un menú al usuario y realice diferentes acciones según su elección, utilizando condicionales y bucles.

### Requisitos del Programa

1. El programa debe mostrar un menú de opciones al usuario en la consola. El menú debe incluir al menos las siguientes opciones:
    - Calcular Área de una Figura (sub-opción: Círculo o Rectángulo).
    - Calcular Edad Futura.
    - Verificar si un Número es Primo.
    - Salir del programa.
2. El programa debe usar un bucle (`while` o `do-while`) para que el menú se muestre repetidamente después de cada operación, hasta que el usuario elija la opción "Salir".
3. Se debe usar una estructura condicional (`switch` es ideal aquí, o `if-else if`) para manejar la opción que el usuario ingresa desde el menú.
4. Para cada opción del menú (excepto "Salir"):
    - Se debe solicitar al usuario la entrada de datos necesaria utilizando `Scanner`.
    - Se deben realizar los cálculos o la lógica requerida utilizando variables, constantes (si aplican, como PI), y operadores.
    - Se deben mostrar los resultados en la consola utilizando `System.out.println()` o `System.out.printf()` (para formato de decimales).
5. Para la opción "Calcular Área de una Figura", dentro de esa sección, se debe presentar un sub-menú o solicitar al usuario si desea calcular el área de un círculo o un rectángulo, utilizando otra estructura condicional.
6. Para la opción "Verificar si un Número es Primo", se debe implementar la lógica para determinar si un número ingresado por el usuario es primo (solo divisible por 1 y por sí mismo, mayor que 1). Esto requerirá el uso de un bucle para verificar divisores.
7. La opción "Salir" debe terminar la ejecución del bucle principal (usando break o modificando la condición del bucle).
8. El código debe estar organizado, ser legible y estar comentado.
