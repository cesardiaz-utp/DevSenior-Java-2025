## Tutoría: Entendiendo la estructura Spring y sus componentes

### Antes de Empezar: Prepara tu Proyecto

Para esta sesión, continuaremos trabajando con el proyecto Spring Boot que creamos en la clase anterior (`introduccion-springboot`).

* Abre tu IDE (Cursor AI, IntelliJ, VS Code).
* Abre el proyecto `introduccion-springboot`.
* Asegúrate de que el JDK esté configurado correctamente.
* Ten a mano tu herramienta para probar APIs (navegador, `curl`, Postman, Insomnia).

### Objetivos de la Sesión:

Al finalizar esta sesión, serás capaz de:

* Reforzar la comprensión de la estructura de carpetas y dependencias de un proyecto Spring Boot.
* Crear endpoints REST usando diferentes métodos HTTP (`GET`, `POST`) y cómo capturar datos de la URL (`@PathVariable`) y de los parámetros de consulta (`@RequestParam`).
* Identificar y resolver errores comunes al desarrollar APIs (404, 400, 500) utilizando logs y herramientas de depuración básicas.
* Utilizar herramientas de IA (como Cursor AI) para diagnosticar errores y mejorar la calidad del código (nombres, documentación).

---

### Actividad 1: Validación del Proyecto y Estructura (15 min)

Vamos a asegurarnos de que nuestro proyecto esté listo y revisar la estructura que definimos.

1.  **Abre tu proyecto en el IDE.**
2.  **Revisa la estructura de paquetes:** En la vista de Proyecto de tu IDE, confirma que tienes los paquetes principales dentro de `src/main/java/com.midominio.curso.introduccion` (o el paquete base que hayas usado):
    * `config`
    * `controller`
    * `model`
    * `repository`
    * `service`
    * Verifica que tu clase principal `IntroduccionSpringbootApplication.java` esté en el paquete base.

3.  **Revisa las dependencias (`pom.xml` si usas Maven):**
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

4.  **Revisa la clase principal (`*Application.java`):**
    * Abre tu clase principal (ej: `IntroduccionSpringbootApplication.java`).
    * Confirma que tiene la anotación `@SpringBootApplication`. Recuerda que esta anotación es la que inicia el proceso de auto-configuración y escaneo de componentes de Spring Boot.

5.  **Ejecuta la aplicación:** Inicia la aplicación desde el método `main()` de tu clase principal. Confirma en la consola que arranca sin errores y que el servidor web (Tomcat) se inicia en el puerto que configuraste (por defecto 8080 o 8081 si lo cambiaste en `application.properties`).

    * *Si hay errores al arrancar:* Revisa los logs en la consola. A menudo son problemas de configuración o dependencias. Usa la IA para ayudarte a entender los mensajes de error si no son claros (ej: "Explica este error en la consola de Spring Boot: [pega aquí el log de error]").

---

### Actividad 2: Creando Endpoints con `@PathVariable` y `@RequestParam` (30 min)

Expandiremos nuestro `SaludoController` para hacerlo más interactivo, pasando datos en la URL.

**Conceptos Clave:**

* `@PathVariable`: Permite extraer un valor de una parte de la ruta de la URL. Ideal para identificar recursos específicos (ej: `/usuarios/123` donde `123` es el ID).
* `@RequestParam`: Permite extraer un valor de los parámetros de consulta de la URL (los que van después del `?`). Ideal para filtrar o paginar colecciones (ej: `/productos?categoria=electronica&pagina=2`).

1.  **Abre la clase `SaludoController.java`** (en el paquete `controller`). Ya debería tener el método `obtenerSaludo()` que creamos antes.

