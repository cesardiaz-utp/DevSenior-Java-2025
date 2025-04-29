## Ejercicio Autónomo: Construyendo un Mini Catálogo de Productos

**Curso:** Introducción a Spring Boot
**Tema:** Aplicando Endpoints REST, Modelos y Estructura
**Objetivo Principal:** Crear una pequeña API REST funcional que permita ver y agregar productos básicos.

---

### Objetivos del Ejercicio:

* Aplicar los conocimientos sobre la estructura de proyectos Spring Boot (`controller`, `model`).
* Crear y configurar un nuevo **Controller** para manejar un recurso diferente (Productos).
* Definir una clase **Modelo** (`Product`) con sus atributos básicos.
* Implementar un endpoint `GET` para obtener una lista de todos los productos.
* Implementar un endpoint `GET` para obtener un producto específico usando `@PathVariable`.
* Implementar un endpoint `POST` para agregar un nuevo producto usando `@RequestBody`.
* Practicar la ejecución de la aplicación y la prueba de los endpoints.

### Contexto del Ejercicio:

Vas a crear la base de una API para un mini catálogo de productos. Por ahora, los productos se almacenarán simplemente en una lista en memoria (lo que significa que se perderán cada vez que reinicies la aplicación). En futuras sesiones, aprenderemos a persistir datos en una base de datos.

### Tu Tarea:

Continúa trabajando en tu proyecto `introduccion-springboot`. Debes crear las siguientes funcionalidades:

1.  **Define la Clase Modelo `Product`:**
    * En el paquete `model`, crea una nueva clase llamada `Product.java`.
    * Esta clase debe tener al menos los siguientes atributos privados:
        * `Long id;` (Para identificar el producto)
        * `String nombre;`
        * `double precio;`
    * Añade un **constructor vacío** (esencial para que Spring/Jackson pueda crear objetos `Product` a partir del JSON entrante en las peticiones POST).
    * Añade un **constructor con todos los argumentos** (útil para crear objetos fácilmente en el código).
    * Añade todos los métodos **getter** y **setter** para los atributos.
    * (Opcional) Añade un método `toString()` para facilitar la impresión del objeto en los logs.

    ```java
    // Esqueleto básico de Product.java
    package com.midominio.curso.introduccion.model; // Asegúrate de usar tu paquete base

    public class Product {
        private Long id;
        private String nombre;
        private double precio;

        // Constructor vacío
        public Product() {
        }

        // Constructor con argumentos
        public Product(Long id, String nombre, double precio) {
            this.id = id;
            this.nombre = nombre;
            this.precio = precio;
        }

        // Getters y Setters (¡debes añadirlos!)
        // public Long getId() { ... }
        // public void setId(Long id) { ... }
        // etc.

        // Opcional: toString()
        // @Override
        // public String toString() { ... }
    }
    ```

2.  **Crea el Controlador `ProductController`:**
    * En el paquete `controller`, crea una nueva clase llamada `ProductController.java`.
    * Anótala con `@RestController`.
    * Dentro de esta clase, crea una **lista estática y mutable** para simular la base de datos en memoria. Inicialízala con algunos productos de ejemplo. Necesitarás importar `java.util.List` y `java.util.ArrayList`.

    ```java
    // Esqueleto básico de ProductController.java
    package com.midominio.curso.introduccion.controller; // Asegúrate de usar tu paquete base

    import com.midominio.curso.introduccion.model.Product; // Importar la clase Product
    import org.springframework.web.bind.annotation.*; // Importar anotaciones de Spring Web
    import java.util.ArrayList; // Importar ArrayList
    import java.util.List; // Importar List
    import java.util.Map; // Podría ser útil si retornas mensajes simples

    @RestController
    @RequestMapping("/api/products") // Opcional pero recomendado: prefijo para todos los endpoints de este controlador
    public class ProductController {

        // Lista estática en memoria para almacenar productos temporalmente
        private static List<Product> products = new ArrayList<>();
        private static Long nextId = 1L; // Para asignar IDs únicos (simulado)

        // Bloque estático para inicializar la lista al cargar la clase
        static {
            products.add(new Product(nextId++, "Laptop", 1200.00));
            products.add(new Product(nextId++, "Mouse", 25.00));
            products.add(new Product(nextId++, "Teclado Mecánico", 90.00));
        }

        // Aquí debes añadir tus métodos para manejar las peticiones
    }
    ```
    * *(Nota: Usar `@RequestMapping` a nivel de clase (`@RequestMapping("/api/products")`) es una buena práctica para agrupar rutas. Así, todos los `@GetMapping`, `@PostMapping`, etc., dentro de esta clase estarán bajo `/api/products`.)*

3.  **Implementa el Endpoint GET para todos los Productos:**
    * Crea un método en `ProductController` que responda a peticiones `GET` en la ruta base (`/api/products`).
    * El método debe retornar la lista estática de productos. Spring Boot la convertirá automáticamente a un array JSON.

    ```java
    // Dentro de ProductController
    @GetMapping // Mapea a GET /api/products (por el @RequestMapping a nivel de clase)
    public List<Product> getAllProducts() {
        return products; // Retorna la lista completa
    }
    ```

