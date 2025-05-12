# Preguntas de Entrevista Técnica: Módulo 1 - Fundamentos Modernos de Programación con Java

Este documento contiene preguntas comunes que podrías encontrar en una entrevista técnica para un puesto junior o de entrada, basadas en los conceptos fundamentales que aprendiste en el Módulo 1. Intenta responderlas por tu cuenta antes de ver las respuestas.

## Sección 1: Fundamentos y Entorno

### 1. ¿Cuál es la diferencia principal entre JDK, JRE y JVM en el ecosistema de Java?

- **JDK (Java Development Kit)**: Es el kit completo para desarrollar aplicaciones Java. Incluye el JRE y herramientas de desarrollo como el compilador (`javac`), el depurador (`jdb`), etc.
- **JRE (Java Runtime Environment)**: Es el entorno necesario para ejecutar aplicaciones Java. Incluye la JVM y las bibliotecas estándar de Java. Si solo quieres ejecutar programas Java, necesitas el JRE.
- **JVM (Java Virtual Machine)**: Es una máquina virtual que ejecuta el bytecode de Java. Permite que los programas Java sean "Write Once, Run Anywhere" (Escribe una vez, ejecuta en cualquier lugar), ya que la JVM interpreta el bytecode para el sistema operativo específico.

### 2 Explica el proceso básico de cómo un archivo `.java` se convierte en un programa ejecutable

Un archivo `.java` (código fuente) es compilado por el compilador de Java (`javac`), que forma parte del JDK, en un archivo `.class` (bytecode). Este bytecode es luego interpretado y ejecutado por la JVM.

### 3. ¿Qué es Git y por qué es importante en el desarrollo de software?

Git es un sistema de control de versiones distribuido. Es importante porque permite rastrear y gestionar cambios en el código a lo largo del tiempo, colaborar eficientemente con otros desarrolladores, revertir a versiones anteriores, y gestionar diferentes líneas de desarrollo (`branches`).

## Sección 2: Variables, Tipos de Datos y Operadores

### 4. ¿Cuál es la diferencia entre un tipo de dato primitivo y un objeto (`String`, por ejemplo) en Java?

Los tipos de datos primitivos (como `int`, `double`, `boolean`, `char`) almacenan el valor directamente en la memoria. Los objetos (instancias de clases, como `String`) almacenan una referencia a la ubicación en memoria donde se encuentra el objeto real. Los primitivos tienen un tamaño fijo, mientras que el tamaño de los objetos puede variar.

### 5. Menciona al menos 4 tipos de datos primitivos en Java y describe brevemente para qué se usan

- `int`: Para números enteros (sin decimales).
- `double`: Para números de punto flotante (con decimales), de doble precisión.
- `boolean`: Para valores lógicos, solo puede ser `true` o `false`.
- `char`: Para un solo carácter (letra, número, símbolo), entre comillas simples.
- Otros posibles: `byte`, `short`, `long`, `float`

### 6. ¿Qué es la palabra clave `final` cuando se aplica a una variable?

La palabra clave `final`, cuando se aplica a una variable, la convierte en una constante. Significa que una vez que la variable ha sido inicializada con un valor, ese valor no puede ser cambiado durante la ejecución del programa.

### 7. Explica la diferencia entre conversión de tipo implícita (widening) y explícita (casting) en Java. Da un ejemplo de cuándo podrías necesitar un casting explícito

- **Implícita (widening)**: Ocurre automáticamente cuando conviertes un tipo de menor capacidad a uno de mayor capacidad (ej. `int` a `double`). No hay pérdida de datos.
- **Explícita (Casting)**: Debes indicarla explícitamente usando `(tipoDato)` cuando conviertes un tipo de mayor capacidad a uno de menor capacidad (ej. `double` a `int`). Puede haber pérdida de datos (truncamiento o desbordamiento).
- **Ejemplo de Casting**: Necesitarías casting explícito si lees un valor decimal (`double`) del usuario y quieres almacenarlo en una variable entera (`int`), sabiendo que se perderá la parte decimal: `int numeroEntero = (int) scanner.nextDouble();`.

### 8. ¿Qué hace el operador `%` (módulo) en Java?

El operador `%` devuelve el residuo de una división. Por ejemplo, `10 % 3` es `1`, porque 10 dividido por 3 es 3 con un residuo de 1. Es útil para verificar si un número es par (`numero % 2 == 0`).

### 9. ¿Cuándo usarías `System.out.printf()` en lugar de `System.out.println()`?

Usarías `System.out.printf()` cuando necesitas mostrar salida formateada, es decir, controlar cómo se presentan los datos. Es especialmente útil para especificar el número de decimales para números de punto flotante (ej. `%.2f`), alinear texto, o insertar variables en una cadena de formato. `System.out.println()` simplemente imprime el valor y añade un salto de línea.

### 10. Explica la inferencia de tipo con `var` (a partir de Java 10). ¿Cuándo es apropiado usarla y cuáles son sus limitaciones?

`var` permite declarar variables locales sin especificar explícitamente su tipo, ya que el compilador lo infiere a partir del valor con el que se inicializa.

- **Apropiado**: Cuando el tipo es obvio a partir del inicializador y usar `var` mejora la legibilidad y concisión.
- **Limitaciones**: Solo para variables locales, debe ser inicializada en la declaración, no puede ser `null`, y no es tipado dinámico (el tipo inferido es fijo).

## Sección 3: Estructuras de Control de Flujo

### 11 ¿Cuál es la diferencia entre los operadores lógicos `&&` y `||`?

