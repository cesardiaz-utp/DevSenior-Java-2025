# Clase 1: El Rol de la IA y Conexión Frontend-Backend

¡Hola a todos! Bienvenidos al **Módulo 9**. En esta primera clase, nos sumergiremos en cómo las tecnologías modernas de IA pueden potenciar nuestro trabajo diario. A menudo, el desarrollo Full Stack implica tareas repetitivas como crear DTOs o servicios base. Veremos cómo usar **Cursor** como un copiloto para automatizar estas tareas, permitiéndonos enfocarnos en la lógica de negocio más compleja.

Además, haremos un repaso fundamental de la comunicación entre **Spring Boot y Angular**, pero con un enfoque muy actual: el uso de los **Angular Signals**. Los `Signals` han revolucionado el manejo del estado, y aprenderemos a usarlos para consumir nuestra API de forma reactiva y eficiente.

## Objetivos de aprendizaje

- Comprender cómo la Inteligencia Artificial puede ser utilizada como una herramienta para generar código y acelerar el desarrollo full stack.
- Recordar y aplicar los conceptos de cómo un backend con Spring Boot expone sus servicios REST.
- Aprender a consumir endpoints del backend desde Angular de forma reactiva, convirtiendo Observables a Signals.

## 1. La IA como Asistente en el Desarrollo Full Stack

La Inteligencia Artificial ha dejado de ser una herramienta del futuro para convertirse en un asistente indispensable para los desarrolladores. Sin embargo, su uso efectivo requiere de disciplina y un enfoque estratégico.

### 1.1. El Desafío del "Vibecoding" y su Uso Adecuado

El término **"Vibecoding"** se refiere a la práctica de generar código con la IA de manera impulsiva o sin un plan claro, a menudo pegando el resultado sin una revisión crítica. Esta mentalidad, aunque tentadora por la velocidad aparente, es una de las principales trampas. Al "Vibecodear", no solo corremos el riesgo de introducir código defectuoso o ineficiente, sino que también perdemos la oportunidad de aprender y entender la lógica subyacente. Un desarrollador que "Vibecodea" podría generar una clase de 200 líneas en segundos, pero si no entiende por qué cada línea está ahí, estará en una posición vulnerable. La falta de comprensión puede llevar a errores sutiles, fallos de seguridad o, en el mejor de los casos, un código difícil de mantener en el futuro.

El uso adecuado de la IA se basa en una mentalidad de **colaboración y revisión**. Debemos ver a la IA como un par de programadores expertos que pueden ayudarnos con tareas tediosas, pero la responsabilidad final del código es siempre nuestra. La IA debe ser un acelerador de la productividad, no una excusa para la pereza mental. La verdadera destreza profesional radica en saber qué delegar a la máquina y qué partes de la lógica de negocio requieren nuestra atención, nuestro criterio y nuestra creatividad.

### 1.2. Ejemplos Prácticos del Uso de la IA en el Desarrollo

A continuación, veremos cómo aplicar estas prácticas en escenarios reales de desarrollo Full Stack, usando la IA como un copiloto inteligente.

#### 1.2.1. Uso de la IA para Generar Código (Backend)

**Escenario**: Necesitamos agregar un nuevo método en nuestro `PropertyService` que encuentre todas las propiedades de una ciudad específica.

**Prompt a la IA**:

> "En Spring Boot, crea un método en PropertyService para encontrar Property por city. El método debe recibir un String city y devolver una lista de PropertyDTO. Utiliza el PropertyRepository y el PropertyMapper para la conversión."

**Respuesta de la IA (fragmento del código)**:

```Java
// Dentro de PropertyService.java
public List<PropertyDTO> findAllByCity(String city) {
    return propertyRepository.findAllByCity(city).stream()
            .map(propertyMapper::toDTO)
            .collect(Collectors.toList());
}
```

Necesitas agregar este método al repositorio

```Java
// Dentro de PropertyRepository.java
public List<Property> findAllByCity(String city);
```

