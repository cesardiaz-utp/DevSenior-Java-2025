# Tutoría: Reactividad con Signals y Comunicación entre Componentes

Esta sección está diseñada para el profesor, ofreciendo una guía sobre cómo reforzar los conceptos clave cubiertos en la Clase 2. El objetivo es consolidar el entendimiento de los estudiantes a través de la revisión, preguntas y posibles micro-ejercicios en vivo.

## 1. Debate sobre Ciclo de Vida

- **Preguntas al grupo**:
  - "¿Por qué es importante entender el ciclo de vida de un componente en Angular?" (Gestión de recursos, inicialización correcta, depuración).
  - "¿Cuándo se llama `ngOnInit()` y para qué tipo de lógica es ideal?" (Una vez, después de entradas; carga inicial de datos).
  - "¿Cuál es la diferencia principal entre `ngOnChanges()` y `ngDoCheck()`?" (`ngOnChanges` solo para `@Inputs`, `ngDoCheck` para detección de cambios manual más profunda).
  - "¿Por qué es crucial implementar `ngOnDestroy()`?" (Evitar fugas de memoria, limpieza de suscripciones/temporizadores).
- **Demostración rápida**: Añadir un `console.log` en `ngOnInit` y `ngOnDestroy` de un componente simple y mostrar cuándo aparecen en la consola al navegar o destruir el componente.

## 2. Debate sobre Signals

- **Preguntas al grupo**:
  - "¿Cuál es la principal diferencia entre cómo manejábamos los datos en la Clase 1 y cómo lo hacemos ahora con Signals?" (Esperar la mención de la reactividad explícita y la llamada como función).
  - "¿Cuándo usarían `signal()` vs. `computed()` vs. `effect()`?" (Pedir ejemplos concretos).
    - `signal()`: Para datos de estado directo (ej., `loading`, `filterCity`).
    - `computed()`: Para datos derivados que dependen de otras Signals (ej., `properties` filtradas).
    - `effect()`: Para efectos secundarios (ej., `console.log`, interacción con APIs externas, actualizar el DOM fuera de Angular).
  - "¿Qué ventajas de rendimiento o depuración creen que ofrecen las Signals?"
- **Demostración rápida**: Modificar un `signal` en el código y mostrar cómo un `computed` o `effect` reacciona en tiempo real en la consola.

## 3. Comunicación entre Componentes

- **Análisis de código en vivo**: Abrir `PropertyList` (padre), `PropertyCardTraditional` (hijo tradicional) y `PropertyCardSignal` (hijo con Signals).
  - **`@Input()` (Padre a Hijo - Tradicional)**:
    - "En `PropertyCardTraditional`, ¿qué propiedad recibe datos del padre? ¿Cómo la declaramos?" (Señalar `@Input() property!: Property;`).
    - "En `PropertyList`, ¿cómo le pasamos los datos a `PropertyCardTraditional`?" (Señalar `[property]="property"` si se estuviera usando).
  - **`input()` (Padre a Hijo - Signals)**:
    - "En `PropertyCardSignal`, ¿cómo declaramos la propiedad que recibe datos del padre?" (Señalar `property = input.required<Property>();`).
    - "¿Cómo accedemos a su valor dentro de la plantilla o la lógica del componente?" (`property()`).
    - "En `PropertyList`, ¿cómo le pasamos los datos a `PropertyCardSignal`?" (Señalar `[property]="property"`).
    - "¿Cuál es la ventaja principal de `input()` sobre `@Input()`?" (Reactividad intrínseca, inmutabilidad, tipado fuerte, menos necesidad de `ngOnChanges`).
  - **`@Output()` y `EventEmitter` (Hijo a Padre - Tradicional)**:
    - "En `PropertyCardTraditional`, ¿cómo notificamos al padre cuando se hace clic en 'Ver Detalles'? ¿Qué usamos para emitir el evento?" (Señalar `@Output() viewDetails = new EventEmitter<number>();` y `this.viewDetails.emit(this.property.id);`).
  - **`output()` (Hijo a Padre - Signals)**:
    - "En `PropertyCardSignal`, ¿cómo declaramos el emisor de eventos?" (Señalar `viewDetails = output<number>();`).
    - "¿Cómo emitimos el evento?" (`this.viewDetails.emit(this.property().id);`).
    - "En `PropertyList`, ¿cómo escuchamos estos eventos de ambos tipos de componentes hijos y qué hacemos cuando los recibimos?" (Señalar `(viewDetails)="handleViewDetails($event)`" y el método `handleViewDetails`).
    - "¿Por qué es importante usar `EventEmitter / output()` y `@Output()` en lugar de que el hijo llame directamente a un método del padre?" (Separación de preocupaciones, reutilización, el hijo no necesita saber sobre la implementación del padre).
- **Preguntas de escenario**:
  - "Si quisiéramos que `PropertyCardSignal` mostrara un mensaje diferente si la propiedad es 'destacada', ¿cómo le pasaríamos esa información desde el padre?" (Usando otro `input()`).
  - "Si quisiéramos que `PropertyCardTraditional` permitiera 'marcar como favorito' y el padre necesitara saber qué propiedad fue marcada, ¿cómo lo haríamos?" (Usando otro `@Output()` para emitir el ID de la propiedad).

## 4. Beneficios de la Refactorización

- **Discusión**:
  - "¿Por qué fue una buena idea extraer la tarjeta de propiedad a un componente separado (`PropertyCardTraditional` / `PropertyCardSignal`)?" (Reutilización, legibilidad del código, separación de preocupaciones, mantenimiento más fácil).
  - "Imagina que queremos cambiar el diseño de las tarjetas de propiedad. ¿Cuántos archivos tendríamos que modificar antes y cuántos ahora?" (Antes: solo `property-list.html`; Ahora: solo `property-card-traditional.html` / `property-card-signal.html` y sus respectivos CSS).
  - "¿Cómo contribuye el uso de componentes standalone a esta modularidad?" (Auto-importación de dependencias, menos `NgModules`).
  