2.  **Crea un endpoint GET que use `@PathVariable`:** Queremos un endpoint como `/saludo/personalizado/{nombre}` donde `{nombre}` sea la parte variable de la ruta.

    * Añade el siguiente método a tu `SaludoController`:

        ```java
        package com.midominio.curso.introduccion.controller;

        import org.springframework.web.bind.annotation.GetMapping;
        import org.springframework.web.bind.annotation.PathVariable;
        import org.springframework.web.bind.annotation.RestController;
        import java.util.Map;

        @RestController
        public class SaludoController {

            // Endpoint que responde a /saludo y retorna JSON
            @GetMapping("/saludo")
            public Map<String, String> obtenerSaludo() {
                return Map.of("mensaje", "¡Hola REST desde Spring Boot!");
            }

            // Nuevo Endpoint que usa @PathVariable
            @GetMapping("/saludo/personalizado/{nombre}")
            public Map<String, String> obtenerSaludoPersonalizado(@PathVariable String nombre) {
                // El nombre de la variable en la ruta ({nombre}) debe coincidir
                // con el nombre del parámetro del método (@PathVariable String nombre).
                // Spring automáticamente extrae el valor de la URL y lo pasa al método.
                String mensaje = "¡Hola " + nombre + "!";
                return Map.of("saludo", mensaje);
            }
        }
        ```
    * **Reinicia la aplicación.**
    * **Prueba el nuevo endpoint:**
        * Abre tu navegador o usa `curl`/Postman.
        * Accede a `http://localhost:[PUERTO]/saludo/personalizado/Alice` (reemplaza `[PUERTO]` por 8080 u 8081).
        * Deberías ver la respuesta JSON: `{"saludo":"¡Hola Alice!"}`.
        * Prueba con otro nombre: `http://localhost:[PUERTO]/saludo/personalizado/Bob`. Deberías ver: `{"saludo":"¡Hola Bob!"}`.

3.  **Crea un endpoint GET que use `@RequestParam`:** Queremos un endpoint como `/saludo/param?nombre=valor&apellido=valor` que salude usando parámetros de consulta.

    * Añade el siguiente método a tu `SaludoController`:

        ```java
        // ... imports y clase SaludoController ...

        @RestController
        public class SaludoController {

            // ... método obtenerSaludo() ...
            // ... método obtenerSaludoPersonalizado() ...

            // Nuevo Endpoint que usa @RequestParam
            @GetMapping("/saludo/param")
            public Map<String, String> obtenerSaludoConParametros(
                    @RequestParam String nombre, // Parámetro 'nombre' es obligatorio por defecto
                    @RequestParam(required = false, defaultValue = "Sin Apellido") String apellido // Parámetro 'apellido' es opcional y tiene un valor por defecto
            ) {
                // Spring automáticamente extrae los valores de los parámetros de consulta
                // (después del ?) y los pasa a los parámetros del método.
                // Los nombres de los parámetros (@RequestParam nombre) deben coincidir
                // con los nombres en la URL (?nombre=...).
                String mensaje = "¡Hola " + nombre + " " + apellido + "!";
                return Map.of("saludoCompleto", mensaje);
            }
        }
        ```
    * **Reinicia la aplicación.**
    * **Prueba el nuevo endpoint:**
        * Accede a `http://localhost:[PUERTO]/saludo/param?nombre=Charlie&apellido=Brown`. Deberías ver: `{"saludoCompleto":"¡Hola Charlie Brown!"}`.
        * Prueba solo con el nombre (apellido opcional): `http://localhost:[PUERTO]/saludo/param?nombre=David`. Deberías ver: `{"saludoCompleto":"¡Hola David Sin Apellido!"}`.
        * Prueba sin el nombre (debería dar error 400 Bad Request): `http://localhost:[PUERTO]/saludo/param?apellido=Algo`. **¡Observa el error 400 y el log de la aplicación!** (Lo depuraremos en la próxima actividad).

---

### Actividad 3: Creando un Endpoint POST con `@RequestBody` (30 min)

Ahora, vamos a crear un endpoint que reciba datos en el cuerpo de la petición, típicamente JSON, usando el método HTTP POST.

**Conceptos Clave:**

* `@PostMapping`: Mapea peticiones HTTP POST a un método. Usado comúnmente para crear nuevos recursos.
* `@RequestBody`: Indica que el parámetro del método debe ser poblado con el contenido del cuerpo de la petición HTTP. Spring Boot, con Jackson (incluido en `spring-boot-starter-web`), intentará convertir automáticamente el JSON entrante a un objeto Java.

