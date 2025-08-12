# Tutoría: El Rol de la IA y la Conexión Frontend-Backend

## 1. Discutiendo sobre el Rol de la IA como Asistente en el Desarrollo

- ¿Qué es el **"Vibecoding"** y por qué es una práctica riesgosa? ¿Pueden dar un ejemplo de una situación en la que el Vibecoding podría causar un problema serio en un proyecto?
- Discutan la diferencia entre usar la IA como un "par de programadores expertos" y como un reemplazo del pensamiento crítico.
- Pongan ejemplos concretos de cómo la IA puede acelerar el desarrollo en las siguientes tareas:
  - Generación de código (ej. DTOs, métodos de servicio).
  - Explicación de código complejo (ej. `Stream API` en Java).
  - Refactorización y mejoras de buenas prácticas (ej. manejo de `null`).
- ¿Cuál es el rol del desarrollador después de que la IA ha generado un fragmento de código? ¿Qué pasos de revisión son indispensables?
- ¿Cómo puede la IA ayudar a generar documentación con Javadoc y TSDoc, y por qué esta documentación es crucial en un equipo?

## 2. Repaso del Backend con Spring Boot

- Describe las cuatro capas principales (`Entity`, `Repository`, `Service`, `Controller`) y explica la responsabilidad única de cada una.
- ¿Por qué es una buena práctica usar **DTOs** en lugar de exponer directamente la entidad de la base de datos en la capa de controlador?
- ¿Qué son los `Records` de Java y por qué son una opción ideal para representar los DTOs?
- Expliquen el propósito de **MapStruct**. ¿Qué problema resuelve y por qué es más eficiente que el mapeo manual?
- En el `PropertyRepository`, ¿cómo sabe Spring Boot qué consulta SQL debe ejecutar cuando se usa un método como `findAllByCity(String city)`?

## 3. Analisando el consumo de la API con Angular y Signals

- En el contexto de la comunicación HTTP, ¿cuál es la diferencia fundamental entre un `Observable` y una `Signal`? ¿Qué beneficios trae cada uno?
- ¿Cuál es la función del método `toSignal()`? ¿Qué problema de gestión de memoria resuelve automáticamente?
- Expliquen por qué es una buena práctica usar la opción `initialValue` al convertir un `Observable` a una `Signal` con `toSignal()`. ¿Qué errores previene en el componente que lo consume?
- Comparen los dos enfoques de consumo en el componente:
  - El uso del `async` pipe con `Observables`.
  - El acceso directo a la `Signal`.
  - ¿Cuál es más declarativo y por qué?
- Describan las ventajas de las nuevas sintaxis de control de flujo, como `@if` y `@for`, frente a sus predecesoras `*ngIf` y `*ngFor`.
