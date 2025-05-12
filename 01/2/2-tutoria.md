# Tutoría: Refuerzo de Variables, Tipos, Constantes y Operadores

## 1. Repaso Rápido y Preguntas Abiertas

- **Objetivo**: Evaluar la comprensión inicial del estudiante sobre los temas de la Clase 2 y resolver dudas generales que puedan tener.
- **Dinámica**:
  - **Iniciar con una pregunta abierta**:
    - ¿Qué tal te fue con el contenido de la Clase 2?
    - ¿Hubo algún concepto que te resultara particularmente desafiante o que te generara dudas?
  - **Realizar preguntas clave para sondear la comprensión**:
    - ¿Cuál es la diferencia fundamental entre `int` y `double`? ¿Cuándo usarías cada uno?
    - ¿Puedes explicar la importancia de la palabra clave `final` y cómo se nombran las constantes?
    - ¿Cuál es la diferencia entre `System.out.print()`, `System.out.println()` y `System.out.printf()`? ¿Cuándo usarías `printf()`?
    - ¿Qué es la inferencia de tipo con var y cuándo sería útil usarla? ¿Tiene alguna limitación?
  - ¿Qué sucede si intentas dividir un int entre otro int (ej. `7 / 2`)? ¿Y si divides un double entre un int (ej. `7.0 / 2`)?
  - Si el estudiante intentó el ejercicio "Conversor de Medidas", pedirle que comparta su código y que explique su lógica. Preguntar sobre los problemas que encontró o los "ahá!" que tuvo. Si no lo hizo, se puede usar la tutoría como una oportunidad guiada para empezar a resolverlo.
  - Anotar las áreas donde el estudiante muestra debilidad para enfocarse en ellas durante las actividades.

## 2. Taller: Descomponiendo Expresiones Aritméticas y Entendiendo Comparaciones

- **Objetivo**: Reforzar la precedencia de operadores aritméticos, el uso de paréntesis y cómo las operaciones (incluidas las comparaciones simples) producen resultados de ciertos tipos.
- **Dinámica**:
  - Presentar al estudiante una serie de expresiones Java y pedirle que las evalúe "a mano", paso a paso, escribiendo el valor y el tipo de dato resultante en cada etapa.
  - **Ejemplos de Expresiones**:
    - `int x = 10; double y = 3.0; var z = x / y;` (Pedir valor y tipo de z).
    - `var res1 = 5 + 8 * 2 - (12 % 5);` (Pedir la evaluación paso a paso y el resultado final).
    - `var res2 = (20 - 5) / 3.0 * 2;` (Enfatizar el cambio de tipo si un operando es double).
  - **Variaciones**:
    - Pedir al estudiante que "rompa" una expresión compleja con paréntesis para forzar un orden diferente y ver cómo cambia el resultado.
    - Discutir la precedencia de operadores y la importancia de los paréntesis para asegurar que las operaciones se realicen en el orden deseado.
    - Resaltar cómo un cambio en el tipo de un operando (int a double) puede afectar el resultado de la división.

## 3. Mini Proyecto: Calculadora de IMC

- **Objetivo**: Aplicar de manera integrada variables, tipos de datos, constantes, operadores, `Scanner`, `System.out.printf()`.
- **Dinámica**:
  - Guiar al estudiante para que cree un nuevo archivo Java (ej. `CalculadoraIMC.java`).
  - Pasos a seguir (guiados por el tutor):
    1. **Entrada de Datos**: Pedir al usuario que ingrese su peso en kilogramos y su altura en metros usando `Scanner`. Sugerir el uso de `var` para las variables si el estudiante se siente cómodo.

        ```java
        // Ejemplo para el estudiante
        import java.util.Scanner;

        public class CalculadoraIMC {
            public static void main(String[] args) {
                Scanner scanner = new Scanner(System.in);

                System.out.print("Ingrese su peso en kilogramos: ");
                var peso = scanner.nextDouble();

                System.out.print("Ingrese su altura en metros: ");
                var altura = scanner.nextDouble();

                // ... resto del código
                scanner.close();
            }
        }
        ```

    2. **Cálculo de IMC:** Implementar la fórmula del IMC: `IMC = peso(kg) / altura(m) ^ 2`. Recordar cómo calcular la potencia (`altura * altura`).
    3. **Salida Formateada:** Mostrar el resultado del IMC con dos decimales utilizando `System.out.printf()`.

  - **Guía y Debugging durante el proyecto:**
    - Observar cómo el estudiante declara las variables (tipos primitivos o `var`).
    - Recordar las consideraciones con `Scanner` (`nextLine()` después de `nextDouble()`).
    - Revisar el uso correcto de `printf()` para el formato de salida deseado.
    - **Errores Comunes a buscar:**
      - Errores de compilación por sintaxis incorrecta.
      - Errores de lógica en el cálculo de IMC (ej. precedencia de operadores).
      - Problemas al leer los datos de entrada.
      - Errores de tipo al intentar asignar valores sin casting adecuado.
    - Guiar al estudiante en la lectura de los mensajes de error del compilador y animarlo a buscar la causa raíz.

## 4. Discusión de Errores Comunes y Estrategias de Depuración

- **Objetivo**: Ayudar al estudiante a identificar, entender y solucionar problemas típicos en el código Java.
- **Temas a Discutir**:
  - **Errores de Precedencia de Operadores**: Reforzar ejemplos del Taller donde un pequeño cambio en los paréntesis alteró el resultado. Preguntar al estudiante si alguna vez tuvo un cálculo incorrecto que luego corrigió al añadir paréntesis.
  - **Errores por Conversión de Tipos**:
    - Recordar que Java es de tipado fuerte.
    - ¿Qué ocurre con `double` a `int` sin casting? (Error de compilación).
    - ¿Qué ocurre con `double` a `int` con casting? (Pérdida de información/truncamiento). Demostrar con un ejemplo claro.
    - **Preguntar**: "¿En qué situación es importante tener cuidado con la precisión de los `double` (ej. para dinero)?" (Introducir la idea de BigDecimal como un adelanto para evitar problemas de precisión en el futuro, sin profundizar).
  - **Errores con `var`**:
    - "Si intentas `var miNumero;` sin inicializarlo, ¿qué error obtienes?" (Recordar que var debe ser inicializado).
  - Validación y Pruebas Manuales:
    - **Herramientas de IDE**: Mencionar que el propio IDE (VSCode) es una herramienta de depuración temprana, mostrando errores de compilación en tiempo real.
    - **IA como Asistente de Debugging**: Explicar cómo pueden usar herramientas de IA (como el propio Gemini, ChatGPT, etc.) para:
      - Pegar mensajes de error del compilador y pedir una explicación clara y sugerencias de solución.
      - Preguntar "¿Por qué este código no funciona como espero?" y describir el problema y el código.
    - **Pruebas Manuales**: Enfatizar la importancia de las pruebas. "¿Cómo sabes si tu conversor de medidas funciona correctamente? ¿Qué valores de entrada probarías y qué resultado esperarías?" (Ej. Metros a Centímetros: `1 metro` -> `100 cm`).

## 5. Cierre y Próximos Pasos

- **Recapitulación**: Resumir los puntos clave reforzados en la tutoría: variables, tipos, constantes, operadores, `var`, entrada/salida, y la importancia de la depuración y validación básica.

- **Animo y Próximos Pasos**:
  - Asegurarse de que el estudiante se siente más cómodo y confiado con los conceptos y las herramientas.
  - Recordar la importancia de la práctica continua y de completar el ejercicio principal de la Clase 2.
  - Animar a revisar el contenido de la próxima clase para estar preparados.
