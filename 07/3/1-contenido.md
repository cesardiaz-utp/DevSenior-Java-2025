# Clase 3: Implementación de Autenticación con JWT (JSON Web Tokens)

¡Hola a todos!
¡Bienvenidos a la clase 3 del Módulo 7! En esta etapa de su aprendizaje, no solo construimos APIs que funcionan, sino que las elevamos a un nivel profesional: **APIs robustas, seguras y confiables**. Esta clase es fundamental, ya que nos sumergiremos en cómo implementar la autenticación con JWT, un pilar para asegurar que solo los usuarios autorizados interactúen con nuestra API.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Integrar JWT** como mecanismo de autenticación sin sesiones en aplicaciones Spring Boot.
- **Implementar la generación, validación y uso de tokens** en APIs RESTful, comprendiendo el ciclo de vida completo de un JWT.

## 1. Configuración Inicial

Para comenzar con la implementación de **JSON Web Tokens** (JWT) en nuestras aplicaciones Spring Boot, es esencial establecer una base sólida configurando las dependencias necesarias en nuestro proyecto y definiendo una clave secreta robusta que será fundamental para la seguridad de nuestros tokens.

### 1.1. Dependencias (`pom.xml`)

La gestión de dependencias en un proyecto Maven (usando `pom.xml`) es crucial. Asegúrate de incluir las siguientes bibliotecas, que son los pilares para la funcionalidad de seguridad de Spring y la manipulación de JWTs:

```XML
<dependencies>
    <!-- Spring Boot Starter Security: Esta dependencia es el corazón de la seguridad en Spring Boot.
         Proporciona todas las funcionalidades básicas y avanzadas para autenticación y autorización,
         incluyendo filtros de seguridad, gestores de autenticación y soporte para UserDetailsService. -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <!-- Spring Boot Starter Web: Esencial para construir aplicaciones web y APIs RESTful.
         Incluye Spring MVC, Tomcat como servidor embebido y otras utilidades para el desarrollo web. -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- JJWT - Java JWT API: Esta es la interfaz principal de la biblioteca Java JWT (jjwt).
         Define las clases y métodos para trabajar con JWTs, como la creación, el parseo y la verificación. -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-api</artifactId>
        <version>0.12.6</version>
    </dependency>
    <!-- JJWT - Java JWT Implementation: Contiene la implementación concreta de la API de JJWT.
         Es necesaria en tiempo de ejecución para que las operaciones con JWTs puedan llevarse a cabo. -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-impl</artifactId>
        <version>0.12.6</version>
        <scope>runtime</scope>
    </dependency>
    <!-- JJWT - Jackson Integration: Proporciona la integración de JJWT con la biblioteca Jackson,
         que es la encargada de la serialización y deserialización de objetos JSON en Java.
         Es fundamental para que JJWT pueda manejar los payloads JSON de los tokens. -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-jackson</artifactId>
        <version>0.12.6</version>
        <scope>runtime</scope>
    </dependency>
    <!-- Spring Boot Starter Test: Incluye dependencias para escribir pruebas unitarias e de integración,
         como JUnit, Mockito y AssertJ, esenciales para garantizar la calidad del código. -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <!-- Spring Security Test: Proporciona utilidades específicas para facilitar las pruebas
         de componentes protegidos por Spring Security, permitiendo simular usuarios autenticados. -->
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 1.2. Clave Secreta para JWT

La **clave secreta** es el elemento de seguridad más crítico en la implementación de JWTs que utilizan algoritmos de firma simétricos (como HS256). Esta clave es la base criptográfica que permite al servidor firmar los tokens que emite y, posteriormente, verificar la autenticidad e integridad de los tokens que recibe.

#### Características de una Clave Secreta Segura

- **Longitud Suficiente**: Para el algoritmo HS256, se recomienda una clave de al menos 256 bits (32 bytes). Una clave más corta es más vulnerable a ataques de fuerza bruta.
- **Aleatoriedad y Complejidad**: Debe ser una cadena de bytes generada aleatoriamente, sin patrones predecibles. Evita usar cadenas simples o fácilmente adivinables.
- **Confidencialidad Absoluta**: **¡Nunca debe ser expuesta públicamente!** Esto incluye no hardcodearla directamente en el código fuente, no subirla a repositorios de código públicos (como GitHub) ni mostrarla en logs o mensajes de error. Si esta clave se ve comprometida, un atacante podría generar tokens válidos, suplantando a cualquier usuario.

#### Estrategias de Almacenamiento Seguro

Se recomienda encarecidamente almacenar la clave secreta en un lugar seguro y accesible solo por la aplicación en tiempo de ejecución:

- **`application.properties` o `application.yml` (para desarrollo local y pruebas)**: Para entornos de desarrollo, puedes definirla aquí. Sin embargo, **nunca uses esta práctica en producción**.

    ```properties
    # application.properties
    # Una clave secreta fuerte debe ser al menos de 256 bits (32 bytes) para HS256.
    # Genera una aleatoria y segura, por ejemplo, usando un generador de claves Base64.
    jwt.secret=UnaClaveSecretaMuyLargaYSeguraQueNadieDebeConocer1234567890ABCDEFGHIKLMNOPQRSTUVWXY
    jwt.expiration=86400000 # 24 horas en milisegundos (86,400,000 ms = 24 * 60 * 60 * 1000)
    ```

- **Variables de Entorno (para producción)**: Esta es la forma más común y segura para entornos de producción. La clave se inyecta en la aplicación como una variable de entorno del sistema operativo donde se ejecuta la aplicación.

    ```bash
    export JWT_SECRET="UnaClaveSecretaMuyLargaYSeguraQueNadieDebeConocer1234567890ABCDEFGHIKLMNOPQRSTUVWXY"
    ```

  Y en `application.properties`: `jwt.secret=${JWT_SECRET}`

- **Servicios de Gestión de Secretos**: Para aplicaciones empresariales y entornos de nube, se utilizan servicios dedicados como HashiCorp Vault, AWS Secrets Manager, Azure Key Vault o Google Secret Manager. Estos servicios gestionan, almacenan y rotan las claves de forma segura.

**Nota sobre la clave secreta**: Para generar una clave segura, puedes usar herramientas en línea que generen cadenas Base64 aleatorias de la longitud adecuada, o incluso código Java como io.jsonwebtoken.`security.Keys.secretKeyFor(SignatureAlgorithm.HS256).getEncoded()` y luego codificar el resultado en Base64 para almacenarlo.

## 2. Qué es un JWT? Encabezado, Payload y Firma

Un **JSON Web Token (JWT)** es un estándar abierto (RFC 7519) que define una forma compacta, segura y autocontenida de transmitir información de forma confiable entre dos partes como un objeto JSON. Esta confianza se logra porque la información está firmada digitalmente, lo que permite verificar tanto su autenticidad (quién lo emitió) como su integridad (que no ha sido alterado).

### 2.1. Características Clave de JWT y sus Implicaciones en la Arquitectura

- **Compacto**: La naturaleza de los JWT como cadenas de texto codificadas en Base64Url los hace muy ligeros. Esto es una ventaja significativa, ya que pueden ser enviados eficientemente en cada solicitud HTTP sin añadir una sobrecarga de red considerable. Son ideales para ser incluidos en parámetros POST, o, lo más común y seguro, dentro de un encabezado HTTP, específicamente el encabezado `Authorization`. Su tamaño reducido contribuye a una mejor latencia y rendimiento de las APIs.
- **Autocontenido**: Una de las mayores ventajas arquitectónicas de JWT es que el _payload_ (carga útil) del token puede contener toda la información necesaria sobre la entidad (generalmente el usuario) y los permisos asociados. Esto significa que, una vez que el token es validado por el servidor, este no necesita realizar consultas adicionales a una base de datos o a otro servicio de autenticación para obtener los detalles del usuario o sus roles. Esta característica es fundamental para construir arquitecturas _stateless_ (sin estado) y es especialmente beneficiosa en entornos distribuidos y de microservicios, donde cada servicio puede validar el token de forma independiente sin depender de una sesión centralizada.
- **Seguro**: Aunque el contenido del _payload_ de un JWT es codificado (en Base64Url) y, por lo tanto, legible por cualquier persona que intercepte el token, la seguridad del JWT reside en su **firma digital**. Esta firma, generada con una clave secreta, garantiza dos aspectos críticos:
  1. **Integridad**: Asegura que el contenido del token (tanto el encabezado como el payload) no ha sido alterado desde que fue emitido por el servidor. Si un atacante intenta modificar cualquier parte del token, la firma dejará de ser válida, y el servidor detectará la manipulación, rechazando el token.
  2. **Autenticidad**: Verifica que el token fue realmente emitido por el servidor que posee la clave secreta utilizada para firmarlo. Esto previene que un atacante cree tokens falsos y se haga pasar por un usuario legítimo.

Un JWT es una cadena de texto que consta de tres partes, separadas por puntos (`.`):

```text
header.payload.signature
```

![Estructura JWT](https://www.miniorange.com/blog/assets/2023/jwt-structure.webp)

Cada una de estas partes es una cadena codificada en Base64Url.

### 2.2. Header (Cabecera)

La cabecera es un objeto JSON que contiene metadatos sobre el token. Generalmente especifica dos elementos clave:

- `alg`: El algoritmo de firma utilizado para generar la firma del token. Los algoritmos comunes incluyen:
  - `HS256` (HMAC con SHA-256): Utiliza una clave simétrica (la misma clave se usa para firmar y verificar). Es el más sencillo de implementar y muy común para APIs.
  - `RS256` (RSA con SHA-256): Utiliza un par de claves asimétricas (una clave privada para firmar y una clave pública para verificar). Es más complejo pero ideal para escenarios donde múltiples servicios necesitan verificar tokens emitidos por un único servicio de autenticación sin compartir la clave privada.
  - `ES256` (ECDSA con SHA-256): Utiliza criptografía de curva elíptica, ofreciendo seguridad similar a RSA con claves más cortas.
- `typ`: El tipo de token, que casi siempre es `JWT`.

**Ejemplo de Header (JSON, antes de ser codificado en Base64Url)**:

```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Una vez codificado en Base64Url, este header se convierte en la primera parte de tu JWT.

