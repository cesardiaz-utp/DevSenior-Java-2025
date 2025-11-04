# Clase 4: Java Records, MapStruct, el Desarrollo Ágil y Scrum

**Hola a todos!**
¡Bienvenidos a la **Clase 4 del Módulo 7**! Hemos progresado mucho, desde los fundamentos de Spring Security y JWT hasta hacer nuestras APIs robustas con manejo de excepciones y validaciones. En esta clase, introduciremos una característica moderna de Java: las clases `Record`. Veremos cómo simplifican la escritura de código, especialmente para DTOs, y cómo se integran perfectamente con Spring Boot, mejorando la concisión y la inmutabilidad de nuestros modelos de datos.

Adicionalmente, daremos un paso atrás para examinar el proceso de desarrollo de software en un contexto de equipo. Nos sumergiremos en los principios del **Desarrollo Ágil** y exploraremos la metodología **Scrum**, fundamental para la entrega eficiente y adaptativa de productos de software. Comprender estas metodologías es tan crucial como las habilidades técnicas para trabajar en entornos profesionales.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Comprender y utilizar las clases `Record` de Java** para crear DTOs inmutables y concisos en Spring Boot.
- **Aplicar `Record` clases** en controladores y servicios de Spring Boot.
- **Implementar el mapeo de objetos utilizando MapStruct** para transformar entre entidades y DTOs (incluyendo DTOs basados en Records).
- **Comprender los principios fundamentales del Desarrollo Ágil**.
- **Explicar la metodología Scrum**, sus roles, ceremonias y artefactos.
- **Identificar cómo el desarrollo Ágil y Scrum** se aplican en el contexto de proyectos de desarrollo de APIs y software en general.

## 1. Java Records: Simplificando DTOs y Más en Spring Boot

**Java Records**, introducidas como una característica estándar en Java 16 (JEP 395), son una nueva forma concisa y declarativa de crear clases inmutables de datos. Están diseñadas específicamente para modelar datos que consisten principalmente en un conjunto de componentes.

### 1.1. ¿Qué son los Records y por qué usarlos?

Los `record`s son una adición potente al lenguaje Java que busca reducir el boilerplate code (código repetitivo) que a menudo se escribe al crear clases que son principalmente contenedores de datos. Por ejemplo, una clase tradicional para representar un objeto de transferencia de datos (DTO) suele requerir un constructor, métodos getters para acceder a sus propiedades, métodos `equals()` y `hashCode()` para comparar instancias, y un método `toString()` para una representación legible. Los `record`s generan todo esto automáticamente.

- **Clases Inmutables de Datos**: Un `record` es, por diseño, una clase `final` que no puede ser extendida. Al declarar un `record` con sus componentes (los campos que lo componen), el compilador de Java genera automáticamente:
  - Un **constructor canónico**: Un constructor público que acepta todos los componentes del record como parámetros e inicializa los campos correspondientes.
  - **Métodos accessor públicos**: Por cada componente, se crea un método con el mismo nombre del componente (ej., `name()` para un componente `name`), que devuelve su valor. Esto elimina la necesidad de escribir manualmente `getName()`.
  - Implementaciones de `equals()` y `hashCode()`: Basadas en el valor de todos los componentes del `record`. Esto asegura que dos `record`s son considerados iguales si y solo si todos sus componentes son iguales, lo cual es fundamental para el uso en colecciones como `HashMap` o `HashSet`.
  - Una implementación de `toString()`: Proporciona una representación útil y legible de la instancia del `record`, mostrando los nombres y valores de todos sus componentes (ej., `Point[x=10, y=20]`).
  - Los componentes son `final`: Una vez que un `record` es creado, sus valores no pueden ser modificados. Esta `inmutabilidad` es una característica clave que aporta beneficios significativos:
    - **Predecibilidad**: El estado de un `record` no cambiará después de su creación, lo que facilita razonar sobre el código y depurar.
    - **Seguridad en hilos (Thread-Safety)**: Como no hay estado mutable compartido, los `records` son inherentemente seguros para usar en entornos multihilo sin necesidad de sincronización adicional.
    - **Simplicidad**: Elimina la complejidad asociada con la gestión de estados mutables, lo que reduce la probabilidad de errores.
- **Concisión**: La sintaxis de los `record`s es increíblemente compacta. Todo lo que antes requería docenas de líneas de código y anotaciones (incluso con Lombok) ahora se puede lograr en una sola línea. Esta concisión no solo reduce la cantidad de código a escribir, sino que también facilita su lectura y comprensión.
- **Claridad de Intención**: Al usar un `record`, se comunica de manera explícita y directa que el propósito principal de esta "clase" es ser un simple contenedor de datos. Esto mejora la legibilidad del código y la intención del diseño.

### 1.2. Sintaxis y Ejemplos Básicos

- **Sintaxis básica**: Un Record para representar un punto en 2D

  ```Java
  public record Point(int x, int y) {}
  ```

- Uso del Record Point:

  ```Java
  Point p1 = new Point(10, 20); // Creación de una instancia usando el constructor canónico
  int xCoord = p1.x();         // Acceso a los componentes (en lugar de p1.getX())
  System.out.println(p1);      // Output: Point[x=10, y=20] (gracias al toString() generado)

  // Comparación automática usando equals()
  Point p2 = new Point(10, 20);
  Point p3 = new Point(5, 15);
  System.out.println(p1.equals(p2)); // Output: true (mismos valores)
  System.out.println(p1.equals(p3)); // Output: false (diferentes valores)
  ```

- Con constructor compacto para validación o transformación

  ```Java
  // Este constructor no tiene parámetros y se ejecuta antes del constructor canónico
  public record Range(int min, int max) {
      public Range { // Este es el constructor compacto
          if (min > max) {
              throw new IllegalArgumentException("El valor 'min' no puede ser mayor que 'max'");
          }
          
          // Se puede realizar normalización aquí, por ejemplo:
          this.min = Math.max(0, min);
      }
  }
  ```

- Uso de Range:

  ```Java
  Range validRange = new Range(1, 10); // OK

  Range invalidRange = new Range(10, 1); // Lanza IllegalArgumentException
  ```

- Con métodos personalizados (comportamiento adicional al de simple contenedor de datos)

  ```Java
  public record Circle(Point center, double radius) {
      public double area() {
          return Math.PI * radius * radius;
      }

      public double circumference() {
          return 2 * Math.PI * radius;
      }
  }
  ```