**Análisis del Ejemplo**: Aquí, la IA nos ha ahorrado el tiempo de escribir el esqueleto del método. Es nuestra responsabilidad, como desarrolladores, asegurarnos de que la firma del método en el repositorio sea correcta y que la lógica de mapeo se ajuste a las necesidades del proyecto.

#### 1.2.2. Uso de la IA para Explicar Código

**Escenario**: El código generado por la IA funciona, pero queremos entender la lógica de la línea `propertyRepository.findAllByCity(city).stream()...`.

**Prompt a la IA**:

> "Explica en detalle el código `propertyRepository.findAllByCity(city).stream().map(propertyMapper::toDTO).collect(Collectors.toList())`. ¿Qué hace cada parte de la cadena y por qué es una buena práctica?"

**Respuesta de la IA**:

> "Claro. Este código utiliza la API de Streams de Java para procesar una lista de objetos de manera funcional.
>
> - `propertyRepository.findAllByCity(city)`: Llama a un método del repositorio para obtener una `List<Property>` de la base de datos, donde la `city` coincide con el valor proporcionado. Este método se genera automáticamente por **Spring Data JPA** a partir de su nombre, un patrón conocido como `Query Method`.
>
> - `.stream()`: Convierte la lista de `Property` en un `Stream`, que es una secuencia de elementos. Esto nos permite usar operaciones funcionales como `map` en un estilo declarativo y sin estado intermedio.
>
> - `.map(propertyMapper::toDTO)`: Esta es la parte clave. Por cada `Property` en el `Stream`, aplica el método `toDTO` del `PropertyMapper`. `propertyMapper::toDTO` es una referencia a un método, una forma concisa de pasar una función a la operación `map`. El resultado es un `Stream` de `PropertyDTO`.
>
> - `.collect(Collectors.toList())`: Recopila todos los `PropertyDTO` del `Stream` y los pone de nuevo en una `List`.
>
>Esta técnica es una buena práctica porque es concisa, legible y declarativa. Describe qué quieres lograr (mapear cada entidad a un DTO), en lugar de cómo hacerlo con un bucle for tradicional, lo que reduce la posibilidad de errores y mejora la legibilidad."

#### 1.2.3. Uso de la IA para Refactorización y Buenas Prácticas

**Escenario**: El método `findAllByCity` es funcional, pero queremos mejorarlo con validación y manejo de errores.

**Prompt a la IA**:

> "El método `findAllByCity` que generaste es bueno. Ahora, refactorízalo para que, si el `city` es nulo o una cadena vacía, se lance una `IllegalArgumentException` con un mensaje claro. ¿Qué otras buenas prácticas de manejo de errores sugieres?"

**Respuesta de la IA (código refactorizado)**:

```Java
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.stream.Collectors;

@Service
public class PropertyService {

    private final PropertyRepository propertyRepository;
    private final PropertyMapper propertyMapper;

    // ... (Constructor)

    public List<PropertyDTO> findAllByCity(String city) {
        // Validación de buenas prácticas
        if (city == null || city.trim().isEmpty()) {
            throw new IllegalArgumentException("La ciudad no puede ser nula o vacía.");
        }

        List<Property> properties = propertyRepository.findAllByCity(city);

        // Retornar una lista vacía si no se encuentran resultados,
        // en lugar de un `null` para evitar NullPointerExceptions.
        if (properties.isEmpty()) {
            return List.of(); // Devuelve una lista inmutable vacía
        }

        // Mapeo utilizando Streams
        return properties.stream()
                .map(propertyMapper::toDTO)
                .collect(Collectors.toList());
    }
}
```

**Análisis del Ejemplo**: La IA no solo refactorizó el código, sino que también propuso una buena práctica crucial: devolver una lista vacía en lugar de `null`. Esta sugerencia previene un error común y hace que la API sea más robusta.

#### 1.2.4. Uso de la IA para Generar Documentación

**Escenario**: Tenemos el código listo y funcionando, pero necesitamos documentarlo para que otros desarrolladores entiendan su propósito y uso.

**Prompt a la IA (Backend)**:

