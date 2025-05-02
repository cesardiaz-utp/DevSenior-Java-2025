# Java Senior con IA

## Modulo 05 - Spring Boot desde Cero – Arquitectura Profesional para Aplicaciones Backend

En este módulo, el estudiante se introduce en el framework Spring Boot, una de las  herramientas más poderosas y utilizadas en la industria para construir aplicaciones backend modernas, escalables y seguras. Se aprende a estructurar un proyecto real con controladores, servicios y repositorios, aplicar buenas prácticas de desarrolo, y utilizar herramientas como Cursor AI para automatizar tareas comunes  y mejorar la calidad del código.

### [Clase 1](1) - Introducción a Spring Boot y Estructura de Proyectos

#### 1.1 Objetivos de aprendizaje

* Comprender la arquitectura y objetivos de Spring Boot.
* Crear un proyecto backend moderno con estructura profesional.
* Iniciar la construcción de APIs RESTful.

#### 1.2 Contenidos

* ¿Qué es Spring Boot y por qué es tan usado?
  * Comparación con otros frameworks y herramientas legadas.
* Generación de proyecto con Spring Initializr y apertura en Cursor AI.
* Estructura de carpetas profesional:
  * controller, service, repository, model, config.
* Configuración básica del archivo application.properties.
* Creación de la primera clase `@RestControler`.
* Primer endpoint: `GET` /saludo que devuelve un mensaje JSON.
* Uso de Cursor AI para generar controladores y documentación automática  del código.

### [Clase 2](2) - Servicios y Lógica de Negocio con Spring

#### 2.1 Objetivos de aprendizaje

* Separar responsabilidades entre controladores y lógica de negocio.
* Implementar servicios limpios y reutilizables.

#### 2.2 Contenidos

* Inversión de dependencias con `@Service` y `@Autowired`.
* **Separación por capas**: patrón Service Layer.
* Creación de clases de servicio para manejar operaciones CRUD lógicas.
* Uso de `@Component`, `@Bean`, `@Configuration` según necesidad.
* **Refactorización asistida por IA**: dividir lógica compleja, crear métodos reutilizables, renombrar correctamente.

### [Clase 3](3) - Principios SOLID y su aplicación práctica en Java

#### 3.1 Objetivos de aprendizaje

* Comprender los 5 principios SOLID como guía para un código mantenible y escalable.
* Aplicar ejemplos concretos en código Java de cada principio.
* Utilizar herramientas como IA para detectar y refactorizar violaciones a SOLID.

#### 3.2 Contenidos

* S: Single Responsibility Principle (SRP)
* O: Open/Closed Principle (OCP)
* L: Liskov Substitution Principle (LSP)
* I: Interface Segregation Principle (ISP)
* D: Dependency Inversion Principle (DIP)
* Aplicación práctica con clases de ejemplo y análisis de código real.
* Refactorización con IA: dividir clases, delegar responsabilidades, abstraer dependencias.

### [Clase 4](4/) - Proyecto API RESTful Completa con Spring Boot

#### 4.1 Objetivos de aprendizaje

* Integrar controladores, servicios y persistencia en un sistema real completo.
* Documentar y publicar la API con estándares profesionales.

#### 4.2 Contenidos

* Diseño del proyecto: entidad principal (`Libro`, `Estudiante`, `Producto`, etc.).
* Implementación completa de rutas REST (`GET`, `POST`, `PUT`, `DELETE`).
* Validaciones con `javax.validation`: `@NotNul`, `@Email`, `@Size`, etc.
* Introducción a Swagger para documentación automática con springdoc openapi.
* Refactorización final con IA: código limpio, responsabilidades claras, documentación completa.