- `&&` (AND): La expresión completa es `true` solo si ambas condiciones son `true`.
- `||` (OR): La expresión completa es `true` si al menos una de las condiciones es `true`.

### 12. Explica la diferencia entre un bucle `while` y un bucle `do-while`. ¿Cuándo usarías cada uno?

- `while`: La condición se evalúa antes de cada iteración. El cuerpo del bucle puede no ejecutarse nunca si la condición es `false` desde el principio.
- `do-while`: El cuerpo del bucle se ejecuta al menos una vez, y luego la condición se evalúa después de cada iteración.
- **Uso**: Usa `while` cuando la ejecución del bucle depende de una condición que podría ser falsa inicialmente. Usa `do-while` cuando necesitas garantizar que el código dentro del bucle se ejecute al menos una vez (ej. para pedir entrada al usuario hasta que sea válida).

### 13. ¿Cuándo es más apropiado usar un bucle `for` en lugar de un `while`?

Un bucle `for` es generalmente más apropiado cuando sabes de antemano cuántas veces necesitas repetir el código (un número fijo de iteraciones) o cuando necesitas iterar sobre un rango numérico donde la inicialización, condición y actualización están claramente definidas. Un `while` es mejor cuando el número de iteraciones es desconocido y depende de que una condición se vuelva falsa.

### 14. Explica la función de las palabras clave `break` y `continue` dentro de un bucle

- `break`: Termina la ejecución del bucle más interno inmediatamente y salta a la primera línea de código después del bucle.
- `continue`: Salta el resto del código en la iteración actual del bucle y pasa a la siguiente iteración (evaluando la condición del bucle nuevamente).

### 15. ¿Cuál es la diferencia entre un `switch` statement tradicional y un `switch` expression?

- `switch` **Statement**: Ejecuta un bloque de código basado en el valor de una expresión. Requiere `break` para evitar el "fall-through" accidental entre `case`s.
- `switch` **Expression**: Es una forma más concisa de switch que devuelve un valor. Utiliza la flecha `->` en lugar de `:` y no requiere `break` (cada rama case solo ejecuta su código asociado y devuelve un valor). Es más seguro y a menudo más legible para asignaciones basadas en múltiples casos.

### 16. ¿Qué es el operador ternario (`? :`) y cuándo lo usarías?

El operador ternario es un operador condicional que toma tres operandos: `condicion ? valor_si_true : valor_si_false;`. Es una forma abreviada de un `if-else` simple que devuelve un valor. Lo usarías para asignaciones simples donde necesitas elegir entre dos valores posibles basándose en una condición booleana.

## Sección 4: Funciones y Modularidad

### 17. ¿Qué es un método (función) en Java y cuáles son los beneficios de usar métodos?

Un método es un bloque de código con nombre que realiza una tarea específica. Los beneficios incluyen la reutilización de código (evitar repetir código), mejorar la organización y legibilidad del programa, y facilitar el mantenimiento y la depuración.

### 18. ¿Cuál es la diferencia entre un método declarado con `void` y un método con un tipo de retorno específico (ej. `int`, `double`, `String`)?

Un método declarado con `void` no devuelve ningún valor después de su ejecución. Un método con un tipo de retorno específico debe usar la palabra clave `return` para enviar un valor de ese tipo de vuelta al lugar donde fue llamado.

### 19. Explica el concepto de parámetros y argumentos en el contexto de los métodos

Los **parámetros** son las variables declaradas en la firma (definición) de un método que actúan como "marcadores de posición" para los datos que el método espera recibir. Los **argumentos** son los valores reales que se pasan a un método cuando se le llama (se invocan). Los argumentos se asignan a los parámetros correspondientes.

### 20. Describe el concepto de "paso por valor" en Java para tipos primitivos. ¿Qué significa para una variable original cuando se pasa a un método?

Para tipos primitivos, Java utiliza paso por valor. Esto significa que cuando pasas una variable primitiva a un método como argumento, se crea una copia del valor de esa variable y esa copia es la que se asigna al parámetro del método. Cualquier modificación que se haga al parámetro dentro del método **no afectará** a la variable original fuera del método.

### 21. ¿Qué es el "alcance" (scope) de una variable en Java? Explica la diferencia entre alcance local y alcance de clase (estático, introducido en M1)

El alcance de una variable define la región del código donde esa variable es accesible y utilizable.

- **Alcance Local**: Las variables declaradas dentro de un método o un bloque de código (`if`, `for`, etc.) tienen alcance local. Solo son accesibles desde el punto de su declaración hasta el final del bloque donde están definidas.
- **Alcance de Clase (Estático)**: Las variables declaradas con la palabra clave `static` directamente dentro de la clase (fuera de cualquier método) tienen alcance de clase. Pertenecen a la clase misma y son compartidas por todos los métodos `static` de esa clase. Son accesibles desde cualquier método `static` de la clase.

### 22. ¿Qué significa "modularidad" en programación y cómo te ayudan los métodos a lograrla?

Modularidad es la práctica de dividir un programa grande y complejo en partes más pequeñas, independientes y manejables, llamadas módulos. Los métodos son la herramienta principal en Java para lograr modularidad, ya que permiten encapsular tareas específicas en unidades con nombre, lo que facilita la organización, comprensión, desarrollo y mantenimiento del código.

---

**Consejo Final para la Entrevista**: Además de saber las respuestas, practica explicar los conceptos con tus propias palabras y da ejemplos de código simple para ilustrar tus puntos.

**¡Mucha suerte!**
