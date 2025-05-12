# Ejercicio Autónomo: Analizador de Números y Secuencias

Este ejercicio te desafiará a aplicar las estructuras condicionales (`if`, `else if`, `else`, `switch`, operador ternario, `switch expression`) y los bucles (`while`, `do-while`, `for`) para analizar números y generar secuencias según la entrada del usuario.

## Objetivos del Ejercicio

- Utilizar correctamente las diferentes estructuras condicionales para tomar decisiones basadas en múltiples criterios.
- Implementar bucles para realizar acciones repetitivas o iterar sobre rangos numéricos.
- Combinar condicionales y bucles para crear lógica de programa más compleja.
- Practicar la lectura de entrada del usuario (`Scanner`) y la presentación de salida formateada (`System.out.printf()`).

## Instrucciones Detalladas

### 1. Crear el Proyecto/Archivo

Crea un nuevo archivo Java llamado `AnalizadorNumeros.java`. Colócalo en la ubicación adecuada dentro de tu estructura de carpetas para el Módulo 1, Clase 3 (ej. `java-course/module-01/class-03/`).

### 2. Estructura Básica y `Scanner`

- Define la estructura básica de la clase `AnalizadorNumeros` con el método `main`.
- Importa y crea un objeto `Scanner` para leer la entrada del usuario.

```Java
import java.util.Scanner;

public class AnalizadorNumeros {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // ... tu código aquí ...

        scanner.close(); // No olvides cerrar el scanner al final
    }
}
```

### 3. Menú Principal con Bucle

- Implementa un bucle (`while` o `do-while`) para mostrar un menú de opciones al usuario repetidamente hasta que elija salir.
- El menú debe tener al menos las siguientes opciones:
  - 1 - Analizar un solo número.
  - 2 - Generar secuencia de números.
  - 0 - Salir.
- Lee la opción del usuario dentro del bucle.

### 4. Manejo de Opciones con Condicionales (`switch` o `if-else if`)

Utiliza una estructura condicional (`switch` es muy adecuado aquí) para ejecutar la lógica correspondiente a la opción elegida por el usuario.

### 5. Opción 1: Analizar un Solo Número

Si el usuario elige la opción 1:

- Solicita al usuario que ingrese un número entero.
- Utiliza estructuras condicionales (`if`, `else if`, `else`) para determinar y mostrar en la consola si el número es:
  - Positivo, Negativo o Cero.
  - Par o Impar (usa el operador módulo %).
  - Mayor que 100, Menor que 100, o Igual a 100.
- Puedes usar el **operador ternario** para alguna de estas verificaciones simples (ej. para determinar si es par o impar y almacenarlo en un `String`).

### 6. Opción 2: Generar Secuencia de Números

Si el usuario elige la opción 2:

- Solicita al usuario que ingrese un número inicial y un número final.
- Solicita al usuario si desea mostrar solo números pares o solo números impares en ese rango (lee esta preferencia como un `String`, ej. "par" o "impar").
- Utiliza un **bucle** (`for` es ideal aquí) para iterar desde el número inicial hasta el número final.
- Dentro del bucle, utiliza una estructura **condicional** (`if`) y el operador módulo (`%`) para verificar si el número actual es par o impar.
- Utiliza la palabra clave `continue` para saltar la impresión de números que no coincidan con la preferencia del usuario (si eligió "par", salta los impares; si eligió "impar", salta los pares).
- Imprime solo los números que cumplen la condición.

### 7. Opción 0: Salir

Si el usuario elige la opción 0, el bucle principal debe terminar. Puedes lograr esto usando `break` dentro del `case 0` del `switch`, o modificando la condición del bucle principal.

### 8. Validación Básica (Opcional, pero recomendada)

Intenta añadir alguna validación básica usando un bucle `while` o `do-while` para asegurar que el usuario ingrese un número válido cuando se le solicita (ej. que ingrese un número entero real, no texto). Esto puede ser un poco más avanzado, pero es una excelente práctica.

### 9. Comentarios y Legibilidad

Asegúrate de que tu código esté bien organizado, indentado correctamente y con comentarios que expliquen la lógica de cada sección (el menú, cada opción del `switch`, los bucles, las condiciones).

## Ejemplo de Interacción (Parcial)

```text
--- Analizador de Números y Secuencias ---

1. Analizar un solo número.
2. Generar secuencia de números.
0. Salir.
Ingrese su opción: 1
Ingrese un número entero: -5
El número -5 es Negativo.
El número -5 es Impar.
El número -5 es Menor que 100.

--- Analizador de Números y Secuencias ---

1. Analizar un solo número.
2. Generar secuencia de números.
0. Salir.
Ingrese su opción: 2
Ingrese el número inicial: 10
Ingrese el número final: 20
¿Mostrar solo 'par' o 'impar'?: par
Secuencia de números pares entre 10 y 20:
10 12 14 16 18 20

--- Analizador de Números y Secuencias ---

1. Analizar un solo número.
2. Generar secuencia de números.
0. Salir.
Ingrese su opción: 0
Saliendo del programa. ¡Adiós!
```

## Criterios de Evaluación (Autónoma)

Al completar este ejercicio, revisa tu propio código y ejecución considerando:

- ¿El menú principal se muestra repetidamente hasta que elijo "Salir"? (Uso correcto de bucle).
- ¿La opción del menú me lleva a la lógica correcta? (Uso correcto de `switch` o `if-else if`).
- En la opción 1, ¿se verifican correctamente las propiedades del número (positivo/negativo/cero, par/impar, comparación con 100)? (Uso correcto de `if/else if/else` y/o operador ternario).
- En la opción 2, ¿el bucle `for` itera correctamente entre los números ingresados?
- En la opción 2, ¿se muestran solo los números pares o impares según mi elección? (Uso correcto de `if` y `continue`).
- ¿El código es legible y está bien comentado?
- ¿Cerré el objeto `Scanner`?

Este ejercicio te dará una práctica sólida en el uso combinado de condicionales y bucles, que son la base para crear programas más dinámicos e interactivos. ¡Mucho éxito!