4.  **Implementa el Endpoint GET para un Producto por ID:**
    * Crea un método en `ProductController` que responda a peticiones `GET` en la ruta `/api/products/{id}`.
    * Usa `@PathVariable Long id` para capturar el ID de la URL.
    * Busca en la lista estática el producto que coincida con el ID proporcionado.
    * Si encuentras el producto, retórnalo. Spring Boot lo convertirá a JSON.
    * Si no encuentras el producto, ¿qué puedes retornar por ahora? Puedes retornar `null`. Spring Boot por defecto retornará un estado 200 OK con cuerpo vacío en ese caso. *(En futuras clases veremos cómo retornar un estado 404 Not Found más apropiado).*

    ```java
    // Dentro de ProductController
    @GetMapping("/{id}") // Mapea a GET /api/products/{id}
    public Product getProductById(@PathVariable Long id) {
        // Itera sobre la lista para encontrar el producto con el ID dado
        for (Product product : products) {
            if (product.getId().equals(id)) {
                return product; // Retorna el producto si lo encuentra
            }
        }
        return null; // Retorna null si no se encuentra (resultará en 200 OK vacío por defecto)
    }
    ```

5.  **Implementa el Endpoint POST para agregar un nuevo Producto:**
    * Crea un método en `ProductController` que responda a peticiones `POST` en la ruta base (`/api/products`).
    * Usa `@RequestBody Product newProduct` para recibir los datos del nuevo producto desde el cuerpo de la petición (Spring Boot/Jackson intentará convertir el JSON entrante a un objeto `Product`).
    * Asigna un nuevo ID único al `newProduct` (puedes usar el contador `nextId` y luego incrementarlo).
    * Agrega el `newProduct` a la lista estática.
    * Retorna el `newProduct` que acabas de agregar (con su ID asignado).

    ```java
    // Dentro de ProductController
    @PostMapping // Mapea a POST /api/products
    public Product addProduct(@RequestBody Product newProduct) {
        // Asigna el siguiente ID disponible y luego incrementa el contador
        newProduct.setId(nextId++);
        // Agrega el nuevo producto a la lista en memoria
        products.add(newProduct);
        // Retorna el producto recién agregado (incluirá el ID asignado)
        return newProduct;
    }
    ```

### Pasos para Completar el Ejercicio:

1.  Implementa las clases `Product` y `ProductController` como se describe arriba en los paquetes correctos (`model` y `controller`).
2.  Asegúrate de añadir todos los imports necesarios en ambas clases.
3.  Asegúrate de añadir los Getters y Setters en la clase `Product`.
4.  Inicia tu aplicación Spring Boot (ejecutando la clase principal con el método `main()`).
5.  Abre tu navegador, `curl`, Postman o Insomnia para probar los endpoints.

### Cómo Probar tus Endpoints:

Asegúrate de usar el puerto correcto si lo cambiaste en `application.properties` (ej: 8081).

* **GET todos los productos:**
    * **URL:** `http://localhost:[PUERTO]/api/products`
    * **Método:** GET
    * **Resultado esperado:** Un array JSON con los 3 productos iniciales.
* **GET un producto por ID:**
    * **URL:** `http://localhost:[PUERTO]/api/products/1` (o 2, 3, etc.)
    * **Método:** GET
    * **Resultado esperado:** Un objeto JSON que representa el producto con ese ID, o una respuesta vacía si el ID no existe.
* **POST para agregar un nuevo producto:**
    * **URL:** `http://localhost:[PUERTO]/api/products`
    * **Método:** POST
    * **Cuerpo (Body):** JSON (raw -> JSON) con los datos del nuevo producto (¡NO incluyas el ID, el servidor lo asignará!).
        ```json
        {
          "nombre": "Tablet",
          "precio": 350.50
        }
        ```
    * **Encabezado (Header):** `Content-Type: application/json`
    * **Resultado esperado:** Un objeto JSON representando el producto que acabas de crear, incluyendo el ID asignado por el servidor (debería ser 4 en la primera adición).
    * *Después de un POST exitoso, prueba a hacer un `GET /api/products` de nuevo para ver si el nuevo producto aparece en la lista.*

### Sugerencias y Desafíos Adicionales (Opcional):

* Usa tu herramienta de IA (Cursor AI) para generar automáticamente los Getters, Setters y el constructor con argumentos en la clase `Product`.
* Usa la IA para generar el Javadoc completo para la clase `Product` y para cada método en `ProductController`. **¡Recuerda revisarlo y completarlo!**
* ¿Qué pasa si intentas agregar un producto con un precio negativo? ¿O con un nombre vacío? (Aún no hemos visto validación, pero piensa en ello).
* Intenta simular errores (como en la sesión práctica) en este nuevo controlador y usa la IA para entender los logs.

¡Manos a la obra! Este ejercicio te ayudará a afianzar los conceptos básicos de la creación de APIs REST con Spring Boot. ¡No dudes en consultar la documentación o usar la IA si te quedas atascado!

---