1.  **Crea una clase Modelo (`Persona`):** Necesitamos un objeto Java simple para representar los datos que recibiremos.

    * En el paquete `model` que creaste antes, crea una nueva clase Java llamada `Persona.java`.
    * Añade el siguiente código (usa tu paquete base correcto):

        ```java
        package com.midominio.curso.introduccion.model;

        // Las clases modelo son POJOs (Plain Old Java Objects)
        // con atributos y métodos getter/setter.
        // Spring Boot usará estos para convertir JSON a objetos y viceversa.

        public class Persona {
            private String nombre;
            private int edad;

            // Constructor vacío es necesario para la deserialización de JSON por Jackson
            public Persona() {
            }

            // Constructor con parámetros (opcional, pero útil)
            public Persona(String nombre, int edad) {
                this.nombre = nombre;
                this.edad = edad;
            }

            // Getters
            public String getNombre() {
                return nombre;
            }

            public int getEdad() {
                return edad;
            }

            // Setters (necesarios para Jackson al deserializar el JSON entrante)
            public void setNombre(String nombre) {
                this.nombre = nombre;
            }

            public void setEdad(int edad) {
                this.edad = edad;
            }

            // Opcional: toString() para depuración
            @Override
            public String toString() {
                return "Persona{" +
                       "nombre='" + nombre + '\'' +
                       ", edad=" + edad +
                       '}';
            }
        }
        ```

2.  **Crea un endpoint POST en `SaludoController` que use `@RequestBody`:**

    * Abre la clase `SaludoController.java` de nuevo.
    * Añade el siguiente import para la clase `Persona`: `import com.midominio.curso.introduccion.model.Persona;` (asegúrate de usar tu paquete base correcto).
    * Añade el siguiente método:

        ```java
        // ... imports ...
        import com.midominio.curso.introduccion.model.Persona; // Importar la clase Persona

        // ... anotación @RestController ...
        public class SaludoController {

            // ... métodos GET anteriores ...

            // Nuevo Endpoint que usa @PostMapping y @RequestBody
            // Mapea peticiones HTTP POST a la ruta /personas
            @PostMapping("/personas")
            // @RequestBody le dice a Spring que lea el cuerpo de la petición
            // y lo intente convertir a un objeto Persona.
            public Map<String, String> crearPersona(@RequestBody Persona persona) {
                // Aquí normalmente guardarías la persona en una base de datos
                // Por ahora, solo la imprimiremos en la consola para verificar que llegó correctamente.
                System.out.println("Recibida nueva persona: " + persona);

                // Retornamos una respuesta simple
                return Map.of("mensaje", "Persona recibida con éxito", "nombreRecibido", persona.getNombre());
            }
        }
        ```

3.  **Reinicia la aplicación.**

4.  **Prueba el nuevo endpoint POST:** Necesitarás una herramienta que permita enviar peticiones POST con un cuerpo JSON.

    * **Usando `curl`:** Abre tu terminal y ejecuta el siguiente comando:

        ```bash
        curl -X POST \
          http://localhost:[PUERTO]/personas \
          -H 'Content-Type: application/json' \
          -d '{"nombre": "Ferran", "edad": 30}'
        ```
        * `-X POST`: Especifica el método HTTP.
        * `http://localhost:[PUERTO]/personas`: La URL del endpoint.
        * `-H 'Content-Type: application/json'`: Indica que el cuerpo de la petición es JSON. ¡Importante para que Spring Boot sepa cómo deserializarlo!
        * `-d '{"nombre": "Ferran", "edad": 30}'`: El cuerpo de la petición en formato JSON.

    * **Usando Postman/Insomnia:**
        * Crea una nueva petición.
        * Selecciona el método **POST**.
        * Introduce la URL: `http://localhost:[PUERTO]/personas`.
        * Ve a la pestaña "Body" (Cuerpo).
        * Selecciona la opción "raw" (crudo).
        * Selecciona "JSON" en el desplegable de formato.
        * Introduce el siguiente JSON en el área de texto:
            ```json
            {
              "nombre": "Ferran",
              "edad": 30
            }
            ```
        * Envía la petición.

    * **Verifica:**
        * En tu herramienta, deberías recibir una respuesta JSON como: `{"mensaje":"Persona recibida con éxito","nombreRecibido":"Ferran"}`.
        * En la consola de tu aplicación Spring Boot, deberías ver la línea impresa: `Recibida nueva persona: Persona{nombre='Ferran', edad=30}`.

---

### Actividad 4: Simulación de Errores Comunes y su Resolución (40 min)

En el desarrollo real, los errores son parte del proceso. Aprender a identificarlos y resolverlos es clave. Vamos a simular algunos de los errores HTTP más comunes y cómo diagnosticarlos.

**Paso 1: ¡Provoca Errores Intencionadamente!**

Asegúrate de que tu aplicación esté corriendo. Luego, prueba los siguientes escenarios y observa la respuesta HTTP (código 4xx o 5xx) y los logs en la consola de tu aplicación:

