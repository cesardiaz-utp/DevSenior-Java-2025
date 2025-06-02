# Ejercicio Autónomo

## Objetivos de Aprendizaje

Al finalizar este ejercicio autoguiado, serás capaz de:

- **Configurar un proyecto Spring Boot** desde cero para implementar la autenticación JWT.
- **Implementar los componentes clave de JWT** (generación, validación, filtros de seguridad) de forma autónoma.
- **Proteger endpoints de API** utilizando JWT y Spring Security.
- **Realizar pruebas exhaustivas** para verificar el flujo de autenticación y la protección de rutas.
- **Identificar y resolver problemas comunes** relacionados con la implementación de JWT.

## Paso 1: Configuración Inicial del Proyecto Spring Boot

1. Crear un Nuevo Proyecto Spring Boot y añadir Dependencias:
    - Spring Security
    - Spring Web
    - Lombok (opcional, pero muy recomendado para reducir código boilerplate)
    - Spring Boot DevTools (opcional, útil para reinicios rápidos durante el desarrollo)

2. Añadir Dependencias de JJWT:
    - Abre el archivo `pom.xml` (si usas Maven) en la raíz de tu proyecto.
    - Localiza la sección `<dependencies>` y añade las siguientes dependencias de JJWT. Asegúrate de usar la versión 0.12.6 o la última versión compatible.

      ```XML
      <dependency>
          <groupId>io.jsonwebtoken</groupId>
          <artifactId>jjwt-api</artifactId>
          <version>0.12.6</version>
      </dependency>
      <dependency>
          <groupId>io.jsonwebtoken</groupId>
          <artifactId>jjwt-impl</artifactId>
          <version>0.12.6</version>
          <scope>runtime</scope>
      </dependency>
      <dependency>
          <groupId>io.jsonwebtoken</groupId>
          <artifactId>jjwt-jackson</artifactId>
          <version>0.12.6</version>
          <scope>runtime</scope>
      </dependency>
      ```

    - Guarda el archivo y permite que tu IDE resuelva las nuevas dependencias (Maven/Gradle sync).

## Paso 2: Configuración de la Clave Secreta y Expiración del JWT

1. **Abrir `application.properties`**: Navega hasta `src/main/resources/application.properties` en tu proyecto.
2. **Añadir Propiedades JWT**:
    - Añade las siguientes líneas. **Es CRÍTICO que generes una clave secreta fuerte y única**. No uses la clave de ejemplo en producción. Puedes buscar en línea "generate base64 string" o "generate secret key for JWT" para obtener una cadena aleatoria de al menos 32 caracteres (para HS256).

      ```properties
      # La clave secreta para firmar y verificar los JWTs. Debe ser larga, aleatoria y confidencial.
      jwt.secret=TuClaveSecretaSuperSeguraParaJWTQueDebeSerLargaYAleatoria1234567890ABCDEFGHIKLMNOPQRSTUVWXY
      # Tiempo de expiración del token en milisegundos. 86400000 ms = 24 horas.
      # Para pruebas, puedes reducirlo a 60000 (1 minuto) para verificar la expiración más rápido.
      jwt.expiration=86400000
      ```

    - Guarda el archivo.

## Paso 3: Creación de los Modelos de Solicitud y Respuesta

1. **Crear Paquete `model`**:
    - En `src/main/java/com/tuempresa/securityjwtdemo` (o tu paquete base), crea un nuevo paquete llamado `model.dto`.
2. **Crear `AuthenticationRequest.java`**:
    - Dentro del paquete `model`, crea un nuevo archivo `AuthenticationRequest.java` y copia el siguiente código:

      ```Java
      package com.tuempresa.securityjwtdemo.model.dto; // Asegúrate de que el paquete coincida con el tuyo

      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;

      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class AuthenticationRequest {
          private String username;
          private String password;
      }
      ```

3. **Crear `AuthenticationResponse.java`**:
    - Dentro del paquete `model.dto`, crea un nuevo archivo `AuthenticationResponse.java` y copia el siguiente código:

      ```Java
      package com.tuempresa.securityjwtdemo.model.dto; // Asegúrate de que el paquete coincida con el tuyo

      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;

      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class AuthenticationResponse {
          private String jwt;
      }
      ```

    - Guarda ambos archivos.

## Paso 4: Implementación de la Utilidad JWT (`JwtUtil`)

1. **Crear Paquete `util`**:
    - En tu paquete base, crea un nuevo paquete llamado `util`.