- Uso de Circle:

  ```Java
  Circle myCircle = new Circle(new Point(0, 0), 5.0);
  System.out.println("Área del círculo: " + myCircle.area());
  ```

Los `record`s se compilan en clases que implementan implícitamente la interfaz `java.lang.Record`, lo que los marca como tipos de datos inmutables y de propósito específico.

### 1.3. Uso de Records en Spring Boot como DTOs

Los `record`s son una elección excelente para usarlos como Data Transfer Objects (DTOs) en tus APIs REST de Spring Boot. Jackson (la biblioteca de serialización/deserialización de JSON por defecto en Spring Boot) tiene soporte nativo para `record`s, lo que significa que puedes usarlos para recibir cuerpos de solicitud JSON (`@RequestBody`) y para enviar respuestas JSON (`ResponseEntity<Record>`) sin ninguna configuración adicional.

#### 1.3.1. Ejemplo de DTO de Solicitud (Request Body)

- **Antes** (clase tradicional):

  ```Java
  public class EventDTO {
      private String name;
      private LocalDate date;
      private String location;
      private Integer maxCapacity;
      // getters, setters, constructores, equals, hashCode, toString
  }
  ```

- **Después** (Java Record):

  ```Java
  import javax.validation.constraints.NotBlank;
  import javax.validation.constraints.NotNull;
  import javax.validation.constraints.Positive;
  import java.time.LocalDate;
  // También puedes usar anotaciones de OpenAPI aquí para la documentación
  import io.swagger.v3.oas.annotations.media.Schema;

  public record EventRequest(
          @NotBlank(message = "El nombre del evento es obligatorio.")
          @Schema(description = "Nombre del evento", example = "Conferencia de IA")
          String name, // Componente para el nombre del evento

          @NotNull(message = "La fecha del evento es obligatoria.")
          @Future(message = "La fecha del evento debe ser en el futuro.")
          @Schema(description = "Fecha del evento (AAAA-MM-DD)", example = "2025-01-15")
          LocalDate date, // Componente para la fecha del evento

          @NotBlank(message = "El lugar del evento es obligatorio.")
          @Schema(description = "Lugar donde se realizará el evento", example = "Centro de Convenciones")
          String location, // Componente para el lugar del evento

          @NotNull(message = "La capacidad máxima del evento es obligatoria.")
          @Positive(message = "La capacidad máxima debe ser un número positivo.")
          @Schema(description = "Capacidad máxima de asistentes", example = "500")
          Integer maxCapacity // Componente para la capacidad máxima del evento
  ) {}
  ```

Como puedes ver, todas las anotaciones de validación de Bean Validation (`@NotBlank`, `@NotNull`, `@Positive`, etc.) y las de documentación (como `@Schema` de OpenAPI) se pueden aplicar directamente a los componentes del `record`.

#### 1.3.2. Uso en Controladores

Spring Boot, a través de Jackson, se encarga de la serialización y deserialización de `record`s de manera transparente. Esto significa que puedes usar `record`s en tus métodos de controlador exactamente como usarías los POJOs tradicionales.

Es crucial entender que, para un manejo de errores global y consistente, tus controladores deben **lanzar excepciones** cuando ocurran problemas de negocio o de validación. Estas excepciones serán interceptadas por tu GlobalExceptionHandler, el cual se encargará de construir y devolver la ResponseEntity con el ErrorResponse adecuado. Esto promueve una clara separación de preocupaciones, manteniendo la lógica de negocio limpia en los servicios y la gestión de errores centralizada y uniforme.

```Java
public record EventResponse(Long id, String name, LocalDate date, String location, Integer maxCapacity) {}
```

```Java
package com.tuempresa.securityjwtdemo.controller;

import com.tuempresa.securityjwtdemo.dto.EventRequest; // Importa tu nuevo Record (para solicitudes)
import com.tuempresa.securityjwtdemo.dto.EventResponse; // Posible Record para respuestas
import com.tuempresa.securityjwtdemo.service.EventService; // Asume un servicio de eventos
import org.springframework.http.HttpStatus; // Para los códigos HTTP
import org.springframework.http.ResponseEntity; // Para las respuestas HTTP
import org.springframework.web.bind.annotation.*;
import javax.validation.Valid; // Necesario para activar Bean Validation en Records

@RestController
@RequestMapping("/api/events")
public class EventController {

    private final EventService eventService; // Inyecta tu servicio de eventos

    // Constructor para inyección de dependencias
    public EventController(EventService eventService) {
        this.eventService = eventService;
    }

    // POST para crear un evento usando el Record EventRequest
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED) // Indica que en caso de éxito, el estado HTTP es 201 Created
    public EventResponse createEvent(@Valid @RequestBody EventRequest eventRequest) {
        // Cuando la validación de Bean Validation (ej. @NotBlank) falla,
        // Spring lanzará automáticamente un MethodArgumentNotValidException.
        // Este será capturado por tu GlobalExceptionHandler,
        // que devolverá una respuesta 400 Bad Request estandarizada.

        // Si ocurre un conflicto de negocio (ej. nombre de evento duplicado),
        // el servicio (eventService) debería lanzar una DataConflictException.
        // eventService.createEvent(eventRequest.toEntity()); // Asume un método de mapeo en EventRequest

        // Retorna el evento creado, posiblemente mapeado a un EventResponse Record
        return eventService.createEvent(eventRequest); // O mapea la entidad guardada a un Record de respuesta
    }

    // GET para obtener un evento por ID, devolviendo un Record de respuesta
    // Asume que EventResponse es un record definido para la respuesta
    @GetMapping("/{id}")
    public EventResponse getEvent(@PathVariable Long id) {
        // Si el evento no se encuentra, el servicio debería lanzar una ResourceNotFoundException (Clase 3),
        // la cual será capturada por el GlobalExceptionHandler y convertida a 404 Not Found.
        return eventService.getEventById(id); // Asume que el servicio devuelve un EventResponse
    }

    // Puedes tener un método para listar todos los eventos, también usando Records para la respuesta
    @GetMapping
    public List<EventResponse> getAllEvents() {
        return eventService.findAllEvents();
    }
}
```

