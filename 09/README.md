# Java Senior con IA

## Modulo 09 - Integración Full Stack Moderna – Spring Boot + Angular con Seguridad JWT

Este módulo avanzado está diseñado para desarrolladores que ya tienen una base en **Spring Boot** y **Angular** y buscan dominar la integración completa de ambas tecnologías, centrándose en la seguridad de las aplicaciones modernas. A lo largo del curso, no solo aprenderemos a conectar el frontend y el backend de manera eficiente, sino que también implementaremos un sistema robusto de autenticación y autorización utilizando **JSON Web Tokens (JWT)**. Exploraremos cómo los `HttpInterceptor` y `Guards` de Angular trabajan en conjunto con **Spring Security** para crear un flujo de seguridad transparente y proteger las rutas y datos de la aplicación. Para finalizar, aplicaremos estos conocimientos en un proyecto práctico completo con la asistencia de herramientas de **Inteligencia Artificial**, demostrando cómo la IA puede actuar como un poderoso copiloto para acelerar el desarrollo y mejorar la productividad.

### [Clase 01](1) - El Rol de la IA y Conexión Frontend-Backend

- **Objetivos de Aprendizaje**:
  - Comprender cómo la Inteligencia Artificial puede ser utilizada como una herramienta para generar código y acelerar el desarrollo full stack.
  - Recordar y aplicar los conceptos de cómo un backend con Spring Boot expone sus servicios REST.
  - Aprender a consumir endpoints del backend desde Angular de forma reactiva utilizando `HttpClient` y convirtiendo `Observables` a `Signals`.

- **Contenidos Propuestos**:
  - **Asistencia de la IA en el Desarrollo Full Stack**: Uso de herramientas de IA para generar automáticamente modelos de datos (entidades y DTOs) y código de servicios o controladores, optimizando el tiempo de codificación.
  - **Repaso del Backend con Spring Boot**: Estructura de un `@RestController` y exposición de endpoints REST para operaciones CRUD.
  - **Consumo de Endpoints con Angular y `Signals`**:
    - Uso del módulo `HttpClient` para realizar peticiones que devuelven `Observables`.
    - Conversión de `Observables` a `Signals` utilizando la función `toSignal()`.
    - Creación de `Data Services` que exponen `Signals` para un manejo de estado más simple y reactivo en los componentes.

### [Clase 02](2) - Seguridad en la Integración: JWT, Interceptores y Guards

- **Objetivos de Aprendizaje**:
  - Implementar un flujo de autenticación completo donde el backend genera un JWT y el frontend lo gestiona **utilizando Signals**.
  - Configurar `HttpInterceptor` en Angular para adjuntar automáticamente el token de seguridad en las peticiones.
  - Proteger las rutas del frontend usando `Guards` que verifican el estado de autenticación a través de una Signal.

- **Contenidos Propuestos**:
  - **Ciclo de Vida del JWT**: Desde la validación de credenciales por parte de Spring Security hasta la generación y envío del token.
  - **Flujo de Autenticación en Angular con Signals**:
    - Creación de un `AuthService` que utiliza un `Signal` para gestionar el estado de autenticación (`isLoggedIn`, `user`, etc.).
    - Almacenamiento seguro del JWT en `localStorage`.
  - **`HttpInterceptor` para la cabecera de Autorización**:
    - Configuración de un interceptor para agregar el JWT a la cabecera `Authorization` de cada petición.
    - Manejo de errores 401 (no autorizado) para redirigir al usuario al login.
  - **`Guards` para la Protección de Rutas**:
    - Implementación de `AuthGuard` que comprueba el valor de la `Signal` de autenticación para controlar el acceso a componentes y rutas específicas.

### [Clase 03](3) - Proyecto Integrador con IA: Sistema de Creación y Diligenciamiento de Exámenes

- **Objetivos de Aprendizaje**:
  - Aplicar los conocimientos de integración y seguridad para construir un sistema completo desde cero.
  - Utilizar la IA como un copiloto de desarrollo para acelerar la creación del backend y el frontend.
  - Desarrollar la lógica de negocio para un sistema de exámenes de selección múltiple con calificación automática, **manejando el estado con Signals**.

- **Contenidos Propuestos**:
  - **Análisis y Diseño del Sistema de Exámenes**:
    - Definición de las entidades clave: `Exam`, `Question`, `AnswerOption`.
    - Creación de las rutas y endpoints necesarios para la gestión de exámenes.
  - **Desarrollo del Backend Asistido por IA**:
    - Generación de los modelos de datos en Spring Boot. **Nota**: Las clases, métodos y variables deben estar en inglés. Por ejemplo: `ExamEntity`, `QuestionService`, `calculateScore()`.
    - Creación de `Repository`, `Service` y `Controller` con asistencia de la IA.
    - Implementación de la lógica de calificación automática en el backend.
  - **Desarrollo del Frontend Asistido por IA con Signals**:
    - Generación de interfaces en Angular que coincidan con los modelos del backend. Nota: Las clases, métodos y variables deben estar en inglés. Por ejemplo: `ExamComponent`, `QuestionService`, `getQuestions()`.
    - Creación de componentes para la administración (creación de exámenes) y para el diligenciamiento por parte del usuario.
    - Creación de componentes y servicios que usan `Signals` para gestionar el estado de los exámenes y las respuestas del usuario.
  - **Integración Final**: Conectar el frontend con el backend, asegurando que las peticiones a los endpoints del examen estén protegidas con JWT.

### [Clase 04](4) - Comunicación en Tiempo Real con WebSockets para una Experiencia Interactiva

- **Objetivos de Aprendizaje**:
  - Comprender el concepto de comunicación en tiempo real y la diferencia con las peticiones REST.
  - Implementar un servidor de **WebSockets en Spring Boot** para enviar actualizaciones en tiempo real.
  - Desarrollar un cliente **WebSocket en Angular** para consumir los datos en tiempo real y actualizar el estado usando `Signals`.

- **Contenidos Propuestos**:
  - **Introducción a WebSockets**:
    - Explicación del modelo de comunicación full-duplex y sus ventajas.
    - Comparación con la comunicación HTTP tradicional.
  - **Implementación de WebSockets en Spring Boot**:
    - Configuración de un servidor WebSocket simple.
    - Envío de mensajes a clientes conectados.
  - **Cliente WebSocket en Angular**:
    - Conexión al servidor WebSocket del backend.
    - Consumo de los mensajes entrantes y actualización del estado de los componentes a través de un Signal.
  - **Ejemplo Práctico**:
    - Adición de una funcionalidad de chat en el sistema de exámenes para que los administradores puedan comunicarse en tiempo real o para enviar notificaciones en vivo sobre un examen.
