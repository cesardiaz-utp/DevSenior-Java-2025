# Clase 2: Reactividad con Signals y Comunicación entre Componentes

**¡Hola a todos!**
¡Bienvenidos a la segunda clase del **Módulo 8**! En la clase anterior, sentamos las bases de nuestro proyecto Angular, configuramos el entorno, comprendimos las Single Page Applications (SPAs) y construimos un listado básico de propiedades inmobiliarias utilizando componentes standalone, TypeScript y Tailwind CSS.

En esta clase, nos sumergiremos en el corazón de la **reactividad moderna en Angular 20: las Signals (Señales)**. Exploraremos cómo las Signals simplifican la gestión del estado y la detección de cambios, haciendo que nuestras aplicaciones sean más eficientes y predecibles. Además, abordaremos un tema crucial en el desarrollo de componentes: la **comunicación entre componentes**. Aprenderemos cómo los componentes pueden interactuar entre sí, pasando datos de padres a hijos y emitiendo eventos de hijos a padres, lo que es esencial para construir interfaces de usuario modulares y dinámicas.

Continuaremos trabajando en nuestro proyecto de ofertas inmobiliarias, refactorizando la lista de propiedades para usar Signals y creando un componente de "tarjeta de propiedad" que se comunicará con el componente de lista principal.

Prepárense para llevar sus habilidades de Angular al siguiente nivel, construyendo interfaces más interactivas y robustas.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Comprender el ciclo de vida de un componente** en Angular y las interfaces asociadas.
- **Utilizar los _lifecycle hooks_** más comunes (`ngOnInit`, `ngOnChanges`, `ngOnDestroy`).
- **Comprender y aplicar el concepto de Signals** en Angular para la gestión de estado reactivo.
- **Utilizar `signal()`, `computed()` y `effect()`** para crear y manipular datos reactivos.
- **Implementar la comunicación de datos de padre a hijo** utilizando el decorador `@Input()` y la función `input()`.
- **Implementar la comunicación de eventos de hijo a padre** utilizando el decorador `@Output()` y `EventEmitter`, y la nueva función `output()`.
- **Refactorizar componentes existentes** para mejorar la modularidad y la reactividad.
- **Aplicar las mejores prácticas** para la comunicación entre componentes en Angular.

## 1. Ciclo de Vida de un Componente en Angular

Cada componente en una aplicación Angular tiene un ciclo de vida que gestiona Angular. Desde su creación hasta su destrucción, un componente pasa por varias fases. Angular proporciona "hooks" (ganchos) de ciclo de vida que te permiten ejecutar código en puntos específicos de la vida de un componente.

Comprender el ciclo de vida es fundamental para escribir aplicaciones eficientes y sin errores, ya que te permite inicializar datos, responder a cambios, limpiar recursos y mucho más en el momento adecuado.

### 1.1. Fases del Ciclo de Vida

El ciclo de vida de un componente se puede visualizar como una serie de eventos que ocurren en un orden predecible. Angular invoca métodos de _lifecycle hooks_ en este orden:

1. **Construcción**: El constructor de la clase del componente se ejecuta primero. Aquí se inyectan las dependencias.
2. **Cambios en las propiedades de entrada**: Angular detecta cambios en las propiedades de entrada (`@Input()` o `input()`) del componente.
3. **Inicialización**: El componente se inicializa después de que Angular haya configurado sus propiedades vinculadas a datos.
4. **Detección de cambios (cada ciclo)**: Angular realiza comprobaciones de detección de cambios para actualizar la vista y verificar si hay cambios en las propiedades.
5. **Contenido proyectado (ng-content)**: Si el componente utiliza proyección de contenido (`<ng-content>`), Angular inicializa y verifica este contenido.
6. **Vista del componente**: Angular inicializa y verifica la vista del propio componente.
7. **Destrucción**: El componente se elimina del DOM.

### 1.2. Interfaces de Ciclo de Vida (Lifecycle Hooks)

Para interactuar con estas fases, implementamos interfaces específicas en nuestra clase de componente. Cada interfaz tiene un método con el prefijo `ng` que Angular llama automáticamente.

Aquí están las interfaces más comunes y sus métodos:

- **`ngOnChanges()` (Implementa `OnChanges`)**
  - **Cuándo se llama**: Se llama antes de `ngOnInit` (si el componente tiene entradas) y cada vez que una propiedad de entrada vinculada (`@Input()` o `input()`) cambia.
  - **Uso**: Ideal para realizar acciones cuando los datos de entrada cambian. Recibe un objeto `SimpleChanges` que contiene los valores actuales y anteriores de las propiedades que han cambiado.
  - **Nota**: Con `input()` (Signals), `ngOnChanges` es menos necesario porque puedes reaccionar a los cambios de la Signal directamente.

  ```Typescript
  import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

  @Component({ /* ... */ })
  export class MyComponent implements OnChanges {
    @Input() data: string = '';

    ngOnChanges(changes: SimpleChanges): void {
      if (changes['data']) {
        console.log('La propiedad data ha cambiado a:', changes['data'].currentValue);
      }
    }
  }
  ```

- **`ngOnInit()` (Implementa `OnInit`)**
  - **Cuándo se llama**: Se llama una vez, después de que Angular haya inicializado todas las propiedades de entrada del componente y haya configurado la vista del componente.
  - **Uso**: El lugar preferido para la lógica de inicialización del componente, como la carga inicial de datos de un servicio, la configuración de suscripciones a Observables (aunque con Signals esto puede cambiar), o la inicialización de estado.

  ```Typescript
  import { Component, OnInit } from '@angular/core';

  @Component({ /* ... */ })
  export class MyComponent implements OnInit {
    message: string = '';

    ngOnInit(): void {
      console.log('Componente inicializado.');
      // Cargar datos iniciales
      this.message = 'Datos cargados al inicio.';
    }
  }
  ```

