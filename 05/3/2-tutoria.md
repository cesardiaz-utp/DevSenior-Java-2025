# Tutoría: Diagnóstico y Refactorización Basada en Principios SOLID con Cursor AI

¡Hola a todos!

Bienvenidos a esta tutoría práctica donde pondremos en práctica los principios SOLID analizando código real (¡el vuestro y algunos ejemplos!) y aplicando refactorizaciones asistidas por **Cursor AI**. El objetivo es que desarrolléis una habilidad crucial: **identificar oportunidades para mejorar el diseño de vuestro código** y saber cómo aplicar SOLID para lograrlo.

## Objetivos de la Sesión

Al finalizar esta tutoría, serás capaz de:

* Evaluar si los principios SOLID están siendo respetados en proyectos de código Java (incluyendo los vuestros).
* Proponer mejoras concretas en el código basándonos en SOLID y asistidos por IA.
* Practicar la aplicación de refactorizaciones comunes para mejorar la estructura del código en equipo, enfocándonos especialmente en el Principio de Inversión de Dependencias (DIP).
* Utilizar **Cursor AI** como un mentor automático para obtener sugerencias de mejora de estructura y diseño orientado a objetos (OOP) y SOLID.

## Actividad 1: Lectura Crítica de Proyectos Entregados (Análisis Antes/Después) (20 minutos)

**Propósito:** Desarrollar tu "ojo" para detectar posibles violaciones de principios SOLID en el código. Veremos ejemplos reales (anónimos de vuestros proyectos) o ejemplos típicos para entender cómo se manifiestan en la práctica.

**Descripción de la Actividad:**

1. El tutor presentará fragmentos de código *anonimizados* de los proyectos entregados o ejemplos típicos que contienen alguna violación a un principio SOLID.
2. Como grupo, analizaremos el fragmento. El tutor guiará la discusión con preguntas como:

    * ¿Qué responsabilidades tiene esta clase/método? ¿Son demasiadas? (Piensa en el **Principio de Responsabilidad Única - SRP**).
    * ¿Sería fácil añadir un nuevo tipo de comportamiento sin modificar esta clase? (Piensa en el **Principio Abierto/Cerrado - OCP**).
    * ¿Esta clase depende de implementaciones concretas o de abstracciones? (Piensa en el **Principio de Inversión de Dependencia - DIP**).
    * ¿Esta interfaz obliga a quien la implementa a tener muchos métodos que no usará? (Piensa en el **Principio de Segregación de Interfaz - ISP**).
    * ¿Si una subclase sustituye a su padre, el código que usa el padre funcionaría igual? (Piensa en el **Principio de Sustitución de Liskov - LSP**).

3. Después de discutir la violación, se mostrará una versión **"después"** del fragmento de código, refactorizado para cumplir con el principio SOLID relevante.
4. Analizaremos la versión refactorizada, explicando cómo se aplica el principio y por qué es una mejora.

**Ejemplo Típico para Análisis (Discusión - Antes y Después):**

El tutor presentará un ejemplo similar al código `BadProductService` que verás en la Actividad 2, pero el foco de esta actividad es la *discusión grupal guiada* y el análisis del "antes" y "después" presentado por el tutor.

## Actividad 2: Detección de Clases Problemáticas (Diagnóstico Práctico con Cursor AI) (25 minutos)

**Propósito:** Practicar la identificación de dos problemas comunes relacionados con SOLID de forma más activa, utilizando Cursor AI para ayudarte en el análisis: clases con demasiadas responsabilidades (violación SRP) y acoplamiento fuerte a implementaciones concretas (violación DIP).

**Descripción de la Actividad:**

1. Trabajaremos con el siguiente ejemplo de código completo en tu entorno Cursor AI. Este es nuestro punto de partida "Antes".
2. Trabajando individualmente o en parejas, deberéis analizar este código en vuestras instancias de Cursor AI.
3. **Detectando Violaciones SRP:**
    * **Tu Tarea:** Abre la clase `BadProductService`. Lee su código y sus métodos. Piensa en cuántas "razones para cambiar" tiene. Si identificas más de una responsabilidad principal, probablemente viola el SRP.
    * **Con Cursor AI:** Selecciona la clase completa `BadProductService` en el editor. Abre el chat de AI (Cmd/Ctrl + L) y escribe un prompt como: "Analyze this Java class (`BadProductService`) for its responsibilities. What different tasks or concerns does it handle?". La respuesta de la IA puede confirmar tu análisis o señalar responsabilidades que no habías notado. Compara tu lista de responsabilidades con la de la IA.
