# Ejercicio Autónomo: Refactorizando con SOLID y Cursor AI

**Objetivo del Ejercicio:**

Aplicarás los principios SOLID (especialmente DIP y SRP) y utilizarás las herramientas de refactorización de **Cursor AI** para mejorar el diseño del código de ejemplo `BadProductService` que analizamos en la tutoría.

**Punto de Partida:**

Tendrás disponible en tu entorno Cursor AI el código del ejemplo "Antes" que vimos, compuesto por las clases `Product.java`, `ConcreteProductRepository.java`, `EmailNotificationService.java`, y `BadProductService.java`.

## Paso 1: Aplicando DIP a la Dependencia del Repositorio

**Concepto:** El Principio de Inversión de Dependencia (DIP) nos dice que debemos depender de abstracciones (interfaces) en lugar de detalles (clases concretas). Actualmente, `BadProductService` depende directamente de `ConcreteProductRepository`. Vamos a invertir esta dependencia.

**Tu Tarea:** Crear una interfaz para el repositorio y hacer que `BadProductService` dependa de esa interfaz.

**Pasos a seguir en Cursor AI:**

1. **Navega a `ConcreteProductRepository.java`:** Abre este archivo en el editor de Cursor AI.
2. **Extrae la Interfaz:** Haz clic derecho sobre el nombre de la clase `ConcreteProductRepository`. En el menú contextual, busca la opción "Refactor" (o similar) y selecciona "Extract Interface".
3. **Configura la Interfaz:**
    * Nombra la nueva interfaz `ProductRepository`.
    * Asegúrate de que el método `void save(Product product)` (y si existe, `Product findById(String id)`) esté seleccionado para incluir en la interfaz.
    * Haz clic en "OK" o "Refactor".
4. **Verifica el Resultado:** Cursor AI creará un nuevo archivo `ProductRepository.java` y modificará `ConcreteProductRepository.java` para que implemente esta nueva interfaz (`class ConcreteProductRepository implements ProductRepository`).
5. **Modifica `BadProductService`:** Abre `BadProductService.java`.
    * Elimina la línea donde se crea la instancia concreta: `ConcreteProductRepository repository = new ConcreteProductRepository();`.
    * Declara un campo privado cuyo tipo sea la *interfaz* que acabas de crear: `private final ProductRepository productRepository;`.
    * **Inyecta la Dependencia:** Crea un constructor para `BadProductService` que acepte un parámetro de tipo `ProductRepository` y asígnalo al campo. Añade la anotación `@Autowired` a este constructor.
        * **Ayuda de Cursor AI:** Después de declarar `private final ProductRepository productRepository;`, a menudo Cursor AI te sugerirá crear automáticamente el constructor con `@Autowired`. Acéptala si aparece, o escribe el constructor y `@Autowired` manualmente; Cursor AI te asistirá con la sintaxis.
6. **Actualiza el Uso del Repositorio:** En el método `addProduct`, cambia la llamada al método `save`: ya no usarás la variable `repository` creada localmente, sino el campo `productRepository` inyectado. La línea `repository.save(product);` se convierte en `productRepository.save(product);`.
7. **Verifica Dependencias:** (Opcional) Usa la función "Show Dependencies" en `BadProductService`. Deberías ver una dependencia a la *interfaz* `ProductRepository`, no a la clase concreta `ConcreteProductRepository`.
8. **Pregunta a Cursor AI:** Abre el chat y pregúntale a Cursor AI: "Looking at the refactored `BadProductService` and `ProductRepository`, how does this change apply the Dependency Inversion Principle?". Lee su explicación.

**Código Resultante (Parcial - `BadProductService` después del Paso 1):**

```java
// ... imports ...
import com.example.yourproject.repository.ProductRepository; // Ahora importamos la INTERFAZ

// Clase de servicio (aún viola SRP, pero ahora sigue DIP para el repositorio)
public class BadProductService {

    // Dependencia a la ABSTRACCIÓN (Interfaz del Repositorio)
    private final ProductRepository productRepository;

    // Constructor Inyectado por Spring (asumimos contexto Spring)
    // @Autowired // Añadir si es un bean de Spring
    public BadProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
        System.out.println("BadProductService initialized with ProductRepository.");
    }

    public void addProduct(Product product) {
        System.out.println("Executing addProduct in BadProductService");

        // --- Responsabilidad 1: Validación ---
        // ... (lógica de validación sigue aquí) ...
        System.out.println("Product validation successful.");

        // --- Responsabilidad 2: Persistencia de Datos ---
        // Llama al método a través de la INTERFAZ inyectada
        productRepository.save(product); // <--- Uso de la interfaz inyectada
        System.out.println("Product saved via injected repository.");

        // --- Responsabilidad 3: Notificación ---
        // DIP Violation: Creación DIRECTA de otra dependencia concreta (aún pendiente)
        EmailNotificationService notificationService = new EmailNotificationService(); // <--- Violación DIP aquí (pendiente)

        // Uso de la dependencia concreta (pendiente)
        notificationService.sendNotification("admin@example.com", "New product added: " + product.getName());
        System.out.println("Notification sent via email.");
    }
    // ...
}
```