- **`ngDoCheck()` (Implementa `DoCheck`)**
  - **Cuándo se llama**: Se llama durante cada ciclo de detección de cambios de Angular, inmediatamente después de `ngOnChanges` y `ngOnInit`.
  - **Uso**: Para implementar tu propia lógica de detección de cambios cuando Angular no puede detectar un cambio por sí mismo (ej., mutaciones dentro de un objeto o array que no cambian la referencia). **Debe usarse con precaución** ya que se ejecuta muy frecuentemente y puede afectar el rendimiento si no se optimiza.

  ```Typescript
  import { Component, DoCheck, KeyValueDiffers } from '@angular/core';

  @Component({ /* ... */ })
  export class MyComponent implements DoCheck {
    // Ejemplo de uso avanzado: detectar cambios en un objeto anidado
    private differ: KeyValueDiffer<string, any>;
    
    constructor(private differs: KeyValueDiffers) {
      this.differ = this.differs.find({}).create();
    }
    
    ngDoCheck(): void {
      const changes = this.differ.diff(this.myObject);
      if (changes) {
        console.log('Cambios detectados en myObject');
      }
    }
  }
  ```

- **`ngAfterContentInit()` (Implementa `AfterContentInit`)**
  - **Cuándo se llama**: Se llama una vez, después de que Angular proyecta el contenido externo (`<ng-content>`) en la vista del componente.
  - **Uso**: Para realizar inicializaciones o manipulaciones del contenido proyectado.
- **`ngAfterContentChecked()` (Implementa `AfterContentChecked`)**
  - **Cuándo se llama**: Se llama después de `ngAfterContentInit` y después de cada `ngDoCheck` si el contenido proyectado ha sido verificado.
  - **Uso**: Para realizar acciones después de que el contenido proyectado ha sido verificado.
- **`ngAfterViewInit()` (Implementa `AfterViewInit`)**
  - **Cuándo se llama**: Se llama una vez, después de que Angular inicializa la vista del componente y las vistas de sus componentes hijos.
  - **Uso**: Para realizar inicializaciones o manipulaciones del DOM del componente o de sus hijos, por ejemplo, acceder a elementos del DOM usando `@ViewChild` o `@ViewChildren`.

  ```Typescript
  import { Component, AfterViewInit, ViewChild, ElementRef } from '@angular/core';

  @Component({
    selector: 'app-my-component',
    template: '<div #myDiv>Hola</div>'
  })
  export class MyComponent implements AfterViewInit {
    @ViewChild('myDiv') myDiv!: ElementRef;

    ngAfterViewInit(): void {
      console.log('Elemento div disponible en la vista:', this.myDiv.nativeElement.textContent);
      this.myDiv.nativeElement.style.color = 'blue';
    }
  }
  ```

- **`ngAfterViewChecked()` (Implementa `AfterViewChecked`)**
  - **Cuándo se llama**: Se llama después de `ngAfterViewInit` y después de cada `ngDoCheck` si la vista del componente y las vistas de sus hijos han sido verificadas.
  - **Uso**: Para realizar acciones después de que la vista ha sido completamente verificada.
- **`ngOnDestroy()` (Implementa `OnDestroy`)**
  - **Cuándo se llama**: Se llama justo antes de que Angular destruya el componente.
  - **Uso**: Es el lugar crucial para realizar la limpieza de recursos para evitar fugas de memoria, como:
    - Desuscribirse de Observables.
    - Limpiar temporizadores (`setTimeout`, `setInterval`).
    - Desvincularse de eventos del DOM.
    - Cualquier otra limpieza necesaria para liberar recursos.

  ```Typescript
  import { Component, OnDestroy } from '@angular/core';
  import { Subscription } from 'rxjs'; // Si usas Observables

  @Component({ /* ... */ })
  export class MyComponent implements OnDestroy {
    private dataSubscription!: Subscription;

    constructor(...) {
      this.dataSubscription = someService.getData().subscribe(...);
    }

    ngOnDestroy(): void {
      console.log('Componente destruido. Realizando limpieza...');
      this.dataSubscription.unsubscribe(); // Si usas Observables
      clearTimeout(myTimer);
    }
  }
  ```

### 1.3. Resumen del Flujo de Ciclo de Vida

1. `constructor()`: Se ejecuta primero para inyectar dependencias.
2. `ngOnChanges()`: Cuando las entradas vinculadas cambian (antes de `ngOnInit` y en cada cambio de entrada).
3. `ngOnInit()`: Una vez, después de que las entradas se inicializan.
4. `ngDoCheck()`: En cada ciclo de detección de cambios.
5. `ngAfterContentInit()`: Una vez, después de que el contenido proyectado se inicializa.
6. `ngAfterContentChecked()`: Después de `ngAfterContentInit` y cada vez que el contenido proyectado se verifica.
7. `ngAfterViewInit()`: Una vez, después de que la vista del componente y sus hijos se inicializan.
8. `ngAfterViewChecked()`: Después de `ngAfterViewInit` y cada vez que la vista del componente y sus hijos se verifican.
9. `ngOnDestroy()`: Justo antes de que el componente sea destruido.

Comprender y utilizar estos _hooks_ te dará un control preciso sobre el comportamiento de tus componentes a lo largo de su existencia.

```Mermaid
graph TD
  A[Constructor] --> B{ngOnChanges?};
  B -- Sí, entradas cambian --> C[ngOnChanges];
  B -- No o primera vez --> D[ngOnInit];
  C --> D;
  D --> E[ngDoCheck];
  E --> F{ngAfterContentInit?};
  F -- Sí, contenido proyectado --> G[ngAfterContentInit];
  F -- No o ya inicializado --> H[ngAfterContentChecked];
  G --> H;
  H --> I{ngAfterViewInit?};
  I -- Sí, vista inicializada --> J[ngAfterViewInit];
  I -- No o ya inicializada --> K[ngAfterViewChecked];
  J --> K;
  K --> E;
  subgraph Destrucción
      L[ngOnDestroy];
  end
  K -- Componente destruido --> L;
```

## 2. Reactividad en Angular: Introducción a Signals (Señales)

En Angular, la reactividad se refiere a cómo la interfaz de usuario responde a los cambios en los datos. Tradicionalmente, Angular ha utilizado Zone.js para detectar cambios, pero con Angular 16 (y consolidado en Angular 20), las **Signals** se han introducido como una nueva y poderosa primitiva para la reactividad, ofreciendo un control más granular y un potencial de rendimiento mejorado.

### 2.1. ¿Qué son las Signals?

Las **Signals** son funciones sin parámetros que devuelven el valor actual de un dato y notifican a los consumidores interesados cuando ese valor cambia. Son un mecanismo de reactividad de bajo nivel que permite a Angular saber exactamente qué partes de la interfaz de usuario necesitan ser actualizadas cuando un dato específico cambia, sin necesidad de ejecutar una detección de cambios global.