> "Genera la documentación Javadoc para la clase `PropertyService` en Spring Boot. Documenta la clase, el constructor y el método `findAllByCity`, explicando el propósito de cada uno, los parámetros y el valor de retorno."

**Respuesta de la IA (fragmento de código con Javadoc)**:

```Java
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.stream.Collectors;

/**
 * Servicio de lógica de negocio para la gestión de propiedades.
 * Esta clase orquesta las operaciones relacionadas con las propiedades,
 * como la búsqueda y el mapeo de datos.
 * Utiliza {@link PropertyRepository} para la persistencia de datos
 * y {@link PropertyMapper} para la conversión entre entidades y DTOs,
 * desacoplando así la lógica de negocio de la capa de acceso a datos.
 */
@Service
public class PropertyService {

    private final PropertyRepository propertyRepository;
    private final PropertyMapper propertyMapper;

    /**
     * Constructor para la inyección de dependencias.
     * Spring se encarga de proveer las instancias de
     * {@link PropertyRepository} y {@link PropertyMapper}
     * a través de este constructor.
     *
     * @param propertyRepository Repositorio para el acceso a datos de propiedades.
     * @param propertyMapper Mapeador para convertir entre DTOs y entidades de propiedad.
     */
    public PropertyService(PropertyRepository propertyRepository, PropertyMapper propertyMapper) {
        this.propertyRepository = propertyRepository;
        this.propertyMapper = propertyMapper;
    }

    /**
     * Busca todas las propiedades en una ciudad específica.
     * El método primero valida el nombre de la ciudad, luego delega
     * la búsqueda al repositorio y finalmente mapea las entidades
     * resultantes a DTOs.
     *
     * @param city El nombre de la ciudad a buscar. No puede ser nulo o vacío.
     * @return Una lista de {@link PropertyDTO} de las propiedades encontradas.
     * Devuelve una lista inmutable vacía si no se encuentran propiedades.
     * @throws IllegalArgumentException si el parámetro 'city' es nulo o vacío.
     */
    public List<PropertyDTO> findAllByCity(String city) {
        // ... (código del método)
    }
}
```

**Prompt a la IA (Frontend)**:

> "Genera la documentación con TSDoc para el `PropertyService` de Angular. Documenta la clase y sus propiedades `propertiesFullState` y `properties`."

**Respuesta de la IA (fragmento de código con TSDoc)**:

```Typescript
import { HttpClient } from '@angular/common/http';
import { Injectable, signal, Signal } from '@angular/core';
import { toSignal } from '@angular/core/rxjs-interop';
import { Observable } from 'rxjs';
import { Property } from '../models/property.model';

/**
 * Servicio de Angular para la gestión de datos de propiedades.
 * Se encarga de la comunicación con el backend a través del `HttpClient` para
 * obtener las propiedades y gestiona el estado de manera reactiva usando Signals,
 * simplificando la gestión de suscripciones y la lógica del componente.
 */
@Injectable({
  providedIn: 'root'
})
export class PropertyService {

  private apiUrl = 'http://localhost:8080/api/properties';

  constructor(private http: HttpClient) { }

  /**
   * Un Signal que contiene la lista de propiedades obtenida del backend.
   * La suscripción al Observable se gestiona automáticamente por `toSignal()`,
   * lo que evita fugas de memoria. El valor inicial es 'undefined' hasta que
   * la petición se completa exitosamente.
   */
  properties: Signal<Property[] | undefined> = toSignal(this.getPropertiesObservable());

  /**
   * Un Signal que contiene la lista de propiedades, pero con un valor inicial
   * de un array vacío. Esto es una buena práctica para evitar que el componente
   * se rompa al renderizar por primera vez mientras espera la respuesta
   * de la petición HTTP. Provee un estado más robusto para los componentes
   * que no necesitan manejar el estado de `undefined`.
   */
  propertiesFullState = toSignal(
    this.http.get<Property[]>(this.apiUrl),
    { initialValue: [] }
  );

  /**
   * Obtiene la lista completa de propiedades desde la API REST.
   * Este método devuelve un Observable, que es la forma estándar de
   * manejar operaciones asíncronas en Angular.
   * @returns Un Observable que emite una lista de objetos de tipo Property.
   */
  getPropertiesObservable(): Observable<Property[]> {
    return this.http.get<Property[]>(this.apiUrl);
  }
}
```

