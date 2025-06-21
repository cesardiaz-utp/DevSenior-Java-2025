# Java Senior con IA

## Modulo 03 - Excepciones, Logging y Pruebas Unitarias

Este módulo se enfoca en herramientas y prácticas esenciales para el desarrollo de software profesional: cómo manejar errores de forma elegante, cómo obtener visibilidad sobre el funcionamiento de la aplicación y cómo garantizar la calidad del código mediante pruebas automatizadas.

### [Clase 01](1) - Gestores de Proyectos, Excepciones y Manejo de Errores en Java

- **Objetivos de aprendizaje**
  - Entender que son los gestors de proyectos Java: Maven y Gradle
  - Comprender el concepto de errores y excepciones en Java y su diferencia.
  - Aprender a utilizar los bloques `try-catch` para capturar y manejar excepciones.
  - Conocer el proceso básico para crear excepciones personalizadas.

- **Contenido**
  - Gestor de proyectos Maven y Gradle.
  - Introducción a Errores y Excepciones: ¿Qué son y por qué manejarlos?
  - Jerarquía de Excepciones de Java: `Throwable`, `Error`, `Exception`, `RuntimeException`.
  - Diferencia entre Excepciones Verificadas (**Checked**) y No Verificadas (**Unchecked**).
  - El Bloque `try-catch`: Sintaxis y flujo de ejecución.
  - Manejo de múltiples excepciones (`catch` múltiples o `|`).
  - El Bloque `finally`: Propósito y uso.
  - Lanzamiento explícito de excepciones con `throw`.
  - Declaración de excepciones con `throws` en la firma de métodos.
  - Creación de Excepciones Personalizadas.
  - Ejemplos prácticos de manejo de excepciones comunes.

### [Clase 02](2) - Logs y Depuración con Log4j2 y SLF4j

- **Objetivos de aprendizaje**
  - Entender la importancia del logging en aplicaciones profesionales.
  - Configurar un sistema de logging básico.
  - Aplicar buenas prácticas de logging para depuración y monitoreo.

- **Contenido**
  - Limitaciones de `System.out.println()` para logging.
  - Importancia del logging en aplicaciones profesionales.
  - Introducción a **SLF4j** (fachada de logging).
  - Introducción a **Log4j2** (implementación de logging).
  - Configuración básica de Log4j2 (`log4j2.xml` para consola y archivos).
  - Niveles de Log: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL` y su jerarquía.
  - Uso del `Logger` en código Java.
  - Sustitución de parámetros en mensajes de log.
  - Logging de excepciones.
  - Diferencias entre depuración con logs y depurador (debugger).

### [Clase 03](3) - Pruebas Unitarias con JUnit y Mockito

- **Objetivos de aprendizaje**
  - Comprender qué son las pruebas unitarias y por qué son una práctica de desarrollo crucial.
  - Aprender a escribir pruebas unitarias básicas usando el framework JUnit 5.
  - Introducir el concepto de simulación (mocking) de dependencias.
  
- **Contenido**
  - Introducción a las Pruebas Unitarias: ¿Qué son y por qué son importantes?
  - Ciclo de una prueba unitaria: `Arrange`, `Act`, `Assert`.
  - **JUnit 5**: Configuración y estructura básica de pruebas.
  - Convenciones de nombres para métodos de prueba.
  - **Mockito**: ¿Por qué y cuándo usar **mocks**?
  - Creación de objetos simulados (mocks) con Mockito.
  - Escribiendo pruebas unitarias para clases con dependencias.

### [Clase 04](4) - Proyecto Integrador usando Excepciones, Logging y pruebas unitarias

- **Objetivos de aprendizaje**
  - Creación de `enums`.
  - Definicion de paquetes y arquitectura por capas.
  - Integrar los conceptos de POO.
  - Aplicar conocimientos adquiridos para diseñar una solución escalable y mantenible.

- **Contenido**
  - Análisis de requerimientos y diseño de clases
  - Definición de interfaces, herencias y comportamientos
  - Validaciones básicas y manejo de errores
  - Documentación de clases y métodos
