# Clase 2: Seguridad en la Integración: JWT, Interceptores y Guard

La seguridad no es un extra, sino un requisito fundamental en el desarrollo de aplicaciones web modernas. Va más allá de simplemente proteger el backend; también implica fortificar el lado del cliente para garantizar que las peticiones sean legítimas y que los usuarios solo tengan acceso a las secciones a las que están autorizados. En esta clase, vamos a explorar cómo construir una defensa robusta y escalable en el frontend usando Angular. Nos enfocaremos en tres conceptos esenciales que trabajan en conjunto para proteger nuestras rutas y la comunicación con el servidor: los **JWT (JSON Web Tokens)**, los Interceptors y los Guards. Al dominar estos pilares, serás capaz de crear aplicaciones que garantizan que solo los usuarios autorizados tengan acceso a los recursos adecuados.

## Objetivos de aprendizaje

Al finalizar esta clase, serás capaz de:

- Comprender la arquitectura de la seguridad sin estado con JWT.
- Entender la estructura y la función de un **JSON Web Token**, incluyendo sus tres partes principales: Header, Payload y Signature.
- Aprender a usar los **Interceptores** de Angular para adjuntar automáticamente un JWT a las peticiones salientes y para manejar errores de autenticación de forma centralizada.
- Saber cómo implementar **Guards** para proteger rutas específicas en el frontend, controlando el acceso del usuario.
- Integrar estos tres conceptos para construir un sistema de seguridad robusto y eficiente en una aplicación Angular.

## 1. ¿Qué es un JWT (JSON Web Token)?

Un **JWT** (pronunciado "jot") es un estándar abierto (`RFC 7519`) que define una forma compacta y autónoma de transmitir información de forma segura entre partes como un objeto JSON. A diferencia de las sesiones tradicionales, que obligan al servidor a mantener un estado (una entrada en memoria o en la base de datos para cada usuario activo), el JWT es un **token sin estado (stateless)**. Esto significa que el servidor no necesita guardar registros de los tokens emitidos. Esta naturaleza sin estado lo hace ideal para arquitecturas de microservicios y APIs RESTful, ya que cada solicitud que el cliente envía al servidor contiene toda la información necesaria para su verificación, sin necesidad de consultas adicionales. Esto mejora la escalabilidad y la eficiencia de la aplicación.

Un JWT se compone de tres partes separadas por puntos (`.`) que se codifican en Base64 URL Safe para ser fácilmente transmitidas en una URL o un encabezado HTTP.

1. **Header (Encabezado)**: Es un objeto JSON que contiene metadatos sobre el token. Aquí se especifica el tipo de token (`JWT`) y el algoritmo de firma utilizado. Por ejemplo:
`{"alg":"HS256", "typ":"JWT"}`.  
    El algoritmo (`alg`) puede ser simétrico como **HS256**, que utiliza una clave secreta única y compartida entre el servidor para firmar y verificar el token, o asimétrico como **RS256**, que usa un par de claves pública/privada, ofreciendo una capa de seguridad más avanzada.
2. **Payload (Carga Útil)**: Esta es la parte más importante del JWT, ya que contiene los claims o "declaraciones" sobre el usuario y el token mismo. Estos claims son pares clave-valor que transportan información. Existen tres tipos de claims:
    - **Claims Estándar**: Son claims predefinidos y recomendados por el estándar para estandarizar la información. Ejemplos comunes incluyen `iss` (emisor del token), `sub` (sujeto, que generalmente es el ID del usuario), `exp` (fecha de expiración, un claim crucial para la seguridad) e `iat` (fecha de emisión).
    - **Claims Públicos**: Son claims personalizados que puedes definir, pero se recomienda registrarlos en el Registro de Claims de IANA para evitar colisiones de nombres. En la práctica, se usan para claims personalizados que no son sensibles.
    - **Claims Privados**: Claims personalizados que se definen entre el servidor y el cliente para transmitir información específica. Por ejemplo, podríamos incluir un claim `role` para almacenar el rol del usuario (`"role": "admin"`) o un `id` del usuario para identificarlo.

    Es crucial recordar que, aunque el payload sea parte de un token seguro, solo está **codificado, no encriptado**. Esto significa que cualquiera puede decodificarlo fácilmente y leer su contenido. Por esta razón, **nunca se debe almacenar información sensible** como contraseñas, números de tarjeta de crédito o datos personales privados en el payload.

3. **Signature (Firma)**: Esta es la parte que garantiza la integridad y autenticidad del token. Se crea tomando el header codificado, el payload codificado y un **"secreto"** que solo el servidor conoce. Estos tres elementos se combinan y se aplica un algoritmo de hash. Por ejemplo, con `HS256`, la fórmula sería `HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`. La firma es la que asegura que el token no haya sido manipulado. Si alguien intenta alterar el header o el payload, la firma resultante será diferente, y el servidor rechazará el token, considerándolo inválido.

## 2. ¿Qué es un Interceptor?

