# Preguntas de Entrevista Técnica: Módulo 7 -Seguridad, JWT y APIs Avanzadas en Spring Boot

## Seccion 1: Manejo de Excepciones Globales y Validaciones Personalizadas

### 1. ¿Por qué es fundamental un `GlobalExceptionHandler` con `@ControllerAdvice` en APIs REST? Beneficios

- **Fundamental**: Uniformiza cómo la API responde a errores, previniendo mensajes poco claros o exposición de información.
- **Beneficios**: Consistencia en respuestas, centralización del código de manejo de errores, separación de preocupaciones (controladores limpios), captura excepciones no controladas.

### 2. Estructura típica de un `GlobalExceptionHandler`: anotaciones clave y su propósito

```Java
@ControllerAdvice // Interceptor global
public class GlobalExceptionHandler {
    @ExceptionHandler(ResourceNotFoundException.class) // Captura excepción
    @ResponseStatus(HttpStatus.NOT_FOUND) // Código HTTP
    public ErrorResponse handleResourceNotFound(ResourceNotFoundException ex) {
        // ... (construye ErrorResponse)
    }
    // ... otros métodos @ExceptionHandler
}
```

- `@ControllerAdvice`: Indica que la clase maneja excepciones globalmente.
- `@ExceptionHandler(ExceptionType.class)`: Marca un método para capturar una excepción específica.
- `@ResponseStatus(HttpStatus.CODE)`: Define el código de estado HTTP para la respuesta.

### 3. Uso de un DTO `ErrorResponse` consistente. ¿Qué información mínima debería incluir?

Un DTO `ErrorResponse` estandariza los mensajes de error de la API.

- **Información Mínima**: `timestamp`, `status` (código HTTP), `error` (tipo de error), `message` (descripción), `path` (ruta de la solicitud, opcional).

### 4. Enumera y explica tres tipos de excepciones comunes que capturarías globalmente y sus códigos de estado HTTP asociados

1. **`ResourceNotFoundException` (Personalizada)**: Recurso no encontrado (ej., ID inexistente).
    - **Código HTTP**: `404 Not Found`.
2. **`DataConflictException` (Personalizada)**: Conflicto de datos (ej., email duplicado, recurso con dependencias al eliminar).
    - **Código HTTP**: `409 Conflict`.
3. **`AccessDeniedException` (Spring Security)**: Usuario autenticado sin permisos para una operación.
    - **Código HTTP**: `403 Forbidden`.

### 5. ¿Cuándo es necesario crear una validación personalizada en Spring Boot, en lugar de usar las estándar de Bean Validation?

- La lógica de validación es **específica del negocio** y no cubierta por anotaciones estándar.
- La validación requiere **acceso a DB o servicios** (ej., verificar unicidad compleja o capacidad).
- Se validan **relaciones entre múltiples campos** de un DTO.
- Se necesita un **mensaje de error muy específico** o lógica reusable.

## Sección 2: Fundamentos de Spring Security

### 6. Explica la diferencia entre autenticación y autorización en APIs REST. Proporciona ejemplos. ¿Por qué es clave su separación?

- **Autenticación**: Verificar _quién eres_ (ej., login con `username`/`password`).
- **Autorización**: Determinar _qué puedes hacer_ una vez autenticado.
- **Separación**: Crucial para seguridad, escalabilidad y mantenibilidad, permitiendo gestionar identidad y permisos de forma independiente.

### 7. ¿Qué es Spring Security y su rol principal en Spring Boot? ¿Cómo se integra en la cadena de filtros?

- **Spring Security**: _Framework_ para autenticación y autorización en aplicaciones Java.
- **Rol Principal**: Proteger la aplicación del acceso no autorizado y manejar la seguridad en general.
- **Integración**: Se inserta en la **cadena de filtros de Servlet**, interceptando solicitudes antes de que lleguen a los controladores para aplicar reglas de seguridad.

### 8. ¿Qué es la autenticación básica (Basic Authentication) y cuándo la usarías?

- **Autenticación Básica**: Un esquema de autenticación HTTP simple donde las credenciales (`username:password`) se envían codificadas en Base64 en el encabezado `Authorization`.
- **Cuándo usar**: Para APIs internas, servicios entre servidores, o cuando la seguridad no es crítica (siempre con HTTPS). No recomendada para clientes de navegador o móviles por enviar credenciales en cada solicitud.

