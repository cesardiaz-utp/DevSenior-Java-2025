# Java Senior con IA

## Modulo 07 - Seguridad, JWT y APIs Avanzadas en Spring Boot

### Clase 3 - Implementación de Autenticación con JWT (JSON Web Tokens)

### [Contenido](1-contenido.md)

1. Configuración Inicial
    1. Dependencias (`pom.xml`)
    2. Clave Secreta para JWT
2. Qué es un JWT? Encabezado, Payload y Firma
    1. Características Clave de JWT y sus Implicaciones en la Arquitectura
    2. Header (Cabecera)
    3. Payload (Carga Útil o Claims)
    4. Signature (Firma)
3. Flujo Completo de Autenticación sin Estado
    1. Modelos de Solicitud y Respuesta para Autenticación
    2. Componente para Generación y Validación de JWT (`JwtUtil.java`)
    3. Servicio de Usuario Personalizado (`MyUserDetailsService.java`)
    4. Controlador de Autenticación (`AuthController.java`)
4. Implementación de Filtros Personalizados y Configuración de Cabeceras
    1. Filtro de Autenticación JWT (`JwtRequestFilter.java`)
    2. Configuración de Spring Security (`SecurityConfig.java`)
    3. Controlador de Prueba (`HelloController.java`)

### [Tutoría](2-tutoria.md)

1. Bienvenida y Recopilación de Dudas Rápidas
2. Presentación del Proyecto de la Tutoría
3. Evaluación de la seguridad, estructura y consistencia del proyecto final del módulo
4. Realizar pruebas completas de flujo autenticado y protección de rutas
5. Presentación del flujo completo: login → token → consumo de recursos protegidos
6. Validación cruzada: pruebas con compañeros para comprobar errores, protección y mensajes
7. Publicación del proyecto seguro en GitHub con documentación clara

#### [Ejercicio autónomo](3-ejercicio.md)

#### [Ejercicios adicionales](4-ejercicios_adicionales.md)