En Angular, un **Interceptor** es una característica poderosa del `HttpClientModule` que te permite interceptar las peticiones HTTP que salen de la aplicación y las respuestas que regresan. Piensa en él como un "middleware" o una tubería por la que pasan todas las peticiones y respuestas, permitiéndote aplicar lógica de manera centralizada y transparente.

La función principal de un interceptor es actuar como un intermediario entre tu aplicación y el servidor. Esto es extremadamente útil para tareas transversales que deben aplicarse a múltiples peticiones, como:

- **Adjuntar el JWT**: En el contexto de la seguridad, el uso más común es tomar el JWT almacenado en el `localStorage` o en alguna variable de servicio y agregarlo automáticamente al encabezado `Authorization` de cada petición saliente. Esto nos evita tener que escribir la misma lógica de añadir el token en cada servicio o componente que haga una petición al servidor.

    ```Typescript
    import { Injectable } from '@angular/core';
    import {
    HttpRequest,
    HttpHandler,
    HttpEvent,
    HttpInterceptor
    } from '@angular/common/http';
    import { Observable } from 'rxjs';

    /**
     * Este interceptor se encarga de agregar el token JWT a todas las peticiones
     * HTTP salientes. Esto asegura que el servidor pueda autenticar al usuario
     * de forma automática en cada llamada a una API protegida.
     */
    @Injectable()
    export class AuthInterceptor implements HttpInterceptor {

    constructor() {}

    intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
        // 1. Obtener el token de autenticación del localStorage.
        //    En un caso real, podrías obtenerlo de un servicio de autenticación.
        const authToken = localStorage.getItem('access_token');

        // 2. Si existe un token, clonamos la petición y le añadimos el encabezado de autorización.
        //    Esto es crucial, ya que las peticiones de Angular son inmutables.
        if (authToken) {
        // El formato estándar es 'Bearer' seguido del token.
        const authRequest = request.clone({
            setHeaders: {
            Authorization: `Bearer ${authToken}`
            }
        });
        // 3. Pasamos la petición modificada al siguiente manejador.
        return next.handle(authRequest);
        }

        // 4. Si no hay token, pasamos la petición original sin modificarla.
        return next.handle(request);
    }
    }
    ```

- **Manejar errores de autenticación**: Un interceptor puede escuchar las respuestas del servidor. Por ejemplo, puede detectar una respuesta con un código de estado `401 Unauthorized` o `403 Forbidden`. Al detectar estos errores, el interceptor puede ejecutar una acción específica, como redirigir al usuario a la página de login, refrescar el token de autenticación (si es una respuesta `401`) o mostrar un mensaje de error global al usuario. Esto centraliza el manejo de errores de autenticación y evita la duplicación de código.

    ```Typescript
    import { Injectable } from '@angular/core';
    import {
    HttpRequest,
    HttpHandler,
    HttpEvent,
    HttpInterceptor,
    HttpErrorResponse
    } from '@angular/common/http';
    import { Observable, throwError } from 'rxjs';
    import { catchError } from 'rxjs/operators';
    import { Router } from '@angular/router';

    /**
     * Este interceptor se encarga de dos tareas principales:
     * 1. Adjuntar el token JWT a todas las peticiones HTTP salientes para la autenticación.
     * 2. Manejar los errores de autenticación, especialmente el código 401 (Unauthorized),
     * redirigiendo al usuario a la página de inicio de sesión.
     */
    @Injectable()
    export class AuthInterceptor implements HttpInterceptor {

    constructor(private router: Router) {}

    intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
        // 1. Obtener el token de autenticación del localStorage.
        const authToken = localStorage.getItem('access_token');

        let authRequest = request;

        // 2. Si existe un token, clonamos la petición y le añadimos el encabezado de autorización.
        if (authToken) {
        authRequest = request.clone({
            setHeaders: {
            Authorization: `Bearer ${authToken}`
            }
        });
        }

        // 3. Pasamos la petición (modificada o no) al siguiente manejador y manejamos los errores.
        return next.handle(authRequest).pipe(
        // Utilizamos `catchError` para interceptar la respuesta de error del servidor.
        catchError((error: HttpErrorResponse) => {
            // 4. Verificamos si el error es de tipo HttpErrorResponse y si el estado es 401.
            if (error.status === 401) {
            // Si el token es inválido o ha expirado, redirigimos al usuario a la página de login.
            this.router.navigate(['/login']);
            }
            // 5. Es importante relanzar el error para que otros componentes
            //    puedan manejarlo si es necesario.
            return throwError(() => error);
        })
        );
    }
    }
    ```

- **Logging y depuración**: Puedes usar un interceptor para registrar todas las peticiones y respuestas, lo cual es muy útil para la depuración y la auditoría. Esto te permite ver qué peticiones se están haciendo, qué datos se están enviando y qué respuestas se están recibiendo, todo en un solo lugar.