2. **Crear `JwtUtil.java`**:
    - Dentro del paquete `util`, crea un nuevo archivo `JwtUtil.java` y copia el siguiente código. **Asegúrate de que el package declarado en la parte superior del archivo coincida con la ruta de tu proyecto**.

      ```Java
      package com.tuempresa.securityjwtdemo.util; // Asegúrate de que el paquete coincida con el tuyo

      import io.jsonwebtoken.Claims;
      import io.jsonwebtoken.Jwts;
      import io.jsonwebtoken.SignatureAlgorithm;
      import io.jsonwebtoken.io.Decoders;
      import io.jsonwebtoken.security.Keys;
      import org.springframework.beans.factory.annotation.Value;
      import org.springframework.security.core.userdetails.UserDetails;
      import org.springframework.stereotype.Component;

      import java.security.Key;
      import java.util.Date;
      import java.util.HashMap;
      import java.util.Map;
      import java.util.function.Function;

      @Component
      public class JwtUtil {

          @Value("${jwt.secret}")
          private String secret;

          @Value("${jwt.expiration}")
          private long expiration;

          private Key getSignInKey() {
              byte[] keyBytes = Decoders.BASE64.decode(secret);
              return Keys.hmacShaKeyFor(keyBytes);
          }

          public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
              final Claims claims = extractAllClaims(token);
              return claimsResolver.apply(claims);
          }

          private Claims extractAllClaims(String token) {
              return Jwts.parserBuilder()
                      .setSigningKey(getSignInKey())
                      .build()
                      .parseClaimsJws(token)
                      .getBody();
          }

          public String extractUsername(String token) {
              return extractClaim(token, Claims::getSubject);
          }

          public Date extractExpiration(String token) {
              return extractClaim(token, Claims::getExpiration);
          }

          private Boolean isTokenExpired(String token) {
              return extractExpiration(token).before(new Date());
          }

          public Boolean validateToken(String token, UserDetails userDetails) {
              final String username = extractUsername(token);
              return (username.equals(userDetails.getUsername()) && !isTokenExpired(token));
          }

          public String generateToken(UserDetails userDetails) {
              Map<String, Object> claims = new HashMap<>();
              return createToken(claims, userDetails.getUsername());
          }

          private String createToken(Map<String, Object> claims, String subject) {
              return Jwts.builder()
                      .setClaims(claims)
                      .setSubject(subject)
                      .setIssuedAt(new Date(System.currentTimeMillis()))
                      .setExpiration(new Date(System.currentTimeMillis() + expiration))
                      .signWith(getSignInKey(), SignatureAlgorithm.HS256)
                      .compact();
          }
      }
      ```

    - Guarda el archivo.

## Paso 5: Creación del Servicio de Detalles de Usuario (`MyUserDetailsService`)

1. **Crear Paquete `service.impl`**:
    - En tu paquete base, crea un nuevo paquete llamado `service.impl`.
2. **Crear `MyUserDetailsService.java`**:
    - Dentro del paquete `service.impl`, crea un nuevo archivo `MyUserDetailsService.java` y copia el siguiente código. Asegúrate de que el package declarado coincida con la ruta de tu proyecto.

      ```Java
      package com.tuempresa.securityjwtdemo.service.impl; // Asegúrate de que el paquete coincida con el tuyo

      import org.springframework.security.core.userdetails.User;
      import org.springframework.security.core.userdetails.UserDetails;
      import org.springframework.security.core.userdetails.UserDetailsService;
      import org.springframework.security.core.userdetails.UsernameNotFoundException;
      import org.springframework.stereotype.Service;

      import java.util.ArrayList;

      @Service
      public class MyUserDetailsService implements UserDetailsService {

          @Override
          public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
              if ("user".equals(username)) {
                  // Contraseña "password" codificada con BCrypt.
                  // Genera tu propia contraseña BCrypt usando BCryptPasswordEncoder().encode("tu_password").
                  return new User("user", "$2a$10$SlYQmyglfiGw/YgE/M7F7.y0Uq0Xk0K0Xk0K0Xk0K0Xk0K0Xk0K", new ArrayList<>());
              } else {
                  throw new UsernameNotFoundException("Usuario no encontrado: " + username);
              }
          }
      }
      ```

    - Guarda el archivo.

## Paso 6: Implementación del Filtro de Solicitudes JWT (`JwtRequestFilter`)

1. **Crear Paquete `config.filter`**:
    - En tu paquete base, crea un nuevo paquete llamado `config.filter`.