4. **Detectando Acoplamiento Fuerte (Violación DIP):**
    * **Tu Tarea:** En el código de `BadProductService`, busca:
        * Campos (`private SomeClass variable;`) cuyo tipo sea una clase concreta (`class SomeClass`) en lugar de una interfaz (`interface SomeInterface`).
        * Lugares donde se crea una nueva instancia de otra clase concreta usando `new NombreDeOtraClaseConcreta()`.
        * Estos son indicios de dependencia de detalles (implementaciones concretas), violando DIP.
    * **Con Cursor AI:**
        * Utiliza la función de visualización de dependencias de Cursor AI. Haz clic derecho sobre la clase `BadProductService` y busca una opción como "Show Dependencies" o "Analyze Dependencies". Observa si el diagrama o la lista muestra dependencias directas a las clases concretas `ConcreteProductRepository` y `EmailNotificationService`.
        * También puedes preguntar a Cursor AI en el chat: "Analyze this BadProductService class. Does it have strong coupling to concrete implementations? Where?". O más directo: "Show dependencies of the BadProductService class. Does it depend on concrete classes instead of interfaces?".

**Ejemplo Completo para Análisis (Punto de Partida "Antes"):**

Este es el código que analizarás en las Actividades 2 y 3. Contiene violaciones de SRP y DIP.

```java
// Product.java - Clase simple de datos (asumimos que existe en un paquete 'model')
package com.example.yourproject.model; // Ejemplo de paquete

public class Product {
    private String id;
    private String name;
    private double price;

    // Constructor
    public Product(String id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    // Getters
    public String getId() { return id; }
    public String getName() { return name; }
    public double getPrice() { return price; }

    // Setters (opcional, solo si necesitas modificar campos después de la creación)
    public void setId(String id) { this.id = id; }
    public void setName(String name) { this.name = name; }
    public void setPrice(double price) { this.price = price; }

    @Override
    public String toString() {
        return "Product{" +
               "id='" + id + '\'' +
               ", name='" + name + '\'' +
               ", price=" + price +
               '}';
    }
}

// ConcreteProductRepository.java - Implementación concreta de acceso a datos (Módulo de Bajo Nivel)
package com.example.yourproject.repository; // Ejemplo de paquete

public class ConcreteProductRepository { // Nombre explícito para resaltar que es concreto

    public ConcreteProductRepository() {
        System.out.println("ConcreteProductRepository initialized.");
    }

    public void save(Product product) {
        // Lógica para guardar el producto en la base de datos (simulado)
        System.out.println("Saving product to concrete repository: " + product.getName());
        // ... aquí iría el código real para interactuar con DB, archivo, etc. ...
    }

    // Otros métodos de acceso a datos, ej:
    public Product findById(String id) {
        System.out.println("Finding product by id: " + id);
        // ... código para buscar en DB ...
        return new Product(id, "Dummy Product", 0.0); // Retorna un dummy para el ejemplo
    }
}

// EmailNotificationService.java - Implementación concreta de notificación (Módulo de Bajo Nivel)
package com.example.yourproject.service.notification; // Ejemplo de paquete

public class EmailNotificationService { // Nombre explícito para resaltar el tipo de notificación

    public EmailNotificationService() {
        System.out.println("EmailNotificationService initialized.");
    }

    public void sendNotification(String recipient, String message) {
        // Lógica para enviar email (simulado)
        System.out.println("Sending email notification to " + recipient + ": " + message);
        // ... aquí iría el código real para enviar emails ...
    }
}


// BadProductService.java - Servicio que maneja lógica y usa dependencias (Violaciones SRP y DIP)
package com.example.yourproject.service; // Ejemplo de paquete

import com.example.yourproject.model.Product; // Importar clase Product
import com.example.yourproject.repository.ConcreteProductRepository; // Importar repositorio concreto
import com.example.yourproject.service.notification.EmailNotificationService; // Importar servicio de notificación concreto

// Clase de servicio que viola SRP y DIP
public class BadProductService { // Nombre explícito para resaltar que tiene problemas

    // Constructor simple
     public BadProductService() {
        System.out.println("BadProductService initialized.");
    }

    // Método que contiene múltiples responsabilidades y violaciones DIP
    public void addProduct(Product product) {
        System.out.println("Executing addProduct in BadProductService");

        // --- Responsabilidad 1: Validación ---
        // Lógica de validación del producto
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

        // --- Responsabilidad 2: Persistencia de Datos ---
        // DIP Violation: Creación DIRECTA de una dependencia concreta (Alto acoplamiento)
        ConcreteProductRepository repository = new ConcreteProductRepository(); // <--- Violación DIP aquí

        repository.save(product); // Uso de la dependencia concreta
        System.out.println("Product saved to repository.");

        // --- Responsabilidad 3: Notificación ---
        // DIP Violation: Creación DIRECTA de otra dependencia concreta (Alto acoplamiento)
        EmailNotificationService notificationService = new EmailNotificationService(); // <--- Violación DIP aquí

        // Uso de la dependencia concreta
        notificationService.sendNotification("admin@example.com", "New product added: " + product.getName());
        System.out.println("Notification sent via email.");

        // --- Podría haber otras responsabilidades aquí, ej: Logging, Auditoría, etc. ---
    }

    // Otros métodos que podrían también contener validación, acceso a datos, o notificaciones,
    // posiblemente duplicando lógica o creando nuevas instancias concretas.
    // public Product getProductDetails(String id) {
    //    // Validación del ID
    //    if (id == null || id.isEmpty()) {...}
    //    ConcreteProductRepository repo = new ConcreteProductRepository(); // Nueva instancia concreta
    //    Product foundProduct = repo.findById(id);
    //    // Lógica de transformación
    //    return foundProduct;
    // }
}
```