1.  **Error 404 Not Found (Ruta Incorrecta):**
    * Intenta acceder en el navegador o con `curl` a una URL que **no existe**: `http://localhost:[PUERTO]/ruta-inexistente`.
    * **Observa:** Deberías obtener una página de "Whitelabel Error Page" con el estado **404 Not Found**.
    * **En la consola:** Busca mensajes indicando que no se encontró un mapeo para esa ruta.
    * **Simula un typo:** Cambia temporalmente el `@GetMapping("/saludo")` a `@GetMapping("/saludoo")` en `SaludoController`. Reinicia. Ahora, intenta acceder a `http://localhost:[PUERTO]/saludo`. ¡Deberías obtener 404 de nuevo! (Porque la ruta correcta es ahora `/saludoo`). Vuelve a corregir el typo (`/saludo`) y reinicia para la siguiente prueba.

2.  **Error 405 Method Not Allowed:**
    * Intenta enviar una petición **POST** a un endpoint que solo acepta **GET**: `http://localhost:[PUERTO]/saludo`.
    * **Usando `curl`:** `curl -X POST http://localhost:[PUERTO]/saludo`
    * **Observa:** Deberías obtener un error con estado **405 Method Not Allowed**.
    * **En la consola:** Verás un log indicando que el método POST no está permitido para esa ruta.

3.  **Error 400 Bad Request (`@RequestParam` Faltante):**
    * Recuerda el endpoint `@GetMapping("/saludo/param")`. Lo definimos con `@RequestParam String nombre`. Por defecto, `nombre` es **obligatorio**.
    * Intenta acceder a `http://localhost:[PUERTO]/saludo/param` **sin** el parámetro `?nombre=...`.
    * **Observa:** Deberías obtener un error con estado **400 Bad Request**.
    * **En la consola:** Busca logs que mencionen `Required request parameter 'nombre' for method ... is not present`.

4.  **Error 400 Bad Request (`@RequestBody` Formato Incorrecto):**
    * Recuerda el endpoint `@PostMapping("/personas")`. Espera un JSON que se pueda convertir a `Persona` (con `nombre` y `edad`).
    * Intenta enviar una petición POST, pero con un JSON **incorrecto**:
    * **Usando `curl`:**
        ```bash
        curl -X POST \
          http://localhost:[PUERTO]/personas \
          -H 'Content-Type: application/json' \
          -d '{"nombreee": "Eva", "edaddd": "treinta"}' # Nombres de campos incorrectos y tipo incorrecto para edad
        ```
    * **Observa:** Deberías obtener un error con estado **400 Bad Request**.
    * **En la consola:** Busca logs que mencionen errores de deserialización de JSON (`HttpMessageNotReadableException` es común).

5.  **Error 500 Internal Server Error (Error en el Código):**
    * Simula un error que ocurre *dentro* de tu código, como un `NullPointerException`.
    * En tu método `obtenerSaludo()` (o cualquiera), añade temporalmente una línea que cause un error:
        ```java
        @GetMapping("/saludo")
        public Map<String, String> obtenerSaludo() {
            String textoNulo = null;
            textoNulo.length(); // Esto causará un NullPointerException
            return Map.of("mensaje", "¡Hola REST desde Spring Boot!");
        }
        ```
    * **Reinicia la aplicación.**
    * Intenta acceder a `http://localhost:[PUERTO]/saludo`.
    * **Observa:** Deberías obtener una página de "Whitelabel Error Page" con el estado **500 Internal Server Error**.
    * **En la consola:** ¡Aquí verás la traza completa del error (`Stack Trace`)! Busca la línea que dice `Caused by: java.lang.NullPointerException`.

**Paso 2: Diagnosticando y Resolviendo Errores (Usando IA)**

Ahora, usa los logs y tu herramienta de IA para entender y solucionar estos errores.

1.  **Identifica el Código HTTP:** El código (404, 400, 405, 500) te da una pista del *tipo* de error.
    * 4xx: Error del cliente (la petición es incorrecta).
    * 5xx: Error del servidor (algo falló al procesar la petición).
