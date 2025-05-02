# Tutoría: Entendiendo la estructura Spring y sus componentes

## Antes de Empezar: Prepara tu Proyecto

Para esta sesión, continuaremos trabajando con el proyecto Spring Boot que creamos en la clase anterior (`introduccion-springboot`).

* Abre tu IDE (Cursor AI, IntelliJ, VS Code).
* Abre el proyecto `introduccion-springboot`.
* Asegúrate de que el JDK esté configurado correctamente.
* Ten a mano tu herramienta para probar APIs (navegador, `curl`, Postman, Insomnia).

## Objetivos de la Sesión

Al finalizar esta sesión, serás capaz de:

* Reforzar la comprensión de la estructura de carpetas y dependencias de un proyecto Spring Boot.
* Crear endpoints REST usando diferentes métodos HTTP (`GET`, `POST`) y cómo capturar datos de la URL (`@PathVariable`) y de los parámetros de consulta (`@RequestParam`).
* Identificar y resolver errores comunes al desarrollar APIs (404, 400, 500) utilizando logs y herramientas de depuración básicas.
* Utilizar herramientas de IA (como Cursor AI) para diagnosticar errores y mejorar la calidad del código (nombres, documentación).

## Actividad 1: Validación del Proyecto y Estructura (15 min)

Vamos a asegurarnos de que nuestro proyecto esté listo y revisar la estructura que definimos.

1. **Abre tu proyecto en el IDE.**
2. **Revisa la estructura de paquetes:** En la vista de Proyecto de tu IDE, confirma que tienes los paquetes principales dentro de `src/main/java/com.midominio.curso.introduccion` (o el paquete base que hayas usado):
    * `config`
    * `controller`
    * `model`
    * `repository`
    * `service`
    * Verifica que tu clase principal `IntroduccionSpringbootApplication.java` esté en el paquete base.
3. **Revisa las dependencias (`pom.xml` si usas Maven):**
    * Abre el archivo `pom.xml`.
    * Busca la sección `<dependencies>`.
    * Confirma que tienes la dependencia `spring-boot-starter-web`. Debería verse algo así:

        ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        ```

    * Esta dependencia es clave, ya que nos trae todo lo necesario para crear APIs REST (Spring MVC, Tomcat embebido, Jackson para JSON, etc.).
4. **Revisa la clase principal (`*Application.java`):**
    * Abre tu clase principal (ej: `IntroduccionSpringbootApplication.java`).
    * Confirma que tiene la anotación `@SpringBootApplication`. Recuerda que esta anotación es la que inicia el proceso de auto-configuración y escaneo de componentes de Spring Boot.
5. **Ejecuta la aplicación:** Inicia la aplicación desde el método `main()` de tu clase principal. Confirma en la consola que arranca sin errores y que el servidor web (Tomcat) se inicia en el puerto que configuraste (por defecto 8080 o 8081 si lo cambiaste en `application.properties`).
    * **Si hay errores al arrancar:** Revisa los logs en la consola. A menudo son problemas de configuración o dependencias. Usa la IA para ayudarte a entender los mensajes de error si no son claros (ej: "Explica este error en la consola de Spring Boot: [pega aquí el log de error]").

## Actividad 2: Creando Endpoints con `@PathVariable` y `@RequestParam` (30 min)

Expandiremos nuestro `SaludoController` para hacerlo más interactivo, pasando datos en la URL.

### Conceptos Clave

* `@PathVariable`: Permite extraer un valor de una parte de la ruta de la URL. Ideal para identificar recursos específicos (ej: `/usuarios/123` donde `123` es el ID).
* `@RequestParam`: Permite extraer un valor de los parámetros de consulta de la URL (los que van después del `?`). Ideal para filtrar o paginar colecciones (ej: `/productos?categoria=electronica&pagina=2`).

1. **Abre la clase `SaludoController.java`** (en el paquete `controller`). Ya debería tener el método `obtenerSaludo()` que creamos antes.
2. **Crea un endpoint GET que use `@PathVariable`:** Queremos un endpoint como `/saludo/personalizado/{nombre}` donde `{nombre}` sea la parte variable de la ruta.
    * Añade el siguiente método a tu `SaludoController`:

        ```java
        @GetMapping("/saludo/personalizado/{nombre}")
        public Map<String, String> obtenerSaludoPersonalizado(@PathVariable String nombre) {
            String mensaje = "¡Hola " + nombre + "!";
            return Map.of("saludo", mensaje);
        }
        ```

    * **Reinicia la aplicación.**
    * **Prueba el nuevo endpoint:**
        * Accede a `http://localhost:[PUERTO]/saludo/personalizado/Alice`.
        * Deberías ver la respuesta JSON: `{"saludo":"¡Hola Alice!"}`.
3. **Crea un endpoint GET que use `@RequestParam`:** Queremos un endpoint como `/saludo/param?nombre=valor&apellido=valor` que salude usando parámetros de consulta.
    * Añade el siguiente método a tu `SaludoController`:

        ```java
        @GetMapping("/saludo/param")
        public Map<String, String> obtenerSaludoConParametros(
                @RequestParam String nombre,
                @RequestParam(required = false, defaultValue = "Sin Apellido") String apellido) {
            String mensaje = "¡Hola " + nombre + " " + apellido + "!";
            return Map.of("saludoCompleto", mensaje);
        }
        ```

    * **Reinicia la aplicación.**
    * **Prueba el nuevo endpoint:**
        * Accede a `http://localhost:[PUERTO]/saludo/param?nombre=Charlie&apellido=Brown`.
        * Deberías ver: `{"saludoCompleto":"¡Hola Charlie Brown!"}`.

## Actividad 3: Simulación de Errores Comunes y su Resolución (40 min)

En el desarrollo real, los errores son parte del proceso. Aprender a identificarlos y resolverlos es clave. Vamos a simular algunos de los errores HTTP más comunes y cómo diagnosticarlos.

### Paso 1: Provoca Errores Intencionadamente

1. **Error 404 Not Found:** Intenta acceder a una URL inexistente.
2. **Error 405 Method Not Allowed:** Envía un POST a un endpoint que solo acepta GET.
3. **Error 400 Bad Request:** Omite parámetros requeridos en un endpoint.
4. **Error 500 Internal Server Error:** Introduce un error en el código, como un `NullPointerException`.

### Paso 2: Diagnosticando y Resolviendo Errores

1. Identifica el código HTTP.
2. Revisa los logs de la aplicación.
3. Usa herramientas de IA para entender y solucionar los errores.

## Conclusiones y Próximos Pasos

En esta sesión, reforzamos la estructura del proyecto, creamos endpoints interactivos y practicamos la resolución de errores comunes. Recuerda que los logs y las herramientas de IA son tus aliados para depurar y mejorar tu código.
