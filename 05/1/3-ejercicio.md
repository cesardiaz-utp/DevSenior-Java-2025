# Ejercicio Autónomo: Construyendo un Mini Catálogo de Productos

**Curso:** Introducción a Spring Boot  
**Tema:** Aplicando Endpoints REST, Modelos y Estructura  
**Objetivo Principal:** Crear una pequeña API REST funcional que permita ver y agregar productos básicos.

## Objetivos del Ejercicio

- Aplicar los conocimientos sobre la estructura de proyectos Spring Boot (`controller`, `model`).
- Crear y configurar un nuevo **Controller** para manejar un recurso diferente (Productos).
- Definir una clase **Modelo** (`Product`) con sus atributos básicos.
- Implementar un endpoint `GET` para obtener una lista de todos los productos.
- Implementar un endpoint `GET` para obtener un producto específico usando `@PathVariable`.
- Implementar un endpoint `POST` para agregar un nuevo producto usando `@RequestBody`.
- Practicar la ejecución de la aplicación y la prueba de los endpoints.

## Contexto del Ejercicio

Vas a crear la base de una API para un mini catálogo de productos. Por ahora, los productos se almacenarán simplemente en una lista en memoria (lo que significa que se perderán cada vez que reinicies la aplicación). En futuras sesiones, aprenderemos a persistir datos en una base de datos.

## Tareas a Realizar

### 1. Define la Clase Modelo `Product`

- En el paquete `model`, crea una nueva clase llamada `Product.java`.
- Esta clase debe tener al menos los siguientes atributos privados:
  - `Long id;` (Para identificar el producto).
  - `String nombre;`
  - `double precio;`
- Añade un **constructor vacío** (esencial para que Spring/Jackson pueda crear objetos `Product` a partir del JSON entrante en las peticiones POST).
- Añade un **constructor con todos los argumentos** (útil para crear objetos fácilmente en el código).
- Añade todos los métodos **getter** y **setter** para los atributos.
- (Opcional) Añade un método `toString()` para facilitar la impresión del objeto en los logs.

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

### 2. Crea el Controlador `ProductController`

- En el paquete `controller`, crea una nueva clase llamada `ProductController.java`.
- Anótala con `@RestController`.
- Dentro de esta clase, crea una **lista estática y mutable** para simular la base de datos en memoria. Inicialízala con algunos productos de ejemplo. Necesitarás importar `java.util.List` y `java.util.ArrayList`.

  ```java
  // Esqueleto básico de ProductController.java
  package com.midominio.curso.introduccion.controller; // Asegúrate de usar tu paquete base

  import com.midominio.curso.introduccion.model.Product; // Importar la clase Product
  import org.springframework.web.bind.annotation.*; // Importar anotaciones de Spring Web
  import java.util.ArrayList; // Importar ArrayList
  import java.util.List; // Importar List

  @RestController
  @RequestMapping("/api/products") // Prefijo para todos los endpoints de este controlador
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

### 3. Implementa los Endpoints

#### a. Endpoint `GET` para todos los Productos

- Crea un método en `ProductController` que responda a peticiones `GET` en la ruta base (`/api/products`).
- El método debe retornar la lista estática de productos. Spring Boot la convertirá automáticamente a un array JSON.

  ```java
  @GetMapping // Mapea a GET /api/products
  public List<Product> getAllProducts() {
      return products; // Retorna la lista completa
  }
  ```

#### b. Endpoint `GET` para un Producto por ID

- Crea un método en `ProductController` que responda a peticiones `GET` en la ruta `/api/products/{id}`.
- Usa `@PathVariable Long id` para capturar el ID de la URL.
- Busca en la lista estática el producto que coincida con el ID proporcionado.
- Si encuentras el producto, retórnalo. Si no, retorna `null` (en futuras clases veremos cómo retornar un estado 404).

  ```java
  @GetMapping("/{id}") // Mapea a GET /api/products/{id}
  public Product getProductById(@PathVariable Long id) {
      for (Product product : products) {
          if (product.getId().equals(id)) {
              return product; // Retorna el producto si lo encuentra
          }
      }
      return null; // Retorna null si no se encuentra
  }
  ```

#### c. Endpoint `POST` para agregar un nuevo Producto

- Crea un método en `ProductController` que responda a peticiones `POST` en la ruta base (`/api/products`).
- Usa `@RequestBody Product newProduct` para recibir los datos del nuevo producto desde el cuerpo de la petición.
- Asigna un nuevo ID único al `newProduct` (puedes usar el contador `nextId` y luego incrementarlo).
- Agrega el `newProduct` a la lista estática.
- Retorna el `newProduct` que acabas de agregar (con su ID asignado).

  ```java
  @PostMapping // Mapea a POST /api/products
  public Product addProduct(@RequestBody Product newProduct) {
      newProduct.setId(nextId++);
      products.add(newProduct);
      return newProduct;
  }
  ```

## Cómo Probar tus Endpoints

Asegúrate de usar el puerto correcto si lo cambiaste en `application.properties` (ej: 8081).

- **GET todos los productos:**
  - **URL:** `http://localhost:[PUERTO]/api/products`
  - **Método:** GET
  - **Resultado esperado:** Un array JSON con los 3 productos iniciales.

- **GET un producto por ID:**
  - **URL:** `http://localhost:[PUERTO]/api/products/1` (o 2, 3, etc.)
  - **Método:** GET
  - **Resultado esperado:** Un objeto JSON que representa el producto con ese ID, o una respuesta vacía si el ID no existe.

- **POST para agregar un nuevo producto:**
  - **URL:** `http://localhost:[PUERTO]/api/products`
  - **Método:** POST
  - **Cuerpo (Body):** JSON (raw -> JSON) con los datos del nuevo producto (¡NO incluyas el ID, el servidor lo asignará!).

    ```json
    {
      "nombre": "Tablet",
      "precio": 350.50
    }
    ```

  - **Encabezado (Header):** `Content-Type: application/json`
  - **Resultado esperado:** Un objeto JSON representando el producto que acabas de crear, incluyendo el ID asignado por el servidor.
