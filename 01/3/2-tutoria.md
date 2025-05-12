# Tutoría: Resolución de Problemas con Lógica y Estructuras de Control

## 1. Repaso Rápido y Sondeo sobre la Clase 3 y el Proyecto

- **Objetivo**: Evaluar cómo les fue a los estudiantes con los conceptos de la clase y el proyecto en clase, identificar puntos débiles.
- **Dinámica**:
  - Iniciar preguntando: "¿Qué tal les parecieron las estructuras de control? ¿Fue fácil entender cuándo usar un `if` vs un `switch`, o un `while` vs un `for`?"
  - Pedirles que comenten sobre su experiencia con el proyecto en clase. ¿Qué parte les resultó más desafiante? ¿Encontraron bucles infinitos o lógica que no funcionaba como esperaban?
  - Repasar brevemente:
    - ¿Cuándo usarían un `if-else if` vs un `switch`?
    - ¿Cuál es la diferencia clave entre `while` y `do-while`?
    - ¿Cuándo es más apropiado usar un `for`?
    - ¿Cuál es la función de `break` y `continue`?
    - ¿Pueden dar un ejemplo rápido de cuándo usarían el operador ternario?
  - Anotar los temas o estructuras que parezcan menos claros para enfocar las actividades de refuerzo.

## 2. Retos de Programación Guiados

- **Objetivo**: Aplicar condicionales y bucles en escenarios prácticos para consolidar la comprensión y desarrollar la lógica.
- **Dinámica**: Presentar los siguientes retos uno por uno. Guiar a los estudiantes a pensarlos algorítmicamente y luego a codificarlos. Fomentar la discusión sobre las diferentes formas de resolver cada reto.

### Reto 1: Validación de Contraseña Simple (Condicionales)

- **Descripción**: Pide al usuario que ingrese una contraseña. Verifica si la contraseña es "JavaEsGenial". Si es correcta, imprime "Acceso concedido". Si no, imprime "Contraseña incorrecta".
- **Conceptos a reforzar**: `Scanner` para `String`, comparación de `String` (`equals` o `equalsIgnoreCase`), `if-else`.

### Reto 2: Sistema de Intentos de Contraseña (Condicionales + Bucle)

- **Descripción**: Modifica el reto anterior. El usuario tiene un número limitado de intentos (ej. 3) para ingresar la contraseña correcta. Usa un bucle (`for` o `while`) para controlar los intentos. Si acierta, sale del bucle y da acceso. Si agota los intentos sin acertar, imprime "Acceso denegado. Intentos agotados."
- **Conceptos a reforzar**: Bucles (`for` o `while`), contador, `if-else` dentro del bucle, `break` para salir del bucle anticipadamente.

### Reto 3: Mini Menú Interactivo Simple (Bucle + Switch/If-Else If)

- **Descripción**: Crea un menú con 2-3 opciones (ej. "Saludar", "Decir Adiós", "Salir"). Usa un bucle (`do-while` recomendado para que el menú se muestre al menos una vez) para mostrar el menú. Lee la opción del usuario y usa un `switch` (o `if-else if`) para ejecutar la acción correspondiente. La opción "Salir" debe terminar el programa.
- **Conceptos a reforzar**: `do-while` loop, `switch-case` (o `if-else if`), `Scanner` para `int` o `String`, `break` para salir del `switch` y/o controlar el bucle.

## 3. Diagnóstico de Errores y Uso del Depurador (Debugger) en VSCode