```Typescript
import { Injectable } from '@angular/core';
import {
  HttpRequest,
  HttpHandler,
  HttpEvent,
  HttpInterceptor,
  HttpErrorResponse
} from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, tap, finalize } from 'rxjs/operators';
import { Router } from '@angular/router';

/**
 * Este interceptor es una solución completa para manejar tres tareas:
 * 1. Adjuntar el token JWT a todas las peticiones HTTP salientes para la autenticación.
 * 2. Manejar los errores de autenticación, especialmente el código 401 (Unauthorized),
 * redirigiendo al usuario a la página de inicio de sesión.
 * 3. Implementar un registro (logging) de las peticiones para la depuración y
 * el monitoreo del rendimiento.
 */
@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  constructor(private router: Router) {}

  intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
    // Registra el inicio de la petición y el tiempo de inicio.
    const startTime = Date.now();
    console.log(`[HTTP START] Petición a: ${request.url}`);

    // 1. Obtener el token de autenticación del localStorage.
    const authToken = localStorage.getItem('access_token');
    let authRequest = request;

    // 2. Si existe un token, clonamos la petición y le añadimos el encabezado.
    if (authToken) {
      authRequest = request.clone({
        setHeaders: {
          Authorization: `Bearer ${authToken}`
        }
      });
    }

    // 3. Pasamos la petición (modificada o no) al siguiente manejador y manejamos los eventos.
    return next.handle(authRequest).pipe(
      // `tap` permite realizar efectos secundarios (como logging) sin modificar la respuesta.
      tap((event: HttpEvent<unknown>) => {
        // Registramos la respuesta exitosa para depuración.
        // `tap` se ejecuta tanto para peticiones exitosas como para errores.
        // Aquí filtramos solo para las respuestas.
        if (event.type === 4) { // HttpEventType.Response
          console.log(`[HTTP SUCCESS] Respuesta recibida de: ${request.url}`, event);
        }
      }),
      // `catchError` intercepta errores del servidor.
      catchError((error: HttpErrorResponse) => {
        // Manejamos el error 401 para redirigir al usuario.
        if (error.status === 401) {
          console.error('[HTTP ERROR] 401 Unauthorized. Redirigiendo a /login.');
          this.router.navigate(['/login']);
        }
        // Relanzamos el error para que otros componentes lo manejen.
        return throwError(() => error);
      }),
      // `finalize` se ejecuta cuando el observable se completa o se emite un error.
      finalize(() => {
        // Calculamos el tiempo total que tomó la petición y lo registramos.
        const endTime = Date.now();
        const duration = endTime - startTime;
        console.log(`[HTTP END] Petición a: ${request.url} completada en ${duration}ms`);
      })
    );
  }
}
```

- **Mostrar un "spinner" de carga**: Un interceptor puede detectar el inicio de una petición y mostrar un indicador de carga global en la interfaz de usuario, y ocultarlo cuando la petición finaliza.

## 3. ¿Qué es un Guard?

Un `Guard` en Angular es una interfaz que le dice al router si puede o no cargar una ruta específica. Actúa como un "guardia de seguridad" que se interpone entre la navegación del usuario y una ruta determinada, controlando el flujo de navegación basándose en una lógica específica, como el estado de autenticación de un usuario.

Existen varios tipos de Guards, cada uno con una función específica en el ciclo de vida de la navegación:

- `CanActivate`: Este es el guard más común para la autenticación. Se utiliza para decidir si un usuario puede acceder a una ruta. Por ejemplo, si un usuario no ha iniciado sesión, el Guard `CanActivate` puede bloquear el acceso a una ruta como `/dashboard`. Si el guard devuelve `true`, la navegación continúa; si devuelve `false`, la navegación se detiene. La mejor práctica es que, si un guard devuelve `false`, redirija al usuario a otra página (como el login), lo cual se logra devolviendo un `UrlTree`.
- `CanDeactivate`: Se usa para decidir si un usuario puede **salir** de una ruta. Esto es útil para prevenir que el usuario abandone un formulario con cambios sin guardar, pidiéndole una confirmación antes de salir.
- `CanLoad`: Este guard es crucial para la optimización. Determina si el router puede cargar un módulo de forma perezosa (lazy-loading). Es útil para evitar que se descargue un módulo completo del que un usuario no tiene permisos de acceso, mejorando así el tiempo de carga inicial de la aplicación.

La principal función del Guard en el contexto de la autenticación es **centralizar la lógica de protección**. En lugar de verificar el estado de autenticación en cada componente, el Guard se encarga de:

- **Verificar el estado de autenticación**: El Guard inyecta un servicio de autenticación (`AuthService`) y llama a un método para verificar si el usuario tiene un token válido o está logueado.
- **Decidir la acción**: Si el servicio de autenticación indica que el usuario no está logueado, el Guard bloquea la navegación y puede usar el `Router` para redirigir al usuario a la página de login.

Combinando estos tres conceptos, podemos construir un sistema de seguridad robusto: el `Guard` protege nuestras rutas, el `Interceptor` automatiza la comunicación segura con el servidor y el `JWT` es el token que lo hace posible.