2. **Crear `JwtRequestFilter.java`**:
    - Dentro del paquete `config.filter`, crea un nuevo archivo `JwtRequestFilter.java` y copia el siguiente código. Asegúrate de que el package declarado coincida con la ruta de tu proyecto.

      ```Java
      package com.tuempresa.securityjwtdemo.config.filter; // Asegúrate de que el paquete coincida con el tuyo

      import com.tuempresa.securityjwtdemo.util.JwtUtil; // Asegúrate de que el paquete coincida con el tuyo
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
      import org.springframework.security.core.context.SecurityContextHolder;
      import org.springframework.security.core.userdetails.UserDetails;
      import org.springframework.security.core.userdetails.UserDetailsService;
      import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
      import org.springframework.stereotype.Component;
      import org.springframework.web.filter.OncePerRequestFilter;

      import javax.servlet.FilterChain;
      import javax.servlet.ServletException;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import java.io.IOException;

      @Component
      public class JwtRequestFilter extends OncePerRequestFilter {

          @Autowired
          private UserDetailsService userDetailsService;

          @Autowired
          private JwtUtil jwtUtil;

          @Override
          protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
                  throws ServletException, IOException {

              final String authorizationHeader = request.getHeader("Authorization");

              String username = null;
              String jwt = null;

              if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
                  jwt = authorizationHeader.substring(7);
                  try {
                      username = jwtUtil.extractUsername(jwt);
                  } catch (Exception e) {
                      System.err.println("Error al extraer username del JWT: " + e.getMessage());
                  }
              }

              if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {

                  UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);

                  if (jwtUtil.validateToken(jwt, userDetails)) {

                      UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken =
                              new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                      usernamePasswordAuthenticationToken
                              .setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                      SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);
                  }
              }
              chain.doFilter(request, response);
          }
      }
      ```

    - Guarda el archivo.

## Paso 7: Configuración de Seguridad de Spring (`SecurityConfig`)

1. **Crear `SecurityConfig.java`**:
    - Dentro del paquete `config`, crea un nuevo archivo `SecurityConfig.java` y copia el siguiente código. Asegúrate de que los `import` de `JwtUtil` y `JwtRequestFilter` coincidan con la ruta de tus paquetes.

      ```Java
      package com.tuempresa.securityjwtdemo.config; // Asegúrate de que el paquete coincida con el tuyo

      import com.tuempresa.securityjwtdemo.filter.JwtRequestFilter; // Asegúrate de que el paquete coincida con el tuyo
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.context.annotation.Bean;
      import org.springframework.security.authentication.AuthenticationManager;
      import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
      import org.springframework.security.config.annotation.web.builders.HttpSecurity;
      import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
      import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
      import org.springframework.security.config.http.SessionCreationPolicy;
      import org.springframework.security.core.userdetails.UserDetailsService;
      import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
      import org.springframework.security.crypto.password.PasswordEncoder;
      import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

      @EnableWebSecurity
      public class SecurityConfig extends WebSecurityConfigurerAdapter {

          @Autowired
          private UserDetailsService userDetailsService;

          @Autowired
          private JwtRequestFilter jwtRequestFilter;

          @Override
          protected void configure(AuthenticationManagerBuilder auth) throws Exception {
              auth.userDetailsService(userDetailsService)
                  .passwordEncoder(passwordEncoder());
          }

          @Override
          protected void configure(HttpSecurity http) throws Exception {
              http.csrf().disable()
                  .authorizeRequests()
                  .antMatchers("/authenticate").permitAll()
                  .anyRequest().authenticated()
                  .and().sessionManagement()
                  .sessionCreationPolicy(SessionCreationPolicy.STATELESS);

              http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
          }

          @Bean
          public PasswordEncoder passwordEncoder() {
              return new BCryptPasswordEncoder();
          }

          @Override
          @Bean
          public AuthenticationManager authenticationManagerBean() throws Exception {
              return super.authenticationManagerBean();
          }
      }
      ```

    - Guarda el archivo.

## Paso 8: Creación del Controlador de Autenticación (`AuthController`)

1. **Crear Paquete `controller`**:
    - En tu paquete base, crea un nuevo paquete llamado `controller`.