### 9. ¿Qué es CSRF (Cross-Site Request Forgery) y cómo lo protege Spring Security?

- **CSRF**: Un ataque donde un atacante engaña al navegador de un usuario autenticado para que envíe una solicitud no deseada a una aplicación web.
- **Protección Spring Security**: Genera un token CSRF único por sesión y lo exige en cada solicitud "unsafe" (POST, PUT, DELETE). Para APIs RESTful _stateless_ con JWT, CSRF suele deshabilitarse porque JWTs son inmunes a ataques CSRF al no basarse en cookies de sesión.

### 10. ¿Qué es CORS (Cross-Origin Resource Sharing) y cómo lo configuras en Spring Boot?

- **CORS**: Un mecanismo de seguridad de los navegadores que restringe que una página web de un origen (ej., `http://localhost:3000`) envíe solicitudes HTTP a un recurso de otro origen (ej., `http://localhost:8080`).
- **Configuración en Spring Boot**:
  - **Global para Spring Security**: Configurando **cors()** dentro del `SecurityFilterChain` para permitir orígenes, métodos y encabezados específicos.
  - **Por controlador/método**: Usando la anotación `@CrossOrigin`.

## Sección 3: JSON Web Tokens (JWT)

### 11. Describe el flujo completo de autenticación y autorización con JWT en una API REST

1. **Login**: Cliente envía credenciales a `/api/auth/authenticate`.
2. **Generación JWT**: Servidor valida y genera un JWT.
3. **Envío JWT**: Servidor devuelve el JWT al cliente.
4. **Almacenamiento Cliente**: Cliente guarda el JWT.
5. **Solicitudes Protegidas**: Cliente envía JWT en el encabezado `Authorization: Bearer <token>`.
6. **Filtro JWT**: Servidor (filtro de Spring Security) intercepta.
7. **Validación JWT**: Filtro valida token (firma, expiración) y extrae usuario/roles.
8. **Contexto Seguridad**: Filtro establece `SecurityContextHolder`.
9. **Autorización**: Spring Security aplica reglas (`@PreAuthorize`).
10. **Acceso**: Si autorizado, la solicitud llega al controlador.

### 12. ¿Qué es un JWT y sus tres partes principales? Explica el propósito de cada parte. ¿Por qué no incluir datos sensibles en el payload?

- **JWT**: Estándar abierto para transmitir información segura como JSON.
- **Partes Principales**:
  1. **Header**: Metadatos (tipo, algoritmo de firma).
  2. **Payload**: "Claims" o afirmaciones (ID de usuario, roles, expiración). **No cifrado, solo codificado en Base64**.
  3. **Signature**: Verificación de integridad (combina Header, Payload y clave secreta).
- **No datos sensibles en Payload**: Porque es legible por cualquiera que tenga el token; no es confidencial.

### 13. Compara el uso de JWTs con sesiones tradicionales. Ventajas y desventajas

- **JWTs (Sin estado)**:
  - **Ventajas**: Escalabilidad (microservicios, multi-dominio), mobile-friendly.
  - **Desventajas**: Revocación compleja, mayor tamaño, exposición si es robado.
- **Sesiones Tradicionales (Con estado)**:
  - **Ventajas**: Revocación sencilla, tokens pequeños.
  - **Desventajas**: Dificultad de escalabilidad horizontal, complejidad en microservicios.

### 14. ¿Cómo se protege la confidencialidad e integridad del JWT? Medidas de seguridad en servidor y cliente

- **Confidencialidad**:
  - **JWE (JSON Web Encryption)**: Para cifrar el _payload_.
  - **HTTPS/SSL/TLS**: Fundamental para cifrar el token en tránsito.
- **Integridad**:
  - **Firma Criptográfica (Signature)**: Verifica que el token no ha sido alterado.
  - **Algoritmos Robustos**: Usar algoritmos fuertes (HS256, RS256).
- **Almacenamiento Seguro (Cliente)**:
  - **`HttpOnly` cookies**: Preferido, no accesible por JavaScript (mitiga XSS).
  - **`localStorage` (con precauciones)**: Accesible por JS, susceptible a XSS.

### 15. Explica al menos dos estrategias de revocación de JWTs y cuándo usar cada una

1. **Listas Negras (Blacklisting)**:
    - **Descripción**: Añadir el JTI del token revocado a una lista. El filtro verifica esta lista en cada solicitud.
    - **Cuándo usar**: Revocación inmediata necesaria (seguridad, cambio de contraseña).