Piensa en una Signal como una caja que contiene un valor. Cuando cambias el valor dentro de la caja, todos los que están "observando" esa caja son notificados automáticamente.

### 2.2. ¿Por qué usar Signals?

1. **Reactividad Explícita y Granular**: Con Signals, la reactividad es explícita. Sabes exactamente qué datos son reactivos y qué partes de tu código dependen de ellos. Esto permite a Angular optimizar la detección de cambios, actualizando solo los componentes afectados, en lugar de verificar toda la aplicación.
2. **Rendimiento Mejorado**: Al reducir la cantidad de trabajo que Angular necesita hacer para detectar cambios, las Signals pueden llevar a un rendimiento significativamente mejorado, especialmente en aplicaciones grandes y complejas.
3. **Simplificación del Código**: Reducen la necesidad de usar Zone.js para la detección de cambios, lo que puede simplificar la depuración y el razonamiento sobre el flujo de datos.
4. **Interoperabilidad con RxJS**: Aunque son una alternativa a RxJS para ciertos escenarios, las Signals y RxJS pueden coexistir y complementarse. Angular proporciona utilidades para convertir Observables a Signals y viceversa.
5. **Preparación para el Flujo Zoneless**: Las Signals son fundamentales para el futuro "Zoneless" de Angular, donde Zone.js podría volverse opcional, permitiendo un control aún mayor sobre la detección de cambios.

### 2.3. Conceptos Clave de Signals

Angular proporciona tres funciones principales para trabajar con Signals:

1. `signal()`: **Crear un valor reactivo**  
    La función `signal()` crea una nueva Signal. Le pasas un valor inicial, y devuelve una función que puedes llamar para leer el valor o para actualizarlo.
    - **Lectura del valor**: Llama a la Signal como una función (ej., `mySignal()`).
    - **Escritura del valor**: Usa el método `.set()` para reemplazar el valor, o `.update()` para modificarlo basándose en su valor actual.

    **Ejemplo**:

    ```Typescript
    import { signal } from '@angular/core';

    // Crear una Signal con un valor inicial
    const count = signal(0);

    console.log(count()); // Lee el valor: 0

    // Actualizar el valor usando .set()
    count.set(5);
    console.log(count()); // Lee el valor: 5

    // Actualizar el valor usando .update() (útil para transformaciones)
    count.update(currentCount => currentCount + 1);
    console.log(count()); // Lee el valor: 6

    // Las Signals también pueden contener objetos o arrays
    const user = signal({ name: 'Alice', age: 30 });
    console.log(user().name); // Alice

    // Para actualizar una propiedad de un objeto en una Signal,
    // debes crear un nuevo objeto (inmutabilidad)
    user.update(currentUser => ({ ...currentUser, age: 31 }));
    console.log(user().age); // 31
    ```

2. `computed()`: **Crear un valor derivado reactivo**  
    La función `computed()` te permite definir un valor que se deriva de una o más Signals existentes. El valor de un `computed` se recalcula automáticamente solo cuando las Signals de las que depende cambian. Esto es muy eficiente, ya que el cálculo se memoiza.

    **Ejemplo**:

    ```Typescript
    import { signal, computed } from '@angular/core';

    const firstName = signal('John');
    const lastName = signal('Doe');

    // Crear un valor computado para el nombre completo
    const fullName = computed(() => `${firstName()} ${lastName()}`);

    console.log(fullName()); // John Doe

    // Cambiar una de las Signals de las que depende
    firstName.set('Jane');
    console.log(fullName()); // Jane Doe (se recalcula automáticamente)

    // Si la Signal no cambia, el valor computado no se recalcula.
    lastName.set('Doe'); // Si se cambiara al mismo valor, no se recalcularía.
    ```

3. `effect()`: **Ejecutar efectos secundarios**  
    La función `effect()` te permite ejecutar código con efectos secundarios (como actualizar el DOM directamente, realizar llamadas a la consola, o interactuar con APIs externas) en respuesta a cambios en una o más Signals. Los `effects` siempre se ejecutan al menos una vez y luego cada vez que una de las Signals que leen cambia.

    **Importante**: Los `effects` deben usarse con moderación, principalmente para sincronizar el estado de Angular con APIs externas o el DOM. La mayor parte de la lógica de tu aplicación debe residir en `computed` o en la lógica de los componentes.

    **Ejemplo**:

    ```Typescript
    import { signal, effect } from '@angular/core';

    const message = signal('Hello');
    const count = signal(0);

    // Un efecto que reacciona a cambios en 'message' y 'count'
    effect(() => {
      console.log(`Mensaje actual: ${message()}, Conteo: ${count()}`);
    });

    message.set('Hola Mundo'); // El efecto se ejecuta
    count.set(1);             // El efecto se ejecuta
    message.set('Hola Mundo'); // El efecto NO se ejecuta si el valor es el mismo
    ```

### 2.4. Signals en Componentes

Puedes usar Signals directamente en las propiedades de tus componentes. Cuando una Signal se lee dentro de la plantilla de un componente, Angular sabe que ese componente depende de esa Signal y solo lo actualizará cuando la Signal cambie.

```Typescript
import { Component, signal } from '@angular/core';
import { CommonModule } from '@angular/common'; // Para usar pipes si es necesario

@Component({
  selector: 'app-my-component',
  imports: [CommonModule],
  template: `
    <p>Conteo: {{ count() }}</p>
    <button (click)="increment()">Incrementar</button>
  `
})
export class MyComponent {
  count = signal(0); // Declara una Signal

  increment() {
    this.count.update(value => value + 1); // Actualiza la Signal
  }
}
```

## 3. Comunicación entre Componentes en Angular

En una aplicación Angular, los componentes son los bloques de construcción fundamentales. Para construir interfaces de usuario complejas y modulares, los componentes necesitan comunicarse entre sí. Angular proporciona mecanismos claros para esta comunicación, principalmente a través de `@Input()` / `input()` y `@Output()` / `output()`.

### 3.1. Comunicación de Padre a Hijo: `@Input()` y `input()`

Cuando un componente padre necesita pasar datos a un componente hijo, se utilizan el decorador `@Input()` o la nueva función `input()`. Esto permite que el componente hijo reciba valores de propiedades del componente padre.

#### 3.1.1. El Decorador `@Input()` (Tradicional)