2. **Crear `AuthController.java`**:
    - Dentro del paquete `controller`, crea un nuevo archivo `AuthController.java` y copia el siguiente código. Asegúrate de que los `import` de tus modelos y `JwtUtil` coincidan con la ruta de tus paquetes.

      ```Java
      package com.tuempresa.securityjwtdemo.controller; // Asegúrate de que el paquete coincida con el tuyo

      import com.tuempresa.securityjwtdemo.model.dto.AuthenticationRequest; // Asegúrate de que el paquete coincida con el tuyo
      import com.tuempresa.securityjwtdemo.model.dto.AuthenticationResponse; // Asegúrate de que el paquete coincida con el tuyo
      import com.tuempresa.securityjwtdemo.util.JwtUtil; // Asegúrate de que el paquete coincida con el tuyo
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.http.ResponseEntity;
      import org.springframework.security.authentication.AuthenticationManager;
      import org.springframework.security.authentication.BadCredentialsException;
      import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
      import org.springframework.security.core.userdetails.UserDetails;
      import org.springframework.security.core.userdetails.UserDetailsService;
      import org.springframework.web.bind.annotation.PostMapping;
      import org.springframework.web.bind.annotation.RequestBody;
      import org.springframework.web.bind.annotation.RestController;

      @RestController
      public class AuthController {

          @Autowired
          private AuthenticationManager authenticationManager;

          @Autowired
          private UserDetailsService userDetailsService;

          @Autowired
          private JwtUtil jwtUtil;

          @PostMapping("/authenticate")
          public ResponseEntity<?> createAuthenticationToken(@RequestBody AuthenticationRequest authenticationRequest) throws Exception {
              try {
                  authenticationManager.authenticate(
                          new UsernamePasswordAuthenticationToken(authenticationRequest.getUsername(), authenticationRequest.getPassword())
                  );
              } catch (BadCredentialsException e) {
                  throw new Exception("Credenciales incorrectas", e);
              }

              final UserDetails userDetails = userDetailsService.loadUserByUsername(authenticationRequest.getUsername());
              final String jwt = jwtUtil.generateToken(userDetails);

              return ResponseEntity.ok(new AuthenticationResponse(jwt));
          }
      }
      ```

    - Guarda el archivo.

## Paso 9: Creación del Controlador de Prueba (`HelloController`)

1. **Crear `HelloController.java`**:
    - Dentro del mismo paquete `controller`, crea un nuevo archivo `HelloController.java` y copia el siguiente código. Asegúrate de que el `package` declarado coincida con la ruta de tu proyecto.

      ```Java
      package com.tuempresa.securityjwtdemo.controller; // Asegúrate de que el paquete coincida con el tuyo

      import org.springframework.web.bind.annotation.GetMapping;
      import org.springframework.web.bind.annotation.RestController;

      @RestController
      public class HelloController {

          @GetMapping("/hello")
          public String hello() {
              return "Hola, ¡has accedido a un recurso protegido con JWT!";
          }
      }
      ```

    - Guarda el archivo.

## Paso 10: Prueba Completa de la Aplicación

1. **Iniciar la Aplicación Spring Boot**:
    - Abre la clase principal de tu aplicación (ej., `SecurityJwtDemoApplication.java` en tu paquete base).
    - Ejecuta el método `main` haciendo clic en el botón de "Run" o ejecutando desde la terminal: `mvn spring-boot:run` (para Maven).
    - Verifica que la aplicación se inicie sin errores en la consola. Deberías ver mensajes como "Started SecurityJwtDemoApplication in X seconds".
2. **Utilizar un Cliente HTTP (Postman/Insomnia/curl)**:
    - **Paso 1: Obtener el Token de Autenticación (`/authenticate`)**
      - Abre Postman, Insomnia o tu cliente HTTP preferido.
      - Crea una nueva solicitud POST.
      - **URL**: `http://localhost:8080/authenticate`
      - **Headers**: Añade el encabezado `Content-Type` con el valor `application/json`.
      - **Body**: Selecciona `raw` y `JSON`, y pega las credenciales:

        ```JSON
        {
            "username": "user",
            "password": "password"
        }
        ```

      - Envía la solicitud.
      - **Verificación**: Deberías recibir una respuesta con un código de estado `200 OK` y un cuerpo JSON similar a:

        ```JSON
        {
          "jwt": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VyIiwiaWF0IjoxNjc4ODg2NDAwLCJleHAiOjE2Nzg4OTAwMDB9.some_signature_here"
        }
        ```

      - Copia este token completo (la cadena larga después de "jwt":).
    - **Paso 2: Acceder a un Recurso Protegido con el Token (`/hello`)**
      - Crea una nueva solicitud **GET**.
      - **URL**: `http://localhost:8080/hello`
      - **Headers**: Añade un nuevo encabezado:
        - **Key**: `Authorization`
        - **Value**: `Bearer <Pega_Aquí_Tu_JWT_Obtenido>` (Asegúrate de incluir `Bearer` - con un espacio al final - antes de pegar el token que copiaste en el paso anterior).
      - Envía la solicitud.
      - **Verificación**: Deberías recibir una respuesta con un código de estado `200 OK` y el mensaje: "Hola, ¡has accedido a un recurso protegido con JWT!".
    - **Paso 3: Prueba de Acceso Denegado (Sin Token)**
      - Crea una nueva solicitud **GET**.
      - **URL**: `http://localhost:8080/hello`
      - **Headers**: No incluyas el encabezado `Authorization`.
      - Envía la solicitud.
      - **Verificación**: Deberías recibir una respuesta con un código de estado `401 Unauthorized`.
    - **Paso 4: Prueba de Acceso Denegado (Token Inválido/Expirado)**
      - Crea una nueva solicitud **GET**.
      - **URL**: `http://localhost:8080/hello`
      - **Headers**: Añade el encabezado `Authorization`, pero:
        - **Opción A (Token Malformado)**: Pega un JWT válido, pero cambia un par de caracteres al azar en la parte del `payload` o `signature`.
        - **Opción B (Token Expirado)**: Si configuraste `jwt.expiration` a un valor bajo (ej., 60000 ms = 1 minuto), espera a que el token expire y luego úsalo.
      - Envía la solicitud.
      - **Verificación**: Deberías recibir una respuesta con un código de estado `401 Unauthorized` o `403 Forbidden`.