Observa cómo en el ejemplo anterior, los métodos de los controladores simplemente **devuelven el tipo de dato que esperan** (un `EventResponse` o un `String` en el caso del ejemplo simplificado de `createEvent`) y **lanzan excepciones** para los errores. La construcción de la `ResponseEntity` y la gestión de los códigos HTTP de error (`400`, `404`, `409`, `500`) es responsabilidad exclusiva del `GlobalExceptionHandler` configurado con `@ControllerAdvice`, haciendo los controladores más limpios y enfocados en la lógica de la API.

**Beneficios de usar `Record`s para DTOs:**

- **Menos código**: La eliminación de constructores manuales, _getters_, `equals()`, `hashCode()` y `toString()` reduce significativamente la cantidad de código repetitivo. Esto hace que tus DTOs sean mucho más compactos y fáciles de leer.
- **Inmutabilidad por defecto**: Al ser inmutables, los `record`s son ideales para DTOs que representan instantáneas de datos en un momento dado. Esta característica reduce la superficie de errores relacionados con el estado mutable y simplifica la lógica en sistemas concurrentes.
- **Legibilidad y Claridad de Intención**: La declaración concisa de un `record` comunica instantáneamente que su propósito es contener datos. Esto mejora la legibilidad del código base y ayuda a otros desarrolladores a comprender rápidamente el contrato de los datos.
- **Integración Transparente**: Spring Boot, gracias a su integración con la biblioteca Jackson, maneja la serialización de `record`s a JSON y la deserialización de JSON a `record`s de forma automática y eficiente, sin necesidad de configuración adicional.

**Consideraciones:**

- Los `record`s son `final` y **no pueden extender otras clases**. Esto fomenta la composición sobre la herencia para modelar relaciones entre datos, que a menudo es una práctica de diseño más robusta.
- No puedes añadir **campos de instancia adicionales** que no estén en la declaración del encabezado del `record`. Todos los componentes del `record` forman parte de su estado canónico.
- Los campos de un `record` son inmutables. Si necesitas que un objeto sea mutable después de su creación (lo cual es raro para DTOs puros), entonces una clase tradicional seguiría siendo la opción adecuada.
- El uso de `record`s requiere **Java 16 o superior**. Asegúrate de que tu versión de JDK en el proyecto sea compatible.

## 2. Mapeo de Objetos con MapStruct

En el desarrollo de APIs RESTful con Spring Boot, es una práctica común utilizar Data Transfer Objects (DTOs) para desacoplar la capa de la base de datos (entidades JPA) de la capa de presentación (la API). Esto mejora la seguridad, la flexibilidad y el control sobre los datos expuestos. Sin embargo, el mapeo manual entre entidades y DTOs puede ser repetitivo y propenso a errores. Aquí es donde **MapStruct** brilla.

### 2.1. ¿Qué es MapStruct?