El decorador `@Input()` es la forma clásica y ampliamente utilizada para pasar datos de un componente padre a un componente hijo. Se coloca sobre una propiedad en el componente hijo para indicar que esa propiedad puede recibir un valor del componente padre.

- **Componente Hijo**: Declara una propiedad con `@Input()`.
- **Componente Padre**: Vincula un valor a esa propiedad del hijo en su plantilla.

##### Ejemplo Conceptual con `@Input()`

```Typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<h2>Hola, {{ name }}!</h2>`
})
export class Child {
  @Input() name: string = ''; // Declara una propiedad de entrada
}
```

```Typescript
// Componente Padre: parent.component.ts
import { Component } from '@angular/core';
import { Child } from './child.component'; // Importa el componente hijo

@Component({
  selector: 'app-parent',
  imports: [Child],
  template: `
    <app-child [name]="parentName"></app-child> <!-- Pasa datos al hijo -->
    <button (click)="changeName()">Cambiar Nombre</button>
  `
})
export class Parent {
  parentName: string = 'Mundo';

  changeName() {
    this.parentName = 'Angular';
  }
}
```

##### Características de `@Input()`

- **Sintaxis Concisa**: Fácil de leer y escribir en la plantilla.
- **Compatibilidad**: Funciona con cualquier tipo de dato (primitivos, objetos, arrays).
- **Detección de Cambios**: Angular detecta automáticamente los cambios en las propiedades de entrada y actualiza el componente hijo. Para reaccionar a estos cambios dentro del hijo (más allá de la simple visualización en la plantilla), se puede usar el _lifecycle_ hook `ngOnChanges` o un _setter_ en la propiedad `@Input()`.
- **Mutabilidad**: Si pasas un objeto o un array, y modificas sus propiedades directamente en el padre, el hijo verá esos cambios sin que la referencia del objeto cambie. Esto puede llevar a problemas si no se maneja con cuidado (efectos secundarios inesperados).

#### 3.1.2. La Función `input()` (Basada en Signals)

La función `input()` es una adición más reciente a Angular (a partir de la versión 16 y consolidado en la v20), diseñada para trabajar en armonía con el nuevo sistema de reactividad basado en Signals. Permite declarar entradas de componentes que son intrínsecamente reactivas y, por defecto, inmutables.

- **Componente Hijo**: En lugar de `@Input()`, llamas a la función `input()` y le pasas un valor por defecto. La propiedad resultante es una **Signal de solo lectura**. Para acceder a su valor, la llamas como una función (ej., `this.message()`).
- **Componente Padre**: La forma de pasar el valor en la plantilla es idéntica a `@Input()`: utilizas _property binding_ (`[ ])`.

##### Ejemplo Conceptual con `input()`

```Typescript
import { Component, input } from '@angular/core'; // Importa 'input'

@Component({
  selector: 'app-child-signal',
  template: `<h2>Hola, {{ message() }}!</h2>`
})
export class ChildSignal {
  // Declara entradas como Signals
  message = input<string>('No message provided (Signal)'); // Valor por defecto
  // También puedes hacer inputs requeridos:
  // requiredData = input.required<number>();
}
```

```Typescript
import { Component, signal } from '@angular/core';
import { ChildSignal } from './child-signal'; // Importa el componente hijo con Signals

@Component({
  selector: 'app-parent-signal',
  imports: [ChildSignal],
  template: `
    <app-child-signal [message]="parentMessage()"></app-child-signal> <!-- Pasa datos al hijo -->
    <button (click)="changeMessage()">Cambiar Mensaje (Signal)</button>
  `
})
export class ParentSignalComponent {
  parentMessage = signal('Hola desde el padre (Signal)!');

  changeMessage() {
    this.parentMessage.set('Mensaje actualizado desde el padre (Signal)!');
  }
}
```

##### Características de `input()`

- **Reactividad Intrínseca**: La entrada es una Signal, lo que significa que es inherentemente reactiva. Cualquier código que "lea" esta Signal (en la plantilla, en un `computed`, o en un `effect`) será notificado y reaccionará automáticamente cuando el valor cambie.
- **Inmutabilidad por Defecto**: Las Signals promueven la inmutabilidad. Si pasas un objeto o un array, y el padre lo modifica, la Signal `input()` en el hijo solo se actualizará si la _referencia_ del objeto cambia. Esto ayuda a prevenir efectos secundarios inesperados y facilita el seguimiento de los cambios.
- **Valores por Defecto y Requeridos**: Puedes definir un valor por defecto para la entrada (`input('valor por defecto')`) o hacerla obligatoria (`input.required<Tipo>()`). Si una entrada requerida no se proporciona, Angular lanzará un error.
- **Transformaciones**: Puedes aplicar una función de transformación a la entrada (`input(defaultValue, { transform: (value) => ... })`) para modificar el valor antes de que sea accesible en el componente hijo.
- **Más Consistente**: Se alinea con el paradigma de Signals de Angular, reduciendo la necesidad de `ngOnChanges` para reaccionar a cambios en las entradas.

### 3.2. Comunicación de Hijo a Padre: `@Output()` y `EventEmitter` vs. `output()`

Cuando un componente hijo necesita notificar al componente padre sobre un evento (ej., un clic de botón, un cambio de valor, una acción completada), se utilizan el decorador `@Output()` junto con `EventEmitter`, o la nueva función `output()`.

#### 3.2.1. El Decorador `@Output()` y `EventEmitter` (Tradicional)

- **Componente Hijo**: Declara una propiedad con `@Output()` que es una instancia de `EventEmitter`. El hijo "emite" eventos a través de esta propiedad.
- **Componente Padre**: Escucha estos eventos utilizando la sintaxis de _event binding_ (`()`) en su plantilla y ejecuta un método en respuesta.

##### Ejemplo Conceptual con `@Output()`

```Typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<button (click)="sendMessage()">Enviar Mensaje al Padre</button>`
})
export class Child {
  @Output() messageEvent = new EventEmitter<string>(); // Declara un emisor de eventos

  sendMessage() {
    this.messageEvent.emit('¡Hola desde el hijo!'); // Emite el evento
  }
}
```

```Typescript
import { Component } from '@angular/core';
import { Child } from './child'; // Importa el componente hijo

@Component({
  selector: 'app-parent',
  imports: [Child],
  template: `
    <app-child (messageEvent)="receiveMessage($event)"></app-child> <!-- Escucha el evento -->
    <p *ngIf="receivedMessage">Mensaje recibido: {{ receivedMessage }}</p>
  `
})
export class ParentComponent {
  receivedMessage: string = '';

  receiveMessage(message: string) {
    this.receivedMessage = message;
  }
}
```