2. **Tokens de Corta Duración con Tokens de Refresco**:
    - **Descripción**: El JWT de acceso dura poco. Un _refresh token_ de larga duración (más seguro) se usa para obtener nuevos JWTs de acceso. El _refresh token_ puede ser revocado.
    - **Cuándo usar**: Equilibrio seguridad-usabilidad (más común).

### 16. ¿Cómo configurarías Spring Security para JWT en Spring Boot? Menciona los componentes clave

- `UserDetailsService`: Carga detalles del usuario/roles.
- `PasswordEncoder`: Codifica/verifica contraseñas.
- `AuthenticationManager`: Autentica usuarios.
- `SecurityFilterChain`: Configura reglas (deshabilitar CSRF/sesiones), añade `JwtRequestFilter`.
- `JwtRequestFilter`: Valida JWTs en cada solicitud.

### 17. Explica el propósito y funcionamiento de un `JwtRequestFilter` en la cadena de filtros

- **Propósito**: Interceptar solicitudes HTTP para validar JWTs y autenticar al usuario para esa solicitud.
- **Funcionamiento**: Extiende `OncePerRequestFilter`. Obtiene el token del encabezado `Authorization`, lo valida, extrae usuario/roles, y establece el `SecurityContextHolder` para permitir la continuación de la solicitud.

### 18. ¿Cómo se gestionan y aplican los roles de usuario (`ROLE_ADMIN`, `ROLE_STUDENT`) en Spring Security con JWT para proteger _endpoints_?

- **Gestión**: Roles se almacenan en DB, cargados por `UserDetailsService`, e incluidos en el JWT (payload).
- **Aplicación**: Spring Security usa el `SecurityContextHolder` (establecido por `JwtRequestFilter`). Se aplican con `@PreAuthorize` en métodos de controlador/servicio (ej., `@PreAuthorize("hasRole('ADMIN')")`) o mediante configuración de URL en `SecurityFilterChain`.

## Sección 4: Java Records, Mapeo con MapStruct, Desarrollo Ágil y Scrum

### 19. ¿Qué son las Java Records y cuál es su propósito principal?

Son una característica de Java 16 para crear clases inmutables y concisas de datos. Su propósito es reducir el _boilerplate_ code para DTOs y clases de valor.

### 20. ¿Cuáles son los principales beneficios de usar Java Records? Menciona al menos tres

- Inmutabilidad por defecto
- Concisión (menos código)
- Claridad de intención
- Seguridad en hilos
- Implementación automática de `equals()`, `hashCode()` y `toString()`.

### 21. ¿Cómo se accede a los componentes de una Java Record? Proporciona un ejemplo simple

Se accede usando el nombre del componente como un método.

```Java
record Point(int x, int y) {}
```

acceso: pointInstance.x().

```Java
pointInstance.x().
```

### 22. ¿Son las Java Records mutables o inmutables? ¿Qué implicación tiene esto?

Son inmutables por defecto.

Esto implica mayor predecibilidad, seguridad en entornos concurrentes y menor propensión a errores de estado.

### 23. ¿Cómo se usan las Java Records como DTOs en Spring Boot? ¿Jackson las soporta de forma nativa?

Se usan directamente como DTOs para solicitudes (`@RequestBody`) y respuestas.

Sí, Jackson las soporta de forma nativa sin configuración adicional.

### 24. ¿Se pueden aplicar anotaciones de validación (ej., `@NotBlank`) directamente a los componentes de una Java `Record`?

Sí, las anotaciones de Bean Validation pueden aplicarse directamente a los componentes del `record`.

### 25. ¿Cuándo sería preferible usar un POJO tradicional en lugar de una Java Record?

Si se necesita mutabilidad de los campos, herencia, o mapeo complejo para entidades JPA con ciclos de vida específicos que requieren setters o proxies.

### 26. ¿Qué problema resuelve MapStruct en APIs RESTful Java?

Resuelve el **mapeo de objetos repetitivo y propenso a errores** entre capas (ej., Entidades JPA a DTOs). Evita escribir manualmente código `getter/setter` para copiar datos.

### 27. Explica la "generación de código en tiempo de compilación" de MapStruct

MapStruct es un **procesador de anotaciones**. Durante la compilación, lee las interfaces `@Mapper` y genera automáticamente clases Java concretas que implementan la lógica de mapeo. Este código generado se compila junto al resto de tu código, resultando en mapeos muy eficientes en tiempo de ejecución.