## Paso 2: Aplicando DIP a la Dependencia del Servicio de Notificación

**Concepto**: Aplicaremos el mismo principio DIP a la dependencia de EmailNotificationService.

**Tu Tarea**: Crear una interfaz para el servicio de notificación y hacer que BadProductService dependa de esa interfaz e inyectarla.

**Pasos a seguir en Cursor AI:**

1. Navega a `EmailNotificationService.java`: Abre este archivo.
1. **Extrae la Interfaz**: Haz clic derecho sobre `EmailNotificationService`. Selecciona "Refactor" -> "Extract Interface".
1. **Configura la Interfaz:**
    * Nombra la nueva interfaz `NotificationService`.
    * Asegúrate de que el método `void sendNotification(String recipient, String message)` esté seleccionado.
    * Haz clic en "OK".
1. **Verifica el Resultado**: Cursor AI creará `NotificationService.java` y modificará `EmailNotificationService.java` para implementarla.
1. Modifica `BadProductService`: Abre `BadProductService.java`.
    * Elimina la línea donde se crea la instancia concreta: `EmailNotificationService notificationService = new EmailNotificationService();`.
    * Declara un campo privado cuyo tipo sea la interfaz `NotificationService`: `private final NotificationService notificationService;`.
    * **Actualiza el Constructor**: Ve al constructor con `@Autowired` que creaste en el Paso 1. Añade un parámetro de tipo `NotificationService` y asígnalo al nuevo campo.
        * **Ayuda de Cursor AI**: Cursor AI te sugerirá actualizar la firma del constructor existente. O puedes escribir el parámetro y la asignación; la IA te asistirá. El constructor ahora inyectará dos dependencias.
    * Asegúrate de que el constructor sigue anotado con `@Autowired`.
1. **Actualiza el Uso de la Notificación**: En el método `addProduct`, la llamada al método `sendNotification` ya usaba la variable con ese nombre. Ahora esa variable se refiere al campo inyectado de tipo `NotificationService`. La línea `notificationService.sendNotification(...)` ya está correcta (si usaste el mismo nombre de variable).
1. **Verifica Dependencias**: (Opcional) Usa "Show Dependencies". Deberías ver dependencias a `ProductRepository` y `NotificationService` (ambas interfaces).
1. **Pregunta a Cursor AI**: "Explain how the refactored `BadProductService` now follows the Dependency Inversion Principle for both its dependencies".

**Código Resultante (Parcial - `BadProductService` después del Paso 2):**

```java
package com.example.yourproject.service;

import com.example.yourproject.model.Product;
import com.example.yourproject.repository.ProductRepository; // Importamos la INTERFAZ
import com.example.yourproject.service.notification.NotificationService; // Importamos la INTERFAZ

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service; // Añadir si es un bean de Spring

// Clase de servicio (aún viola SRP, pero ahora sigue DIP)
// @Service // Añadir si esta clase es un bean de Spring
public class BadProductService { // Podríamos empezar a pensar en renombrarla

    // Dependencias a las ABSTRACCIONES (Interfaces)
    private final ProductRepository productRepository;
    private final NotificationService notificationService;

    // Constructor Inyectado por Spring (asumimos contexto Spring) - ¡Ahora inyecta ambas interfaces!
    @Autowired
    public BadProductService(ProductRepository productRepository, NotificationService notificationService) {
        this.productRepository = productRepository;
        this.notificationService = notificationService;
        System.out.println("BadProductService initialized with dependencies.");
    }

    public void addProduct(Product product) {
        System.out.println("Executing addProduct in BadProductService");

        // --- Responsabilidad 1: Validación ---
        // Lógica de validación del producto (Aún pendiente de refactorizar SRP)
        if (product == null) {
            throw new IllegalArgumentException("Product cannot be null");
        }
        if (product.getName() == null || product.getName().isEmpty()) {
            throw new IllegalArgumentException("Product name cannot be empty");
        }
        if (product.getPrice() < 0) {
             throw new IllegalArgumentException("Product price cannot be negative");
        }
        // ... otras reglas de validación ...
        System.out.println("Product validation successful.");

        // --- Uso de Dependencia (Persistencia de Datos) ---
        // Llama al método a través de la INTERFAZ inyectada (DIP aplicado aquí)
        productRepository.save(product);
        System.out.println("Product saved via injected repository.");

        // --- Uso de Dependencia (Notificación) ---
        // Llama al método a través de la OTRA INTERFAZ inyectada (DIP aplicado aquí)
        notificationService.sendNotification("admin@example.com", "New product added: " + product.getName());
        System.out.println("Notification sent via injected notification service.");

        // Nota: Esta clase *aún* tiene múltiples responsabilidades (validación, salvar, notificar).
        // Refactorizar para SRP sería el siguiente paso lógico.
    }
    // ...
}
```