**Análisis del Ejemplo**: La IA no solo generó la documentación, sino que también proporcionó explicaciones claras y concisas sobre el propósito de cada elemento. Esto acelera el proceso de documentación y promueve un código más comprensible y colaborativo.

## 2. Repaso del Backend con Spring Boot: Las Capas de la Aplicación

Para construir una API REST robusta y mantenible en Spring Boot, seguimos una arquitectura de capas bien definida. Esto nos ayuda a separar las responsabilidades y a mantener nuestro código organizado. Las capas principales son:

### 2.1. La capa de Entidad (`Entity`)

Esta es la representación de nuestros datos en la base de datos. Un `Entity` es una clase POJO (Plain Old Java Object) que mapea directamente a una tabla de la base de datos a través de anotaciones de **JPA** (Java Persistence API).

```Java
@Entity
public class Property {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private String city;
    private double price;
    private int bedrooms;
    private int bathrooms;
    private String imageUrl;
    // Getters, setters, constructores...
}
```

### 2.2. La capa de Repositorio (`Repository`)

Esta capa se encarga de la comunicación con la base de datos. En Spring Boot, usamos **Spring Data JPA** para crear repositorios que, a través de la herencia de la interfaz `JpaRepository`, nos ofrecen métodos `CRUD` (Create, Read, Update, Delete) listos para usar sin tener que escribir una sola línea de código SQL.

```Java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface PropertyRepository extends JpaRepository<Property, Long> {
}
```

### 2.3. La capa de Servicio (`Service`)

Es la capa de lógica de negocio. Un servicio utiliza los repositorios para interactuar con la base de datos y, a menudo, es el lugar donde se implementan reglas de negocio complejas. En esta capa también se realiza el mapeo entre las Entidades y los DTOs.

#### Data Transfer Objects (DTO) con `Records`

Los **DTOs** son objetos de transferencia de datos que se utilizan para enviar datos entre las capas de la aplicación o entre el backend y el frontend. A partir de Java 16, podemos usar `Records`, que son clases inmutables y concisas, perfectas para este propósito.

```Java
public record PropertyDTO(
    Long id,
    String address,
    String city,
    double price,
    int bedrooms,
    int bathrooms,
    String imageUrl
) {}
```

#### Mapeo con MapStruct

Para evitar el tedioso y propenso a errores mapeo manual entre `Entity` y `DTO`, usamos **MapStruct**, una librería que genera automáticamente el código de mapeo en tiempo de compilación. Solo necesitamos definir una interfaz:

```Java
import org.mapstruct.Mapper;
import org.mapstruct.factory.Mappers;

@Mapper(componentModel = "spring")
public interface PropertyMapper {
    PropertyMapper INSTANCE = Mappers.getMapper(PropertyMapper.class);

    PropertyDTO toDTO(Property property);
    Property toEntity(PropertyDTO propertyDTO);
}
```

Ahora, en nuestro servicio, podemos utilizar el `mapper` inyectado para realizar la conversión.

```Java
@Service
public class PropertyService {

    private final PropertyRepository propertyRepository;
    private final PropertyMapper propertyMapper;

    public PropertyService(PropertyRepository propertyRepository, PropertyMapper propertyMapper) {
        this.propertyRepository = propertyRepository;
        this.propertyMapper = propertyMapper;
    }

    public List<PropertyDTO> findAll() {
        return propertyRepository.findAll().stream()
            .map(propertyMapper::toDTO)
            .collect(Collectors.toList());
    }
}
```

### 2.4. La capa de Controlador (`Controller`)

Esta es la capa de presentación que expone los endpoints **REST** que el frontend consumirá. Recibe peticiones HTTP, delega la lógica al `Service` y devuelve una respuesta.