## Actividad 3: Taller Práctico: Aplicando el Principio de Inversión de Dependencias (DIP) (35 minutos)

**Propósito**: Refactorizar código de forma práctica para aplicar el Principio de Inversión de Dependencias (DIP), la piedra angular del diseño flexible en Spring, utilizando interfaces y la inyección por constructor, asistido por Cursor AI.

**Descripción de la Actividad (Seguir la guía del tutor en tu propio Cursor AI):**

1. Utilizaremos el código de ejemplo completo de la Actividad 2 que viola el DIP (la clase `BadProductService` que usa directamente `ConcreteProductRepository` y `EmailNotificationService`).
1. **Paso 1: Identificar las Dependencias Concretas a Invertir**: Identifica las clases `ConcreteProductRepository` y`EmailNotificationService` como las dependencias concretas que `BadProductService` crea directamente usando `new`.
1. **Paso 2: Crear las Abstracciones (Interfaces) con Cursor AI:**
    * **Tu Tarea**: En Cursor AI, navega a la clase `ConcreteProductRepository.java`.
    * **Tu Tarea**: Haz clic derecho sobre el nombre de la clase (`ConcreteProductRepository`). Busca las opciones de refactorización y selecciona "Extract Interface" (o similar).
    * **Tu Tarea**: Dale un nombre significativo a la nueva interfaz, como `ProductRepository`. Asegúrate de que la nueva interfaz `ProductRepository` contenga el método `void save(Product product);` (y `Product findById(String id);` si quieres refactorizar también `getProductDetails` posteriormente).
    * **Resultado Esperado**: Cursor AI creará un nuevo archivo **ProductRepository.java** con la interfaz y modificará `ConcreteProductRepository.java` para que ahora implemente esta interfaz.
    * **Tu Tarea**: Repite este proceso para `EmailNotificationService.java`. Crea una interfaz llamada `NotificationService` que contenga el método `void sendNotification(String recipient, String message);`.
    * **Resultado Esperado**: Tendrás dos nuevas interfaces (`ProductRepository`, `NotificationService`) y tus clases concretas implementando estas interfaces.