##### Características de `@Output()`

- **Sintaxis Establecida**: Método bien conocido y ampliamente utilizado.
- **Flexibilidad**: Permite emitir cualquier tipo de dato como carga útil del evento.
- **Acoplamiento Suelto**: El hijo no necesita saber qué hace el padre con el evento, solo que lo emite.

#### 3.2.2. La Función `output()` (Basada en Signals)

La función `output()` es la contraparte basada en Signals de `@Output()`. Proporciona una forma más moderna y, en algunos casos, más tipada y sencilla de manejar la emisión de eventos desde un componente hijo.

- **Componente Hijo**: Llama a la función `output()` para crear un emisor de eventos. El resultado es un objeto con un método `emit()` que se utiliza para disparar el evento.
- **Componente Padre**: La forma de escuchar el evento en la plantilla es idéntica a `@Output()`: utilizas _event binding_ (`()`).

##### Ejemplo Conceptual con `output()`

```Typescript
import { Component, output } from '@angular/core'; // Importa 'output'

@Component({
  selector: 'app-child-signal-output',
  template: `<button (click)="sendMessage()">Enviar Mensaje al Padre (Signal Output)</button>`
})
export class ChildSignalOutput {
  // Declara un emisor de eventos usando output()
  // Puedes especificar el tipo de dato que emitirá
  messageEvent = output<string>();

  sendMessage() {
    this.messageEvent.emit('¡Hola desde el hijo con Signal Output!'); // Emite el evento
  }
}
```

```Typescript
import { Component, signal } from '@angular/core';
import { CommonModule } from '@angular/common'; // Para *ngIf
import { ChildSignalOutput } from './child-signal-output'; // Importa el componente hijo

@Component({
  selector: 'app-parent-signal-output',
  imports: [CommonModule, ChildSignalOutput],
  template: `
    <app-child-signal-output (messageEvent)="receiveMessage($event)"></app-child-signal-output> <!-- Escucha el evento -->
    <p *ngIf="receivedMessage()">Mensaje recibido: {{ receivedMessage() }}</p>
  `
})
export class ParentSignalOutput {
  receivedMessage = signal<string>('');

  receiveMessage(message: string) {
    this.receivedMessage.set(message);
  }
}
```

##### Características de `output()`

- **Tipado Fuerte**: Fomenta un tipado más explícito del evento que se emite.
- **Consistencia con Signals**: Se alinea con el paradigma de Signals de Angular, ofreciendo una API más unificada.
- **Simplificación**: Elimina la necesidad de importar `EventEmitter` y crear una nueva instancia, haciendo el código un poco más conciso.
- **Transformaciones (futuro)**: Aunque aún no es una característica principal, `output()` tiene el potencial de integrar transformaciones de eventos de manera similar a `input()`.

### 3.3. Comparación y Cuándo Usar Cada Uno

| Característica | `@Input()` (Tradicional) | `input()` (Basado en Signals) | `@Output()` (Tradicional) | `output()` (Basado en Signals) |
| --- | --- | --- | --- | --- |
| **Tipo de Propiedad** | Propiedad de clase normal. | Signal de solo lectura. | Instancia de `EventEmitter`. | Objeto con método `emit()`. |
| **Reactividad** | Basado en la detección de cambios de Zone.js. Requiere `ngOnChanges` o setters para reaccionar a cambios dentro del componente. | Intrínsecamente reactivo. Los cambios son notificados directamente a los consumidores de la Signal. | No es una propiedad reactiva intrínseca, es un emisor de eventos. | No es una propiedad reactiva intrínseca, es un emisor de eventos. |
| **Acceso al Valor** | Directo (`this.propertyName`). | Como una función (`this.propertyName()`). | Se llama a `emit()` en la instancia. | Se llama a `emit()` en el objeto devuelto por `output()`. |
| **Valor Inicial** | Se asigna directamente en la declaración o en el constructor. Puede ser `undefined` en el constructor si no se pasa. | Se pasa como primer argumento a `input()`. Siempre tiene un valor definido desde el constructor. | Se inicializa con `new EventEmitter()`. | Se declara con `output<Tipo>()` o `output()`. |
| **Requerido/Opcional** | Opcional por defecto. Requiere `!` o `undefined` en TypeScript. | Explícitamente opcional (`input(defaultValue)`) o requerido (`input.required()`). | Siempre opcional (si no se escucha, no pasa nada). | Siempre opcional (si no se escucha, no pasa nada). |
| **Inmutabilidad** | No forzada. Si se pasa un objeto, las mutaciones internas no disparan `ngOnChanges` por defecto. | Promueve la inmutabilidad. Solo se notifica el cambio si la _referencia_ de la Signal cambia. | N/A (emisión de eventos). | N/A (emisión de eventos). |
| **Transformación** | Se maneja con _setters_ en la propiedad. | Soporte integrado para funciones transform. | N/A. | Potencial futuro para transformaciones de eventos. |
| **Uso Principal** | Para la mayoría de las entradas de datos en componentes existentes y nuevos. | Preferido para nuevas aplicaciones y refactorizaciones que buscan una reactividad más explícita y eficiente. | Para la mayoría de las emisiones de eventos en componentes existentes y nuevos. | Preferido para nuevas aplicaciones y refactorizaciones que buscan una API más moderna y consistente con Signals. |

#### ¿Cuándo usar cuál?

- **Usa `input()` (Signals) para**:
  - **Nuevos componentes**: Si estás creando un componente desde cero en un proyecto Angular 16+ (o v20), es la opción recomendada para las entradas.
  - **Refactorización**: Si estás actualizando componentes existentes y quieres migrar a un modelo de reactividad basado en Signals, o si necesitas las ventajas de la inmutabilidad y las transformaciones.
  - **Componentes que se benefician de la reactividad granular**: Cuando la lógica del componente hijo necesita reaccionar de forma muy específica a los cambios en sus entradas sin depender de la detección de cambios global.
- **Usa `@Input()` para**:
  - **Proyectos existentes**: Si estás trabajando en un proyecto Angular más antiguo que no ha migrado a Signals, o si el equipo aún no está familiarizado con el paradigma de Signals.
  - **Simplicidad**: Para entradas muy simples donde no necesitas la reactividad granular de Signals o transformaciones complejas.
  - **Compatibilidad con librerías**: Algunas librerías o patrones de terceros pueden aún depender de la forma tradicional de `@Input()`.