### 28. ¿Qué dependencias de Maven/Gradle son necesarias para MapStruct en Spring Boot?

- `mapstruct` (API principal, dependencia de _runtime_).
- `mapstruct-processor` (procesador de anotaciones, dependencia de _compile-time_, configurado en `maven-compiler-plugin`).

### 29. ¿Cómo configurar una interfaz de mapper para que sea un componente de Spring?

Añade el atributo `componentModel = "spring"` a la anotación `@Mapper` de tu interfaz:

```Java
@Mapper(componentModel = "spring")
public interface MyMapper { /* ... */ }
```

Esto le indica a MapStruct que genere el mapper como un _bean_ de Spring, permitiendo su inyección.

### 30. Ejemplo de interfaz de mapper para entidad JPA a DTO (Java Record). Incluye mapeo de campos diferentes o ignorados

```Java
// Entidad: OrderEntity (id, productCode, totalAmount, orderPlacedDate)
// DTO (Record): OrderResponse (id, code, amount, purchaseDate)

@Mapper(componentModel = "spring")
public interface OrderMapper {
    @Mapping(source = "productCode", target = "code")
    @Mapping(source = "totalAmount", target = "amount")
    @Mapping(source = "orderPlacedDate", target = "purchaseDate")
    OrderResponse toDto(OrderEntity entity);

    List<OrderResponse> toDtoList(List<OrderEntity> entities);

    @Mapping(target = "id", ignore = true)
    OrderEntity toEntity(OrderRequest request);
}
```

### 31. ¿Dónde y cómo usarías una instancia de un mapper de MapStruct en un servicio de Spring Boot?

Se usa principalmente en la **capa de servicio** (`@Service`). Se inyecta la interfaz del mapper y se llaman a sus métodos para transformar objetos entre DTOs y entidades.

```Java
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final OrderMapper orderMapper; // Inyectado
    // ...
    public OrderResponse getOrderById(Long id) {
        OrderEntity order = orderRepository.findById(id).orElseThrow(...);
        return orderMapper.toDto(order); // Usando el mapper
    }
}
```

## Sección 5: Desarrollo Ágil y Scrum

### 32. Explica los cuatro valores del Manifiesto Ágil y su importancia. Implicaciones prácticas para un equipo

- **Valores**:
  1. **Individuos e interacciones** sobre procesos y herramientas: Prioriza la colaboración humana.
  2. **Software funcionando** sobre documentación exhaustiva: Enfoque en entregar valor real.
  3. **Colaboración con el cliente** sobre negociación contractual: Involucramiento constante del cliente.
  4. **Respuesta al cambio** sobre seguir un plan: Adaptabilidad a requisitos cambiantes.

- **Implicaciones**:
  - **Importancia**: Impulsan la flexibilidad, la entrega continua y la adaptación en el desarrollo.
  - **Implicaciones**: Fomenta comunicación directa, ciclos cortos de entrega, retroalimentación constante y planes flexibles.

### 33. Diferencias entre ágil y cascada. Ventajas del enfoque ágil

- **Cascada**: Secuencial, lineal, fases rígidas.
- **Ágil**: Iterativo, incremental, ciclos cortos (Sprints).
- **Ventajas Ágil**: Mayor adaptabilidad al cambio, entrega temprana de valor, menor riesgo, mejor calidad por retroalimentación continua, mayor satisfacción del cliente, mejora continua del equipo.

### 34. Describe los tres roles de Scrum (Product Owner, Scrum Master, Development Team): función, autoridad y responsabilidades. ¿Cómo interactúan?

- **Roles**:
  1. **Product Owner (PO)**: Maximiza valor del producto. Responsable del Product Backlog.
  2. **Scrum Master (SM)**: Líder de servicio, facilita Scrum, elimina impedimentos.
  3. **Development Team**: Construye el incremento. Auto-organizado y multifuncional.

- **Interacción**: Colaboran en planificación, el PO define qué, el Equipo cómo, el SM asegura el proceso.

### 35. ¿Qué significan "auto-organizado" y "multifuncional" para el Development Team? ¿Por qué son cruciales para el éxito del equipo y la calidad del incremento?

- **Auto-organizado**: El equipo decide cómo hacer el trabajo. Fomenta apropiación e innovación.
- **Multifuncional**: El equipo tiene todas las habilidades necesarias para completar el trabajo sin dependencias externas.

