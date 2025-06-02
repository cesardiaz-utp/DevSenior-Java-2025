# Clase 2: Fundamentos de Seguridad en Aplicaciones Spring Boot

¡Hola a todos!

Bienvenidos a la segunda clase del Módulo 7. Después de haber consolidado nuestras habilidades con la persistencia de datos, los DTOs y el manejo de excepciones, es momento de abordar un pilar fundamental en el desarrollo de cualquier aplicación web: la **seguridad**.

En esta clase, nos sumergiremos en los **fundamentos de la seguridad en aplicaciones Spring Boot** utilizando **Spring Security**, la herramienta estándar de la industria. Entenderemos por qué es crucial proteger nuestras APIs, cómo Spring Security nos facilita esta tarea con sus componentes y flujo de trabajo, y cómo podemos empezar a implementar autenticación y autorización básicas.

Prepárense para aprender a proteger sus _endpoints_ y a controlar quién tiene acceso a qué.

¡Vamos a ello!

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Comprender los fundamentos de la seguridad en aplicaciones web modernas**: Entenderás la importancia de la autenticación y la autorización, y cómo se aplican en un contexto web.
- **Implementar mecanismos básicos de autenticación y autorización con Spring Security**: Podrás configurar tu aplicación Spring Boot para proteger endpoints y controlar el acceso de los usuarios.
**Conocer el ciclo de vida de una solicitud protegida**: Entenderás cómo Spring Security intercepta y procesa las solicitudes para aplicar las reglas de seguridad.

## 1. ¿Qué es Spring Security?

**Spring Security** es un _framework_ poderoso y altamente configurable que provee servicios de seguridad para aplicaciones basadas en Spring. Es la solución de facto para la seguridad en aplicaciones Java del ecosistema Spring.