- **Usa `output()` (Signals) para**:
  - **Nuevos componentes**: Si estás creando un componente desde cero en un proyecto Angular 16+ (o v20) y quieres una API más moderna y consistente con Signals para la emisión de eventos.
  - **Consistencia**: Si ya estás utilizando `input()` para las entradas y quieres mantener una coherencia en la forma de declarar las interfaces de tu componente.
- Usa `@Output()` con `EventEmitter` para:
  - **Proyectos existentes**: Si estás trabajando en un proyecto Angular más antiguo o si el equipo prefiere la API tradicional.
  - **Familiaridad**: Es el método más conocido y documentado durante mucho tiempo.

En general, la tendencia en Angular es hacia el modelo de Signals, por lo que `input()` y `output()` son el futuro de las interfaces de componentes. Adoptarlos te permitirá aprovechar al máximo las optimizaciones de rendimiento y la claridad del flujo de datos que ofrece el nuevo sistema de reactividad de Angular.

Espero que esta explicación detallada te sea muy útil para comprender y aplicar ambos métodos de comunicación padre-hijo y hijo-padre en tus proyectos Angular.

## 4. Refactorizando el Frontend de Ofertas Inmobiliarias

Ahora aplicaremos los conceptos de Signals y comunicación entre componentes para mejorar nuestro listado de propiedades. Refactorizaremos la "tarjeta" de cada propiedad en componentes hijos separados (`PropertyCardTraditional` y `PropertyCardSignal`) y usaremos Signals para la gestión del estado en el componente padre.

### 4.1. Crear el Componente Hijo `PropertyCardTraditional` (Enfoque Tradicional)

Este componente usará el enfoque tradicional de `@Input()` y `EventEmitter`.

1. **Genera el componente PropertyCardTraditional**:
    - Abre tu terminal en la raíz del proyecto.
    - Ejecuta el comando para generar un componente standalone dentro de la misma carpeta que `property-list`.

    ```Terminal
    ng generate component features/properties/components/property-card-traditional
    ```

2. **Implementa la lógica en `src/app/features/properties/components/property-card-traditional/property-card-traditional.ts`**:
    - Importa `Input`, `Output`, `EventEmitter` y la interfaz `Property`.
    - Declara una propiedad `@Input()` para recibir la `property` del padre.
    - Declara un `@Output()` para emitir un evento cuando se haga clic en el botón "Ver Detalles".

    ```Typescript
    import { Component, Input, Output, EventEmitter } from '@angular/core';
    import { CommonModule, CurrencyPipe } from '@angular/common'; // Importa CurrencyPipe
    import { Property } from '../../models/property.model'; // Importa la interfaz Property

    @Component({
      selector: 'app-property-card-traditional', // Selector específico para esta versión
      imports: [CommonModule, CurrencyPipe], // Añade CurrencyPipe a los imports
      templateUrl: './property-card-traditional.html',
      styleUrl: './property-card-traditional.css'
    })
    export class PropertyCardTraditionalComponent {
      @Input() property!: Property; // Recibe una propiedad del componente padre

      // Emite un evento cuando se hace clic en "Ver Detalles"
      @Output() viewDetails = new EventEmitter<number>();

      constructor() { }

      onViewDetailsClick(): void {
        this.viewDetails.emit(this.property.id); // Emite el ID de la propiedad
      }
    }
    ```

3. **Diseña la plantilla (`src/app/features/properties/components/property-card-traditional/property-card-traditional.html`) con Tailwind CSS**:
    - Copia el HTML de una sola tarjeta de propiedad de la clase anterior.
    - Asegúrate de que las referencias a `property` usen las propiedades del `@Input()`.
    - Ajusta el botón "Ver Detalles" para que use el método `onViewDetailsClick()`.

    ```HTML
    <div class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 overflow-hidden">
      <img
        [src]="property.imageUrl"
        [alt]="property.address"
        class="w-full h-48 object-cover rounded-t-xl"
        onerror="this.onerror=null;this.src='https://placehold.co/400x300/CCCCCC/666666?text=No+Image';"
      />
      <div class="p-6">
        <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ property.address }}</h2>
        <p class="text-gray-700 text-sm mb-4 line-clamp-3">{{ property.description }}</p>
        <div class="flex justify-between items-center mt-auto">
          <span class="text-3xl font-extrabold text-indigo-600">{{ property.price | currency:'USD':'symbol':'1.0-0' }}</span>
          <span class="px-3 py-1 rounded-full text-sm font-semibold bg-blue-100 text-blue-800">
            {{ property.bedrooms }} Hab. | {{ property.bathrooms }} Baños
          </span>
        </div>
        <button
          class="mt-6 w-full bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md hover:shadow-lg"
          (click)="onViewDetailsClick()"
        >
          Ver Detalles (Tradicional)
        </button>
      </div>
    </div>
    ```