```Java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
@RequestMapping("/api/properties")
public class PropertyController {

    private final PropertyService propertyService;

    public PropertyController(PropertyService propertyService) {
        this.propertyService = propertyService;
    }

    @GetMapping
    public List<PropertyDTO> getAllProperties() {
        return propertyService.findAll();
    }
}
```

## 3. Consumo de Endpoints con Angular y Signals

### 3.1. Configuración de Peticiones HTTP en Angular

Para proyectos con la arquitectura `standalone` de Angular 20, la configuración de peticiones HTTP es muy sencilla. Simplemente necesitas proveer los servicios HTTP en el archivo `app.config.ts` para que puedan ser inyectados en tus servicios y componentes.

```Java
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [provideHttpClient()] // <-- Proveer los servicios HTTP
};
```

Una vez configurado, puedes usar el servicio `HttpClient` para realizar peticiones inyectándolo en el constructor de tus servicios.

### 3.2. Peticiones con Observables y Conversión a Signals

En Angular, el servicio `HttpClient` devuelve un `Observable`, que es un flujo de datos asíncrono. La función `toSignal()` es la clave para esta conversión, ya que toma un `Observable` y lo transforma en una `Signal`, gestionando automáticamente la suscripción y desuscripción.

A continuación, separamos los enfoques para que puedas ver sus diferencias.

#### Ejemplo con Observables (Enfoque Tradicional)

En este enfoque, el servicio expone un método que devuelve un `Observable`. El componente que lo consume es responsable de suscribirse para obtener los datos y, si es necesario, gestionar la desuscripción. El componente debe manejar la lógica de carga y errores manualmente.

```Typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { Property } from '../models/property.model'; // Suponiendo que tienes este modelo

/**
 * Servicio de Angular para la gestión de datos de propiedades.
 * Utiliza Observables para manejar la comunicación con el backend.
 */
@Injectable({
  providedIn: 'root'
})
export class PropertyServiceWithObservables {

  private apiUrl = 'http://localhost:8080/api/properties';

  constructor(private http: HttpClient) { }

  /**
   * Obtiene la lista completa de propiedades desde la API REST.
   * Este método devuelve un Observable, que es la forma estándar de
   * manejar operaciones asíncronas en Angular.
   * @returns Un Observable que emite una lista de objetos de tipo Property.
   */
  getProperties(): Observable<Property[]> {
    return this.http.get<Property[]>(this.apiUrl);
  }
}
```

Ahora, veamos cómo un componente consumiría este servicio. El enfoque más común y limpio es usar el `async` pipe, que gestiona automáticamente la suscripción y la desuscripción.

**Ejemplo en un `PropertyListComponent` que usa Observables**:

```Typescript
import { Component, inject } from '@angular/core';
import { CommonModule, AsyncPipe, CurrencyPipe } from '@angular/common';
import { PropertyServiceWithObservables } from '../services/property.service';
import { Observable } from 'rxjs';
import { Property } from '../models/property.model';

@Component({
  selector: 'app-property-list-observables',
  standalone: true,
  imports: [CommonModule, AsyncPipe, CurrencyPipe],
  template: `
    <!-- Se usa el control de flujo @if para manejar el estado del Observable -->
    @if (properties$ | async; as properties) {
      <div class="table-container">
        <table class="w-full text-left table-auto">
          <thead>
            <tr>
              <th class="py-2 px-4 border-b">ID</th>
              <th class="py-2 px-4 border-b">Dirección</th>
              <th class="py-2 px-4 border-b">Precio</th>
            </tr>
          </thead>
          <tbody>
            @for (property of properties; track property.id) {
              <tr>
                <td class="py-2 px-4 border-b">{{ property.id }}</td>
                <td class="py-2 px-4 border-b">{{ property.address }}</td>
                <td class="py-2 px-4 border-b">{{ property.price | currency:'USD':'symbol':'1.0-2' }}</td>
              </tr>
            }
          </tbody>
        </table>
      </div>
    } @else {
      <div class="flex items-center justify-center p-4">
        <p>Cargando propiedades...</p>
      </div>
    }
  `,
  styleUrls: ['./property-list.component.css']
})
export class PropertyListObservablesComponent {
  // Inyectamos el servicio
  private propertyService = inject(PropertyServiceWithObservables);

  // Declaramos una propiedad que es un Observable de un array de propiedades
  // El '$' al final del nombre de la variable es una convención para indicar que es un Observable
  properties$: Observable<Property[]> = this.propertyService.getProperties();
}
```