2.  **Revisa los Logs de la Aplicación:** Los logs en la consola son tu mejor amigo. Contienen mensajes detallados sobre lo que salió mal. Busca líneas que empiecen con `ERROR` o que contengan trazas de excepciones (`Exception in thread "..." ...`).
3.  **Usa tu Herramienta de IA (Cursor AI u otra):**
    * Copia el log de error completo o las líneas más relevantes.
    * Pregunta a la IA: **"Explica este log de error de Spring Boot: [pega el log aquí]"**
    * Pregunta a la IA: **"Estoy recibiendo un error 400 Bad Request al llamar a este endpoint: [pega el código del método]. La URL que uso es: [pega la URL]. ¿Qué podría estar mal?"**
    * Pregunta a la IA: **"Mi aplicación me da un error 500 Internal Server Error y el log muestra un NullPointerException en esta línea: [pega la línea o el fragmento de código]. ¿Cómo puedo solucionar esto?"**
    * Pregunta a la IA: **"¿Por qué obtengo un 404 Not Found al acceder a /mi-ruta si creo que la definí correctamente?"** (La IA te recordará que revises la URL y el mapeo en el controlador).

**Tu Tarea:**
* Provoca cada uno de los errores mencionados.
* Observa el código de estado y los logs.
* Usa la IA para que te explique el error.
* Corrige el código o la URL para solucionar el error.
* Verifica que el endpoint funcione correctamente después de la corrección.

---

### Actividad 5: Revisión y Mejora del Código con IA (15 min)

Las herramientas de IA pueden ayudarte a mejorar la calidad de tu código y tu documentación.

1.  **Nombres Claros:**
    * Abre tu `SaludoController` y la clase `Persona`.
    * Usa la función de chat o un prompt en tu herramienta de IA.
    * Pregunta a la IA: **"Revisa los nombres de las clases, métodos y variables en este código [pega el código del controlador y la clase Persona]. ¿Son claros? ¿Hay alguna sugerencia según las convenciones de Java/Spring?"**
    * Analiza las sugerencias. ¿Ayudan a que el código sea más legible? (Por ejemplo, quizás sugiera usar `getNombrePersona` en lugar de `getNombre` en contextos donde podría haber ambigüedad, aunque en este caso simple no es estrictamente necesario).

2.  **Generación de Documentación (Javadoc):**
    * Usa la función de generación de Javadoc de tu IDE o pídele a la IA que lo haga.
    * Selecciona el método `crearPersona()` en `SaludoController`. Pídele a la IA: **"Genera Javadoc para este método."**
    * Haz lo mismo para la clase `Persona` y sus métodos getter/setter.
    * **¡Muy Importante!** Revisa el Javadoc generado. A menudo es un esqueleto. Complétalo explicando en detalle qué hace el método, qué espera en el `@param Persona persona` y qué retorna. Por ejemplo, para `crearPersona`:

        ```java
        /**
         * Recibe los datos de una persona en formato JSON y los procesa.
         * Actualmente, solo imprime los datos recibidos en la consola.
         *
         * @param persona El objeto Persona deserializado del cuerpo de la petición JSON.
         * @return Un Map con un mensaje de confirmación y el nombre de la persona recibida.
         */
        @PostMapping("/personas")
        public Map<String, String> crearPersona(@RequestBody Persona persona) {
            // ... código ...
        }
        ```
    * La IA es una herramienta para *asistencia*, no un reemplazo de tu comprensión y trabajo.

---

### Conclusiones y Próximos Pasos (10 min + Q&A)

En esta sesión práctica, hemos reforzado la estructura del proyecto, creado endpoints con diferentes formas de recibir datos de entrada (`@PathVariable`, `@RequestParam`, `@RequestBody`) y, muy importante, practicado la identificación y resolución de errores comunes usando los logs y la ayuda de IA.

**Recordad:**

* La estructura por capas ayuda a organizar el código.
* `@RestController` + Verbos HTTP (`@GetMapping`, `@PostMapping`, etc.) mapean peticiones a métodos.
* `@PathVariable` y `@RequestParam` son para datos en la URL; `@RequestBody` es para datos en el cuerpo (como JSON).
* Los logs de la aplicación y los códigos de estado HTTP son cruciales para depurar.
* Las herramientas de IA son asistentes poderosos, pero siempre debéis entender y verificar su output.

**Próximos Pasos Sugeridos para Seguir Practicando:**

* Añadir lógica de negocio real en la capa `service`.
* Implementar endpoints `PUT` y `DELETE`.
* Validar los datos recibidos en `@RequestBody` (ej: con `@Valid` y Bean Validation).
* Empezar a interactuar con datos reales o simulados (aunque aún no usemos base de datos).