## Paso 3: Aplicando SRP al Extraer la Lógica de Validación (Opcional pero Recomendado)

**Concepto**: El Principio de Responsabilidad Única (SRP) nos dice que una clase debe tener solo una razón para cambiar. Actualmente, nuestro BadProductService (que podríamos renombrar a ProductService después) aún maneja la validación además de coordinar el guardado y la notificación. Vamos a mover la validación a su propia clase.

**Tu Tarea**: Crear un componente dedicado a la validación de productos y hacer que el servicio dependa de él.

**Pasos a seguir en Cursor AI:**

1. Navega a `BadProductService.java`: Abre este archivo.
1. **Identifica el Bloque de Validación**: Selecciona todo el bloque de código que realiza la validación del producto al inicio del método `addProduct`.
1. `Extrae a un Nuevo Método (Paso Intermedio)`: Opcional, pero a veces útil. Haz clic derecho sobre la selección y usa "Refactor" -> "Extract Method". Nombra el método `validateProduct`.
1. **Extrae a una Nueva Clase (Aplicando SRP)**: Selecciona el bloque de código de validación (si no lo extrajiste a un método, selecciona las líneas directamente; si lo extrajiste, selecciona la llamada al método `validateProduct` o el cuerpo del método `validateProduct`). Haz clic derecho y busca "Refactor" -> "Extract Class" (o similar).
    * Configura la Nueva Clase: Nómbrala `ProductValidator`.
    * Asegúrate de que los métodos relevantes (el método de validación) se muevan a la nueva clase.
    * **Resultado Esperado**: Cursor AI creará una nueva clase `ProductValidator` con el método de validación y, si es inteligente, modificará `BadProductService` para que use una instancia de `ProductValidator`.
1. **Crear la Interfaz para el Validador (Aplicando DIP nuevamente)**: Es buena práctica depender de interfaces. Si Cursor AI no creó una interfaz automáticamente para `ProductValidator` durante la extracción, navega a `ProductValidator.java`, haz clic derecho en el nombre de la clase y selecciona "Extract Interface". Nombra la interfaz `ProductValidator` (la clase de implementación podría ser `BasicProductValidator`).
1. **Modificar `BadProductService` (ahora `ProductService`) para Usar la Interfaz del Validador**: Abre la clase de servicio refactorizada.
    * Declara un nuevo campo privado para el validador, usando la interfaz: `private final ProductValidator productValidator;`.
    * Actualiza el constructor con `@Autowired` para aceptar un parámetro de tipo `ProductValidator` y asignarlo al campo.
    * En el método `addProduct`, reemplaza el bloque de validación (o la llamada al método `validateProduct` si lo extrajiste antes) por una llamada al método de validación del validador inyectado: `productValidator.validate(product);`.
    * **Renombrar la Clase de Servicio**: Ahora que `BadProductService` ya no es tan "mala" (al menos con respecto a estas violaciones), re-nómbrala a algo más apropiado como `ProductService` usando la función de renombrar de Cursor AI (Shift + F6), que actualizará todas las referencias.
1. **Verifica Dependencias**: Usa "Show Dependencies" en la clase ProductService renombrada. Deberías ver dependencias a ProductRepository, NotificationService y ProductValidator (todas interfaces).
1. **Pregunta a Cursor AI**: "Looking at the refactored ProductService and ProductValidator, how do these classes now follow the Single Responsibility Principle?".

**Código Resultante (Parcial - Clases después del Paso 3):**