Llevan a mayor compromiso, toma de decisiones descentralizada, eficiencia y un incremento de producto más completo y de alta calidad.

### 36. Enumera los cinco eventos principales de Scrum: propósito, time-box y participantes. ¿Cómo contribuyen a la inspección y adaptación?

1. **Sprint**: Contenedor de 1-4 semanas para crear un incremento.
2. **Sprint Planning**: (8h/mes) Definir qué y cómo para el Sprint.
3. **Daily Scrum**: (15 min/día) Sincronizar progreso del equipo y adaptar el plan diario.
4. **Sprint Review**: (4h/mes) Demostrar incremento a stakeholders, obtener feedback, adaptar Product Backlog.
5. **Sprint Retrospective**: (3h/mes) Inspeccionar proceso del equipo, identificar mejoras.

Cada evento es un punto de inspección y adaptación de los artefactos y procesos de Scrum.

### 37. Objetivo del Daily Scrum y participantes. ¿Por qué 15 minutos? ¿Cómo fomenta la sincronización y colaboración?

- **Objetivo**: Inspeccionar el progreso del Development Team hacia el Sprint Goal y adaptar el plan diario.
- **Participantes**: Principalmente el Development Team.
- **15 minutos**: Fuerza concisión y enfoque.
- **Fomento**: Sincroniza al equipo sobre el estado y los impedimentos, promueve la ayuda mutua y la comunicación continua.

### 38. Importancia del Sprint Review y Retrospectiva del Sprint para la mejora continua del producto y del equipo

- **Sprint Review (Producto)**: Retroalimentación directa de stakeholders sobre el software funcional. Asegura que se construye el producto correcto.
- **Retrospectiva del Sprint (Equipo)**: El equipo reflexiona sobre su proceso de trabajo. Fomenta la mejora continua de la eficiencia, comunicación y calidad del equipo.

### 39. Define los tres artefactos de Scrum (Product Backlog, Sprint Backlog, Incremento): qué representa cada uno, nivel de detalle y responsable

1. **Product Backlog**: Lista priorizada de todo lo que se necesita en el producto. Detalles variables. Responsable: Product Owner.
2. **Sprint Backlog**: Ítems del Product Backlog seleccionados para el Sprint + plan detallado de cómo lograrlo. Responsable: Development Team.
3. **Incremento**: Suma del trabajo completado en un Sprint + anteriores. Software funcional, utilizable y potencialmente entregable. Responsable: Development Team.

### 40. ¿Qué significa que el Product Backlog sea "DEEP"? ¿Por qué es vital para la planificación y priorización?

"DEEP" (Detailed Appropriately, Estimated, Emergent, Prioritized) describe un Product Backlog saludable:
  
- **Vital**: Permite priorización efectiva, facilita la planificación del Sprint, se adapta a cambios, y ofrece transparencia.

### 41. Relación entre "Incremento" y "Definición de Terminado" (`Definition of Done`). Importancia para la calidad del software

- **Relación**: Un "Incremento" solo está "terminado" si cumple con la "**Definición de Terminado**" **(DoD)**, un acuerdo de calidad del equipo.
- **Importancia**: Asegura calidad consistente, transparencia, reduce deuda técnica, y hace el incremento potencialmente liberable.

### 42. ¿Cómo fomenta Scrum la colaboración, transparencia y responsabilidad compartida? Mecanismos específicos

- **Colaboración**: Daily Scrum, Sprint Planning, Reviews, Retrospectivas, equipo multifuncional.
- **Transparencia**: Artefactos visibles, Daily Scrum, Reviews.
- **Responsabilidad Compartida**: Sprint Goal colectivo, equipo auto-organizado, DoD compartida.

### 43. ¿Cómo superan los equipos ágiles los desafíos comunes del trabajo en grupo (conflictos, falta de comunicación, dependencias, silos de conocimiento)? Ejemplos de prácticas

- **Conflictos**: Discusión abierta, SM facilita.
- **Comunicación**: Daily Scrum, herramientas de comunicación, refinamiento.
- **Dependencias**: Identificación temprana en planificación, SM elimina bloqueos.
- **Silos de Conocimiento**: Multifuncionalidad, programación en pareja, revisiones de código.

### 44. Importancia de la retroalimentación continua (cliente e interna) en Scrum para la calidad del producto y la mejora del proceso