### 2.3. Payload (Carga Útil o Claims)

El _payload_ es el corazón del JWT, conteniendo las "claims" (declaraciones) o afirmaciones sobre una entidad (normalmente el usuario) y datos adicionales. Estas claims son pares clave-valor que transmiten información. Existen tres tipos de claims:

- **Registered Claims (Claims Registradas)**: Son un conjunto de claims predefinidas por el estándar JWT que no son obligatorias, pero se recomiendan por su utilidad y para asegurar la interoperabilidad entre diferentes sistemas. Son nombres de claims cortos para mantener el tamaño del token compacto.
  - **`iss` (issuer)**: Identifica al emisor del token. Por ejemplo, el nombre de tu aplicación o dominio (`"https://api.miempresa.com"`).
  - **`sub` (subject)**: Identifica al sujeto del token. Generalmente, es el identificador único del usuario (`"user_id_456"` o el nombre de usuario).
  - **`aud` (audience)**: Identifica a la audiencia para la que el token está destinado. Puede ser el nombre de la aplicación cliente o un servicio específico ("mi-frontend-app").
  - **`exp` (expiration time)**: Un timestamp (en segundos desde la época Unix) que indica el tiempo de expiración del token. Después de esta fecha, el token debe ser rechazado. Es crucial para limitar la ventana de oportunidad de un atacante si el token es robado.
  - **`iat` (issued at)**: Un timestamp que indica el tiempo en que el token fue emitido. Útil para determinar la antigüedad del token.
  - **`jti` (JWT ID)**: Un identificador único para el JWT. Puede ser usado para prevenir ataques de "replay" (donde un token interceptado se usa repetidamente) o para implementar la revocación de tokens.
- **Public Claims (Claims Públicas)**: Pueden ser definidas libremente por quienes usan JWT, pero para evitar colisiones de nombres, deben ser registradas en el IANA JSON Web Token Registry o definirse como un URI que contenga un espacio de nombres resistente a colisiones. Son menos comunes en implementaciones típicas que las Registered o Private Claims.
- **Private Claims (Claims Privadas)**: Son claims personalizadas creadas para compartir información específica entre las partes que acuerdan su uso (por ejemplo, tu servidor backend y tu aplicación frontend). No son ni Registered ni Public Claims y no tienen un significado predefinido por el estándar JWT.
  - Ejemplos: `roles` (ej. `["ADMIN", "USER", "VIEWER"]`), `email` (`"usuario@example.com"`), `department` (`"IT"`), `custom_user_id` (`"uuid-abc-123"`).
  - Es importante recordar que, aunque las claims privadas son útiles para transportar datos necesarios, no deben contener información altamente sensible que deba mantenerse confidencial, ya que el payload es solo codificado en Base64Url y, por lo tanto, legible por cualquier persona que tenga el token. Si necesitas transmitir datos sensibles de forma confidencial, estos deberían ser cifrados antes de ser incluidos en el payload, o el token debería usarse solo como un identificador para acceder a la información sensible almacenada de forma segura en el servidor.

