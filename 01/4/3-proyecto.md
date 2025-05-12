# Proyecto del Módulo 1: Sistema Básico de Registro de Estudiantes

Este es tu desafío para consolidar todo lo aprendido en el Módulo 1: variables, tipos de datos, constantes, operadores, entrada/salida por consola, estructuras condicionales, bucles y, ahora, funciones y modularidad.

## Objetivo del Proyecto

Crear un programa en consola que simule un sistema muy básico para gestionar la información de un solo estudiante a la vez, utilizando funciones para organizar las diferentes tareas.

## Requisitos y Funcionalidades (Implementar como Métodos)

Tu programa principal (`main`) debe actuar como un orquestador que llama a diferentes métodos para realizar las siguientes tareas:

1. Mostrar el menú:
    - Este método debe encargarse únicamente de imprimir las opciones del menú principal en la consola.
    - Ejemplo de menú:

      ```text
      --- Sistema de Registro de Estudiantes ---

      1. Registrar datos de un estudiante
      2. Mostrar datos del estudiante actual
      3. Calcular promedio de notas del estudiante actual
      4. Validar una nota individual
      0. Salir
      Ingrese su opción:
      ```

2. Registrar la información estudiante:
    - Este método debe solicitar al usuario el **nombre** del estudiante y **tres notas** (pueden ser números decimales).
    - Debe utilizar el objeto `Scanner` pasado como parámetro para leer la entrada del usuario.
    - Debe almacenar el nombre y las tres notas en variables locales dentro de este método.
    - **Importante**: Para poder usar estos datos en otras funciones (como `mostrarInfoEstudiante` o `calcularPromedioEstudiante`), deberás pensar cómo pasar esta información. Se recomienda usar variables de alcance de clase (`static`) para almacenar el nombre y las notas del estudiante actual.
3. Mostrar información de estudiante:
    - Este método debe imprimir en la consola el nombre y las tres notas del estudiante que fue registrado por última vez (utilizando las variables de alcance de clase).
    - Debe manejar el caso en que aún no se ha registrado ningún estudiante (ej. `estudianteActualNombre` es "N/A").
4. Calcular promedio del estudiante:
    - Este método debe calcular el promedio de las tres notas del estudiante actual (utilizando las variables de alcance de clase).
    - Debe **retornar** el valor del promedio (un double).
5. Validar nota:
    - Este método debe recibir una nota como **parámetro**.
    - Debe verificar si la nota está dentro de un rango válido (ej. entre 0 y 100, inclusive).
    - Debe **retornar** `true` si la nota es válida, y `false` si no lo es.
    - Este método sirve para validar las notas ingresadas por el usuario. Si una nota no es válida, se debe informar al usuario. Quien llame a este método debe seguir pidiendo la nota hasta que sea válida.
6. Validar nombre:
    - Este método debe recibir un nombre como **parámetro**.
    - Debe verificar si el nombre no está vacío o solo contiene espacios en blanco.
    - Debe retornar `true` si el nombre es válido, y `false` si no lo es.
    - Quien llame a este método debe seguir pidiendo la información hasta que sea válida.
7. Lógica del `main`:
    - El método `main` debe contener el bucle principal que muestra el menú y lee la opción del usuario.
    - Debe usar una estructura condicional para llamar a los métodos correspondientes según la opción elegida.
    - Debe manejar la opción "Salir" para terminar el bucle y el programa.
    - Debe llamar a "Registrar la información estudiante" cuando el usuario elija esa opción.
    - Cuando el usuario elija calcular el promedio, debe llamar a "Calcular promedio del estudiante", recibir el valor de retorno y luego imprimirlo formateado (ej. con dos decimales usando `printf`).
    - Cuando el usuario elija mostrar los datos, debe llamar a "Mostrar información de estudiante".
    - El `main` es responsable de crear y cerrar el objeto `Scanner`.

## Recomendaciones

- Aborda el proyecto paso a paso. Primero, haz que el menú funcione. Luego, implementa cada función una por una.
- Utiliza los conceptos de validación para asegurar que los datos ingresados sean correctos. Usa bucles para seguir pidiendo la entrada hasta que sea válida.
- Comenta tu código para explicar la lógica de cada método y sección.
- Utiliza `System.out.printf()` para mostrar los resultados numéricos (como el promedio) con un formato limpio (ej. 2 decimales).
- No olvides el control de versiones con Git: crea un nuevo commit para tu proyecto RETO y súbelo a GitHub.

Este proyecto es una excelente oportunidad para integrar todo lo que has aprendido. La tutoría de esta semana estará enfocada en ayudarte a resolver dudas y superar los desafíos que encuentres al construir este sistema.

**¡Mucho éxito!**