#### Ejemplo con Signals (Enfoque Moderno)

Con la introducción de `Signals`, podemos convertir un `Observable` en una `Signal` usando la función `toSignal()`. Esto simplifica enormemente la gestión del estado, ya que la suscripción y desuscripción se manejan automáticamente. El componente solo necesita leer el valor de la `Signal`. La reactividad se vuelve más explícita y fácil de seguir. El `initialValue` es una buena práctica para evitar que el componente se rompa al renderizar sin datos, proporcionando un estado inicial seguro.

```Typescript
import { HttpClient } from '@angular/common/http';
import { Injectable, signal, Signal } from '@angular/core';
import { toSignal } from '@angular/core/rxjs-interop';
import { Observable } from 'rxjs';
import { Property } from '../models/property.model'; // Suponiendo que tienes este modelo

/**
 * Servicio de Angular para la gestión de datos de propiedades.
 * Utiliza Signals para gestionar el estado de manera reactiva,
 * simplificando la gestión de suscripciones y la lógica del componente.
 */
@Injectable({
  providedIn: 'root'
})
export class PropertyServiceWithSignals {

  private apiUrl = 'http://localhost:8080/api/properties';

  constructor(private http: HttpClient) { }

  /**
   * Un Signal que contiene la lista de propiedades, pero con un valor inicial
   * de un array vacío. Esto es una buena práctica para evitar que el componente
   * se rompa al renderizar por primera vez mientras espera la respuesta
   * de la petición HTTP. Provee un estado más robusto para los componentes
   * que no necesitan manejar el estado de `undefined`.
   * El `Observable` se gestiona automáticamente por `toSignal()`.
   */
  propertiesFullState = toSignal(
    this.http.get<Property[]>(this.apiUrl),
    { initialValue: [] } // Valor inicial opcional
  );
}
```

Ahora, veamos la diferencia en el componente. Con el enfoque de `Signals`, el código se vuelve más declarativo y limpio, ya que no nos preocupamos por la suscripción.

**Ejemplo en un `PropertyListComponent` que usa Signals**:

```Typescript
import { Component, inject } from '@angular/core';
import { CommonModule, CurrencyPipe } from '@angular/common';
import { PropertyServiceWithSignals } from '../services/property.service';

@Component({
  selector: 'app-property-list-signals',
  standalone: true,
  imports: [CommonModule, CurrencyPipe],
  template: `
    <!-- Aquí se consume la Signal directamente, sin necesidad de 'async' pipe -->
    @if (propertyService.propertiesFullState(); as properties) {
      <div class="table-container">
        <table class="w-full text-left table-auto">
          <thead>
            <tr>
              <th class="py-2 px-4 border-b">ID</th>
              <th class="py-2 px-4 border-b">Dirección</th>
              <th class="py-2 px-4 border-b">Precio</th>
            </tr>
          </thead>
          <tbody>
            @for (property of properties; track property.id) {
              <tr>
                <td class="py-2 px-4 border-b">{{ property.id }}</td>
                <td class="py-2 px-4 border-b">{{ property.address }}</td>
                <td class="py-2 px-4 border-b">{{ property.price | currency:'USD':'symbol':'1.0-2' }}</td>
              </tr>
            }
          </tbody>
        </table>
      </div>
    } @else {
      <div class="flex items-center justify-center p-4">
        <p>Cargando propiedades...</p>
      </div>
    }
  `,
  styleUrls: ['./property-list.component.css']
})
export class PropertyListSignalsComponent {
  propertyService = inject(PropertyServiceWithSignals);
}
```

Como puedes ver, el componente es mucho más limpio y no necesita preocuparse por desuscribirse del `Observable`, ya que `toSignal()` se encarga de todo.