1. **Paso 3: Modificar la Clase Dependiente (`BadProductService`) para Usar las Interfaces:**
    * **Tu Tarea**: Navega de vuelta a la clase `BadProductService.java`.
    * **Tu Tarea**: Elimina las líneas donde se crean las instancias concretas: `ConcreteProductRepository repository = new ConcreteProductRepository();` y `EmailNotificationService notificationService = new EmailNotificationService();`.
    * **Tu Tarea**: Declara campos privados cuyo tipo sean las interfaces que acabas de crear: `private final ProductRepository productRepository;` y `private final NotificationService notificationService;`. Usar `final` es una buena práctica cuando se inyecta por constructor.
    * **Tu Tarea**: Elimina el constructor simple `public BadProductService()`.
    * **Tu Tarea**: Crea un nuevo constructor para `BadProductService` que acepte un parámetro de tipo `ProductRepository` y otro de tipo `NotificationService`. Asigna estos parámetros a los campos `productRepository` y `notificationService`.
    * **Tu Tarea**: Añade la anotación `@Autowired` encima del constructor.
    * **Con Cursor AI**: Después de declarar los campos `private final ...`, Cursor AI a menudo te sugerirá automáticamente crear el constructor con `@Autowired` aceptando esos tipos. Acéptala si aparece. Si no, escribe el constructor y `@Autowired` manualmente; Cursor AI te asistirá con la sintaxis.
    * **Tu Tarea**: Dentro del método `addProduct`, cambia las llamadas a métodos para que usen los campos de interfaz: `productRepository.save(product);` y `notificationService.sendNotification("admin@example.com", "New product added: " + product.getName());`. La lógica interna que usa estos métodos no necesita cambiar.
    * **Resultado Esperado**: La clase `BadProductService` refactorizada (que ahora podríamos renombrar a `ProductService` o `ProductManagementService`) debería verse similar a esto:

    ```Java
    package com.example.yourproject.service;

    import com.example.yourproject.model.Product;
    import com.example.yourproject.repository.ProductRepository; // Usamos la INTERFAZ
    import com.example.yourproject.service.notification.NotificationService; // Usamos la INTERFAZ

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service; // Añadir anotación si es un bean de Spring

    // Clase de servicio refactorizada (Aplicación del DIP)
    @Service // Ejemplo si esta clase es un bean de Spring
    public class ProductService { // Renombrada para reflejar que ahora está mejor diseñada

        // Dependencias a las ABSTRACCIONES (Interfaces)
        private final ProductRepository productRepository;
        private final NotificationService notificationService;

        // Inyección por constructor - Spring inyectará implementaciones CONCRETAS de las INTERFACES
        @Autowired
        public ProductService(ProductRepository productRepository, NotificationService notificationService) {
            this.productRepository = productRepository;
            this.notificationService = notificationService;
            System.out.println("ProductService initialized with dependencies.");
        }

        // Método que contiene múltiples responsabilidades y violaciones SRP (aún pendiente de refactorizar SRP)
        public void addProduct(Product product) {
            System.out.println("Executing addProduct in ProductService");

            // --- Responsabilidad 1: Validación ---
            // Lógica de validación del producto
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
            // Llama al método a través de la INTERFAZ inyectada
            productRepository.save(product);
            System.out.println("Product saved via injected repository.");

            // --- Uso de Dependencia (Notificación) ---
            // Llama al método a través de la OTRA INTERFAZ inyectada
            notificationService.sendNotification("admin@example.com", "New product added: " + product.getName());
            System.out.println("Notification sent via injected notification service.");

            // Nota: Esta clase *aún* tiene múltiples responsabilidades (validación, salvar, notificar).
            // Refactorizar para SRP sería el siguiente paso lógico, quizás extrayendo la validación a otra clase.
        }

        // Otros métodos refactorizados para usar las interfaces inyectadas
        // public Product getProductDetails(String id) {
        //    // Validación del ID (Podría ir a un validador)
        //    if (id == null || id.isEmpty()) {...}
        //    Product foundProduct = productRepository.findById(id); // Usa la interfaz inyectada
        //    // Lógica de transformación
        //    return foundProduct;
        // }
    }
    ```

1. **Paso 4: Asegurar que Spring Gestiona las Implementaciones (Contexto Spring Boot):**
    * **Tu Tarea**: Confirma que tus implementaciones concretas (`ConcreteProductRepository.java` y `EmailNotificationService.java`) estén anotadas con `@Component` o `@Service` para que Spring las detecte y las gestione como beans. Alternativamente, podrían estar definidas usando métodos `@Bean` en una clase `@Configuration`.
    * **Con Cursor AI**: Puedes usar la función "Find Usages" (buscar usos) en las interfaces (`ProductRepository`, `NotificationService`) para ver dónde se están inyectando (en `ProductService`). También puedes preguntar a Cursor AI si `ConcreteProductRepository` o `EmailNotificationService` están registrados como beans.
1. **Paso 5: Verificar la Refactorización:**
    * **Tu Tarea**: Si estás trabajando en un proyecto Spring Boot real, asegúrate de que tus implementaciones concretas (`ConcreteProductRepository`, `EmailNotificationService`) estén configuradas como beans (anotándolas o usando `@Bean`). Si tenías un controlador que llamaba a `addProduct`, ejecuta la aplicación y verifica que la lógica se ejecuta y los mensajes de `save` y `sendNotification` aparecen en la consola.
    * **Con Cursor AI**: Usa la función "Show Dependencies" en la clase `ProductService` refactorizada. Los diagramas/listas deben mostrar ahora las dependencias a las interfaces (`ProductRepository`, `NotificationService`), no a las clases concretas originales.