## Criterios de Autoevaluación (Autónoma)

Utiliza esta lista de verificación para evaluar tu propia implementación y asegurarte de que has comprendido y aplicado correctamente los conceptos de JWT.

- ¿Tu aplicación Spring Boot se inicia sin errores en la consola?
- ¿Puedes enviar una solicitud POST a `/authenticate` con `username: user` y `password: password` y recibir un JWT válido en la respuesta?
- ¿El JWT recibido tiene las tres partes (Header, Payload, Signature) separadas por puntos?
- ¿Puedes pegar tu JWT en `https://jwt.io/` y ver las claims (`sub`, `iat`, `exp`) correctamente decodificadas?
- ¿Puedes enviar una solicitud GET a `/hello` incluyendo el JWT en el encabezado `Authorization: Bearer <tu_token>` y recibir el mensaje "Hola, ¡has accedido a un recurso protegido con JWT!"?
- ¿Al intentar acceder a `/hello` sin el encabezado Authorization, recibes un `401 Unauthorized`?
- ¿Al intentar acceder a `/hello` con un token malformado o expirado, recibes un `401 Unauthorized` o `403 Forbidden`?
- ¿Todas tus clases (`AuthenticationRequest`, `AuthenticationResponse`, `JwtUtil`, `MyUserDetailsService`, `JwtRequestFilter`, `SecurityConfig`, `AuthController`, `HelloController`) están en los paquetes correctos (`model`, `util`, `service.impl`, `config.filter`, `config`, `controller`)?
- ¿Has añadido todas las dependencias necesarias en tu `pom.xml` para Spring Security y JJWT?
- ¿Tu clave secreta (`jwt.secret`) en `application.properties` es larga y compleja, y no está hardcodeada directamente en el código Java?
- ¿El `JwtUtil` contiene métodos claros para generar, extraer y validar tokens?
- ¿El `JwtRequestFilter` intercepta correctamente las solicitudes y utiliza el `JwtUtil` para validar el token antes de establecer el contexto de seguridad?
- ¿Tu `SecurityConfig` está configurado para ser `STATELESS` y deshabilita `CSRF`, y permite el acceso a `/authenticate` mientras protege otras rutas?
- ¿El `MyUserDetailsService` carga los detalles del usuario y la contraseña está codificada con `BCrypt`?
- ¿El `AuthController` utiliza `AuthenticationManager` para la autenticación inicial y luego `JwtUtil` para generar el token?
- ¿Comprendes por qué `localStorage` no es el lugar más seguro para almacenar JWTs y cuáles son las alternativas?
- ¿Entiendes la importancia de la expiración del token y el concepto de refresh tokens (aunque no se implementen en este ejercicio)?
- ¿Sabes por qué la clave secreta nunca debe ser compartida o expuesta públicamente?
- ¿Tienes claro que el payload de un JWT no está cifrado y, por lo tanto, no debe contener información sensible?

Si has marcado la mayoría de los puntos con un "sí", ¡excelente trabajo! Has logrado una comprensión sólida e implementación funcional de la autenticación JWT. Si encuentras dificultades en algún punto, revisa la sección de "Contenido" y las "Pistas/Ayudas" del ejercicio.
