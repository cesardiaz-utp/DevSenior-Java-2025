# Ejercicios adicionales

Estos ejercicios te presentan escenarios. Tu tarea es diseñar las clases necesarias, identificar las relaciones de herencia o implementación de interfaces, definir los atributos y métodos adecuados, aplicar el encapsulamiento y demostrar el polimorfismo.

Recuerda crear el modelo de datos antes de iniciar el desarrollo de cada ejercicio.

## Ejercicio 1: Gestión de Sonidos de Animales

En un sistema para un zoológico virtual, necesitas representar diferentes tipos de animales y simular los sonidos que hacen. Cada animal tiene un nombre y una edad. Los perros ladran, los gatos maúllan, y las vacas mugen. Quieres poder referirte a ellos de manera general como animales, pero que al pedirles que hagan un sonido, cada uno responda de su manera específica.

**Funcionalidades esperadas**:

- Representar las características comunes a todos los animales.
- Representar las características específicas de cada tipo de animal (perro, gato, vaca).
- Permitir crear instancias de cada tipo de animal con su nombre y edad.
- Cada animal debe poder "hacer su sonido" característico.
- Debes poder tener una variable que pueda referirse a cualquier tipo de animal y, a través de ella, hacer que el animal emita su sonido correcto.

## Ejercicio 2: Cálculo de Áreas de Figuras

Necesitas un programa para calcular el área de diferentes figuras geométricas básicas como círculos y rectángulos. Todas las figuras tienen un color. El cálculo del área es diferente para cada tipo de figura. Quieres poder calcular el área de cualquier figura sin importar su tipo específico, siempre que sepas que es una figura.

**Funcionalidades esperadas**:

- Representar las características comunes a todas las figuras (como el color).
- Representar las características únicas de cada figura (como el radio de un círculo o las dimensiones de un rectángulo).
- Permitir crear instancias de círculos y rectángulos con sus propiedades.
- Cada figura debe poder calcular su propia área.
- Debes poder tener una variable que pueda referirse a un círculo o un rectángulo indistintamente y, a través de esa variable, obtener el área calculada correctamente para la figura específica.

## Ejercicio 3: Comportamientos de Vuelo y Nado

En un simulador, hay diferentes tipos de seres que pueden moverse. Algunos pueden volar, otros pueden nadar, y algunos solo pueden caminar. Necesitas una forma de modelar estos comportamientos de manera flexible. Las aves vuelan, los peces nadan, y los patos pueden nadar y volar. Quieres poder indicar que un ser "puede volar" o "puede nadar" sin importar si es un ave, un pez o un pato, y activar ese comportamiento si lo tienen.

**Funcionalidades esperadas**:

- Representar las características básicas de los seres (quizás un nombre).
- Modelar la capacidad de volar.
- Modelar la capacidad de nadar.
- Representar específicamente a las aves (que vuelan), los peces (que nadan) y los patos (que vuelan y nadan).
- Permitir crear instancias de aves, peces y patos.
- Debes poder tener variables que representen la capacidad de volar o nadar, y asignarles los seres que tengan esa capacidad para activar el comportamiento correspondiente.

## Ejercicio 4: Procesamiento de Documentos

Imagina que tienes diferentes tipos de documentos, como reportes y presentaciones. Ambos tienen un título y un autor. Los reportes tienen un número de páginas, mientras que las presentaciones tienen un número de diapositivas. Quieres poder "procesar" cualquier documento de una manera general, pero que la acción específica de procesamiento (quizás imprimir un resumen o un mensaje) se adapte al tipo de documento. Además, quieres poder calcular el "tamaño" del documento de diferentes maneras (ej. por páginas o por diapositivas), quizás usando el mismo nombre de método pero con diferentes parámetros.

**Funcionalidades esperadas**:

- Representar las características comunes a todos los documentos (título, autor).
- Representar las características específicas de reportes (páginas) y presentaciones (diapositivas).
- Permitir crear instancias de reportes y presentaciones.
- Cada tipo de documento debe tener una forma de "procesarse", con una implementación específica para reportes y presentaciones.
- Cada tipo de documento debe tener una forma de calcular su "tamaño", quizás con diferentes opciones de cálculo (sobrecarga).
- Debes poder tener una variable que pueda referirse a un reporte o una presentación y, a través de ella, activar el procesamiento correcto.