**Ejemplo de Payload (JSON, antes de ser codificado en Base64Url)**:

```JSON
{
  "sub": "user_123",
  "name": "Alice Wonderland",
  "email": "alice@example.com",
  "roles": ["USER", "EDITOR"],
  "iat": 1678886400, // Representa 2023-03-15 00:00:00 UTC
  "exp": 1678890000  // Representa 2023-03-15 01:00:00 UTC (1 hora después)
}
```

### 2.4. Signature (Firma)

La firma es la parte más crítica del JWT en términos de seguridad, ya que es la que garantiza la integridad y autenticidad del token. Se calcula utilizando el _header_ codificado, el _payload_ codificado y una **clave secreta** que solo el emisor del token (tu servidor de autenticación) conoce.

El proceso de creación de la firma es el siguiente:

1. Se toma el `header` (ya codificado en Base64Url).
2. Se toma el `payload` (ya codificado en Base64Url).
3. Se concatenan ambas partes con un punto en el medio, formando una cadena: `base64UrlEncode(header) + "." + base64UrlEncode(payload)`.
4. Esta cadena resultante se pasa a través del algoritmo de firma especificado en el _header_ (`alg`), utilizando la **clave secreta** que el servidor posee.

**Fórmula conceptual para HMAC SHA256 (HS256)**:

`HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret_key)`

La firma resultante se añade como la tercera parte del JWT. Cuando el servidor recibe un JWT de un cliente, realiza el mismo cálculo de firma utilizando la clave secreta que posee. Si la firma calculada por el servidor coincide exactamente con la firma presente en el token recibido, el servidor puede confiar en que el token no ha sido alterado desde su emisión y que fue generado por una fuente de confianza (es decir, por sí mismo o por un servicio de autenticación conocido que comparte la misma clave secreta). Si las firmas no coinciden, el token se considera inválido, lo que indica que ha sido manipulado o que fue emitido por una fuente no autorizada, y la solicitud es rechazada.

## 3. Flujo Completo de Autenticación sin Estado

La autenticación con JWT sigue un flujo eficiente y seguro, ideal para arquitecturas sin estado como las APIs REST, donde el servidor no mantiene información de sesión entre solicitudes. Este enfoque mejora la escalabilidad y simplifica la gestión de sesiones.