4. **Actualiza la lógica en `src/app/features/properties/components/property-list/property-list.ts`**
    - Importa `PropertyCardTraditional`.
    - Agrega el método `handleViewDetails(property)` para manejar el evento que se generará en el componente hijo.

    ```Typescript
    import { Component, OnInit } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { Property } from '../../models/property';
    import { PropertyCardTraditional } from '../property-card-traditional/property-card-traditional';

    @Component({
      selector: 'app-property-list',
      imports: [CommonModule, PropertyCardTraditional],
      templateUrl: './property-list.html',
      styleUrl: './property-list.css'
    })
    export class PropertyList implements OnInit {
      allProperties: Property[] = [];
      filterCity: string = '';
      loading: boolean = true;
      error: string | null = null;
      properties: Property[] = [];

      ngOnInit() {
        this.loading = true;
        this.error = null;

        // Simulate an API call
        setTimeout(() => {
          const simulatedData: Property[] = [
            {
              id: 1,
              address: '123 Calle del Sol',
              city: 'Madrid',
              price: 350000,
              bedrooms: 3,
              bathrooms: 2,
              imageUrl: 'https://placehold.co/400x300/E0E0E0/333333?text=Casa+Moderna',
              description: 'Amplia casa familiar con jardín y piscina, cerca de servicios.'
            },
            {
              id: 2,
              address: '456 Avenida de la Luna',
              city: 'Barcelona',
              price: 280000,
              bedrooms: 2,
              bathrooms: 1,
              imageUrl: 'https://placehold.co/400x300/D0D0D0/444444?text=Apartamento+Céntrico',
              description: 'Apartamento luminoso en el centro de la ciudad, con vistas.'
            },
            {
              id: 3,
              address: '789 Plaza Mayor',
              city: 'Sevilla',
              price: 180000,
              bedrooms: 1,
              bathrooms: 1,
              imageUrl: 'https://placehold.co/400x300/C0C0C0/555555?text=Estudio+Acogedor',
              description: 'Estudio acogedor ideal para estudiantes o parejas, buena ubicación.'
            },
            {
              id: 4,
              address: '101 Calle del Río',
              city: 'Valencia',
              price: 450000,
              bedrooms: 4,
              bathrooms: 3,
              imageUrl: 'https://placehold.co/400x300/B0B0B0/666666?text=Chalet+Lujo',
              description: 'Chalet de lujo con amplios espacios y acabados de alta calidad.'
            }
          ];
          this.allProperties = simulatedData; // Guarda todas las propiedades
          this.properties = this.allProperties; // Inicialmente muestra todas
          this.loading = false;
        }, 1500);
      }

      searchProperties(): void {
        if (!this.filterCity) {
          this.properties = this.allProperties; // Si el filtro está vacío, muestra todas
        } else {
          this.properties = this.allProperties.filter(property =>
            property.city.toLowerCase().includes(this.filterCity.toLowerCase())
          );
        }
      }

      handleViewDetails(propertyId: number): void {
        console.log(`Ver detalles de la propiedad con ID: ${propertyId}`);
        alert(`Has hecho clic en la propiedad con ID: ${propertyId}`); // Usar alert solo para demostración
      }
    }
    ```

