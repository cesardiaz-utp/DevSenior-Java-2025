# Tutoría: Implementación de Autenticación con JWT

## 1. Bienvenida y Recopilación de Dudas Rápidas

Antes de sumergirnos en las actividades, me gustaría abrir un espacio para que compartan cualquier duda rápida que les haya surgido al revisar el contenido teórico de la clase. ¿Hay algún concepto de JWT, su estructura o el flujo de autenticación que les genere inquietud?

## 2. Presentación del Proyecto de la Tutoría

Para esta tutoría, utilizaremos el proyecto base de Spring Boot que hemos estado construyendo. Este proyecto ya cuenta con las dependencias necesarias y la estructura inicial para que podamos enfocarnos directamente en la implementación de JWT. En particular, nos centraremos en:

- El endpoint `/authenticate` para el proceso de login.
- Un endpoint `/hello` que actuará como un recurso protegido.

La idea es que, al final de la tutoría, puedan ver cómo el JWT se genera en el login y cómo es necesario para acceder al recurso protegido.

## 3. Evaluación de la seguridad, estructura y consistencia del proyecto final del módulo

- **Descripción**: En esta actividad inicial, dedicaremos tiempo a revisar el proyecto de Spring Boot desarrollado por cada estudiante. El objetivo es asegurar que la implementación de JWT se haya realizado siguiendo las mejores prácticas de seguridad y que la estructura del código sea consistente y fácil de entender. Se prestará especial atención a:
  - **Configuración de la clave secreta**: ¿Está bien ubicada en `application.properties` o como variable de entorno? ¿Es suficientemente compleja?
  - **Manejo de la expiración del token**: ¿Se ha configurado un tiempo de vida adecuado para el JWT?
  - **Integración de Spring Security**: ¿Las configuraciones en `SecurityConfig` son correctas y coherentes con el uso de JWT (ej., `stateless`, `csrf().disable()`)?
  - **Organización del código**: ¿Las clases `JwtUtil`, `JwtRequestFilter`, `AuthController`, etc., están en sus paquetes correctos y siguen una lógica clara?
  - **Manejo de errores**: ¿La aplicación responde adecuadamente a intentos de acceso no autorizado o con tokens inválidos?

## 4. Realizar pruebas completas de flujo autenticado y protección de rutas

- **Descripción**: Esta actividad es la validación práctica de la implementación. Utilizaremos herramientas como Postman o Insomnia para simular el comportamiento de un cliente real. Cada estudiante deberá demostrar el flujo completo:
  - **Obtención del Token**: Realizar una solicitud `POST` al endpoint `/authenticate` con credenciales válidas y verificar que se recibe un JWT en la respuesta.
  - **Acceso a Recursos Protegidos**: Utilizar el JWT obtenido para realizar solicitudes GET a un endpoint protegido (ej., `/hello`). Se verificará que el acceso es exitoso solo con un token válido.
  - **Pruebas de Denegación de Acceso**: Intentar acceder al recurso protegido sin un token, con un token malformado, o con un token expirado, y verificar que la API responde con los códigos de estado HTTP esperados (`401 Unauthorized`, `403 Forbidden`).

## 5. Presentación del flujo completo: login → token → consumo de recursos protegidos

- **Descripción**: Cada estudiante (o en pequeños grupos) tendrá la oportunidad de presentar su implementación. Deberán explicar el viaje de una solicitud desde que el usuario intenta iniciar sesión, cómo se genera el JWT, cómo el cliente lo almacena y lo envía en solicitudes posteriores, y cómo el servidor lo valida para permitir el acceso a los recursos. Se fomentará la discusión sobre las decisiones de diseño tomadas.
- **Enfoque: Se hará hincapié en la naturaleza _stateless_ de la autenticación con JWT y cómo esto beneficia la escalabilidad y la arquitectura de microservicios.

## 6. Validación cruzada: pruebas con compañeros para comprobar errores, protección y mensajes

- **Descripción**: Para fomentar el aprendizaje colaborativo y la detección de posibles fallos, los estudiantes intercambiarán sus URLs de API (o proyectos si es posible) y realizarán pruebas entre sí. El objetivo es que cada uno intente "romper" la seguridad de la API de un compañero, buscando vulnerabilidades como:
  - Acceso no autorizado a rutas protegidas.
  - Respuestas inesperadas con tokens inválidos.
  - Mensajes de error que revelen demasiada información interna.
- **Beneficio**: Esta actividad no solo ayuda a identificar errores, sino que también mejora la comprensión de las implicaciones de seguridad y la importancia de una buena gestión de errores y mensajes.

## 7. Publicación del proyecto seguro en GitHub con documentación clara

- **Descripción**: Como actividad final, se guiará a los estudiantes en el proceso de publicar su proyecto de Spring Boot seguro en un repositorio de GitHub. Esto incluirá:
  - **Creación del repositorio**: Asegurarse de que el repositorio sea público o privado según la preferencia, pero siempre con buenas prácticas.
  - **Configuración de `.gitignore`**: Excluir archivos sensibles como la clave secreta (`application.properties` si contiene la clave directamente, o asegurar que la variable de entorno no se suba).
  - **Documentación (README.md)**: Crear un archivo `README.md` claro y conciso que explique cómo clonar el proyecto, cómo configurarlo (especialmente la clave secreta), cómo ejecutarlo y cómo probar los endpoints de autenticación y los protegidos. La documentación es crucial para la colaboración y la mantenibilidad.

## Cierre y Próximos Pasos

Para concluir esta tutoría, hemos cubierto la implementación completa de la autenticación JWT, desde la generación hasta la validación y el uso en un entorno Spring Boot. Han aprendido a configurar las dependencias, manejar la clave secreta, crear los componentes esenciales de JWT y Spring Security, y probar el flujo de autenticación.

Como próximo paso, los animo a completar el **Ejercicio Práctico Autoguiado** de la clase. Este ejercicio les permitirá reforzar lo visto en clase y experimentar de forma independiente con la configuración de JWT.