```java
// ProductValidator.java - Nueva Interfaz
package com.example.yourproject.service.validation; // Ejemplo de paquete

import com.example.yourproject.model.Product;

public interface ProductValidator {
    void validate(Product product);
}

// BasicProductValidator.java - Implementación del Validador
package com.example.yourproject.service.validation; // Ejemplo de paquete

import com.example.yourproject.model.Product;
import org.springframework.stereotype.Component; // O @Service

// @Component // Para que Spring la gestione e inyecte
public class BasicProductValidator implements ProductValidator {

    @Override
    public void validate(Product product) {
        System.out.println("Executing validation in BasicProductValidator");
        if (product == null) {
            throw new IllegalArgumentException("Product cannot be null");
        }
        if (product.getName() == null || product.getName().isEmpty()) {
            throw new IllegalArgumentException("Product name cannot be empty");
        }
        if (product.getPrice() < 0) {
             throw new IllegalArgumentException("Product price cannot be negative");
        }
        // ... otras reglas de validación ...
        System.out.println("Product validation successful.");
    }
}

// ProductService.java - Clase de Servicio Refactorizada (Aplica DIP y SRP)
package com.example.yourproject.service;

import com.example.yourproject.model.Product;
import com.example.yourproject.repository.ProductRepository; // Usamos la INTERFAZ del repositorio
import com.example.yourproject.service.notification.NotificationService; // Usamos la INTERFAZ de notificación
import com.example.yourproject.service.validation.ProductValidator; // Usamos la INTERFAZ del validador

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service // Indicamos que es un bean de servicio de Spring
public class ProductService { // Renombrada y mejor diseñada

    // Dependencias inyectadas a las ABSTRACCIONES (Interfaces)
    private final ProductRepository productRepository;
    private final NotificationService notificationService;
    private final ProductValidator productValidator; // Nueva dependencia al validador

    // Constructor Inyectado por Spring - ¡Ahora inyecta las TRES interfaces!
    @Autowired
    public ProductService(ProductRepository productRepository,
                          NotificationService notificationService,
                          ProductValidator productValidator) { // Nuevo parámetro
        this.productRepository = productRepository;
        this.notificationService = notificationService;
        this.productValidator = productValidator; // Asignación
        System.out.println("ProductService initialized with all dependencies.");
    }

    // Método addProduct - ¡Ahora enfocado en coordinar!
    public void addProduct(Product product) {
        System.out.println("Executing addProduct in ProductService");

        // --- Delega la Validación ---
        productValidator.validate(product); // <--- Llama al validador inyectado

        // --- Uso de Dependencia (Persistencia de Datos) ---
        productRepository.save(product);
        System.out.println("Product saved via injected repository.");

        // --- Uso de Dependencia (Notificación) ---
        notificationService.sendNotification("admin@example.com", "New product added: " + product.getName());
        System.println("Notification sent via injected notification service.");

        // Ahora este método addProduct tiene una única responsabilidad:
        // Orquestar el proceso de añadir un producto, delegando las tareas específicas
        // a sus colaboradores inyectados.
    }
    // ... Otros métodos del servicio ...
}
```

## Revisión Final y Verificación

* **Tu Tarea**: Revisa el código final de `ProductService`, `ProductValidator`, `ProductRepository` y `NotificationService`. ¿Cómo se ve el código comparado con el `BadProductService` original?
* **Tu Tarea**: Discute contigo mismo (o con un compañero) cómo este nuevo diseño aplica SRP (cada clase hace una cosa) y DIP (las clases de alto nivel dependen de abstracciones).
* **Tu Tarea**: (Opcional) Si estás en un proyecto Spring Boot funcional, asegúrate de que `BasicProductValidator`, `ConcreteProductRepository` y `EmailNotificationService` estén anotados con `@Component` o `@Service` para que Spring pueda inyectarlos donde se solicitan las interfaces.
* **Tu Tarea**: (Opcional) Si tenías un controlador que usaba el servicio, verifica que ahora se inyecta el nuevo `ProductService` (Spring lo hará automáticamente si está configurado) y que la aplicación funciona.

**Qué esperamos lograr**: Habrás completado un ciclo de diagnóstico y refactorización aplicando principios SOLID con la ayuda de Cursor AI. Este es el tipo de proceso que realizarás continuamente como desarrollador para mejorar la calidad de tu código.

---

¡Excelente trabajo! Ahora tienes experiencia práctica en la aplicación de SOLID y en el uso de herramientas de IA para asistirte en este proceso.