5. **Actualiza la plantilla `rc/app/features/properties/components/property-list/property-list.html`**
    - Reemplaza el contenido de la tarjeta con el selector de `PropertyCardTraditional`.

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen">
      <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12 drop-shadow-lg">Ofertas Inmobiliarias</h1>

      <!-- Sección de Filtro -->
      <div class="mb-8 flex flex-col sm:flex-row items-center justify-center gap-4 p-6 bg-white rounded-xl shadow-md">
        <input type="text" placeholder="Filtrar por ciudad (ej. Madrid)"
          class="flex-grow p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-lg"
          [(ngModel)]="filterCity">
        <button
          class="w-full sm:w-auto bg-indigo-600 text-white py-3 px-6 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md"
          (click)="searchProperties()">
          Buscar
        </button>
      </div>

      @if (loading) {
        <div class="flex justify-center items-center h-64">
          <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
          <p class="ml-4 text-2xl text-blue-600">Cargando propiedades...</p>
        </div>
      } @else if (error) {
        <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
          <p class="font-bold mb-2">¡Error al cargar las propiedades!</p>
          <p>{{ error }}</p>
        </div>
      } @else if (properties.length === 0) {
        <div class="text-center p-8 text-2xl text-gray-500 bg-yellow-50 border border-yellow-300 rounded-lg shadow-md">
          <p class="font-bold mb-2">No hay propiedades disponibles en este momento.</p>
          <p>Por favor, inténtalo de nuevo más tarde o añade nuevas propiedades.</p>
        </div>
      } @else {
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
          @for (property of properties; track property.id) {
           <app-property-card-traditional [property]="property" (viewDetails)="handleViewDetails($event)"></app-property-card-traditional>
          }
        </div>
      }
    </div>
    ```

### 4.2. Crear el Componente Hijo `PropertyCardSignal` (Enfoque con Signals)

Este componente usará el nuevo enfoque de input() y output().

1. **Genera el componente `PropertyCardSignal`**:
    - Abre tu terminal en la raíz del proyecto.
    - Ejecuta el comando:

      ```Terminal
      ng generate component features/properties/components/property-card-signal
      ```

2. **Implementa la lógica en `src/app/features/properties/components/property-card-signal/property-card-signal.ts`**:
    - Importa `input`, `output` y la interfaz `Property`.
    - Declara una propiedad `input()` para recibir la `property`.
    - Declara un `output()` para emitir un evento.

    ```Typescript
    import { Component, input, output } from '@angular/core'; // Importa input y output
    import { CommonModule, CurrencyPipe } from '@angular/common'; // Importa CurrencyPipe
    import { Property } from '../../models/property.model'; // Importa la interfaz Property

    @Component({
      selector: 'app-property-card-signal', // Selector específico para esta versión
      imports: [CommonModule, CurrencyPipe], // Añade CurrencyPipe a los imports
      templateUrl: './property-card-signal.html',
      styleUrl: './property-card-signal.css'
    })
    export class PropertyCardSignal {
      // Recibe una propiedad del componente padre usando input()
      property = input.required<Property>();

      // Emite un evento cuando se hace clic en "Ver Detalles" usando output()
      viewDetails = output<number>();

      constructor() { }

      onViewDetailsClick(): void {
        this.viewDetails.emit(this.property().id); // Emite el ID de la propiedad (llama a la Signal)
      }
    }
    ```

3. **Diseña la plantilla (`src/app/features/properties/components/property-card-signal/property-card-signal.html`) con Tailwind CSS**:
    - Es idéntica a la versión tradicional, pero las referencias a property ahora son funciones (`property()`).
    - Ajusta el botón "Ver Detalles" para que use el método `onViewDetailsClick()`.

    ```HTML
    <div class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 overflow-hidden">
      <img
        [src]="property().imageUrl"
        [alt]="property().address"
        class="w-full h-48 object-cover rounded-t-xl"
        onerror="this.onerror=null;this.src='https://placehold.co/400x300/CCCCCC/666666?text=No+Image';"
      />
      <div class="p-6">
        <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ property().address }}</h2>
        <p class="text-gray-700 text-sm mb-4 line-clamp-3">{{ property().description }}</p>
        <div class="flex justify-between items-center mt-auto">
          <span class="text-3xl font-extrabold text-indigo-600">{{ property().price | currency:'USD':'symbol':'1.0-0' }}</span>
          <span class="px-3 py-1 rounded-full text-sm font-semibold bg-blue-100 text-blue-800">
            {{ property().bedrooms }} Hab. | {{ property().bathrooms }} Baños
          </span>
        </div>
        <button
          class="mt-6 w-full bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md hover:shadow-lg"
          (click)="onViewDetailsClick()"
        >
          Ver Detalles (Signals)
        </button>
      </div>
    </div>
    ```

4. **Actualiza la lógica en `src/app/features/properties/components/property-list/property-list.ts`**
    - Importa `signal`, `computed` y `effect` desde `@angular/core`.
    - Importa `PropertyCardSignal`.
    - Cambia las propiedades a Signals

    ```Typescript
    import { Component, computed, effect, OnInit, signal } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { FormsModule } from '@angular/forms';
    import { Property } from '../../models/property';
    import { PropertyCardSignal } from "../property-card-signal/property-card-signal";

    @Component({
      selector: 'app-property-list',
      imports: [CommonModule, FormsModule, PropertyCardSignal],
      templateUrl: './property-list.html',
      styleUrl: './property-list.css'
    })
    export class PropertyList implements OnInit {
      allProperties = signal<Property[]>([]);
      filterCity = signal<string>('');
      loading = signal<boolean>(true);
      error = signal<string | null>(null);

      properties = computed(() => {
        const currentFilter = this.filterCity().toLowerCase();
        if (!currentFilter) {
          return this.allProperties();
        } else {
          return this.allProperties().filter(property =>
            property.city.toLowerCase().includes(currentFilter)
          );
        }
      });

      constructor() {
        // Opcional: un efecto para ver los cambios en el filtro
        effect(() => {
          console.log(`El filtro de ciudad ha cambiado a: ${this.filterCity()}`);
        });
      }

      ngOnInit() {
        this.loading.set(true);
        this.error.set(null);

        // Simulate an API call
        setTimeout(() => {
          this.allProperties.set([
            {
              id: 1,
              address: '123 Calle del Sol',
              city: 'Madrid',
              price: 350000,
              bedrooms: 3,
              bathrooms: 2,
              imageUrl: 'https://placehold.co/400x300/E0E0E0/333333?text=Casa+Moderna',
              description: 'Amplia casa familiar con jardín y piscina, cerca de servicios.'
            },
            {
              id: 2,
              address: '456 Avenida de la Luna',
              city: 'Barcelona',
              price: 280000,
              bedrooms: 2,
              bathrooms: 1,
              imageUrl: 'https://placehold.co/400x300/D0D0D0/444444?text=Apartamento+Céntrico',
              description: 'Apartamento luminoso en el centro de la ciudad, con vistas.'
            },
            {
              id: 3,
              address: '789 Plaza Mayor',
              city: 'Sevilla',
              price: 180000,
              bedrooms: 1,
              bathrooms: 1,
              imageUrl: 'https://placehold.co/400x300/C0C0C0/555555?text=Estudio+Acogedor',
              description: 'Estudio acogedor ideal para estudiantes o parejas, buena ubicación.'
            },
            {
              id: 4,
              address: '101 Calle del Río',
              city: 'Valencia',
              price: 450000,
              bedrooms: 4,
              bathrooms: 3,
              imageUrl: 'https://placehold.co/400x300/B0B0B0/666666?text=Chalet+Lujo',
              description: 'Chalet de lujo con amplios espacios y acabados de alta calidad.'
            }
          ]);
          this.loading.set(false);
        }, 1500);
      }

      // Otros metodos existentes

      searchProperties(): void {
        console.log('Buscando propiedades con filtro:', this.filterCity());
      }

      onFilterInputChange(event: Event): void {
        const inputElement = event.target as HTMLInputElement;
        this.filterCity.set(inputElement.value); // Actualiza la Signal
      }

      handleViewDetails(propertyId: number): void {
        console.log(`Ver detalles de la propiedad con ID: ${propertyId}`);
        alert(`Has hecho clic en la propiedad con ID: ${propertyId}`); // Usar alert solo para demostración
      }
    }
    ```

5. **Actualiza la plantilla `rc/app/features/properties/components/property-list/property-list.html`**
    - Ahora, para acceder a los valores de las Signals en la plantilla, debes llamarlas como funciones (ej., `loading()`, `properties()`. `error()`).
    - Reemplaza el contenido de la tarjeta con el selector de `PropertyCardSignal`.

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen">
      <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12 drop-shadow-lg">Ofertas Inmobiliarias</h1>

      <!-- Sección de Filtro -->
      <div class="mb-8 flex flex-col sm:flex-row items-center justify-center gap-4 p-6 bg-white rounded-xl shadow-md">
        <input type="text" placeholder="Filtrar por ciudad (ej. Madrid)"
          class="flex-grow p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-lg"
          [ngModel]="filterCity()" 
        (input)="onFilterInputChange($event)">
        <button
          class="w-full sm:w-auto bg-indigo-600 text-white py-3 px-6 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md"
          (click)="searchProperties()">
          Buscar
        </button>
      </div>

      @if (loading()) {
        <div class="flex justify-center items-center h-64">
          <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
          <p class="ml-4 text-2xl text-blue-600">Cargando propiedades...</p>
        </div>
      } @else if (error()) {
        <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
          <p class="font-bold mb-2">¡Error al cargar las propiedades!</p>
          <p>{{ error() }}</p>
        </div>
      } @else if (properties().length === 0) {
        <div class="text-center p-8 text-2xl text-gray-500 bg-yellow-50 border border-yellow-300 rounded-lg shadow-md">
          <p class="font-bold mb-2">No hay propiedades disponibles en este momento.</p>
          <p>Por favor, inténtalo de nuevo más tarde o añade nuevas propiedades.</p>
        </div>
      } @else {
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
        @for (property of properties(); track property.id) {
          <app-property-card-signal [property]="property" (viewDetails)="handleViewDetails($event)"></app-property-card-signal>
          }
      </div>
      }
    </div>
    ```

### 4.3. Verificar y Probar

1. Asegúrate de que tu servidor de desarrollo de Angular esté corriendo (`ng serve`).
2. Abre tu navegador en `http://localhost:4200/`.
3. Deberías ver el mismo listado de propiedades, pero ahora cada tarjeta es un componente independiente utilizando el enfoque de Signals.
4. Prueba el filtro de ciudad.
5. Haz clic en el botón "Ver Detalles (Signals)" de una tarjeta y verifica que el mensaje de alerta (o el `console.log`) muestre el ID correcto de la propiedad, demostrando la comunicación de hijo a padre con Signals.