- **Objetivo**: Enseñar a los estudiantes a encontrar y entender errores, y a usar una herramienta fundamental: el depurador.
- **Dinámica**:
  - **Errores Típicos de Lógica y Tipo**: Presentar fragmentos de código que contengan errores comunes relacionados con la Clase 3:
    - Bucle infinito (ej. olvidar actualizar el contador en un `while`).
    - Lógica incorrecta en la condición de un bucle (`<` en lugar de `<=`).
    - Uso incorrecto de operadores lógicos (`&&` en lugar de `||`).
    - "Fall-through" accidental en un `switch` (si usaron el `switch` tradicional).
    - Errores de conversión de tipos dentro de bucles o condicionales.
    - Problemas con `Scanner` (`nextLine()` después de `nextInt()`).
  - **Introducción al Debugger de VSCode**:
    - Explicar qué es un depurador y por qué es invaluable para entender el flujo de ejecución y el estado de las variables.
    - Demostrar cómo:
      - Poner **puntos de interrupción** (**breakpoints**): Hacer clic al lado del número de línea.
      - Iniciar la depuración (botón "Run and Debug" o F5).
      - **Recorrer el código (stepping)**:
        - "Step Over" (F10): Ejecuta la línea actual y pasa a la siguiente.
        - "Step Into" (F11): Si la línea actual es una llamada a método, entra en ese método.
        - "Step Out" (Shift+F11): Sale del método actual y regresa a donde fue llamado.
        - "Continue" (F5): Continúa la ejecución hasta el próximo breakpoint o el final del programa.
      - **Inspeccionar variables**: Mostrar el panel "Variables" para ver los valores de las variables en el punto de interrupción actual.
      - **Ventana "Debug Console"**: Mostrar cómo se pueden evaluar expresiones o imprimir valores en tiempo de depuración.
  - **Práctica Guiada de Depuración**: Tomar uno de los fragmentos de código con errores (o un error que un estudiante haya tenido en su proyecto) y, paso a paso, usar el depurador para encontrar la causa del problema. Animar a los estudiantes a replicar el proceso en sus propias máquinas.

## 4. Desarrollo Colaborativo: “Calculadora de Lógica”

- **Objetivo**: Aplicar los conceptos de la clase y practicar la resolución de problemas de forma conjunta.
- **Dinámica**: Como grupo (o en parejas si es remoto), desarrollar un pequeño programa que resuelva problemas simples que requieran condicionales y/o bucles.
- **Posibles Mini-Proyectos Colaborativos**:
  - **Clasificador de Números**: Pide al usuario un número y, usando condicionales, di si es positivo, negativo o cero, y si es par o impar.
  - **Contador con Salto**: Pide al usuario un número inicial y final. Usa un bucle for para contar desde el inicio hasta el final, pero usa continue para saltar la impresión de un número específico (ej. el 7).
  - **Sumador hasta un Límite**: Pide al usuario números repetidamente usando un bucle while. Suma los números. Si la suma supera un límite (ej. 100), usa break para terminar el bucle y muestra la suma final.
- **Rol del Tutor**: Facilitar la discusión, guiar la lógica, sugerir estructuras de control apropiadas, y ayudar a escribir el código en tiempo real (compartiendo pantalla).

## 5. Feedback de Buenas Prácticas en Código

- **Objetivo**: Introducir hábitos de codificación que mejoran la legibilidad, mantenibilidad y evitan errores.
- **Temas a Discutir** (usando ejemplos del proyecto en clase o los retos):
  - **Nombres de Variables y Constantes**: Recordar `camelCase` para variables y `UPPER_SNAKE_CASE` para constantes. ¿Son descriptivos los nombres?
  - **Indentación y Espaciado**: Por qué es crucial para la legibilidad (mostrar código bien y mal indentado). VSCode ayuda con esto.
  - **Comentarios**: ¿El código explica _por qué_ se hace algo, no solo qué se hace? Comentarios para partes complejas o no obvias.
  - **Uso de Llaves `{}`**: Siempre usar llaves con `if`, `else`, `for`, `while`, `do-while`, `switch` incluso si es una sola línea.
  - **Condiciones Claras**: Evitar condiciones excesivamente complejas. Si una condición es muy larga, ¿se puede simplificar o dividir?
  - **Evitar Bucles Infinitos**: Recordar la importancia de la condición de salida.
  - **Cierre de Scanner**: Recordar `scanner.close()` al final.
  - **Uso Apropiado de `var`**: ¿Se usó var donde el tipo era obvio? ¿Se evitó donde hacía el código menos claro?

## 6. Cierre y Próximos Pasos

- **Recapitulación**: Resumir los conceptos clave reforzados: aplicación de condicionales/bucles, depuración, buenas prácticas.
- **Animo**: Felicitar por el progreso y la superación de los retos.
- **Próximo Ejercicio Autónomo**: Mencionar que habrá un ejercicio autónomo para seguir practicando estos conceptos de control de flujo.
- **Preguntas Finales**: Abrir el espacio para cualquier última duda.
