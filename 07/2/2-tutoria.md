# Tutoría: Fundamentos de Seguridad en Aplicaciones

## 1. Bienvenida y Recopilación de Dudas Rápidas

¡Hola a todos! Es un gusto darles la bienvenida a esta tutoría de la Clase 2 del Módulo 7. Hoy vamos a sentar las bases de la seguridad en Spring Boot. Antes de comenzar, ¿tienen alguna duda rápida sobre los conceptos de autenticación, autorización o el papel de Spring Security?

## 2. Presentación del Proyecto de la Tutoría

Para esta tutoría, trabajaremos con un proyecto Spring Boot básico. El objetivo es que, al final de la sesión, puedan proteger endpoints específicos y entender cómo Spring Security gestiona el acceso. Nos centraremos en:

- Configurar Spring Security.
- Definir usuarios y roles en memoria (inicialmente).
- Proteger rutas con reglas de acceso.
- Realizar pruebas con un cliente HTTP (ej., Postman).

## 3. Configuración Inicial del Proyecto

**Descripción**: Iniciaremos creando un nuevo proyecto Spring Boot desde **Spring Initializr** o utilizando uno existente, asegurándonos de incluir la dependencia `spring-boot-starter-security`. Una vez el proyecto esté configurado, observaremos y discutiremos el comportamiento de seguridad por defecto que Spring Boot aplica automáticamente. Esto incluye la generación de una contraseña aleatoria en la consola para el usuario "user" y la redirección a una página de login básica para peticiones de navegador, así como la aplicación de autenticación HTTP Basic para APIs. Esta observación inicial nos ayudará a entender qué estamos personalizando en los pasos siguientes.

## 4. Configuración de `SecurityConfig`

**Descripción**: Crearemos la clase `SecurityConfig`, que será el corazón de nuestra configuración de seguridad.

- **Parte A**: `PasswordEncoder`: Implementaremos el `BCryptPasswordEncoder` como un `@Bean`. Explicaremos por qué es crucial usar un codificador de contraseñas robusto como `BCrypt` y nunca almacenar contraseñas en texto plano.
- **Parte B**: **Usuarios en Memoria**: Configuraremos el `AuthenticationManagerBuilder` para definir usuarios directamente en memoria. Crearemos un usuario "user" con el rol USER y un usuario "admin" con los roles ADMIN y USER. Discutiremos que, aunque esto es útil para demostraciones, en una aplicación real los usuarios se cargarían desde una base de datos.
- **Parte C**: **Reglas de Autorización HTTP**: Configuraremos el método `configure(HttpSecurity http)` para establecer las reglas de acceso a nuestras rutas:
  - Deshabilitaremos la protección CSRF (`.csrf().disable()`) y explicaremos por qué esta es una práctica aceptable y común para APIs RESTful stateless.
  - Permitiremos acceso público (`.permitAll()`) a cualquier ruta que comience con `/public/**`.
  - Restringiremos el acceso a las rutas que comiencen con `/admin/**` solo a usuarios que posean el rol `ADMIN` (`.hasRole("ADMIN")`).
  - Estableceremos que cualquier otra solicitud (`.anyRequest()`) requerirá autenticación (`.authenticated()`).
  - Habilitaremos la autenticación HTTP Basic (`.httpBasic()`) como el mecanismo de autenticación para esta clase, sentando las bases para la transición a JWT.

## 5. Implementación de `MyUserDetailsService`

- **Descripción**: Crearemos nuestra propia implementación de la interfaz `UserDetailsService` llamada `MyUserDetailsService`.
  - **Parte A**: **Carga de Usuarios**: Modificaremos el método `loadUserByUsername` para "simular" la carga de usuarios. En lugar de una conexión real a una base de datos (que se abordará en ejercicios posteriores), este servicio devolverá los mismos usuarios "user" y "admin" definidos previamente, pero ahora centralizando la lógica de carga de detalles del usuario. Esto nos permite desacoplar la lógica de autenticación de la fuente de datos.
  - **Parte B**: **Inyectar `UserDetailsService`**: Inyectaremos esta implementación personalizada en nuestra clase `SecurityConfig` y configuraremos el `AuthenticationManagerBuilder` para que utilice `myUserDetailsService` en lugar de la configuración `inMemoryAuthentication()` directa. Esto demuestra cómo Spring Security delega la carga de usuarios a un servicio específico.

## 6. Creación de Controladores de Prueba

- **Descripción**: Implementaremos la clase `TestController` que contendrá tres _endpoints_ HTTP simples para probar nuestras configuraciones de seguridad:
  - `/public/welcome`: Un endpoint que estará accesible para cualquier usuario, sin necesidad de autenticación.
  - `/authenticated/hello`: Un endpoint que requerirá que el usuario esté autenticado, sin importar su rol específico.
  - `/admin/dashboard`: Un endpoint que solo será accesible para usuarios que posean el rol `ADMIN`.
- Estos controladores nos permitirán verificar visualmente que las reglas de seguridad están siendo aplicadas correctamente.

## 7. Pruebas con Postman/Insomnia

- Descripción: Utilizaremos una herramienta cliente HTTP como Postman o Insomnia para realizar pruebas exhaustivas de cada endpoint bajo diferentes escenarios:
  - Acceder a `/public/welcome` sin credenciales.
  - Acceder a `/authenticated/hello` con las credenciales de `user:password` y `admin:admin_pass` usando autenticación HTTP Basic.
  - Acceder a `/admin/dashboard` con las credenciales de admin:admin_pass usando HTTP Basic.
  - Intentar acceder a `/admin/dashboard` con las credenciales de `user:password` para verificar la denegación de acceso por rol.
  - Intentar acceder a cualquier ruta protegida sin proporcionar ninguna credencial o con credenciales incorrectas, observando las respuestas `401 Unauthorized` o `403 Forbidden`.

Esta fase es crucial para confirmar que la seguridad está funcionando como se espera.

## 8. Cierre

En esta clase, hemos cubierto los fundamentos de Spring Security: entendiendo la diferencia entre autenticación y autorización, configurando reglas de seguridad básicas y preparando nuestro `UserDetailsService`. Esta es la base sobre la cual construiremos sistemas más complejos.
