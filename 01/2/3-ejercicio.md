# Ejercicio Autónomo: Calculadora de Compra Simple

Este ejercicio te permitirá aplicar los conceptos de variables, tipos de datos, constantes, operadores y la interacción básica por consola (`Scanner` y `System.out`) para simular el cálculo de una compra simple.

## Objetivos del Ejercicio

- Declarar y utilizar variables (incluyendo `var`) para almacenar diferentes tipos de datos numéricos y de texto.
 Definir y usar constantes (`final`) para valores fijos (como una tasa de impuesto).
- Realizar cálculos utilizando operadores aritméticos (`*`, `+`, `-`).
- Obtener entrada de datos del usuario utilizando la clase `Scanner`.
- Mostrar resultados formateados en la consola utilizando `System.out.printf()`.

## Instrucciones detalladas

### 1. Crear el Proyecto/Archivo

Crea un nuevo archivo Java y nómbralo `CalculadoraCompra.java`. Colócalo en la ubicación adecuada dentro de tu estructura de carpetas para el Módulo 1, Clase 2 (ej. `java-course/module-01/class-02/`).

### 2. Estructura Básica del Programa

Dentro de `CalculadoraCompra.java`, define la estructura básica de una clase Java con el método `main`. Recuerda importar la clase `Scanner`.

```Java
import java.util.Scanner; // Necesario para leer la entrada del usuario

public class CalculadoraCompra {

    public static void main(String[] args) {
        // Tu código irá aquí
    }
}
```

### 3. Definir Constante de Impuesto

Declara una constante `final` de tipo `double` para representar la tasa de impuesto (por ejemplo, 15% se representaría como `0.15`). Utiliza un nombre descriptivo en `UPPER_SNAKE_CASE`.

```Java
// Dentro del método main
final double TASA_IMPUESTO = 0.15; // 15% de impuesto
```

### 4. Obtener Entrada del Usuario

- Crea un objeto Scanner para leer la entrada del usuario.
- Muestra mensajes en la consola solicitando al usuario:
  - El precio unitario del artículo (puede ser decimal).
  - La cantidad de artículos comprados (debe ser un número entero).
  - La cantidad de dinero con la que el cliente paga (puede ser decimal).
- Lee estos valores utilizando los métodos apropiados de `Scanner` (`nextDouble()`, `nextInt()`). Puedes usar `var` para declarar las variables que almacenarán estos valores.

```Java
// Dentro del método main, después de la constante
Scanner scanner = new Scanner(System.in);

System.out.println("--- Calculadora de Compra ---");

System.out.print("Ingrese el precio unitario del artículo: ");
var precioUnitario = scanner.nextDouble(); // Infiere double

System.out.print("Ingrese la cantidad de artículos: ");
var cantidad = scanner.nextInt(); // Infiere int

System.out.print("Ingrese la cantidad pagada por el cliente: ");
var cantidadPagada = scanner.nextDouble(); // Infiere double

// IMPORTANTE: Consumir el salto de línea después del último nextInt() o nextDouble()
scanner.nextLine();
```

### 5. Realizar los Cálculos

- Declara variables (usando `var` o el tipo explícito) para almacenar los resultados intermedios y finales.
- Realiza los siguientes cálculos utilizando los operadores aritméticos y la constante de impuesto:
  - Calcula el subtotal (precio unitario * cantidad).
  - Calcula el monto del impuesto (subtotal * tasa de impuesto).
  - Calcula el total de la compra (subtotal + monto del impuesto).
  - Calcula el cambio que se le debe dar al cliente (cantidad pagada - total de la compra).

```Java
// Dentro del método main, después de la entrada de datos

// Cálculos
var subtotal = precioUnitario * cantidad;
var montoImpuesto = subtotal * TASA_IMPUESTO;
var totalCompra = subtotal + montoImpuesto;
var cambio = cantidadPagada - totalCompra;
```

### 6. Mostrar los Resultados Formateados

- Imprime los resultados de los cálculos en la consola.
- Utiliza `System.out.printf()` para mostrar los valores monetarios (subtotal, impuesto, total, cambio) con **dos decimales**. Incluye texto descriptivo para cada valor.

```Java
// Dentro del método main, después de los cálculos

System.out.println("\n--- Resumen de la Compra ---"); // \n para un salto de línea

// Usamos %.2f para formatear a 2 decimales y %n para el salto de línea
System.out.printf("Subtotal: $%.2f%n", subtotal);
System.out.printf("Impuesto (%.0f%%): $%.2f%n", TASA_IMPUESTO * 100, montoImpuesto); // Mostrar tasa como porcentaje
System.out.printf("Total de la Compra: $%.2f%n", totalCompra);
System.out.printf("Cantidad Pagada: $%.2f%n", cantidadPagada);
System.out.printf("Cambio: $%.2f%n", cambio);

System.out.println("----------------------------");
```

### 7. Mensaje de Despedida y Cierre del `Scanner`

- Imprime un mensaje final de despedida.
- Cierra el objeto `Scanner`.

```Java
// Dentro del método main, después de mostrar resultados
System.out.println("¡Gracias por su compra!");

// Cierra el objeto Scanner para liberar recursos
scanner.close();
```

### 8. Prueba y Depuración

- Compila y ejecuta tu programa.
- Introduce diferentes valores para precio, cantidad y cantidad pagada.
- Verifica que los cálculos son correctos (puedes hacer las cuentas a mano o con otra calculadora para comparar).
- Asegúrate de que los resultados se muestran con el formato de dos decimales esperado.
- Si encuentras errores (de compilación o lógicos), revisa los mensajes y tu código paso a paso.

### 9. Documentación (Comentarios)

Añade comentarios a tu código para explicar la constante, la entrada de datos, cada cálculo y la salida de resultados.

### 10. Control de Versiones (Git)

Si estás usando Git, añade tu nuevo archivo (`git add .`), haz un commit con un mensaje claro (ej. `"feat: Added simple purchase calculator"`) y sube tus cambios (`git push origin main`).