![Spring Security](https://adictosaltrabajo.com/wp-content/uploads/2020/05/spring_security-e1588763146414.jpg)

Sus dos áreas principales de enfoque son:

- **Autenticación**: Proceso de verificar la identidad de un usuario (¿quién eres?). Esto generalmente implica verificar credenciales (nombre de usuario y contraseña) contra una base de datos, un directorio LDAP, o un servicio de OAuth2.
- **Autorización**: Proceso de determinar si un usuario autenticado tiene permiso para realizar una acción o acceder a un recurso específico (¿qué puedes hacer?). Esto se basa en roles o permisos asignados al usuario.

### 1.1. Componentes Básicos de Spring Security

![Componentes Spring Security](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*kdfpQR9jkiYbHID9kOfzIQ.jpeg)

- `SecurityFilterChain`: Es el corazón de Spring Security. Es una cadena de filtros (servlets) que interceptan cada solicitud HTTP entrante antes de que llegue a tu `Controller`. Cada filtro es responsable de una parte específica del proceso de seguridad (autenticación, autorización, gestión de sesiones, etc.).

  ![SecurityFilterChain](https://i0.wp.com/waynestalk.com/wp-content/uploads/2020/08/SpringSecurityAndServlet.png?w=600&ssl=1)

- `AuthenticationManager`: La interfaz principal para realizar la autenticación. Es a donde se delega la verificación de las credenciales de un usuario.
- `AuthenticationProvider`: Son las implementaciones de `AuthenticationManager` que saben cómo autenticar a un usuario para un tipo específico de credencial o fuente de datos (ej., `DaoAuthenticationProvider` para bases de datos, `LdapAuthenticationProvider` para LDAP).

  ![AuthenticationManager y AuthenticationProvider](https://dz2cdn1.dzone.com/storage/temp/17477984-authentication-manager-provider-implementations.jpg)

- `UserDetailsService`: Una interfaz clave que se usa para recuperar los datos de un usuario (nombre de usuario, contraseña, roles/autoridades) de una fuente de datos (ej., base de datos, memoria) durante el proceso de autenticación.

  ![UserDetailsService](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjdAYaadtn1x_il3wRJuxsCqse_2r2O71zHNvX2E33_kwh0Qyn0U8P2-239EfJcMdi0GoHPyxG_USoqMPCrOJml2MhewsjGZ6BoRXY_cUBv4h39-5yMCRQ5Chh_xeRXpDZez_wnrqJ2OLQ/w728-h390-no/)

- `GrantedAuthority`: Representa un permiso o rol que se le ha otorgado a un usuario (ej., `ROLE_ADMIN`, `ROLE_USER`).
- `SecurityContextHolder`: Un mecanismo que Spring Security utiliza para almacenar los detalles del usuario autenticado (el objeto `Authentication`) en el hilo de ejecución actual, lo que permite que la aplicación acceda a la información del usuario en cualquier parte del código.

  ![SecurityContextHolder](https://i0.wp.com/waynestalk.com/wp-content/uploads/2020/08/SecurityContextHolder.png?w=500&ssl=1)

### 1.2. Flujo de Autenticación Básico (Username/Password)

![Spring Security Flow](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*f1NZoTCuXYPzZlVAnmKhbA.png)

1. Una solicitud HTTP llega al servidor.
2. El `SecurityFilterChain` intercepta la solicitud.
3. Si la solicitud requiere autenticación y el usuario no está autenticado, Spring Security intenta autenticarlo (ej., redirigiendo a una página de **login** o esperando credenciales en la cabecera).
4. El usuario envía sus credenciales (nombre de usuario y contraseña).
5. Spring Security crea un objeto `Authentication` (inicialmente no autenticado) con estas credenciales.
6. Este objeto `Authentication` es pasado al `AuthenticationManager`.
7. El `AuthenticationManager` delega a uno o más `AuthenticationProviders`.
8. Un `AuthenticationProvider` adecuado usa el `UserDetailsService` para cargar los detalles del usuario de la fuente de datos (memoria, base de datos, LDAP, etc.).
9. El `AuthenticationProvider` compara la contraseña proporcionada con la contraseña cargada (después de aplicar _hashing_ por el `PasswordEncoder`).
10. Si las credenciales son válidas, el `AuthenticationProvider` crea un objeto `Authentication` completamente autenticado, que incluye los roles/autoridades del usuario.
11. Este objeto `Authentication` se almacena en el `SecurityContextHolder`.
12. La solicitud continúa su procesamiento y el usuario ahora está autenticado para cualquier verificación de autorización posterior.

## 2. Seguridad por defecto en Spring Boot

Cuando añades la dependencia `spring-boot-starter-security` a un proyecto Spring Boot, obtienes una configuración de seguridad "por defecto" muy potente:

- **Todas las rutas están protegidas por defecto**: Cualquier intento de acceso a tu API o cualquier otra ruta de tu aplicación requerirá autenticación.
- **Página de login generada automáticamente**: Spring Security genera una página de login básica (`/login`) si no provees una.
- **Usuario y contraseña por defecto**: En el arranque de la aplicación, Spring Boot genera una contraseña aleatoria para el usuario por defecto `user` y la imprime en la consola. Esta es la que debes usar para iniciar sesión por primera vez.
- **Protección CSRF (Cross-Site Request Forgery)**: Habilitada por defecto para solicitudes que modifiquen el estado (POST, PUT, DELETE).
- **Protección contra Clickjacking (X-Frame-Options)**, **Seguridad de cabeceras HTTP**, etc.

Esta configuración por defecto es útil para empezar rápidamente, pero casi siempre necesitarás personalizarla para tus necesidades específicas.

## 3. Configuración de seguridad personalizada con `SecurityFilterChain`

Para personalizar el comportamiento de Spring Security, necesitas crear una clase de configuración que extienda o defina un _bean_ de `SecurityFilterChain`.

### 3.1. Pasos para una configuración básica personalizada

1. Crea una clase de configuración (ej., `SecurityConfig`) en un paquete apropiado (ej., `config`).
2. Anótala con `@Configuration` y `@EnableWebSecurity`. `@EnableWebSecurity` es crucial para habilitar la configuración de seguridad de Spring.
3. Define un _bean_ de `SecurityFilterChain` que toma un `HttpSecurity` como argumento. Este objeto `HttpSecurity` es el punto de entrada para construir la configuración de seguridad.

    ```Java
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.web.SecurityFilterChain;

    @Configuration
    @EnableWebSecurity // Habilita la seguridad web de Spring
    public class SecurityConfig {

        @Bean
        public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
            http
                // Deshabilita CSRF si es una API REST sin sesiones
                .csrf(csrf -> csrf.disable())
                // Define las reglas de autorización para las solicitudes HTTP
                .authorizeHttpRequests(authz -> authz
                    // Aquí definiremos nuestras reglas de rutas
                    // .requestMatchers("/public/**").permitAll()
                    // .anyRequest().authenticated()
                )
                // Habilita autenticación HTTP Basic (útil para APIs REST)
                .httpBasic(httpBasic -> {}); // O .formLogin(form -> {}); para login con formulario

            return http.build();
        }
    }
    ```

    **Nota sobre CSRF**: En el contexto de APIs REST sin estado (sin sesiones de usuario persistentes en el servidor, comúnmente con JWTs), la protección CSRF suele deshabilitarse (`.csrf(csrf -> csrf.disable())`), ya que el servidor no mantiene el estado de la sesión que un ataque CSRF intentaría explotar. Para APIs que sí usan sesiones, o si se desea una protección extra incluso con JWTs, se puede configurar, pero para este ejercicio la simplificación es válida.

### 3.2. Protección CSRF (Cross-Site Request Forgery)

**CSRF** es un tipo de ataque en el que un atacante engaña a un usuario autenticado para que realice una acción no deseada en una aplicación web. Esto se logra enviando una solicitud maliciosa desde un sitio web diferente al de la aplicación.

Spring Security proporciona protección contra ataques CSRF mediante la generación de un token único para cada sesión de usuario. Este token se incluye en cada solicitud que modifica el estado del servidor (POST, PUT, DELETE). El servidor verifica que el token en la solicitud coincida con el token almacenado en la sesión del usuario. Si los tokens no coinciden, la solicitud se rechaza.

En las versiones más recientes de Spring Security, la protección CSRF está habilitada por defecto para las solicitudes que modifican el estado del servidor. Sin embargo, es posible que debas habilitarla explícitamente en tu configuración de seguridad.

```Java
// ...existing code...
import org.springframework.security.config.annotation.web.configurers.CsrfConfigurer;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {

    // ...

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> {
                csrf.ignoringRequestMatchers("/api/public/**"); // Excluir rutas públicas si es necesario
            })
            // ...
        return http.build();
    }
}
```

En este ejemplo:

- `.csrf(csrf -> ...)` habilita la protección CSRF.
- `csrf.ignoringRequestMatchers("/api/public/**")` excluye las rutas que coinciden con el patrón `/api/public/**` de la protección CSRF. Esto puede ser útil para rutas públicas que no requieren autenticación.

Para incluir el token CSRF en las solicitudes desde el cliente, puedes usar una de las siguientes opciones:

- **Formularios HTML**: Spring Security automáticamente incluye el token CSRF en los formularios HTML generados con la biblioteca de etiquetas de Spring.
- **Cabecera HTTP**: Puedes obtener el token CSRF del `Cookie` llamado `XSRF-TOKEN` y agregarlo como una cabecera HTTP llamada `X-XSRF-TOKEN` en tus solicitudes AJAX.

**Nota**: En APIs REST sin estado que utilizan JWT, la protección CSRF es menos relevante, ya que el servidor no mantiene el estado de la sesión. Sin embargo, aún puede ser útil en algunos casos, especialmente si la API se utiliza en un contexto donde un atacante podría inyectar código malicioso en el cliente (por ejemplo, a través de un ataque XSS).

### 3.3. Protección CORS (Cross-Origin Resource Sharing)

**CORS** es un mecanismo de seguridad que restringe las solicitudes HTTP que un script web puede hacer a un dominio diferente al dominio desde el que se sirvió el script. Esto ayuda a prevenir ataques XSS (Cross-Site Scripting).

Si tu aplicación Spring Boot necesita recibir solicitudes de un dominio diferente, debes configurar CORS para permitir estas solicitudes. Puedes configurar CORS de varias maneras en Spring Security:

- **Anotación `@CrossOrigin`**: Puedes usar la anotación `@CrossOrigin` en tus controladores para permitir solicitudes desde dominios específicos.

```Java
package com.yourcompany.yourapp.controller;

import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@CrossOrigin(origins = "http://localhost:4200") // Permite solicitudes desde http://localhost:4200
public class CourseController {

    @GetMapping("/api/cursos")
    public String getCursos() {
        return "Lista de cursos";
    }
}
```

- **Configuración global en `SecurityFilterChain`**: Puedes configurar CORS globalmente en tu `SecurityFilterChain` creando un `CorsConfigurationSource` bean.

```Java
// filepath: src/main/java/com/yourcompany/yourapp/config/SecurityConfig.java
package com.yourcompany.yourapp.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

import java.util.Arrays;
import java.util.List;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    // ...

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .cors(cors -> cors.configurationSource(corsConfigurationSource()))
            // ...
        return http.build();
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(List.of("http://localhost:4200", "https://example.com")); // Dominios permitidos
        configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS")); // Métodos permitidos
        configuration.setAllowedHeaders(Arrays.asList("Authorization", "Content-Type", "X-Requested-With")); // Cabeceras permitidas
        configuration.setAllowCredentials(true); // Permitir credenciales (cookies, autorización)
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration); // Aplica la configuración a todas las rutas
        return source;
    }
}
```

- `cors(cors -> cors.configurationSource(corsConfigurationSource()))` habilita CORS y configura el `CorsConfigurationSource`.
- `CorsConfigurationSource` define los orígenes, métodos y encabezados permitidos.
- `setAllowedOrigins` especifica los dominios que pueden acceder a la API.
- `setAllowedMethods` especifica los métodos HTTP permitidos.
- `setAllowedHeaders` especifica los encabezados permitidos.
- `setAllowCredentials(true)` permite el envío de cookies y otra información de autenticación en las solicitudes CORS.

Es importante configurar CORS correctamente para evitar problemas de seguridad y permitir que tu aplicación funcione correctamente con otros dominios.

### 4. Rutas públicas vs. rutas protegidas

Dentro del método `securityFilterChain`, la parte más importante es cómo configuras el `authorizeHttpRequests`. Aquí es donde defines qué rutas requieren autenticación y/o autorización, y cuáles son accesibles públicamente.

- `authorizeHttpRequests(authz -> authz...)`: Es el método que inicia la configuración de autorización para las solicitudes HTTP.
- `requestMatchers()` / `antMatchers()`: Estos métodos se usan para especificar patrones de URL a los que se aplicarán las reglas de autorización.
  - `requestMatchers()`: Es la forma más moderna (Spring Security 6+).
  - `antMatchers()`: Es la forma más antigua, aún funcional pero `requestMatchers()` es preferible.
  - Puedes usar comodines: `*` para cualquier secuencia de caracteres, `**` para cualquier ruta en cualquier subdirectorio.
    - Ej: `"/api/public/**"` coincidiría con `/api/public/foo` y `/api/public/foo/bar`.
    - Ej: `"/api/cursos/*"` coincidiría con `/api/cursos/123` pero no `/api/cursos/list`.
- **Reglas de Acceso**:
  - `.permitAll()`: Permite el acceso sin necesidad de autenticación ni autorización. Son rutas "públicas".
  - `.authenticated()`: Requiere que el usuario esté autenticado (haya iniciado sesión), pero no impone requisitos de rol específicos.
  - `.hasRole("ROL_ESPECIFICO")`: Requiere que el usuario esté autenticado Y tenga el rol especificado. Spring Security automáticamente añade el prefijo `ROLE_` si usas `hasRole()`, así que si tu rol en la base de datos es `ADMIN`, en el código pones `hasRole("ADMIN")`.
  - `.hasAuthority("PERMISO_ESPECIFICO")`: Requiere que el usuario esté autenticado Y tenga la autoridad (permiso) especificada. No añade prefijo.
  - `.anyRequest()`: Un comodín que coincide con cualquier solicitud que no haya sido emparejada por reglas anteriores. Generalmente se coloca al final y se usa para definir una regla por defecto (ej., `.anyRequest().authenticated()` para proteger todo lo demás).

**Orden de las reglas**: Es crucial el orden en que defines las reglas. Spring Security evalúa las reglas de arriba hacia abajo. La primera regla que coincide con la URL se aplica. Por lo tanto, las reglas más específicas deben ir antes de las reglas más generales.

```Java
// Ejemplo de SecurityFilterChain con rutas públicas y protegidas
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .csrf(csrf -> csrf.disable()) // Deshabilitar CSRF para APIs sin sesiones (o manejarlo adecuadamente)
        .authorizeHttpRequests(authz -> authz
            // 1. Permitir acceso público a Swagger UI
            .requestMatchers("/v3/api-docs/**", "/swagger-ui/**", "/swagger-ui.html").permitAll()
            // 2. Permitir acceso público a una ruta específica, ej. /api/publicaciones
            .requestMatchers("/api/publicaciones/**").permitAll()
            // 3. Proteger las rutas de admin (solo usuarios con rol ADMIN)
            .requestMatchers("/api/admin/**").hasRole("ADMIN")
            // 4. Proteger las rutas de cursos (solo usuarios autenticados)
            .requestMatchers("/api/cursos/**").authenticated()
            // 5. Cualquier otra solicitud DEBE estar autenticada
            .anyRequest().authenticated()
        )
        // Configuración de autenticación (ej. HTTP Basic)
        .httpBasic(httpBasic -> {}); // Esto activa la autenticación HTTP Basic

    return http.build();
}
```

## 5. Autenticación de Usuarios con `UserDetailsService`

Para la autenticación, Spring Security necesita saber dónde y cómo obtener los detalles de los usuarios. Una forma sencilla de empezar, especialmente para entornos de desarrollo o demostraciones, es definir usuarios "en memoria". Esto se hace a través de un bean que provee una implementación de `UserDetailsService`.

La interfaz `UserDetailsService` tiene un único método: `UserDetails loadUserByUsername(String username)`. Este método es invocado por Spring Security cuando un usuario intenta iniciar sesión, y se espera que devuelva un objeto `UserDetails` que contenga el nombre de usuario, la contraseña (codificada) y los roles/autoridades del usuario.

### 5.1. Usuarios en Memoria

Para usuarios en memoria, Spring Security provee una implementación conveniente: `InMemoryUserDetailsManager`.

1. **Define un `PasswordEncoder`**: Las contraseñas **nunca** deben almacenarse ni compararse en texto plano. Siempre deben ser _hasheadas_. Spring Security requiere un `PasswordEncoder`. `BCryptPasswordEncoder` es una opción segura y muy común.

    ```Java
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
    import org.springframework.security.crypto.password.PasswordEncoder;

    @Configuration
    public class SecurityConfig { // Misma clase SecurityConfig de antes

        @Bean
        public PasswordEncoder passwordEncoder() {
            return new BCryptPasswordEncoder();
        }

        // ... resto del SecurityFilterChain y UserDetailsService
    }
    ```

2. Define el `UserDetailsService` (con usuarios en memoria):

    ```Java

    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.core.userdetails.User;
    import org.springframework.security.core.userdetails.UserDetails;
    import org.springframework.security.core.userdetails.UserDetailsService;
    import org.springframework.security.provisioning.InMemoryUserDetailsManager;
    import org.springframework.security.crypto.password.PasswordEncoder; // Importar PasswordEncoder

    @Configuration
    public class SecurityConfig {

        private final PasswordEncoder passwordEncoder; // Inyectar PasswordEncoder

        public SecurityConfig(PasswordEncoder passwordEncoder) {
            this.passwordEncoder = passwordEncoder;
        }

        // ... passwordEncoder bean

        @Bean
        public UserDetailsService userDetailsService() {
            // Contraseñas deben ser codificadas usando el PasswordEncoder
            UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder.encode("password")) // Codificar la contraseña
                .roles("USER") // Rol asignado
                .build();

            UserDetails admin = User.builder()
                .username("admin")
                .password(passwordEncoder.encode("adminpass")) // Codificar la contraseña
                .roles("ADMIN", "USER") // Múltiples roles
                .build();

            return new InMemoryUserDetailsManager(user, admin);
        }
    }
    ```

### 5.2. Usuarios en Base de Datos

Si bien los usuarios en memoria son geniales para pruebas rápidas, en una aplicación real, las credenciales de los usuarios se almacenan en una base de datos. Para integrar Spring Security con tu base de datos, necesitas implementar tu propio `UserDetailsService`.

1. **Crea la Entidad `User`**: Esta entidad representará a tus usuarios en la base de datos.

    ```Java
    package com.yourcompany.yourapp.entity;

    import jakarta.persistence.*;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    import lombok.AllArgsConstructor;
    import java.util.Set;

    @Entity
    @Table(name = "app_users") // 'user' es una palabra reservada en algunas DBs
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public class User {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;

        @Column(unique = true, nullable = false)
        private String username;

        @Column(nullable = false)
        private String password; // ¡Esta contraseña debe estar hasheada!

        // Roles o Autoridades. Puedes usar un String para roles simples
        // o una entidad separada para manejar roles más complejos (relación ManyToMany)
        // Ejemplo simple con String para roles:
        @ElementCollection(fetch = FetchType.EAGER) // Carga los roles inmediatamente
        @CollectionTable(name = "user_roles", joinColumns = @JoinColumn(name = "user_id"))
        @Column(name = "role")
        private Set<String> roles; // Ej. "ROLE_USER", "ROLE_ADMIN"
    }
    ```

2. **Crea el Repositorio `UserRepository`**: Una interfaz `JpaRepository` para interactuar con la entidad `User`.

    ```Java
    package com.yourcompany.yourapp.repository;

    import com.yourcompany.yourapp.entity.User;
    import org.springframework.data.jpa.repository.JpaRepository;
    import java.util.Optional;

    public interface UserRepository extends JpaRepository<User, Long> {
        Optional<User> findByUsername(String username); // Spring Data JPA lo implementa por ti
    }
    ```

3. **Crea tu Implementación Personalizada de `UserDetailsService`**:

    ```Java
    package com.yourcompany.yourapp.service.impl;

    import com.yourcompany.yourapp.entity.User;
    import com.yourcompany.yourapp.repository.UserRepository;
    import org.springframework.security.core.GrantedAuthority;
    import org.springframework.security.core.authority.SimpleGrantedAuthority;
    import org.springframework.security.core.userdetails.UserDetails;
    import org.springframework.security.core.userdetails.UserDetailsService;
    import org.springframework.security.core.userdetails.UsernameNotFoundException;
    import org.springframework.stereotype.Service;

    import java.util.Collection;
    import java.util.stream.Collectors;

    @Service // Marca esta clase como un componente de servicio de Spring
    public class CustomUserDetailsService implements UserDetailsService {

        private final UserRepository userRepository;

        public CustomUserDetailsService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }

        @Override
        public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
            // Busca el usuario en la base de datos por el nombre de usuario
            User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("Usuario no encontrado: " + username));

            // Convierte los roles del usuario de tu entidad a GrantedAuthority
            // Spring Security espera GrantedAuthority, SimpleGrantedAuthority es una implementación común.
            Collection<? extends GrantedAuthority> authorities = user.getRoles().stream()
                .map(role -> new SimpleGrantedAuthority(role)) // Asumiendo que los roles son "ROLE_USER", "ROLE_ADMIN" etc.
                .collect(Collectors.toList());

            // Construye y retorna un objeto UserDetails de Spring Security
            // La contraseña ya debe estar codificada en la DB
            return new org.springframework.security.core.userdetails.User(
                user.getUsername(),
                user.getPassword(),
                authorities
            );
        }
    }
    ```

4. **Integra tu `CustomUserDetailsService` en `SecurityConfig`**:

- Ya no necesitas el _bean_ `InMemoryUserDetailsManager`. Spring Security detectará automáticamente tu `CustomUserDetailsService` porque está anotado con `@Service`.
- Asegúrate de que tu `SecurityConfig` inyecte la `CustomUserDetailsService` si es necesario para alguna lógica, aunque a menudo Spring Security la detecta y la usa por defecto si es la única `UserDetailsService` disponible.

```Java
package com.yourcompany.yourapp.config;

import com.yourcompany.yourapp.service.impl.CustomUserDetailsService; // Importa tu servicio
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    private final CustomUserDetailsService customUserDetailsService; // Inyecta tu UserDetailsService
    // private final PasswordEncoder passwordEncoder; // Mantén la inyección si la necesitas directamente

    public SecurityConfig(CustomUserDetailsService customUserDetailsService) {
        this.customUserDetailsService = customUserDetailsService;
        // this.passwordEncoder = passwordEncoder; // Si inyectas PasswordEncoder también
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/v3/api-docs/**", "/swagger-ui/**", "/swagger-ui.html").permitAll()
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/cursos/**").authenticated()
                .anyRequest().authenticated()
            )
            .httpBasic(httpBasic -> {});

        // Spring Security automáticamente usará tu CustomUserDetailsService
        // debido a que es un bean de tipo UserDetailsService.
        // Si tuvieras múltiples UserDetailsService o quisieras ser explícito,
        // podrías configurar:
        // .userDetailsService(customUserDetailsService)

        return http.build();
    }

    // Ya no necesitas el bean de InMemoryUserDetailsManager aquí,
    // porque usaremos el CustomUserDetailsService.
}
```

**¡Importante!**

- Cuando crees usuarios en tu base de datos (ya sea manualmente o mediante un _script_ de inicialización), asegúrate de que sus contraseñas estén codificadas usando el `PasswordEncoder` que configuraste (ej., `BCryptPasswordEncoder`). Nunca almacenes contraseñas en texto plano.
- El prefijo `ROLE_` es una convención de Spring Security para roles cuando usas `hasRole()`. Si tus roles en la base de datos no tienen este prefijo, deberás añadirlo en tu `CustomUserDetailsService` al convertir los roles a `SimpleGrantedAuthority` (ej., `new SimpleGrantedAuthority("ROLE_" + role)`).

## 6. Protección a nivel de Método con `@PreAuthorize`

Además de proteger rutas completas a través de `SecurityFilterChain`, Spring Security ofrece un nivel más granular de control de acceso: la **seguridad a nivel de método**. Esto te permite aplicar reglas de autorización directamente en los métodos de tus servicios o controladores, lo cual es ideal cuando diferentes usuarios tienen permisos para realizar diferentes acciones dentro de una misma ruta.

Para habilitar la seguridad a nivel de método, debes agregar una anotación `@EnableMethodSecurity` en tu configuración de seguridad:

```Java
// Dentro de tu clase SecurityConfig
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity; // Importa esta anotación

@Configuration
@EnableWebSecurity
@EnableMethodSecurity // Habilita la seguridad a nivel de método
public class SecurityConfig {
    // ... resto de tu configuración de beans
}
```

Una vez habilitada, puedes usar anotaciones como `@PreAuthorize` directamente en tus métodos:

- `@PreAuthorize("hasRole('ADMIN')")`: Solo permite la ejecución del método si el usuario autenticado tiene el rol `ADMIN`.
- `@PreAuthorize("hasAnyRole('ADMIN', 'MANAGER')")`: Permite la ejecución si el usuario tiene cualquiera de los roles especificados.
- `@PreAuthorize("isAuthenticated()")`: Requiere que el usuario esté autenticado.
- `@PreAuthorize("hasAuthority('READ_PRIVILEGE')")`: Requiere una autoridad (permiso) específica.
- `@PreAuthorize("principal.username == #username")`: Una expresión SpEL (Spring Expression Language) que permite lógica más compleja, como verificar si el nombre de usuario del principal autenticado coincide con un argumento del método.

```Java
package com.yourcompany.yourapp.service;

import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Service;
// ... otras importaciones

@Service
public class CourseService {

    // Cualquier usuario autenticado puede ver todos los cursos
    public List<Course> getAllCourses() {
        // ... lógica para obtener todos los cursos
        return Collections.emptyList(); // Ejemplo
    }

    // Solo usuarios con rol ADMIN pueden añadir nuevos cursos
    @PreAuthorize("hasRole('ADMIN')")
    public Course addCourse(Course course) {
        // ... lógica para guardar un curso
        return course; // Ejemplo
    }

    // Solo usuarios autenticados que sean ADMIN pueden eliminar un curso
    // O si el usuario autenticado tiene el rol 'MANAGER' y el id del curso es par (ejemplo complejo)
    @PreAuthorize("hasRole('ADMIN') or (hasRole('MANAGER') and #id % 2 == 0)")
    public void deleteCourse(Long id) {
        // ... lógica para eliminar un curso
    }

    // Solo un usuario puede ver su propio perfil de usuario
    @PreAuthorize("principal.username == #username")
    public UserDetails getUserProfile(String username) {
        // ... lógica para obtener perfil de usuario
        return null; // Ejemplo
    }
}
```

La seguridad a nivel de método complementa la protección a nivel de URL de `SecurityFilterChain`, permitiéndote aplicar reglas de acceso muy precisas a la lógica de negocio de tu aplicación.

## 7. Manejo de Errores de Autorización

Cuando un usuario intenta acceder a un recurso para el cual no tiene los permisos necesarios, Spring Security lanza una excepción `AccessDeniedException`. Es importante manejar esta excepción para proporcionar una respuesta adecuada al usuario, en lugar de mostrar una página de error genérica.

Para personalizar el manejo de `AccessDeniedException`, puedes crear un `ExceptionHandler` global utilizando la anotación `@ControllerAdvice`. Este `ExceptionHandler` capturará la excepción y te permitirá definir la respuesta que se enviará al cliente.

```Java
package com.yourcompany.yourapp.config;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class RestExceptionHandler {

    @ExceptionHandler(AccessDeniedException.class)
    public ResponseEntity<ApiErrorResponse> handleAccessDeniedException(AccessDeniedException ex, HttpServletRequest request) {
        ApiErrorResponse errorResponse = new ApiErrorResponse(
            HttpStatus.FORBIDDEN, // Código 403
            "Acceso denegado: No tienes permiso para acceder a este recurso.",
            request.getRequestURI()
        );
        return new ResponseEntity<>(errorResponse, HttpStatus.FORBIDDEN);
    }
    // Otros ExceptionHandlers...
}
```

## 8. Ejercicio Práctico: Protección de una API de Cursos

**Objetivo**: Proteger una API RESTful de gestión de cursos para que solo usuarios autenticados puedan acceder a sus endpoints. Además, se definirá una ruta pública para una "página de inicio" simulada.

**Requisitos**:

1. **Dependencias**: Asegúrate de tener `spring-boot-starter-web`, `spring-boot-starter-security`, `spring-boot-starter-validation`, `org.postgresql:postgresql` y `lombok`.
2. **Configuración de Base de Datos**: Asegúrate de tener `application.properties` configurado para PostgreSQL.
3. **API de Cursos (Mínima)**:
    - Crea una **Entidad** `Course` (con ID, título, descripción).
    - Crea un `CourseRepository` (extiende `JpaRepository`).
    - Crea un `CourseService` y su implementacion con métodos `getAllCourses()`, `getCourseById()`, `addCourse()` y `deleteCourse()`.
    - Crea un `CourseController` con endpoints `GET /api/cursos`, `GET /api/cursos/{id}`, `POST /api/cursos` y `DELETE /api/cursos/{id}`.
4. **Gestión de Usuarios (Base de Datos)**:
    - Crea una **Entidad** `User` como se mostró en la sección 5.3, incluyendo `username`, `password` (hasheado) y `roles`.
    - Crea un `UserRepository` como se mostró en la sección 5.3.
    - Crea tu `CustomUserDetailsService` como se mostró en la sección 5.3.
    - **Inicializa la base de datos**: Inserta al menos un usuario `user` con rol `USER` y un usuario `admin` con rol `ADMIN` (y `USER` si lo deseas) en tu tabla `app_users` (con las contraseñas hasheadas usando `BCryptPasswordEncoder`). Puedes usar un _script_ `data.sql` o insertar directamente.
5. **Configuración de Seguridad**:
    - Crea una clase SecurityConfig anotada con `@Configuration`, `@EnableWebSecurity` y `@EnableMethodSecurity`.
    - Define un `PasswordEncoder` (`BCryptPasswordEncoder`) como un _bean_.
    - Tu `CustomUserDetailsService` (`@Service`) será detectado y usado automáticamente.
    - Configura el `SecurityFilterChain` para:
      - Deshabilitar CSRF para simplificar las pruebas con clientes REST (`.csrf(csrf -> csrf.disable())`).
      - Habilitar la autenticación HTTP Basic (`.httpBasic(httpBasic -> {});`).
      - Permitir acceso público a:
        - `/public/home` (ruta de "bienvenida" simulada).
        - `/v3/api-docs/**`, `/swagger-ui/**`, `/swagger-ui.html` (para acceso a Swagger UI).
      - Proteger todas las rutas bajo `/api/cursos/**` para que solo sean accesibles por usuarios **autenticados**.
      - Cualquier otra solicitud debe estar **autenticada**.
6. **Protección a Nivel de Método (`@PreAuthorize`)**:
    - En tu `CourseService` o `CourseController`, usa `@PreAuthorize`:
      - El método `addCourse()` solo debe ser accesible por usuarios con rol `ADMIN`.
      - El método `deleteCourse()` solo debe ser accesible por usuarios con rol `ADMIN`.
      - Los métodos `getAllCourses()` y `getCourseById()` deben ser accesibles por cualquier usuario `authenticated()`.
7. **Endpoint de Prueba Pública**:
    - Crea un `HomeController` simple con un _endpoint_ `GET /public/home` que retorne un `String` "Bienvenido a la página pública!".

**Pasos para la Prueba**:

1. Inicia la aplicación Spring Boot.
2. Accede a `http://localhost:8080/public/home` en tu navegador. Deberías poder verlo sin necesidad de login.
3. Accede a `http://localhost:8080/api/cursos` en tu navegador. Deberías ser redirigido a una página de login o se te pedirá credenciales.
4. Usa un cliente REST (Postman/Insomnia) o `curl`:
    - Intenta `GET http://localhost:8080/api/cursos` sin autenticación. Deberías recibir un `401 Unauthorized`.
    - Intenta `GET http://localhost:8080/api/cursos` con autenticación HTTP Basic usando `user/password`. Deberías recibir un `200 OK` con la lista de cursos.
    - Intenta `POST http://localhost:8080/api/cursos` con `user/password`. Deberías recibir un `403 Forbidden` debido al `@PreAuthorize`.
    - Intenta `POST http://localhost:8080/api/cursos` con `admin/adminpass`. Deberías recibir un `201 Created` (o el código de éxito que configures) si los datos son válidos.
    - Intenta `DELETE http://localhost:8080/api/cursos/{id}` con `user/password`. Deberías recibir un `403 Forbidden`.
    - Intenta `DELETE http://localhost:8080/api/cursos/{id}` con `admin/adminpass`. Deberías poder eliminar el curso.
    - Accede a `http://localhost:8080/swagger-ui.html`. Debería ser accesible sin login. Intenta probar los _endpoints_ desde Swagger, usando la funcionalidad de "Authorize" para introducir las credenciales Basic Auth y observar los resultados de `401` y `403`.