![MapStruct](https://adictosaltrabajo.com/wp-content/uploads/2020/03/Captura-de-pantalla-2020-03-25-a-las-11.18.11.png)

[**MapStruct**](https://mapstruct.org/) es un generador de código que simplifica el mapeo entre tipos Java. A diferencia de las bibliotecas de mapeo basadas en reflexión (como ModelMapper o Dozer), MapStruct genera las implementaciones de los mappers en tiempo de compilación. Esto ofrece varias ventajas clave:

- **Rendimiento**: Al generar código Java puro, el mapeo es tan rápido como si lo hubieras escrito a mano, sin la sobrecarga de la reflexión en tiempo de ejecución.
- **Seguridad en tiempo de compilación**: Si un campo cambia de nombre o tipo en una de tus clases, MapStruct detectará el problema durante la compilación, evitando errores en tiempo de ejecución que podrían ser difíciles de depurar.
- **Código legible y depurable**: Puedes ver y depurar el código generado por MapStruct, lo cual es útil para entender cómo se realizan los mapeos.
- **Manejo de colecciones**: MapStruct soporta automáticamente el mapeo de listas y otras colecciones.
- **Extensibilidad y Personalización**: Permite definir reglas de mapeo complejas, conversiones personalizadas y el manejo de campos con nombres diferentes.

### 2.2. Integración y Uso Básico con Spring Boot

1. **Añadir Dependencias (`pom.xml`)**: Necesitarás el procesador de anotaciones de MapStruct y la dependencia de `org.mapstruct.Mapping` en tu `pom.xml`.

    ```XML
    <properties>
        <!-- Usar la versión más reciente estable -->
        <org.mapstruct.version>1.6.3</org.mapstruct.version>
    </properties>
    <dependencies>
        <!-- ... otras dependencias ... -->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>${org.mapstruct.version}</version> 
        </dependency>
    </dependencies>
    ...
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <annotationProcessorPaths>
                        <path>
                            <groupId>org.mapstruct</groupId>
                            <artifactId>mapstruct-processor</artifactId>
                            <version>${org.mapstruct.version}</version>
                        </path>
                        <!-- Si usas Lombok, también inclúyelo aquí para evitar conflictos -->
                        <!-- <path>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                            <version>${lombok.version}</version>
                        </path>
                        <path>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok-mapstruct-binding</artifactId>
                            <version>0.2.0</version>
                        </path> -->
                    </annotationProcessorPaths>
                </configuration>
            </plugin>
        </plugins>
    </build>
    ```

    **Nota**: Si estás usando Java 17+, es posible que necesites configurar el `maven-compiler-plugin` para usar `target 17` y `source 17`.

2. **Modelos**: Supón que tienes la entidad `Event`, el DTO `EventRequest` (record) y el DTO de respuesta `EventResponse` (record).

    ```Java
    @Entity
    public class Event {
        @Id @GeneratedValue
        private Long id;
        private String name;
        private LocalDate date;
        private String location;
        private Integer maxCapacity;
        // getters y setters
    }
    ```

    ```Java
    public record EventRequest(String name, LocalDate date, String location, Integer maxCapacity) {}
    ```

    ```Java
    public record EventResponse(Long id, String name, LocalDate date, String location, Integer maxCapacity) {}
    ```

3. **Crear una Interfaz de Mapper**: Define una interfaz con las anotaciones de MapStruct. Esta interfaz le dice a MapStruct qué clases mapear.

    ```Java
    package com.tuempresa.tuproyecto.mapper;

    import com.tuempresa.tuproyecto.entity.Event; // Tu entidad JPA
    import com.tuempresa.tuproyecto.dto.EventRequest; // Tu Record de solicitud
    import com.tuempresa.tuproyecto.dto.EventResponse; // Tu Record de respuesta
    import org.mapstruct.Mapper;
    import org.mapstruct.Mapping;
    import org.mapstruct.factory.Mappers;

    @Mapper(componentModel = "spring") // Hace que MapStruct genere un componente de Spring (@Component)
    public interface EventMapper {

        // Mapea un EventRequest (Record) a una entidad Event
        // MapStruct detecta automáticamente los accesores (ej. name() para EventRequest)
        @Mapping(target = "id", ignore = true) // Ignora el ID al crear/actualizar
        Event toEntity(EventRequest eventRequest);

        // Mapea una entidad Event a un EventResponse (Record)
        EventResponse toDto(Event event);

        // Mapea una lista de entidades Event a una lista de EventResponse Records
        List<EventResponse> toDtoList(List<Event> events);
    }
    ```

    `@Mapper(componentModel = "spring")`: Esta anotación es clave. Le indica a MapStruct que genere el mapper como un componente de Spring, lo que te permite inyectarlo en tus servicios o controladores utilizando @Autowired o inyección por constructor.

4. **Uso en Servicios**: Inyecta tu interfaz de mapper en tu servicio y úsala para convertir entre DTOs y entidades.

    ```Java
    import com.tuempresa.tuproyecto.entity.Event;
    import com.tuempresa.tuproyecto.repository.EventRepository;
    import com.tuempresa.tuproyecto.dto.EventRequest;
    import com.tuempresa.tuproyecto.dto.EventResponse;
    import com.tuempresa.tuproyecto.mapper.EventMapper; // Importa tu mapper
    import org.springframework.stereotype.Service;
    import java.util.List;
    import java.util.Optional;

    @Service
    public class EventService {

        private final EventRepository eventRepository;
        private final EventMapper eventMapper; // Inyecta el mapper

        public EventService(EventRepository eventRepository, EventMapper eventMapper) {
            this.eventRepository = eventRepository;
            this.eventMapper = eventMapper;
        }

        public EventResponse createEvent(EventRequest eventRequest) {
            Event event = eventMapper.toEntity(eventRequest); // Mapea el Record a entidad
            Event savedEvent = eventRepository.save(event);
            return eventMapper.toDto(savedEvent); // Mapea la entidad guardada a Record de respuesta
        }

        public EventResponse getEventById(Long id) {
            Optional<Event> eventOptional = eventRepository.findById(id);
            // Si el evento no se encuentra, lanzar ResourceNotFoundException (Clase 3)
            Event event = eventOptional.orElseThrow(() -> new ResourceNotFoundException("Evento no encontrado con ID: " + id));
            return eventMapper.toDto(event); // Mapea la entidad a Record de respuesta
        }

        public List<EventResponse> getAllEvents() {
            List<Event> events = eventRepository.findAll();
            return eventMapper.toDtoList(events); // Mapea la lista de entidades a lista de Records
        }

        // ... otros métodos CRUD
    }
    ```

### 2.3. Compatibilidad con Java Records

MapStruct funciona de forma nativa con Java Records. Cuando mapeas un Record a otra clase (o viceversa), MapStruct utiliza los accessors generados automáticamente (`.fieldName()`) para Records y los setters o constructores para las otras clases. Esto significa que la declaración del mapper es idéntica a la que usarías con POJOs tradicionales, lo cual es una gran ventaja en términos de concisión y mantenimiento.

## 3. Desarrollo Ágil y Scrum: Organizando Equipos de Software

En el mundo actual del desarrollo de software, la forma en que los equipos organizan su trabajo es tan importante como las tecnologías que utilizan. Las metodologías ágiles, y en particular Scrum, son marcos de trabajo ampliamente adoptados que promueven la entrega incremental y adaptativa de valor.

### 3.1. Metodologías de desarrollo

#### 3.1.1 Cascada (Waterfall)

![Waterfall](https://management.org/wp-content/uploads/2022/03/5-Stages-of-the-Waterfall-Model.jpg)

El modelo en cascada, también conocido como _Waterfall_, es una metodología de desarrollo de software lineal y secuencial. Se caracteriza por dividir el proceso de desarrollo en fases distintas y consecutivas, donde cada fase debe completarse y validarse antes de que la siguiente pueda comenzar. Es un enfoque tradicional que se asemeja a la forma en que se gestionan los proyectos de ingeniería civil o de fabricación, donde los requisitos son estables y bien definidos desde el principio.

**Fases Típicas del Modelo en Cascada:**

1. **Requisitos (Requirements)**: Se recopilan y documentan todos los requisitos del sistema de manera exhaustiva. El objetivo es tener una comprensión completa de lo que el cliente necesita antes de pasar a la siguiente fase.
2. **Diseño (Design)**: Basándose en los requisitos, se diseña la arquitectura del sistema, la base de datos, la interfaz de usuario y los componentes de software. Se crean diagramas y especificaciones detalladas.
3. **Implementación (Implementation)**: Los desarrolladores escriben el código del software de acuerdo con las especificaciones de diseño. Cada módulo se desarrolla y se prueba unitariamente.
4. **Pruebas o verificacion (Testing)**: Una vez que el software está implementado, se realizan pruebas exhaustivas para identificar y corregir defectos. Esto incluye pruebas de integración, pruebas de sistema y pruebas de aceptación.
5. **Despliegue/Mantenimiento (Deployment/Maintenance)**: El software se entrega al cliente y se pone en producción. La fase de mantenimiento implica la corrección de errores, la mejora del rendimiento y la adaptación a nuevos entornos o requisitos.

**Características y Principios Clave:**

- **Secuencialidad y Linealidad**: Las fases se ejecutan en un orden estricto, una tras otra.
- **Documentación Exhaustiva**: Cada fase produce una gran cantidad de documentación que sirve como entrada para la siguiente fase.
- **Planificación Detallada**: Se realiza una planificación muy detallada al inicio del proyecto.
- **Poca Flexibilidad al Cambio**: Una vez que una fase se completa y se "congela", es muy costoso y difícil volver atrás para realizar cambios en fases anteriores.
- **Entrega Única**: El producto final se entrega al cliente solo al final del ciclo de vida del proyecto.

#### 3.1.2 Rational Unified Process (RUP)

![RUP](https://ingsoftw123.wordpress.com/wp-content/uploads/2013/11/4276c-diagramageneralderup.png)

El Proceso Unificado de Rational (RUP) es una metodología de desarrollo de software que proporciona un enfoque disciplinado para asignar tareas y responsabilidades dentro de una organización de desarrollo. Su objetivo es producir software de alta calidad que satisfaga las necesidades de sus usuarios finales dentro de un cronograma y presupuesto predecibles.

RUP se basa en un conjunto de principios y mejores prácticas clave, como el desarrollo iterativo, la gestión de requisitos, el uso de arquitecturas basadas en componentes, el modelado visual del software, la verificación de la calidad del software y el control de cambios en el software.

El ciclo de vida de RUP consta de cuatro fases secuenciales:

1. **Inicio**: En esta fase, se define el alcance del proyecto y se crea un caso de negocio.
2. **Elaboración**: En esta fase, se analiza el dominio del problema, se establece una base arquitectónica, se desarrolla el plan del proyecto y se eliminan los elementos de mayor riesgo del proyecto.
3. **Construcción**: En esta fase, todos los componentes y características restantes se desarrollan e integran en el producto, y todas las características se prueban a fondo.
4. **Transición**: En esta fase, el software se implementa en la comunidad de usuarios.

Cada fase consta de una o más iteraciones, en las que se produce una versión ejecutable del software. Este enfoque iterativo permite la retroalimentación temprana del usuario y la adaptación a los requisitos cambiantes.

### 3.2. ¿Qué es el Desarrollo Ágil?

El Desarrollo Ágil es una filosofía de desarrollo de software que enfatiza la flexibilidad, la colaboración, la entrega continua y la adaptación al cambio. Nació como una respuesta a las limitaciones de las metodologías tradicionales ("cascada"), que a menudo resultaban en proyectos lentos, inflexibles y que no satisfacían las necesidades cambiantes del cliente. Se basa en los valores y principios del Manifiesto Ágil, publicado en 2001:

- **Individuos e interacciones sobre procesos y herramientas**: Este es un pilar fundamental para el trabajo en grupo. Significa que, si bien tener procesos bien definidos y herramientas sofisticadas puede ser útil, el éxito de un proyecto depende en última instancia de la **calidad de la comunicación y la colaboración entre los miembros del equipo**. Priorizar las conversaciones cara a cara (o virtuales sincrónicas), la resolución conjunta de problemas y el respeto mutuo entre los desarrolladores, testers, Product Owners y otros stakeholders, generará un mejor resultado que simplemente seguir un manual de procesos. Un equipo que interactúa proactivamente para identificar y eliminar obstáculos, compartir conocimientos y ayudarse mutuamente, es mucho más resiliente y productivo.
- **Software funcionando sobre documentación exhaustiva**: El objetivo principal del desarrollo es entregar software que funcione y que aporte valor al cliente de manera temprana y continua. La documentación es importante para la comprensión y el mantenimiento, pero no debe ser un fin en sí misma, ni retrasar la entrega de funcionalidades clave. En un contexto de trabajo en grupo, esto implica que el equipo debe enfocarse en la **construcción colaborativa de código de alta calidad que se pueda probar y entregar**, y que sirva como una forma de "documentación viva". Las discusiones y decisiones se centran en cómo hacer que el software funcione, y la documentación se crea de forma incremental y justo a tiempo, a menudo en colaboración con el cliente.
- **Colaboración con el cliente sobre negociación contractual**: En lugar de definir todos los requisitos en un contrato inicial y luego ejecutarlo rígidamente, el desarrollo ágil promueve una **colaboración constante y activa con el cliente** a lo largo de todo el ciclo de vida del proyecto. Para el equipo de desarrollo, esto significa que el cliente no es una entidad distante, sino un miembro activo en el proceso. Se busca la **retroalimentación frecuente y directa**, a menudo a través de demostraciones de software funcionando. Esta interacción continua asegura que el equipo esté construyendo el producto correcto, alineado con las necesidades cambiantes del negocio, y fomenta una relación de confianza y un entendimiento compartido del objetivo.
- **Respuesta al cambio sobre seguir un plan**: El mundo de los negocios es dinámico, y los requisitos de software pueden cambiar. Las metodologías ágiles abrazan el cambio como una oportunidad para entregar un producto superior, en lugar de verlo como un problema que debe evitarse a toda costa. Esto se logra mediante ciclos de desarrollo cortos y la capacidad de pivotar rápidamente. En el trabajo en grupo, esto requiere que el equipo sea **adaptable y resiliente**. La planificación no es estática; es un proceso continuo que se ajusta a medida que se aprende más sobre el problema o las necesidades del mercado. Los equipos deben estar preparados para ajustar sus prioridades y su enfoque, lo que demanda una comunicación constante y una toma de decisiones colaborativa.

Los principios ágiles adicionales que complementan el manifiesto incluyen la entrega temprana y continua de software valioso, la auto-organización de los equipos, la búsqueda constante de la excelencia técnica, la simplicidad y la reflexión regular para ajustar y mejorar el proceso.

### 3.3. Desarrollo tradicional vs ágil

La gestión ágil, es cada vez más popular como una alternativa a las metodologías tradicionales de gestión de proyectos, especialmente entre los equipos de desarrollo de software y TI. Cuando intenta decidir qué metodología de gestión de proyectos utilizar, es importante comprender algunas de las diferencias clave entre los enfoques de gestión de proyectos ágiles y tradicionales, ya que cada uno tiene sus propias ventajas y desventajas.

En todo proyecto existen 3 variables relacionadas, el llamado “triángulo de hierro”:

- **El alcance**, definirá las tareas necesarias para alcanzar las características que deseamos obtener de nuestro producto.
- **El tiempo**, es la duración aproximada en que se entregará el producto.
- **El costo**, son los recursos que se van a destinar para la ejecución del proyecto.

![triángulo de hierro](https://anibalgoicochea.com/wp-content/uploads/2012/02/cambio-de-paradigma-con-metodologc3adas-agiles-como-scrum.png)

Implícitamente dentro de este alcance el líder del proyecto debe establecer el ámbito de la calidad y seguridad de nuestro proyecto conociendo cuál es el alcance del proyecto, el tiempo requerido y del presupuesto con el que cuenta para completarlo.

| Característica | Desarrollo Tradicional (Cascada) | Desarrollo Ágil |
| --- | --- | --- |
| **Enfoque** | Lineal y secuencial. Las fases se completan una tras otra. | Iterativo e incremental. El proyecto se divide en pequeños ciclos (sprints). |
| **Planificación** | Exhaustiva y por adelantado. Los requisitos se definen al inicio. | Flexible y adaptable. Los requisitos pueden cambiar durante el proyecto. |
| **Entregas** | Se entrega un único producto final al terminar el proyecto. | Se realizan entregas funcionales y parciales al final de cada iteración. |
| **Cliente** | Participación limitada, principalmente al inicio y al final del proyecto. | Colaboración y comunicación constante con el equipo de desarrollo. |
| **Cambios** | Difíciles y costosos de implementar una vez que una fase ha terminado. | Son bienvenidos y se gestionan en cada iteración. |
| **Documentación** | Extensa y detallada desde el principio. | La necesaria para el desarrollo, se valora más el software funcional. |

#### ¿Cuándo usar cada uno?

- **Desarrollo Tradicional**: Es adecuado para proyectos con requisitos muy claros, estables y bien definidos desde el principio, donde el riesgo de cambios es bajo. Por ejemplo, la construcción de un edificio.
- **Desarrollo Ágil**: Es ideal para proyectos en entornos cambiantes, donde los requisitos no están completamente definidos o pueden variar. Es muy utilizado en el desarrollo de software, ya que permite una rápida adaptación y entrega de valor al cliente.

### 3.4. Scrum: Un Marco Ágil Popular

![Scrum](https://www.ilunion.com/sites/default/files/styles/webp/public/2025-01/Etapas_SCRUM_0.jpeg.webp?itok=C3rcVjL9)

**Scrum** es el marco de trabajo ágil más popular y ampliamente adoptado. Es un marco liviano que ayuda a las personas, equipos y organizaciones a generar valor a través de soluciones adaptativas para problemas complejos. Scrum se define por sus roles, eventos y artefactos, que trabajan en conjunto para crear un ciclo de inspección y adaptación continuo.

[![¿Qué es SCRUM?](https://markdown-videos-api.jorgenkh.no/youtube/XDOayQIuC78?width=640&height=360)](https://www.youtube.com/watch?v=XDOayQIuC78)

#### Roles en Scrum (Los 3 Pilares del Equipo Scrum)

![Scrum Roles Stakeholders](https://media.deloitte.com/is/image/deloitte/Deloitte-ES-tecnologia-roles-scrum-4x1:Desktop?$Responsive$&fmt=webp&fit=stretch,1&dpr=on,1.25)

- **Product Owner (PO - Propietario del Producto)**
  - **Función Principal**: Es la persona responsable de maximizar el valor del producto que construye el Equipo de Desarrollo. Actúa como el puente entre el cliente/negocio y el equipo.
  - **Responsabilidades Clave**:
    - Gestionar y ser el único responsable del `Product Backlog` (creación, refinamiento, ordenamiento y visibilidad). Esto implica trabajar en estrecha **colaboración con el Equipo de Desarrollo** para asegurar que los ítems del _backlog_ sean claros y listos para ser implementados, y con los _stakeholders_ para entender y priorizar sus necesidades.
    - Asegurar que los elementos del Product Backlog sean claros, concisos y entendidos por el Equipo de Desarrollo, fomentando la **comunicación bidireccional y el entendimiento compartido**.
    - Comunicar la visión del producto y los objetivos de negocio a todo el equipo, manteniendo a todos **alineados y motivados** hacia un objetivo común.
    - Tomar decisiones sobre qué funcionalidades deben construirse y en qué orden, basándose en el **valor para el negocio y la factibilidad técnica** discutida con el equipo.

- **Scrum Master (SM)**:
  - **Función Principal**: Es un líder de servicio para el Equipo Scrum. Su objetivo es ayudar al equipo a entender y aplicar Scrum, eliminando impedimentos y asegurando que el proceso sea efectivo.
  - **Responsabilidades Clave**:
    - **Facilitar las ceremonias de Scrum** (Sprint Planning, Daily Scrum, Sprint Review, Retrospective), asegurando que sean productivas y que el equipo se adhiera a las reglas de tiempo y propósito.
    - **Entrenar y mentorizar** al Product Owner y al Equipo de Desarrollo en las prácticas y valores de Scrum. Esto incluye ayudar al equipo a mejorar sus habilidades de colaboración y auto-organización.
    - Ayudar a **resolver obstáculos e impedimentos** que bloqueen al equipo, actuando como un facilitador para que el equipo pueda concentrarse en su trabajo principal. Esto a menudo implica interactuar con personas fuera del equipo para despejar el camino.
    - Proteger al equipo de interrupciones externas, creando un **ambiente de trabajo enfocado y seguro** que fomente la productividad y la cohesión del equipo.
    - Fomentar la **auto-organización y la multifuncionalidad** dentro del equipo, capacitando a los miembros para que tomen decisiones colectivas y compartan responsabilidades.

- **Development Team (Equipo de Desarrollo)**:
  - **Función Principal**: Son el grupo de profesionales que construyen el Incremento del producto. Son **auto-organizados** (deciden cómo hacer el trabajo para lograr el objetivo del Sprint) y **multifuncionales** (tienen todas las habilidades necesarias para entregar un Incremento sin depender excesivamente de personas externas al equipo).
  - **Responsabilidades Clave**:
    - **Planificar el `Sprint Backlog`** de forma conjunta, decidiendo qué pueden comprometerse a entregar en un Sprint. Esto requiere una **colaboración intensa** para descomponer los ítems del Product Backlog en tareas, estimar el esfuerzo y asignarse el trabajo.
    - **Convertir los elementos del Product Backlog seleccionados en un "Incremento" funcional** al final de cada Sprint. Esto implica **trabajo colaborativo diario**, ayudándose mutuamente para superar desafíos técnicos y de implementación.
    - Ser responsables colectivamente de la **calidad del trabajo** y de la entrega de valor. El equipo comparte la responsabilidad del éxito y del fracaso.
    - **Colaborar estrechamente y de forma continua** con el Product Owner y el Scrum Master, así como con otros _stakeholders_ cuando sea necesario, para clarificar requisitos, resolver dudas y demostrar el progreso.
    - No hay títulos jerárquicos internos (ej., "líder técnico") dentro del Equipo de Desarrollo. Todos son desarrolladores que trabajan juntos para un objetivo común, fomentando un **ambiente de igualdad y responsabilidad compartida**.

#### Eventos (Ceremonias) en Scrum

Los eventos de Scrum son oportunidades fijas en el tiempo (_time-boxed_) para la inspección y adaptación, y son fundamentales para la colaboración efectiva del equipo.

![Los eventos de Scrum](https://media.deloitte.com/is/image/deloitte/Scrum-inforgraphic-2?$Responsive$&wid=1440&fmt=webp&fit=stretch,1&dpr=off)

- **Sprint**: Es el corazón de Scrum y un contenedor de tiempo fijo (generalmente de 1 a 4 semanas, siendo 2 semanas lo más común). Dentro de cada Sprint, todos los demás eventos de Scrum tienen lugar. Un nuevo Sprint comienza inmediatamente después de la finalización del anterior. El objetivo es entregar un "Incremento" de producto potencialmente liberable. El Sprint en sí mismo es una oportunidad para que el equipo trabaje en conjunto de forma ininterrumpida y enfocada.

- **Sprint Planning (Planificación del Sprint)**:
  - **Cuándo**: Al inicio de cada Sprint.
  - **Duración**: _Time-boxed_ a 8 horas para un Sprint de un mes (proporcionalmente menos para Sprints más cortos).
  - **Propósito**: El Equipo Scrum colabora intensamente para definir:
    - **¿Qué se va a entregar en este Sprint?** (Sprint Goal y elementos seleccionados del Product Backlog). Esta es una **negociación activa y colaborativa** entre el Product Owner y el Equipo de Desarrollo, donde el equipo se compromete con lo que cree que es factible.
    - **¿Cómo se va a conseguir ese trabajo?** (Planificación detallada de tareas que forman el Sprint Backlog). El Equipo de Desarrollo **se auto-organiza** para dividir el trabajo, identificar dependencias y estimar el esfuerzo, lo que fomenta el sentido de pertenencia y la responsabilidad compartida.

- **Daily Scrum (Scrum Diario / Stand-up)**:
  - **Cuándo**: Cada día del Sprint, a la misma hora y en el mismo lugar.
  - **Duración**: _Time-boxed_ a 15 minutos.
  - **Propósito**: Es una reunión clave de **sincronización y planificación para el Equipo de Desarrollo**. No es un informe de estado para el Scrum Master o el Product Owner (aunque pueden asistir). El equipo inspecciona su progreso hacia el `Sprint Goal` y adapta el `Sprint Backlog` para el día siguiente. Es una oportunidad diaria para que los miembros del equipo **colaboren activamente, identifiquen impedimentos y coordinen sus esfuerzos**. Las preguntas comunes que el equipo se hace ayudan a enfocar la discusión en el progreso colectivo:
    - ¿Qué hice ayer que ayudó al Equipo de Desarrollo a lograr el objetivo del Sprint?
    - ¿Qué haré hoy para ayudar al Equipo de Desarrollo a lograr el objetivo del Sprint?
    - ¿Veo algún impedimento que me impida a mí o al Equipo de Desarrollo lograr el objetivo del Sprint?"

- **Sprint Review (Revisión del Sprint)**:
  - **Cuándo**: Al final de cada Sprint.
  - **Duración**: _Time-boxed_ a 4 horas para un Sprint de un mes.
  - **Propósito**: El Equipo Scrum y los _stakeholders_ clave (clientes, usuarios, gerentes) colaboran para inspeccionar el **"Incremento"** completado y adaptar el Product Backlog si es necesario. El Equipo de Desarrollo **demuestra el software funcional** que ha creado durante el Sprint, y se produce una **discusión abierta y transparente** sobre el progreso, el estado del producto y las posibles futuras funcionalidades. Esta es una interacción vital para asegurar que el equipo esté construyendo el producto correcto y para obtener retroalimentación directa y valiosa.

- **Sprint Retrospective (Retrospectiva del Sprint)**:
  - **Cuándo**: Después del Sprint Review y antes del siguiente Sprint Planning.
  - **Duración**: _Time-boxed_ a 3 horas para un Sprint de un mes.
  - **Propósito**: Es una reunión de **mejora continua** para el Equipo Scrum. El equipo inspecciona cómo trabajaron durante el Sprint en términos de procesos, herramientas, relaciones e interacciones. El objetivo es identificar qué salió bien, qué se puede mejorar, y crear un plan de acciones concretas para el próximo Sprint. Es un espacio seguro para la **reflexión honesta y la colaboración** para optimizar el rendimiento del equipo, la comunicación y la calidad del trabajo.

#### Artefactos de Scrum

Los artefactos de Scrum representan el trabajo o el valor para proporcionar transparencia y oportunidades de inspección y adaptación.

![Artefactos Scrum](https://media.deloitte.com/is/image/deloitte/Deloitte-ES-tecnologia-artefactos-Scrum:Desktop?$Responsive$&fmt=webp&fit=stretch,1&dpr=on,1.25)

- **Product Backlog (Pila del Producto)**:
  - **Descripción**: Una lista dinámica, priorizada y ordenada de todo el trabajo que se necesita realizar en el producto. Contiene características, requisitos, mejoras y correcciones de errores, a menudo expresados como Historias de Usuario. Es la única fuente de trabajo para el Equipo de Desarrollo.
  - **Características**: Es **_"DETAILED Appropriately, Estimated, Emergent, Prioritized" (DEEP)_**. Nunca está "terminado", evoluciona constantemente. Es gestionado por el Product Owner, pero su contenido y priorización son el resultado de la colaboración constante con el Equipo de Desarrollo y los _stakeholders_.
  - **Responsable**: Product Owner.

- **Sprint Backlog (Pila del Sprint)**:
  - **Descripción**: Un subconjunto de elementos del Product Backlog que el Equipo de Desarrollo ha seleccionado para el Sprint actual, junto con el plan del equipo para entregar el Incremento y lograr el Sprint Goal. Es un plan muy detallado y visible para el equipo.
  - **Características**: Solo el Equipo de Desarrollo puede modificarlo, lo que refuerza su auto-organización y su compromiso colectivo con el trabajo del Sprint.
  - **Responsable**: Equipo de Desarrollo.

- **Increment (Incremento)**:
  - **Descripción**: La suma de todos los elementos del Product Backlog completados durante un Sprint, más el valor de todos los Increments de los Sprints anteriores. Debe ser un producto funcional, utilizable y **potencialmente entregable**, independientemente de si se decide lanzarlo al mercado o no.
  - **Características**: Es el resultado tangible del trabajo colaborativo del Sprint que cumple la **"Definición de Terminado" (`Definition of Done`)**, un acuerdo del equipo sobre qué significa que un elemento esté "hecho".
  - **Responsable**: Equipo de Desarrollo.

### 3.5. Beneficios en el Desarrollo de APIs y Software

La adopción de metodologías ágiles como Scrum ofrece numerosos beneficios que son particularmente relevantes para el desarrollo de APIs y software moderno:

- **Flexibilidad y Adaptación**: Las APIs a menudo tienen requisitos que evolucionan rápidamente a medida que las aplicaciones que las consumen (frontends, móviles) también cambian o surgen nuevas necesidades de negocio. Scrum permite a los equipos responder rápidamente a estos cambios, adaptando el `Product Backlog` y priorizando en cada Sprint. Esta flexibilidad fomenta una **mentalidad de equipo adaptable** donde los miembros están preparados para ajustar su enfoque de manera colaborativa.
- **Entrega Temprana y Continua de Valor**: En lugar de esperar un gran lanzamiento monolítico, el desarrollo de APIs en un enfoque ágil permite entregar _endpoints_ funcionales y módulos de API en incrementos cortos. Esto significa que los equipos de frontend o los clientes de la API pueden comenzar a integrar y probar antes, proporcionando retroalimentación temprana y acelerando el tiempo de comercialización. Esta cadencia de entrega requiere una **colaboración constante y una "Definición de Terminado" clara** dentro del equipo para asegurar que cada incremento sea de alta calidad y esté listo para su uso.
- **Calidad Asegurada y Pruebas Continuas**: La naturaleza incremental del desarrollo ágil facilita la integración constante y las pruebas tempranas. Al entregar pequeños "incrementos" de software funcional en cada Sprint, el equipo puede realizar **pruebas exhaustivas y continuas** a medida que se construye el producto. Esto permite identificar y corregir defectos más pronto en el ciclo de vida del desarrollo, reduciendo significativamente el costo y el esfuerzo de las correcciones. La capacidad de probar cada incremento asegura que las nuevas funcionalidades de la API se integren sin problemas y funcionen como se espera, mejorando la calidad general del producto de manera progresiva.
- **Mejora Continua**: Las retrospectivas regulares son una oportunidad invaluable para que el equipo reflexione sobre sus procesos de desarrollo de APIs, herramientas (ej., configuración de CI/CD para APIs), prácticas de codificación (ej., adherencia a principios RESTful) y **dinámicas de equipo**. Esto lleva a una mejora constante de la calidad de la API, de la eficiencia del equipo y de la **cohesión del grupo**. Los equipos ágiles se esfuerzan por aprender y adaptarse juntos.
- **Colaboración Activa y Transparencia**: Scrum promueve una comunicación constante y fluida, que es el **corazón del trabajo en grupo efectivo**. Para el desarrollo de APIs, esto es vital para colaborar estrechamente con los equipos de frontend, móviles o terceros que serán los consumidores de la API. La transparencia de los artefactos (`Product Backlog`, `Sprint Backlog`) asegura que todos los _stakeholders_ tengan visibilidad del progreso y los desafíos, facilitando una **colaboración más informada y alineada**.
- **Gestión de Riesgos Reducida**: Al entregar software funcional en ciclos cortos, los riesgos se identifican y abordan más temprano. Si un _endpoint_ de API no funciona como se esperaba o un requisito es malinterpretado, se detecta y corrige rápidamente, evitando grandes retrabajos al final del proyecto. Esta mitigación de riesgos es un esfuerzo **colectivo** del equipo, que inspecciona el progreso diariamente y se adapta para resolver problemas de manera conjunta.

### 3.6. Aplicando Scrum en Equipos Pequeños o Distribuidos

Scrum es flexible y puede adaptarse tanto a equipos grandes como pequeños, así como a equipos que trabajan de manera remota o distribuida. Para equipos pequeños (3-5 personas), las ceremonias suelen ser más ágiles y la comunicación más directa, pero es igualmente importante mantener la disciplina de los eventos y la transparencia en los artefactos.

#### Recomendaciones para equipos pequeños o distribuidos

- Utiliza videollamadas para las ceremonias Scrum si el equipo no está en la misma ubicación.
- Mantén los tableros de tareas y el Product Backlog siempre visibles y actualizados para todos los miembros.
- Fomenta la comunicación asíncrona (chats, foros) para resolver dudas fuera de las reuniones.
- Adapta la duración de las ceremonias según el tamaño y madurez del equipo.

#### Herramientas ágiles recomendadas

- [Jira](https://www.atlassian.com/software/jira): Gestión profesional de Sprints, Backlogs y tableros Kanban/Scrum.
- [Trello](https://trello.com/): Tableros visuales simples y colaborativos, ideales para equipos pequeños.
- [Azure Boards](https://azure.microsoft.com/en-us/services/devops/boards/): Integración con pipelines y repositorios, útil para equipos que usan Azure DevOps.
- [ClickUp](https://clickup.com/): Gestión integral de tareas, documentos y Sprints.
- [Slack](https://slack.com/): Comunicación instantánea y canales temáticos para equipos distribuidos.

El uso de estas herramientas facilita la colaboración, la transparencia y el seguimiento del progreso, especialmente cuando los miembros del equipo no comparten la misma oficina.

## Recursos y Referencias

- [Documentación oficial de Java Records (Oracle)](https://docs.oracle.com/en/java/javase/17/language/records.html)
- [MapStruct - Documentación oficial](https://mapstruct.org/documentation/stable/reference/html/)
- [Spring Boot - Documentación](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Scrum Guide (Guía Oficial de Scrum)](https://scrumguides.org/)
- [Manifiesto Ágil](https://agilemanifesto.org/iso/es/manifesto.html)
- [Bean Validation (Jakarta Validation)](https://jakarta.ee/specifications/bean-validation/3.0/jakarta-bean-validation-spec-3.0.html)
- [OpenAPI Specification](https://swagger.io/specification/)