![JWT authentication flow](https://docs.oracle.com/en/applications/jd-edwards/administration/9.2.x/eotsc/images/jwt_token.png)

1. **Inicio de Sesión (Login)**:
    - El usuario inicia el proceso enviando sus credenciales (generalmente nombre de usuario y contraseña) a un endpoint de autenticación específico en el servidor (ej. `/authenticate` o `/login`).
    - Esta solicitud inicial es una petición POST y debe realizarse **siempre sobre HTTPS** para proteger las credenciales del usuario de posibles interceptaciones en tránsito.
2. **Generación del Token**:
    - El servidor recibe las credenciales y las valida. Esto implica verificar el nombre de usuario y la contraseña contra una base de datos de usuarios o un servicio de identidad.
    - Si las credenciales son válidas, el servidor genera un JWT. Este token encapsula información sobre el usuario (como su ID, nombre, roles y tiempo de expiración) en su _payload_, y luego lo firma digitalmente utilizando una **clave secreta** que solo el servidor conoce.
3. **Envío del Token al Cliente**:
    - Una vez generado, el servidor envía el JWT recién creado de vuelta al cliente como parte de la respuesta HTTP de inicio de sesión exitosa.
    - Típicamente, el JWT se incluye en el cuerpo de la respuesta JSON (ej. `{ "jwt": "eyJhbG..." }`), aunque también podría enviarse en un encabezado personalizado o una cookie.
4. **Almacenamiento del Token en el Cliente**:
    - Una vez que el cliente recibe el JWT, debe almacenarlo de forma segura para poder incluirlo en futuras solicitudes. Las opciones comunes de almacenamiento en el navegador incluyen:
      - `localStorage`: Fácil de usar y persistente entre sesiones del navegador. Sin embargo, es vulnerable a ataques de Cross-Site Scripting (XSS), ya que cualquier código JavaScript malicioso inyectado en la página podría acceder y robar el token.
      - `sessionStorage`: Similar a `localStorage`, pero los datos se eliminan cuando se cierra la pestaña o la ventana del navegador. También es vulnerable a XSS.
      - Cookies `HttpOnly` y `Secure`: Consideradas más seguras contra XSS porque el JavaScript del lado del cliente no puede acceder a ellas (debido a la bandera HttpOnly). La bandera `Secure` asegura que la cookie solo se envíe sobre HTTPS. Sin embargo, pueden presentar desafíos con la protección CSRF si no se implementan medidas adicionales (aunque los JWTs en sí mismos mitigan esto si no se basan en sesiones) y con arquitecturas de microservicios que requieren que el token sea accesible por múltiples dominios (problemas de CORS). La elección depende del nivel de seguridad requerido y la arquitectura específica de la aplicación.
5. **Uso del Token en Peticiones**:
    - Para acceder a cualquier recurso o _endpoint_ protegido en el servidor, el cliente debe incluir el JWT en cada solicitud HTTP subsiguiente.
    - El estándar dicta que el token se envíe en el encabezado `Authorization` con el prefijo `Bearer`. Este prefijo indica que el token es un "token de portador", lo que significa que el poseedor del token es quien tiene derecho a acceder al recurso.
    - Ejemplo de encabezado HTTP: `Authorization: Bearer <tu_json_web_token_aqui>`
6. **Validación Automática en el Servidor**:
    - Cuando el servidor recibe una solicitud con un JWT en el encabezado `Authorization`, un filtro de seguridad (que implementaremos en Spring Security) intercepta la solicitud.
    - Este filtro extrae el JWT del encabezado y realiza una serie de verificaciones:
      - **Verificación de la Firma**: Utiliza la misma clave secreta que usó para firmar el token para verificar que la firma es válida. Si la firma no coincide, el token es inválido y la solicitud es rechazada.
      - **Verificación de Expiración**: Comprueba la claim `exp` (expiration time) para asegurar que el token no ha caducado.
      - **Otras Validaciones de Claims**: Puede verificar otras claims como `iss` (emisor), `aud` (audiencia) o `jti` (ID del JWT) si se implementan mecanismos de revocación.
    - Si todas las validaciones son exitosas, el servidor confía en la información contenida en el payload (ej. el ID de usuario y los roles) y utiliza esta información para autorizar la solicitud. Si alguna validación falla, la solicitud es denegada (típicamente con un estado HTTP `401 Unauthorized` o `403 Forbidden`).

Para implementar este flujo en nuestra aplicación Spring Boot, utilizaremos los siguientes componentes clave:

### 3.1. Modelos de Solicitud y Respuesta para Autenticación

Estas clases simples servirán como DTOs (Data Transfer Objects) para estructurar los datos que se envían y reciben durante el proceso de autenticación.

```Java
@Data // Anotación de Lombok para generar automáticamente getters, setters, etc.
@NoArgsConstructor // Constructor sin argumentos requerido por Jackson para deserialización
@AllArgsConstructor // Constructor con todos los argumentos para fácil inicialización
public class AuthenticationRequest {
    private String username; // Nombre de usuario proporcionado por el cliente
    private String password; // Contraseña proporcionada por el cliente
}
```

```java
@Data // Anotación de Lombok para generar automáticamente getters, setters, etc.
@NoArgsConstructor // Constructor sin argumentos requerido por Jackson para deserialización
@AllArgsConstructor // Constructor con todos los argumentos para fácil inicialización
public class AuthenticationResponse {
    private String jwt; // El token JWT que el cliente usará para futuras solicitudes
}
```

### 3.2. Componente para Generación y Validación de JWT (`JwtUtil.java`)

Esta clase actuará como una utilidad centralizada para todas las operaciones relacionadas con JWT. Encapsulará la lógica para crear nuevos tokens, extraer información de tokens existentes y validar su autenticidad e integridad.

```Java
package com.example.securityjwt.util;

import io.jsonwebtoken.Claims; // Representa el payload del JWT, un mapa de declaraciones
import io.jsonwebtoken.Jwts; // Clase principal de JJWT para construir y parsear JWTs
import io.jsonwebtoken.SignatureAlgorithm; // Algoritmos de firma soportados (ej. HS256)
import io.jsonwebtoken.io.Decoders; // Utilidad para decodificar cadenas de Base64
import io.jsonwebtoken.security.Keys; // Utilidad para generar claves criptográficas seguras
import org.springframework.beans.factory.annotation.Value; // Para inyectar valores desde application.properties
import org.springframework.security.core.userdetails.UserDetails; // Interfaz de Spring Security para detalles del usuario
import org.springframework.stereotype.Component; // Marca la clase como un componente de Spring

import java.security.Key; // Interfaz para claves criptográficas
import java.util.Date; // Para manejar fechas de emisión y expiración
import java.util.HashMap; // Para construir el mapa de claims
import java.util.Map; // Interfaz para el mapa de claims
import java.util.function.Function; // Para extraer claims de forma genérica

@Component // Indica a Spring que esta clase es un componente gestionado por el contenedor de Spring
public class JwtUtil {

    @Value("${jwt.secret}") // Inyecta el valor de la propiedad 'jwt.secret' definida en application.properties
    private String secret; // La clave secreta utilizada para firmar y verificar los JWTs

    @Value("${jwt.expiration}") // Inyecta el valor de la propiedad 'jwt.expiration'
    private long expiration; // Tiempo de vida del token en milisegundos (ej. 24 horas)

    /**
     * Obtiene la clave de firma a partir del secreto codificado en Base64.
     * Esta clave es utilizada para firmar los JWTs durante su creación y para
     * verificar la firma de los JWTs recibidos.
     * @return La clave criptográfica segura para la firma HMAC SHA256.
     */
    private Key getSignInKey() {
        // Decodifica la cadena secreta (que está en Base64) a un array de bytes.
        byte[] keyBytes = Decoders.BASE64.decode(secret);
        // Crea una clave HMAC SHA para la firma a partir de los bytes decodificados.
        return Keys.hmacShaKeyFor(keyBytes);
    }

    /**
     * Extrae un claim específico del token JWT utilizando una función de resolución.
     * Este es un método genérico para obtener cualquier claim del payload.
     * @param token El token JWT del que se extraerá el claim.
     * @param claimsResolver Una función que toma un objeto Claims (el payload) y devuelve el valor deseado.
     * @param <T> El tipo de dato del claim a extraer.
     * @return El valor del claim extraído.
     */
    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token); // Primero extrae todas las claims del token
        return claimsResolver.apply(claims); // Luego aplica la función de resolución para obtener el claim específico
    }

    /**
     * Extrae todas las claims (el payload completo) de un token JWT.
     * Este método también realiza la validación de la firma del token.
     * @param token El token JWT del que se extraerán todas las claims.
     * @return Un objeto Claims que contiene todas las declaraciones del token.
     * @throws io.jsonwebtoken.JwtException Si el token es inválido (ej. firma incorrecta, expirado).
     */
    private Claims extractAllClaims(String token) {
        return Jwts.parserBuilder() // Inicia el constructor del parser de JWT
                .setSigningKey(getSignInKey()) // Establece la clave de firma para la verificación del token
                .build() // Construye el parser de JWT
                .parseClaimsJws(token) // Parsea el token JWT y verifica su firma (JSON Web Signature)
                .getBody(); // Obtiene el cuerpo (payload) del token como un objeto Claims
    }

    /**
     * Extrae el nombre de usuario (el 'subject' claim) del token JWT.
     * @param token El token JWT.
     * @return El nombre de usuario (String).
     */
    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject); // Utiliza el extractor genérico para obtener el 'subject'
    }

    /**
     * Extrae la fecha de expiración (el 'exp' claim) del token JWT.
     * @param token El token JWT.
     * @return La fecha de expiración (Date).
     */
    public Date extractExpiration(String token) {
        return extractClaim(token, Claims::getExpiration); // Utiliza el extractor genérico para obtener el 'expiration'
    }

    /**
     * Verifica si el token JWT ha expirado.
     * @param token El token JWT.
     * @return true si el token ha expirado (su fecha de expiración es anterior a la fecha actual), false en caso contrario.
     */
    private Boolean isTokenExpired(String token) {
        return extractExpiration(token).before(new Date()); // Compara la fecha de expiración del token con la fecha y hora actuales
    }

    /**
     * Valida un token JWT contra los detalles de un usuario.
     * Verifica que el nombre de usuario en el token coincide con el del UserDetails
     * y que el token no ha expirado.
     * @param token El token JWT a validar.
     * @param userDetails Los detalles del usuario cargados por UserDetailsService.
     * @return true si el token es válido para el usuario dado, false en caso contrario.
     */
    public Boolean validateToken(String token, UserDetails userDetails) {
        final String username = extractUsername(token); // Extrae el nombre de usuario del token
        // Compara el nombre de usuario extraído con el del UserDetails y verifica si el token no ha expirado
        return (username.equals(userDetails.getUsername()) && !isTokenExpired(token));
    }

    /**
     * Genera un token JWT para un usuario dado.
     * @param userDetails Los detalles del usuario para quien se generará el token.
     * @return El token JWT generado como una cadena de texto.
     */
    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        // Aquí podrías añadir claims adicionales al payload del token, como roles del usuario,
        // ID de usuario, etc. Esto permite que el token contenga más información útil.
        // Ejemplo:
        // if (userDetails instanceof CustomUserDetails) {
        //     claims.put("userId", ((CustomUserDetails) userDetails).getUserId());
        //     claims.put("roles", userDetails.getAuthorities().stream().map(GrantedAuthority::getAuthority).collect(Collectors.toList()));
        // }
        return createToken(claims, userDetails.getUsername()); // Llama al método privado para construir y firmar el token
    }

    /**
     * Crea el token JWT con las claims, el sujeto, la fecha de emisión y expiración, y lo firma.
     * Este es el método interno que construye la cadena JWT final.
     * @param claims Un mapa de claims a incluir en el payload del token.
     * @param subject El sujeto del token (normalmente el nombre de usuario o un ID único).
     * @return El token JWT compacto y firmado.
     */
    private String createToken(Map<String, Object> claims, String subject) {
        return Jwts.builder() // Inicia el constructor de JWT
                .setClaims(claims) // Establece las claims personalizadas en el payload
                .setSubject(subject) // Establece el 'subject' (quién es el token)
                .setIssuedAt(new Date(System.currentTimeMillis())) // Establece la fecha de emisión (momento actual)
                .setExpiration(new Date(System.currentTimeMillis() + expiration)) // Establece la fecha de expiración
                .signWith(getSignInKey(), SignatureAlgorithm.HS256) // Firma el token con la clave secreta y el algoritmo HS256
                .compact(); // Compacta el token en su formato final (cadena Base64Url)
    }
}
```

### 3.3. Servicio de Usuario Personalizado (`MyUserDetailsService.java`)

Spring Security requiere una implementación de la interfaz `UserDetailsService`. Esta interfaz es utilizada por Spring para cargar los detalles de un usuario (nombre de usuario, contraseña codificada, roles/autoridades) cuando se intenta autenticar. En una aplicación real, este servicio interactuaría con una base de datos o un repositorio de usuarios para recuperar la información. Para este ejemplo, simularemos la carga de un usuario "hardcodeado".

```Java
package com.example.securityjwt.service;

import org.springframework.security.core.userdetails.User; // Clase User de Spring Security para representar los detalles del usuario
import org.springframework.security.core.userdetails.UserDetails; // Interfaz para los detalles del usuario
import org.springframework.security.core.userdetails.UserDetailsService; // Interfaz principal para cargar usuarios
import org.springframework.security.core.userdetails.UsernameNotFoundException; // Excepción si el usuario no se encuentra
import org.springframework.stereotype.Service; // Marca la clase como un servicio de Spring

import java.util.ArrayList; // Para las autoridades (roles) del usuario

@Service // Indica a Spring que esta clase es un componente de servicio gestionado por el contenedor
public class MyUserDetailsService implements UserDetailsService {

    /**
     * Carga los detalles de un usuario por su nombre de usuario.
     * Este método es invocado por Spring Security durante el proceso de autenticación.
     *
     * **IMPORTANTE:** En una aplicación de producción real, este método interactuaría
     * con una base de datos (por ejemplo, utilizando Spring Data JPA) o un servicio
     * de directorio (como LDAP) para recuperar la información completa del usuario,
     * incluyendo su contraseña codificada y sus roles/permisos asociados.
     *
     * Por simplicidad en este ejemplo, estamos utilizando un usuario "hardcodeado".
     *
     * @param username El nombre de usuario a buscar.
     * @return Un objeto UserDetails que contiene la información del usuario (username, password, authorities).
     * @throws UsernameNotFoundException Si el usuario con el nombre de usuario especificado no es encontrado.
     */
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // Simulación de carga de usuario desde una base de datos.
        // En un escenario real, aquí tendrías una consulta a tu repositorio de usuarios.
        if ("user".equals(username)) {
            // Retorna un objeto UserDetails de Spring Security.
            // El primer parámetro es el nombre de usuario.
            // El segundo parámetro es la contraseña codificada.
            // "$2a$10$SlYQmyglfiGw/YgE/M7F7.y0Uq0Xk0K0Xk0K0Xk0K0Xk0K0Xk0K" es la contraseña "password" codificada con BCrypt.
            // El tercer parámetro es una lista de autoridades/roles del usuario (vacía por ahora, pero se expandirá en el futuro).
            return new User("user", "$2a$10$SlYQmyglfiGw/YgE/M7F7.y0Uq0Xk0K0Xk0K0Xk0K0Xk0K0Xk0K", new ArrayList<>());
        } else {
            // Si el nombre de usuario no coincide, se lanza una excepción indicando que el usuario no fue encontrado.
            throw new UsernameNotFoundException("Usuario no encontrado: " + username);
        }
    }
}
```

### 3.4. Controlador de Autenticación (`AuthController.java`)

Este controlador es el punto de entrada para el proceso de autenticación. Expone un endpoint (`/authenticate`) al que los clientes enviarán sus credenciales. Si la autenticación es exitosa, el controlador generará un JWT y lo devolverá al cliente.

```Java
package com.example.securityjwt.controller;

import com.example.securityjwt.model.AuthenticationRequest; // Modelo para la solicitud de autenticación
import com.example.securityjwt.model.AuthenticationResponse; // Modelo para la respuesta de autenticación
import com.example.securityjwt.util.JwtUtil; // Utilidad para manejar JWTs
import org.springframework.beans.factory.annotation.Autowired; // Para inyección de dependencias
import org.springframework.http.ResponseEntity; // Para construir respuestas HTTP
import org.springframework.security.authentication.AuthenticationManager; // Gestor de autenticación de Spring Security
import org.springframework.security.authentication.BadCredentialsException; // Excepción para credenciales incorrectas
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken; // Objeto de autenticación para Spring Security
import org.springframework.security.core.userdetails.UserDetails; // Detalles del usuario
import org.springframework.security.core.userdetails.UserDetailsService; // Servicio para cargar detalles del usuario
import org.springframework.web.bind.annotation.PostMapping; // Anotación para mapear solicitudes POST
import org.springframework.web.bind.annotation.RequestBody; // Anotación para mapear el cuerpo de la solicitud a un objeto Java
import org.springframework.web.bind.annotation.RestController; // Marca la clase como un controlador REST

@RestController // Indica que esta clase es un controlador REST de Spring, capaz de manejar solicitudes HTTP
public class AuthController {

    @Autowired
    private AuthenticationManager authenticationManager; // Inyecta el AuthenticationManager de Spring Security, que orquesta la autenticación

    @Autowired
    private UserDetailsService userDetailsService; // Inyecta el UserDetailsService para cargar los detalles del usuario

    @Autowired
    private JwtUtil jwtUtil; // Inyecta nuestra utilidad para generar y validar JWTs

    /**
     * Endpoint para autenticar usuarios y generar un JWT.
     * Recibe un objeto AuthenticationRequest (que contiene el nombre de usuario y la contraseña)
     * en el cuerpo de la solicitud POST.
     * @param authenticationRequest La solicitud de autenticación con las credenciales del usuario.
     * @return ResponseEntity con el JWT si la autenticación es exitosa (HTTP 200 OK), o un error si falla.
     * @throws Exception Si las credenciales son incorrectas o ocurre otro error durante la autenticación.
     */
    @PostMapping("/authenticate") // Mapea las solicitudes POST a la ruta /authenticate
    public ResponseEntity<?> createAuthenticationToken(@RequestBody AuthenticationRequest authenticationRequest) throws Exception {
        try {
            // Intenta autenticar al usuario utilizando el AuthenticationManager.
            // Se crea un UsernamePasswordAuthenticationToken con las credenciales proporcionadas.
            // Si la autenticación falla (ej. credenciales incorrectas), el AuthenticationManager
            // lanzará una excepción, como BadCredentialsException.
            authenticationManager.authenticate(
                    new UsernamePasswordAuthenticationToken(authenticationRequest.getUsername(), authenticationRequest.getPassword())
            );
        } catch (BadCredentialsException e) {
            // Captura la excepción de credenciales incorrectas y lanza una excepción más genérica
            // para evitar exponer detalles internos de seguridad al cliente.
            throw new Exception("Credenciales incorrectas", e);
        }

        // Si la autenticación es exitosa (no se lanzó ninguna excepción),
        // carga los detalles completos del usuario utilizando el UserDetailsService.
        // Esto es necesario para generar el JWT con la información correcta del usuario.
        final UserDetails userDetails = userDetailsService.loadUserByUsername(authenticationRequest.getUsername());
        // Genera el JWT utilizando nuestra utilidad JwtUtil con los detalles del usuario cargados.
        final String jwt = jwtUtil.generateToken(userDetails);

        // Retorna una respuesta HTTP 200 OK (ResponseEntity.ok())
        // con el JWT encapsulado en un objeto AuthenticationResponse en el cuerpo de la respuesta.
        return ResponseEntity.ok(new AuthenticationResponse(jwt));
    }
}
```

## 4. Implementación de Filtros Personalizados y Configuración de Cabeceras

Una vez que el cliente obtiene un JWT tras una autenticación exitosa, el siguiente paso es cómo ese token se utiliza para acceder a recursos protegidos en el servidor. Esto requiere una configuración específica en Spring Security para interceptar cada solicitud entrante, validar el JWT presente en el encabezado `Authorization` y, si es válido, establecer el contexto de seguridad para esa solicitud.

### 4.1. Filtro de Autenticación JWT (`JwtRequestFilter.java`)

Este filtro es una pieza fundamental en la cadena de seguridad de Spring Security. Se ejecutará **una vez por cada solicitud HTTP** (`OncePerRequestFilter`), interceptando el JWT del encabezado `Authorization`, validándolo y, si es válido, estableciendo la autenticación en el `SecurityContextHolder`. Esto permite que los controladores y otros componentes de Spring Security reconozcan al usuario autenticado para esa solicitud.

```Java
package com.example.securityjwt.filter;

import com.example.securityjwt.util.JwtUtil; // Importa nuestra utilidad para manejar JWTs
import org.springframework.beans.factory.annotation.Autowired; // Para inyección de dependencias
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken; // Objeto de autenticación para Spring Security
import org.springframework.security.core.context.SecurityContextHolder; // Contexto de seguridad de Spring, donde se almacena la autenticación
import org.springframework.security.core.userdetails.UserDetails; // Detalles del usuario cargados por UserDetailsService
import org.springframework.security.core.userdetails.UserDetailsService; // Servicio para cargar detalles del usuario
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource; // Para establecer detalles adicionales de la autenticación
import org.springframework.stereotype.Component; // Marca la clase como un componente de Spring
import org.springframework.web.filter.OncePerRequestFilter; // Clase base para filtros que se ejecutan una vez por solicitud

import javax.servlet.FilterChain; // Cadena de filtros, permite pasar la solicitud al siguiente filtro
import javax.servlet.ServletException; // Excepción de servlet
import javax.servlet.http.HttpServletRequest; // Objeto de solicitud HTTP
import javax.servlet.http.HttpServletResponse; // Objeto de respuesta HTTP
import java.io.IOException; // Excepción de I/O

@Component // Indica a Spring que esta clase es un componente gestionado por el contenedor
public class JwtRequestFilter extends OncePerRequestFilter {

    @Autowired
    private UserDetailsService userDetailsService; // Inyecta nuestra implementación personalizada de UserDetailsService

    @Autowired
    private JwtUtil jwtUtil; // Inyecta nuestra utilidad para generar y validar JWTs

    /**
     * Este método se ejecuta una vez por cada solicitud HTTP.
     * Es el corazón del filtro JWT: intercepta el token, lo valida y establece la autenticación.
     * @param request La solicitud HTTP entrante.
     * @param response La respuesta HTTP saliente.
     * @param chain La cadena de filtros para continuar el procesamiento de la solicitud.
     * @throws ServletException Si ocurre un error de servlet.
     * @throws IOException Si ocurre un error de I/O.
     */
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {

        // 1. Intenta obtener el encabezado "Authorization" de la solicitud HTTP.
        // Este encabezado es donde el cliente enviará el JWT (ej. "Bearer eyJhbG...").
        final String authorizationHeader = request.getHeader("Authorization");

        String username = null;
        String jwt = null;

        // 2. Verifica si el encabezado de autorización existe y tiene el formato "Bearer ".
        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            // Extrae el token JWT eliminando el prefijo "Bearer " (que tiene 7 caracteres).
            jwt = authorizationHeader.substring(7);
            // Extrae el nombre de usuario del token utilizando nuestra utilidad JwtUtil.
            try {
                username = jwtUtil.extractUsername(jwt);
            } catch (Exception e) {
                // Si la extracción falla (ej. token malformado, expirado antes de la validación completa),
                // simplemente no se establecerá el nombre de usuario y la autenticación fallará más adelante.
                // Podrías loggear el error aquí para depuración.
                System.err.println("Error al extraer username del JWT: " + e.getMessage());
            }
        }

        // 3. Si se logró extraer un nombre de usuario del token Y no hay una autenticación existente
        // en el contexto de seguridad de Spring (es decir, el usuario no ha sido autenticado aún para esta solicitud).
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {

            // 4. Carga los detalles completos del usuario utilizando el UserDetailsService.
            // Esto es crucial para obtener los roles y otras autoridades asociadas al usuario.
            UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);

            // 5. Valida el token JWT: verifica la firma, la expiración y que el nombre de usuario
            // extraído del token coincide con el nombre de usuario de los UserDetails cargados.
            if (jwtUtil.validateToken(jwt, userDetails)) {

                // 6. Si el token es válido, crea un objeto de autenticación de Spring Security.
                // Este objeto representa al usuario autenticado.
                // - El primer parámetro es el principal (UserDetails).
                // - El segundo parámetro son las credenciales (null para JWT, ya que el token es la credencial).
                // - El tercer parámetro son las autoridades/roles del usuario.
                UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken =
                        new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());

                // 7. Establece detalles adicionales de la autenticación, como la dirección IP del cliente,
                // que pueden ser útiles para auditorías o seguridad.
                usernamePasswordAuthenticationToken
                        .setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                // 8. Establece el objeto de autenticación en el contexto de seguridad de Spring.
                // Esto es lo que indica a Spring Security que el usuario está autenticado para esta solicitud
                // y permite que los mecanismos de autorización (ej. @PreAuthorize) funcionen.
                SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);
            }
        }
        // 9. Continúa con la cadena de filtros de Spring Security.
        // Si la autenticación fue exitosa, la solicitud procederá a los controladores protegidos.
        // Si no, Spring Security manejará la denegación de acceso (ej. lanzando un 401 Unauthorized).
        chain.doFilter(request, response);
    }
}
```

### 4.2. Configuración de Spring Security (`SecurityConfig.java`)

Esta clase es el centro de la configuración de Spring Security para nuestra aplicación. Aquí personalizaremos cómo se autentican los usuarios, qué rutas están protegidas y, lo más importante, cómo se integra nuestro `JwtRequestFilter` en la cadena de seguridad.

```Java
package com.example.securityjwt.config;

import com.example.securityjwt.filter.JwtRequestFilter; // Nuestro filtro JWT personalizado
import org.springframework.beans.factory.annotation.Autowired; // Para inyección de dependencias
import org.springframework.context.annotation.Bean; // Para declarar beans de Spring
import org.springframework.security.authentication.AuthenticationManager; // Gestor de autenticación
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder; // Para configurar el gestor de autenticación
import org.springframework.security.config.annotation.web.builders.HttpSecurity; // Para configurar la seguridad HTTP
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity; // Habilita la seguridad web de Spring
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter; // Clase base para configurar la seguridad
import org.springframework.security.config.http.SessionCreationPolicy; // Política de creación de sesiones
import org.springframework.security.core.userdetails.UserDetailsService; // Servicio para cargar detalles del usuario
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder; // Codificador de contraseñas BCrypt
import org.springframework.security.crypto.password.PasswordEncoder; // Interfaz para codificadores de contraseñas
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter; // Filtro de autenticación estándar de Spring

@EnableWebSecurity // Habilita la configuración de seguridad web de Spring en esta clase.
public class SecurityConfig extends WebSecurityConfigurerAdapter { // Extiende para personalizar la configuración de seguridad.

    @Autowired
    private UserDetailsService userDetailsService; // Inyecta nuestra implementación personalizada de UserDetailsService.

    @Autowired
    private JwtRequestFilter jwtRequestFilter; // Inyecta nuestro filtro JWT personalizado.

    /**
     * Configura el AuthenticationManager para usar nuestro UserDetailsService
     * y el PasswordEncoder. Esto le dice a Spring Security cómo encontrar los
     * detalles del usuario y cómo verificar las contraseñas durante la autenticación.
     * @param auth El constructor del AuthenticationManager.
     * @throws Exception Si ocurre un error durante la configuración.
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService) // Especifica el servicio que cargará los detalles del usuario.
            .passwordEncoder(passwordEncoder()); // Especifica el codificador de contraseñas a utilizar para la verificación.
    }

    /**
     * Configura las reglas de seguridad HTTP, incluyendo la protección CSRF,
     * la autorización de solicitudes y la política de gestión de sesiones.
     * Este es el método clave para definir las reglas de acceso a nuestras APIs.
     * @param http El objeto HttpSecurity para configurar las reglas de seguridad.
     * @throws Exception Si ocurre un error durante la configuración.
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable() // Deshabilita la protección CSRF (Cross-Site Request Forgery).
                    // Esto es una práctica común y segura para APIs RESTful que utilizan JWTs
                    // para la autenticación, ya que los JWTs son inherentemente inmunes a CSRF
                    // (no se basan en cookies de sesión que puedan ser robadas).
            .authorizeRequests() // Comienza la configuración de reglas de autorización para las solicitudes.
            .antMatchers("/authenticate").permitAll() // Permite el acceso a la ruta /authenticate sin necesidad de autenticación.
                                                     // Esta es la ruta donde los usuarios enviarán sus credenciales para obtener un JWT.
            .anyRequest().authenticated() // Todas las demás solicitudes (cualquier otra ruta) requieren autenticación.
                                         // Esto significa que un JWT válido debe estar presente para acceder a ellas.
            .and().sessionManagement() // Configura la gestión de sesiones.
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS); // Configura Spring Security para no crear ni usar sesiones HTTP.
                                                                     // Esto es CRUCIAL para la autenticación basada en JWT,
                                                                     // ya que cada solicitud es autocontenida y no depende
                                                                     // de un estado de sesión en el servidor.

        // Añade nuestro filtro JWT personalizado (JwtRequestFilter) antes del filtro estándar
        // UsernamePasswordAuthenticationFilter de Spring Security.
        // Esto asegura que el JWT se procese y el contexto de seguridad se establezca
        // ANTES de que Spring Security intente autenticar con usuario/contraseña
        // (lo cual no queremos para recursos protegidos con JWT).
        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
    }

    /**
     * Define un bean para el codificador de contraseñas.
     * BCryptPasswordEncoder es un algoritmo de hash de contraseñas altamente recomendado por su seguridad
     * y resistencia a ataques de fuerza bruta.
     * @return Una instancia de BCryptPasswordEncoder.
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(); // Usa BCrypt para codificar y verificar contraseñas de forma segura.
    }

    /**
     * Expone el AuthenticationManager como un bean de Spring.
     * Esto permite que sea inyectado en otros componentes (como AuthController)
     * para realizar la autenticación programáticamente.
     * @return Una instancia del AuthenticationManager.
     * @throws Exception Si ocurre un error al obtener el AuthenticationManager.
     */
    @Override
    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```

### 4.3. Controlador de Prueba (`HelloController.java`)

Este es un controlador simple que expone un endpoint protegido. Solo los usuarios que hayan sido autenticados exitosamente con un JWT válido podrán acceder a este recurso, lo que nos permitirá verificar que nuestra configuración de seguridad funciona correctamente.

```Java
package com.example.securityjwt.controller;

import org.springframework.web.bind.annotation.GetMapping; // Anotación para mapear solicitudes HTTP GET
import org.springframework.web.bind.annotation.RestController; // Marca la clase como un controlador REST

@RestController // Indica a Spring que esta clase es un controlador REST y sus métodos devuelven datos directamente
public class HelloController {

    /**
     * Endpoint de ejemplo que está protegida por JWT.
     * Para acceder a este recurso, el cliente debe proporcionar un JWT válido
     * en el encabezado 'Authorization: Bearer <token>'.
     * @return Un mensaje de bienvenida que indica que el acceso fue exitoso.
     */
    @GetMapping("/hello") // Mapea las solicitudes GET a la ruta /hello
    public String hello() {
        return "Hola, ¡has accedido a un recurso protegido con JWT!";
    }
}
```

### 5. Consideraciones de Seguridad y Buenas Prácticas

La implementación de JWT es una herramienta poderosa para la seguridad de APIs, pero su uso incorrecto puede introducir vulnerabilidades significativas. Es crucial entender y aplicar las siguientes consideraciones y buenas prácticas:

- **Almacenamiento del Token en el Cliente**: La forma en que el cliente almacena el JWT es una de las decisiones de seguridad más importantes.
  - `localStorage` y `sessionStorage`: Son fáciles de usar para almacenar tokens en el navegador. Sin embargo, son **vulnerables a ataques de Cross-Site Scripting (XSS)**. Si un atacante logra inyectar código JavaScript malicioso en tu página web (por ejemplo, a través de una vulnerabilidad en un formulario de comentarios), ese código podría acceder y robar el JWT almacenado, permitiendo al atacante suplantar al usuario.
  - Cookies `HttpOnly` y `Secure`: Son generalmente consideradas más seguras contra XSS porque la bandera HttpOnly impide que el JavaScript del lado del cliente acceda a la cookie. La bandera `Secure` asegura que la cookie solo se envíe sobre HTTPS. Sin embargo, pueden ser vulnerables a ataques de Cross-Site Request Forgery (CSRF) si no se implementan medidas adicionales (aunque los JWTs en sí mismos, al ser tokens de portador, mitigan esto si no se basan en sesiones). También pueden presentar desafíos con la gestión de CORS en arquitecturas de microservicios con múltiples dominios.
- **Expiración del Token y Tokens de Refresco**:
  - Define un tiempo de expiración (`exp` claim) **razonable y corto** para tus JWTs (access tokens), por ejemplo, entre 15 y 30 minutos. Los tokens de corta duración reducen drásticamente la ventana de oportunidad para un atacante si un token es robado.
  - Para mantener una buena experiencia de usuario sin requerir un nuevo inicio de sesión constante, implementa un mecanismo de "**refresh token**" (token de refresco). Un refresh token es un token de larga duración (ej. días o semanas) que se usa solo para solicitar un nuevo access token cuando el actual expira. El refresh token debe almacenarse de forma más segura (idealmente en una cookie `HttpOnly` y `Secure`) y debe ser de un solo uso o tener mecanismos de revocación robustos. Este enfoque desacopla la vida útil del token de acceso (corto y frecuente) de la vida útil de la sesión del usuario (larga y menos frecuente).
- **Revocación de Tokens**: Por su naturaleza stateless, los JWTs no pueden ser "revocados" directamente una vez emitidos hasta que expiran. Para escenarios donde la revocación inmediata es necesaria (ej. el usuario cambia su contraseña, se cierra sesión forzosamente, se detecta un token comprometido), puedes implementar una "**lista negra**" (blacklist) o una "lista blanca" (whitelist) en el servidor.
  - **Blacklist**: Almacena los IDs de los tokens (`jti` claim) que han sido revocados en una base de datos rápida (ej. Redis). El servidor consulta esta lista en cada solicitud para verificar si el token ha sido invalidado. Esto introduce un pequeño estado en el servidor, pero es necesario para la revocación.
  - **Whitelist**: Almacena los IDs de los tokens válidos. Solo los tokens presentes en esta lista son aceptados.
- **Clave Secreta**: La seguridad de tus JWTs depende directamente de la confidencialidad y fortaleza de tu clave secreta.
  - **Generación**: Utiliza generadores de claves criptográficamente seguros para crear claves largas, aleatorias y complejas.
  - **Almacenamiento**: **Nunca la hardcodees en el código fuente ni la subas a repositorios públicos**. Almacénala en variables de entorno, servicios de gestión de secretos (ej. HashiCorp Vault, AWS Secrets Manager, Azure Key Vault) o archivos de configuración protegidos y fuera del control de versiones.
  - **Rotación**: Considera rotar tu clave secreta periódicamente para mitigar el riesgo si se ve comprometida.
- **HTTPS (SSL/TLS)**: Siempre, sin excepción, utiliza HTTPS para todas las comunicaciones entre el cliente y el servidor. HTTPS cifra el tráfico de red, protegiendo los JWTs (y las credenciales iniciales de inicio de sesión) de ser interceptados y leídos por atacantes en tránsito. Sin HTTPS, un atacante podría fácilmente robar los tokens y suplantar a los usuarios.
- **No almacenar información sensible en el Payload**: Aunque el payload de un JWT está codificado en Base64Url, **no está cifrado**. Esto significa que cualquier persona con acceso al token puede decodificarlo y leer su contenido. Por lo tanto, **nunca** incluyas información altamente sensible o confidencial (como contraseñas, números de tarjetas de crédito, datos de salud protegidos, etc.) directamente en el payload del JWT. Solo la firma garantiza la integridad, no la confidencialidad. Si necesitas transmitir datos sensibles de forma segura, deberías cifrarlos antes de incluirlos en el token, o usar el token como un identificador para acceder a la información sensible almacenada de forma segura en el servidor.