**Resultado Esperado**: Habrás refactorizado exitosamente un código para aplicar el DIP, ganando experiencia práctica con la creación de interfaces, la inversión de dependencia y el uso de la inyección de dependencias de Spring, todo ello asistido por las herramientas de refactorización de Cursor AI. Ahora podrías crear una nueva implementación de `ProductRepository` (ej. `JpaProductRepository`) o `NotificationService` (ej. `SMSNotificationService`) y decirle a Spring que la inyecte en `ProductService` (cambiando la configuración de Spring, no la clase `ProductService`) sin modificar `ProductService`, demostrando la flexibilidad ganada.

## Actividad 4: Cursor AI como Mentor Automático para OOP y SOLID (30 minutos)

**Propósito**: Explorar cómo puedes usar Cursor AI de forma proactiva como un mentor personal para obtener feedback y sugerencias sobre el diseño orientado a objetos y el cumplimiento de los principios SOLID en tu propio código o en cualquier otro código que estés leyendo.

**Descripción de la Actividad:**

1. Abre tu proyecto personal (o continúa trabajando con los fragmentos de código de ejemplo más grandes proporcionados si los hay).
1. Explora Sugerencias Generales de Diseño:
    * **Tu Tarea**: Selecciona una clase completa (preferiblemente una que creas que podría mejorarse o que sea central en tu diseño).
    * **Tu Tarea**: Abre el chat de AI (Cmd/Ctrl + L) y escribe un prompt solicitando una revisión de diseño. Sé específico si puedes: "Analyze this Java class from an Object-Oriented Design and SOLID principles perspective. What are its strengths and weaknesses? How could I improve its structure and adherence to SOLID?".
    * **Tu Tarea**: Analiza las sugerencias de la IA. ¿Identifica "olores de código" (code smells)? ¿Sugiere aplicar SRP dividiendo la clase o métodos? ¿Habla sobre acoplamiento o dependencia? ¿Menciona oportunidades para usar herencia o composición de forma diferente?
1. **Consulta sobre Principios Específicos en tu Código:**
    * **Tu Tarea**: Elige una parte de tu código (una clase, una interfaz, un método) y céntrate en un principio SOLID específico.
    * **Tu Tarea**: Pregúntale a Cursor AI: "Looking at this code block, does it follow the [Nombre del Principio, ej., Single Responsibility Principle]? Why or why not?".
    * **Tu Tarea**: Si la IA o tú identificáis una violación, pídele opciones de refactorización: "How could I refactor this code to better follow the [Nombre del Principio]? Give me a code example.".
1. **Refactorización Proactiva Basada en IA:**
    * **Tu Tarea**: Identifica un método que parece hacer demasiado o es difícil de leer.
    * **Tu Tarea**: Selecciona el código y pide a Cursor AI: "Suggest refactoring options for this method to improve readability and potentially apply SRP at the method level.".
    * **Tu Tarea**: Si la sugerencia de la IA implica refactorizaciones como "Extract Method", usa las herramientas integradas de Cursor AI para aplicarla de forma segura.
    * **Tu Tarea**: Si identificas código muy similar en dos lugares, pregúntale a Cursor AI si se puede generalizar para evitar duplicados, lo que se relaciona con principios como DRY (Don't Repeat Yourself) y a menudo mejora el diseño general.

**Resultado Esperado**: Aprenderás a usar Cursor AI como un compañero de revisión de código y diseño. Te sentirás más cómodo pidiendo feedback sobre tu código y utilizando las sugerencias de la IA como punto de partida para mejorar el diseño de tu software según los principios OOP y SOLID de forma autónoma.

## Cierre y Próximos Pasos (10 minutos)

* **Recapitulación**: Hemos diagnosticado violaciones SOLID en código real (ejemplos), practicado la refactorización para aplicar DIP, y explorado cómo usar Cursor AI como un mentor de diseño para encontrar y corregir problemas en vuestro propio código.
* **La Refactorización es Continua**: Recordad que el diseño de software no es estático. A medida que vuestro proyecto crece y cambian los requisitos, es probable que surjan nuevas oportunidades para refactorizar y aplicar mejor los principios SOLID. La revisión de código regular (manual y asistida por IA) es clave.
* **Próximos Pasos**: Continuad aplicando estos principios en vuestro propio proyecto y en futuros trabajos. Haced que el análisis y la refactorización sean parte de vuestro proceso de desarrollo habitual. Intentad aplicar otros principios SOLID (OCP, ISP) en vuestras refactorizaciones.
* **Espacio para Preguntas Finales.**