- **Calidad del Producto**: Feedback del cliente (Sprint Review) asegura construir lo correcto. Feedback interno (Code Reviews) asegura calidad técnica.
- **Mejora del Proceso**: Retrospectiva del Sprint permite aprender y optimizar la forma de trabajar del equipo.

## Sección 6: Preguntas de Reflexión y Problemas (General)

### 45. Diseño de un nuevo _endpoint_ REST complejo (ej. "registrar pedido con ítems y descuentos"). ¿Cómo aplicarías DTOs (Records), validaciones y manejo de excepciones del módulo?

- **DTOs (Records)**: Usaría `OrderRequest` (con `customerId`, `items`, `discountCode`) y `OrderItemRequest` (`productId`, `quantity`) como Records de entrada. Para salida, `OrderResponse` y `OrderItemResponse` como Records.
- **Validaciones**: `@NotBlank`, `@Min(1)` en DTOs. Crearía una validación personalizada `@ValidItemsAvailable` en `OrderRequest` para verificar disponibilidad de productos y validez del código de descuento. Usaría `@Valid` en el controlador.
- **Manejo de Excepciones**: El servicio lanzaría excepciones de negocio (ej., `ResourceNotFoundException` para producto, `IllegalStateException` para stock, `InvalidDiscountCodeException` para descuento inválido). El `GlobalExceptionHandler` las mapearía a `400 Bad Request`, `404 Not Found` o `409 Conflict` con un `ErrorResponse` consistente.

### 46. Escenario donde una excepción de negocio personalizada (con `4xx`) es más apropiada que un 200 OK con error en el cuerpo. Justifica tu elección

**Escenario**: Intento de inscripción en curso con capacidad máxima excedida.

- **`200 OK` con error en cuerpo (Incorrecto)**: Semánticamente incorrecto; `200` implica éxito. El cliente debe parsear el cuerpo para detectar error, complicando la lógica.
- **`Excepción personalizada` (`CourseCapacityExceededException`) mapeada a `400 Bad Request` (Correcto)**: Indica semánticamente que la solicitud no es válida debido al estado del recurso. Permite al cliente detectar errores fácilmente por el código HTTP y mantiene el controlador limpio.

### 47. ¿Cómo asegurarías la mantenibilidad y escalabilidad de una API Spring Boot creciente? Menciona al menos cinco prácticas o patrones arquitectónicos

1. **Arquitectura por Capas**: Separación de responsabilidades (Controller, Service, Repository).
2. **DTOs y MapStruct**: Desacopla la API de las entidades, automatiza mapeo.
3. **Manejo de Excepciones Global**: Respuestas de error consistentes y centralizadas.
4. **Validaciones Exhaustivas**: Previene datos corruptos desde la entrada.
5. **Paginación/Filtrado Estándar**: Manejo eficiente de grandes conjuntos de datos.
6. **rincipios RESTful**: Diseño de endpoints coherente y predecible.
7. **Documentación (OpenAPI/Swagger)**: Claridad para consumidores de API.
8. **Contenedorización (Docker)**: Facilita despliegue y escalabilidad.

### 48. Tu enfoque metódico para depurar un problema complejo en una API Spring Boot, especialmente si es de seguridad (JWT) o mapeo (MapStruct)

1. **Replicar**: Reproducir el problema en dev/test.
2. **Análisis de Logs**: Buscar errores/advertencias.
3. **Postman/Insomnia**: Probar solicitudes con variantes (JWTs, DTOs).
4. **Depurador (IDE)**:
    - **JWT**: Breakpoints en `JwtRequestFilter` (`doFilterInternal`) para inspeccionar token y `SecurityContextHolder`.
    - **MapStruct**: Breakpoints en métodos de servicio que usan el mapper, inspeccionar el **código generado** por MapStruct.
    - **Validaciones/Excepciones**: Breakpoints en `GlobalExceptionHandler`.
5. **Herramientas de Red**: Inspeccionar encabezados/cuerpos/códigos HTTP.
6. **Revisión de Código**: Búsqueda manual de errores.
7. **Simplificación del Caso**: Crear un caso mínimo reproducible.

---

**Consejo Final para la Entrevista**: Asegúrate de dominar los conceptos básicos. No es solo saber _qué es_, sino _por qué_ y _cuándo_ usarlo. Adicionalmente, mantén una actitud positiva y profesional. Reconoce si no sabes algo, pero muestra tu disposición para aprender.

**¡Mucha suerte!**
