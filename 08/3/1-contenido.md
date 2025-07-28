# Clase 3: Navegación (Routing) y Servicios

¡Hola a todos!
¡Bienvenidos a la tercera clase del **Módulo 8**! En la clase anterior, hicimos un gran avance al introducir las **Signals** para la reactividad y profundizamos en la **comunicación entre componentes** (`input()` y `output()`). Refactorizamos nuestra lista de cursos universitarios para aprovechar estas poderosas características.

En esta clase, llevaremos nuestras aplicaciones al siguiente nivel, transformándolas de simples listas a **Single Page Applications (SPAs)** más completas, con múltiples vistas. Para ello, exploraremos el **sistema de enrutamiento (routing)** de Angular, que nos permite navegar entre diferentes "páginas" o vistas dentro de la misma aplicación sin recargar el navegador. Además, introduciremos un concepto fundamental para la modularidad y la reusabilidad del código: los **Servicios y la Inyección de Dependencias**. Aprenderemos cómo los servicios pueden centralizar la lógica de negocio y la gestión de datos, y cómo inyectarlos en nuestros componentes de manera eficiente.

Prepárense para construir una experiencia de usuario más fluida y una arquitectura de aplicación más robusta.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Comprender el propósito y los beneficios de los Servicios** en Angular.
- **Crear y registrar servicios** utilizando la Inyección de Dependencias.
- **Utilizar servicios para centralizar la lógica de negocio** y la gestión de datos (simulando una API).
- **Comprender el concepto de enrutamiento** en una Single Page Application (SPA).
- **Configurar rutas** en una aplicación Angular.
- **Implementar navegación declarativa** (`routerLink`) **y programática** (`Router` service).
- **Recibir parámetros de ruta** en componentes utilizando la función `input()` (Angular 20+).
- **Recibir parámetros de ruta** de manera tradicional utilizando `ActivatedRoute`.

## 1. Servicios y Inyección de Dependencias

Los servicios en Angular son clases especializadas que encapsulan lógica de negocio, acceso a datos, o cualquier funcionalidad transversal que no esté directamente ligada a la interfaz de usuario de un componente. Su propósito principal es desacoplar la lógica de la presentación, permitiendo que los componentes se centren en lo que deben hacer: mostrar datos y manejar la interacción del usuario. Son ideales para compartir datos, estados y comportamientos entre múltiples componentes, promoviendo la reusabilidad y una clara separación de responsabilidades dentro de la aplicación.

### 1.1. ¿Por qué usar Servicios?

- **Separación de Responsabilidades**: Los servicios permiten que los componentes se mantengan "delgados" y centrados únicamente en la lógica de la vista y la interacción directa con el usuario. Toda la lógica compleja, como la obtención de datos de una API, el procesamiento de información o la gestión del estado global, se delega a los servicios. Esto hace que cada parte de tu aplicación sea más fácil de entender, desarrollar y depurar. Por ejemplo, un componente de lista de productos solo se encargaría de renderizar los productos y de reaccionar a los clics, mientras que un `ProductService` se encargaría de ir a buscar esos productos al servidor.
- **Reusabilidad**: Una vez que defines una funcionalidad en un servicio, puedes inyectar y utilizar esa misma instancia de servicio en cualquier componente, directiva o incluso en otros servicios que lo necesiten. Esto evita la duplicación de código y asegura la consistencia en el comportamiento de tu aplicación. Imagina un `UserService` que maneja la autenticación y los datos del usuario; este servicio puede ser inyectado en un `LoginComponent`, un `ProfileComponent` y un `HeaderComponent`, todos compartiendo la misma lógica y el mismo estado del usuario.
- **Mantenibilidad**: Al centralizar la lógica en servicios, los cambios o actualizaciones en esa lógica solo necesitan realizarse en un único lugar. Si la forma en que tu aplicación interactúa con una API cambia (por ejemplo, la URL base o el formato de los datos), solo necesitas modificar el servicio correspondiente, y todos los componentes que lo utilicen se beneficiarán automáticamente de esos cambios sin requerir modificaciones. Esto simplifica enormemente el mantenimiento y la evolución de aplicaciones grandes y complejas.
- **Probabilidad (Testeability)**: Los servicios son clases de TypeScript que no tienen dependencias directas del DOM o de la interfaz de usuario. Esto los hace mucho más fáciles de probar de forma aislada (pruebas unitarias). Puedes simular sus dependencias y verificar que su lógica de negocio funciona correctamente, sin necesidad de renderizar componentes o interactuar con el navegador, lo que acelera el ciclo de desarrollo y mejora la calidad del software.

### 1.2. Creación de un Servicio

Para marcar una clase como un servicio que puede ser inyectado por el sistema de inyección de dependencias de Angular, se utiliza el decorador `@Injectable()`.

```Typescript
// src/app/shared/services/data.service.ts (Ejemplo genérico)
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root' // Hace que el servicio sea un singleton y esté disponible en toda la aplicación
})
export class DataService {
  private _data = signal<any[]>([]); // Ejemplo de Signal para datos, gestionada internamente por el servicio

  constructor() {
    // El constructor de un servicio es el lugar ideal para inyectar otros servicios
    // o para realizar configuraciones iniciales que no dependan del estado del componente.
    console.log('DataService inicializado.');
  }

  // Métodos para interactuar con los datos
  getData(): Promise<any[]> {
    // Simula una operación asíncrona, como una llamada HTTP a una API
    return new Promise(resolve => {
      setTimeout(() => {
        console.log('Obteniendo datos desde DataService...');
        resolve(this._data()); // Devuelve el valor actual de la Signal
      }, 500); // Retardo simulado de 500ms
    });
  }

  // Método para actualizar los datos, que los componentes pueden llamar
  updateData(newData: any[]): void {
    this._data.set(newData);
    console.log('Datos actualizados en DataService:', this._data());
  }

  // ... otros métodos de lógica de negocio o acceso a datos
}
```

- `@Injectable()`: Este decorador es fundamental. Le dice a Angular que esta clase es un proveedor de servicios y que puede tener dependencias que deben ser inyectadas en su constructor. Sin este decorador, Angular no sabría cómo crear una instancia de tu servicio ni cómo resolver sus propias dependencias.
- `providedIn: 'root'`: Esta propiedad dentro del decorador `@Injectable()` es una forma moderna y eficiente de "proveer" el servicio. Cuando `providedIn: 'root'` se utiliza, Angular crea una única instancia de este servicio (un singleton) y la registra a nivel de la aplicación raíz. Esto significa que cualquier componente o servicio en cualquier parte de tu aplicación que solicite este servicio recibirá la misma instancia, lo que es ideal para servicios que gestionan el estado global o que interactúan con recursos compartidos (como una API backend). Elimina la necesidad de añadir el servicio al array providers de un `@NgModule` o `@Component` específico, lo que simplifica la configuración y permite que el servicio sea "tree-shakable" (es decir, si el servicio no se usa, no se incluye en el bundle final de la aplicación, optimizando el tamaño).

### 1.3. Inyección de Dependencias (mediante constructor)

La Inyección de Dependencias (DI) es un patrón de diseño que Angular utiliza para construir y entregar partes de la aplicación (como servicios) a otras partes que las necesitan (como componentes). En lugar de que un componente cree sus propias instancias de los servicios que necesita, Angular se encarga de "inyectar" esas instancias en el constructor del componente.

Para usar un servicio en un componente, simplemente lo declaras como un parámetro en el constructor del componente, especificando su tipo. Angular, al detectar esta declaración de tipo, busca un proveedor para ese tipo de servicio en su "árbol de inyectores" y, si lo encuentra, crea o reutiliza una instancia del servicio y la pasa al constructor del componente.

```Typescript
// En un componente (ej. src/app/features/properties/components/property-list/property-list.ts)
import { Component, OnInit } from '@angular/core';
import { DataService } from '../../shared/services/data.service'; // Importa el servicio que necesitas

@Component({
  selector: 'app-my-component',
  // ...
})
export class MyComponent implements OnInit {
  // Angular detecta 'private dataService: DataService' y automáticamente inyecta una instancia de DataService
  constructor(private dataService: DataService) {
    console.log('Componente inicializado, DataService inyectado.');
  }

  ngOnInit(): void {
    this.dataService.getData().then(data => {
      console.log('Datos obtenidos del servicio:', data);
      // Usar los datos para actualizar el estado del componente (ej. una Signal)
      // this.myLocalSignal.set(data);
    }).catch(error => {
      console.error('Error al obtener datos:', error);
    });
  }

  // Puedes llamar a métodos del servicio desde otros métodos del componente
  someAction(): void {
    this.dataService.updateData(['nuevo', 'conjunto', 'de', 'datos']);
  }
}
```

Este patrón de DI promueve el código modular, desacoplado y fácil de probar, ya que los componentes no tienen que preocuparse por cómo se crean sus dependencias, sino solo por usarlas.

### 1.4. Uso de la función `inject()` (Angular 14+)

A partir de Angular 14, se introdujo la función `inject()` como una alternativa moderna y flexible a la inyección de dependencias a través del constructor. Esta función es particularmente útil en los siguientes escenarios:

- **Fuera del constructor de una clase**: Puedes usar `inject()` en cualquier función o contexto de inicialización donde no tengas acceso al constructor de una clase, como en el inicializador de una propiedad de clase, dentro de una función regular, o en los nuevos `effects` de Signals. Esto es especialmente relevante para los componentes y directivas `standalone`, donde a menudo se desea una inicialización más concisa.
- **Para propiedades de clase inicializadas**: Permite inyectar dependencias directamente en la declaración de una propiedad, haciendo el código más limpio y evitando la necesidad de un constructor solo para la inyección.
- **En funciones auxiliares o factorías**: Si tienes funciones que necesitan acceder a servicios de Angular pero no son clases de Angular (como un componente o un servicio), `inject()` te permite obtener esos servicios.
- **Mejor integración con Signals `effect` y `computed`**: Dentro de las funciones `effect` o `computed` de Signals, `inject()` es la forma recomendada para obtener servicios, ya que estos contextos no tienen un constructor tradicional.

#### Consideraciones importantes al usar `inject()`

- **Contexto de Inyección**: La función `inject()` debe ser llamada dentro de un contexto de inyección. Esto significa que debe ser ejecutada durante la fase de construcción o inicialización de un componente, directiva, pipe o servicio. No puedes llamarla en un contexto arbitrario, como dentro de un `setTimeout` o un manejador de eventos que se ejecuta mucho después de la inicialización del componente.
- **No es un reemplazo total**: Aunque `inject()` es muy potente, la inyección por constructor sigue siendo perfectamente válida y a menudo preferible para dependencias principales de una clase, ya que hace que las dependencias sean explícitas y facilita la lectura del código. `inject()` brilla en escenarios donde la inyección por constructor no es posible o es menos ergonómica.

```Typescript
import { inject } from '@angular/core';
import { MyService } from './my.service'; // Tu servicio

// Dentro de un componente standalone o directiva:
@Component({
  selector: 'app-my-component',
  // ...
})
export class MyComponent {
  // Inyección directamente en la declaración de la propiedad
  myService = inject(MyService);

  // También puedes inyectar en el constructor si lo prefieres, como antes
  // constructor(private myService: MyService) {}

  constructor() {
    // También puedes usar inject() dentro del constructor si necesitas lógica adicional
    // const anotherService = inject(AnotherService);
    console.log('Servicio inyectado con inject():', this.myService);

    // Ejemplo de uso de inject() dentro de un effect de Signals
    effect(() => {
      // Acceder a myService aquí es seguro porque el effect se ejecuta en un contexto de inyección
      console.log('Effect ejecutado, accediendo a myService:', this.myService);
      // this.myService.someMethod();
    });
  }
}

// Ejemplo de uso de inject() en una función auxiliar (fuera de una clase)
// Nota: Esta función debe ser llamada dentro de un contexto de inyección válido.
function configureSomething(): void {
  const configService = inject(ConfigService);
  console.log('Configuración obtenida con inject():', configService.getSettings());
}
```

La función `inject()` es una adición valiosa al ecosistema de Angular, proporcionando una forma más flexible y moderna de manejar la inyección de dependencias, especialmente a medida que las aplicaciones evolucionan hacia un modelo más basado en funciones y Signals.

## 2. Enrutamiento (Routing) en Angular

El enrutamiento es una característica clave de las Single Page Applications (SPAs). Permite que la aplicación cargue diferentes vistas o componentes en función de la URL del navegador, sin necesidad de recargar la página completa. Esto proporciona una experiencia de usuario fluida y rápida, similar a una aplicación de escritorio.

**Conceptos Clave**:

- **`Routes` (Rutas)**: Un array de objetos que define la relación entre una URL y un componente. Cada objeto de ruta tiene al menos una propiedad `path` (la URL) y una propiedad `component` (el componente a cargar).
- `RouterModule`: El módulo de Angular que proporciona las funcionalidades de enrutamiento. En aplicaciones standalone, se configura a través de la función `provideRouter()`.
- `RouterOutlet`: Una directiva que actúa como un "marcador de posición" en la plantilla de tu componente raíz (`app.html`). Angular renderiza el componente asociado a la ruta actual dentro de este `RouterOutlet`.
- `routerLink`: Una directiva para la navegación declarativa. Se usa en elementos HTML (como `<a>` o `<button>`) para crear enlaces que, al hacer clic, cambian la URL y activan la ruta correspondiente.
- **`Router` Service**: Un servicio inyectable que permite la navegación programática (desde el código TypeScript). Es útil para navegar después de una acción del usuario, una validación de formulario, o una llamada a una API.

### 2.1. Configuración de Rutas

La configuración de rutas en Angular se realiza definiendo un array de objetos `Routes`. Cada objeto en este array representa una ruta y especifica qué componente debe cargarse cuando la URL del navegador coincide con el `path` definido.

En Angular 17+ con componentes standalone, las rutas se definen en un archivo separado (comúnmente `app.routes.ts`) y se configuran en `app.config.ts` utilizando `provideRouter`().

#### 2.1.1. Modo Tradicional (Carga Eager)

En el modo tradicional o "carga eager", todos los componentes asociados a las rutas se cargan y se incluyen en el bundle inicial de la aplicación, independientemente de si el usuario visita esa ruta o no. Esto es adecuado para aplicaciones pequeñas o para rutas que son visitadas con mucha frecuencia.

##### 2.1.1.1. Ejemplo de `app.routes.ts` para carga eager

```Typescript
import { Routes } from '@angular/router';
import { Home } from './home/home'; // Importa directamente el componente
import { About } from './about/about'; // Importa directamente el componente
import { Contact } from './contact/contact'; // Importa directamente el componente

export const routes: Routes = [
  { path: '', component: Home }, // Ruta para la página de inicio
  { path: 'about', component: About }, // Ruta para la página "Acerca de"
  { path: 'contact', component: Contact }, // Ruta para la página de contacto
  { path: '**', redirectTo: '' } // Ruta comodín para cualquier otra URL no definida, redirige a inicio
];
```

##### 2.1.1.2. Configuración en `app.config.ts`

```Typescript
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router';

import { routes } from './app.routes'; // Importa tus rutas

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withComponentInputBinding()) // Provee el enrutador con tus rutas y habilita el binding de inputs
  ]
};
```

##### 2.1.1.3. Ventajas de la Carga Eager

- Simplicidad de configuración.
- Los componentes están disponibles inmediatamente una vez que la aplicación se carga.

##### 2.1.1.4. Desventajas de la Carga Eager

- El tamaño del bundle inicial de la aplicación puede ser grande, lo que afecta el tiempo de carga inicial.
- Se cargan componentes que quizás nunca sean visitados por el usuario.

#### 2.1.2. Lazy Loading (Carga Perezosa)

El Lazy Loading es una técnica de optimización que permite cargar módulos, componentes o rutas solo cuando son necesarios (es decir, cuando el usuario navega a esa ruta). Esto reduce significativamente el tamaño del bundle inicial de la aplicación, mejorando el tiempo de carga y la experiencia del usuario, especialmente en aplicaciones grandes.

Para implementar Lazy Loading, en lugar de especificar un `component` directamente, se utiliza la propiedad `loadChildren` o `loadComponent` junto con una función de importación dinámica (`import()`). Esta función devuelve una Promesa que resuelve con el módulo, el archivo de rutas o el componente que se cargarán de forma perezosa.

##### 2.1.2.1. Lazy Loading de Componentes Individuales con `loadComponent` (Angular 14+)

Con la introducción de los componentes `standalone`, Angular 14+ simplificó aún más el Lazy Loading al permitir cargar perezosamente componentes individuales directamente, sin necesidad de un archivo de rutas de características o un módulo. Esto es ideal para rutas que apuntan a un solo componente grande o a una vista única.

**Ejemplo de `src/app/app.routes.ts` usando `loadComponent`**:

```Typescript
import { Routes } from '@angular/router';
import { Home } from './home/home';

export const routes: Routes = [
  { path: '', component: Home },
  {
    path: 'dashboard', // Ruta para el dashboard
    loadComponent: () => import('./features/dashboard/dashboard').then(m => m.DashboardComponent)
  },
  {
    path: 'settings', // Ruta para la configuración
    loadComponent: () => import('./features/settings/settings').then(m => m.SettingsComponent)
  },
  // También puedes combinarlo con rutas con parámetros
  {
    path: 'products/:id',
    loadComponent: () => import('./features/products/product-detail/product-detail').then(m => m.ProductDetailComponent)
  },
  // Si tienes una sección de propiedades con múltiples rutas internas, seguirías usando loadChildren
  {
    path: 'properties',
    loadChildren: () => import('./features/properties/properties.routes').then(m => m.PROPERTIES_ROUTES)
  },
  { path: '**', redirectTo: '' }
];
```

- `path: 'dashboard'`: Esta es la ruta para el componente del dashboard.
- `loadComponent: () => import('./features/dashboard/dashboard').then(m => m.DashboardComponent)`:
  - `import('./features/dashboard/dashboard')`: Esta importación dinámica crea un chunk separado para el `DashboardComponent` y sus dependencias.
  - `.then(m => m.DashboardComponent)`: Una vez que la Promesa se resuelve, accedemos directamente al componente `DashboardComponent` exportado desde el archivo. Angular lo cargará y renderizará cuando se navegue a `/dashboard`.

##### 2.1.2.2. Lazy Loading de Rutas de Características (Feature Routes) con `loadChildren`

Es una práctica común organizar las rutas de una aplicación en "rutas de características" o "módulos de características". Con los componentes standalone, esto se traduce en agrupar las rutas relacionadas con una funcionalidad específica en un archivo de rutas separado. Por ejemplo, podrías tener rutas para "propiedades" (`properties.routes.ts`), "usuarios" (`users.routes.ts`), etc. Esto mejora la modularidad y la mantenibilidad del código.

**Define las rutas de una característica en un archivo separado (ej., `properties.routes.ts`)**:

```Typescript
// src/app/features/properties/properties.routes.ts
import { Routes } from '@angular/router';
import { PropertyList } from './components/property-list/property-list';
import { PropertyDetail } from './components/property-detail/property-detail';

export const PROPERTIES_ROUTES: Routes = [
  { path: '', component: PropertyList }, // Ruta base para /properties
  { path: ':id', component: PropertyDetail }, // Ruta para /properties/:id
];
```

**Nota**: Se recomienda exportar el array de rutas con un nombre descriptivo (ej., `PROPERTIES_ROUTES`) para facilitar su importación.

**Configura la ruta de Lazy Loading en el archivo de rutas principal (`app.routes.ts`)**:

```Typescript
// src/app/app.routes.ts
import { Routes } from '@angular/router';
import { Home } from './home/home'; // Un componente que se carga eager (ejemplo)

export const routes: Routes = [
  { path: '', component: Home }, // Ruta de inicio, cargada eager
  {
    path: 'properties', // Cuando la URL sea /properties, carga perezosamente las rutas de propiedades
    loadChildren: () => import('./features/properties/properties.routes').then(m => m.PROPERTIES_ROUTES)
  },
  // Otras rutas de lazy loading o eager
  { path: '**', redirectTo: '' }
];
```

- `path: 'properties'`: Esta es la ruta padre. Cuando el navegador navegue a `/properties` o cualquier sub-ruta como `/properties/123`, Angular intentará cargar el chunk JavaScript asociado a las rutas de `properties`.
- `loadChildren: () => import('./features/properties/properties.routes').then(m => m.PROPERTIES_ROUTES)`:
  - `import('./features/properties/properties.routes')`: Esta es una importación dinámica de ES2020. Le indica a Webpack (o Vite) que genere un chunk JavaScript separado para este archivo y todas sus dependencias. La importación devuelve una Promesa.
  - `.then(m => m.PROPERTIES_ROUTES)`: Una vez que la Promesa se resuelve (es decir, el chunk se ha cargado con éxito), accedemos a la propiedad `PROPERTIES_ROUTES` del objeto `m` (que representa el módulo importado). Esta es la colección de rutas que Angular debe usar para el sub-árbol de enrutamiento de 'properties'.

##### 2.1.2.3. Diferencias Clave entre `loadComponent` y `loadChildren`

- `loadComponent`: Se utiliza para cargar un componente individual (que debe ser `standalone`). Es más directo y simplifica el Lazy Loading para rutas que solo necesitan renderizar un único componente, sin la necesidad de un archivo de rutas dedicado para una sola vista.
- `loadChildren`: Se utiliza para cargar un conjunto de rutas (normalmente definidas en un archivo de rutas separado, como `properties.routes.ts`) o un módulo de Angular (para aplicaciones basadas en `NgModule`). Es ideal para organizar grandes secciones de la aplicación que tienen su propia estructura de enrutamiento interna, permitiendo que todas las rutas de esa sección se carguen perezosamente juntas.

##### 2.1.2.4. Ventajas del Lazy Loading (aplicable a `loadChildren` y `loadComponent`)

- **Mejora el tiempo de carga inicial**: El bundle inicial es más pequeño porque solo se carga el código esencial para la primera vista.
- **Optimización de recursos**: Se evita cargar código que el usuario podría no necesitar.
- **Escalabilidad**: Facilita el desarrollo de aplicaciones grandes, ya que el equipo puede trabajar en diferentes características de forma más independiente.

##### 2.1.2.5. Desventajas del Lazy Loading

- Retraso inicial al navegar a una ruta cargada perezosamente por primera vez (aunque suele ser mínimo).
- Configuración ligeramente más compleja que la carga eager.

### 2.2. `RouterOutlet` en `app.html`

El componente raíz de tu aplicación necesita un `<router-outlet>` donde se cargarán los componentes de las rutas.

```Typescript
<!-- src/app/app.html -->
<router-outlet></router-outlet>
```

### 2.3. Navegación Declarativa con `routerLink`

Para crear enlaces en tu plantilla:

```Typescript
<!-- Ejemplo: En una tarjeta de propiedad para ir a detalles -->
<a [routerLink]="['/properties', property.id]">Ver Detalles</a>
```

### 2.4. Navegación Programática con `Router` Service

Para navegar desde tu código TypeScript:

```Typescript
// Ejemplo: Después de una acción del usuario
import { Router } from '@angular/router';

constructor(private router: Router) {} // Inyecta el Router service

navigateToPropertyDetails(propertyId: number): void {
  this.router.navigate(['/properties', propertyId]); // Navega a /properties/123
}
```

### 2.5. Acceso a Parámetros de Ruta

#### 2.5.1 `input()` (Angular 20+) - _Recomendado_

En Angular 20+, la forma recomendada y más limpia para acceder a los parámetros de ruta es utilizando la función `input()`. Si habilitas `withComponentInputBinding()` en la configuración de tu enrutador (como se muestra en `app.config.ts`), Angular automáticamente mapeará los parámetros de la URL a propiedades `input()` de tu componente.

```Typescript
// En PropertyDetailComponent.ts o CourseDetailComponent.ts
import { Component, input, OnInit } from '@angular/core';
// ... otras importaciones

@Component({
  selector: 'app-property-detail',
  standalone: true,
  // ...
})
export class PropertyDetail implements OnInit {
  // El nombre 'id' debe coincidir con el parámetro de ruta definido en properties.routes.ts (:id)
  propertyId = input<string | undefined>(); // Recibe el ID de la ruta como una Signal

  // Para convertirlo a número y usarlo en un efecto o en ngOnInit
  numericPropertyId = input(0, {
    transform: (value: string | number | undefined) => {
      if (typeof value === 'string') {
        return parseInt(value, 10);
      }
      return value || 0;
    }
  });

  constructor() {
    // Puedes usar un efecto para reaccionar cuando el ID cambia
    effect(() => {
      console.log('ID de propiedad recibido (Signal):', this.propertyId());
      console.log('ID de propiedad numérico (Signal):', this.numericPropertyId());
      // Aquí podrías llamar al servicio para cargar los detalles del curso
      // this.propertyService.getPropertyById(this.numericPropertyId());
    });
  }

  ngOnInit(): void {
    // Si necesitas lógica de inicialización que dependa del ID, puedes usarlo aquí
    if (this.numericPropertyId()) {
      // Cargar detalles de la propiedad
    }
  }
}
```

##### Ventajas de `input()`

- Más conciso y legible.
- Se integra perfectamente con Signals para reactividad automática.
- Reduce la necesidad de suscripciones manuales y la gestión de memoria (`unsubscribe`).
- Promueve un código más limpio al eliminar la necesidad de inyectar `ActivatedRoute` para casos simples.

#### 2.5.2. `ActivatedRoute` (Alternativa/Legado)

`ActivatedRoute` es un servicio inyectable que proporciona acceso a información sobre la ruta asociada con el componente que se ha activado. Esto incluye los parámetros de la URL, los datos de la ruta, los parámetros de consulta y los fragmentos. Es la forma tradicional de acceder a los parámetros de ruta en Angular.

Puedes acceder a los parámetros de ruta de dos maneras principales:

1. **Usando el `paramMap` (Observable)**:  
    Esta es la forma preferida cuando los parámetros de la ruta pueden cambiar mientras el componente está activo (por ejemplo, si navegas de `/products/1` a `/products/2` sin destruir y recrear el componente). `paramMap` es un Observable que emite un nuevo `ParamMap` cada vez que los parámetros de la ruta cambian. Debes suscribirte a este Observable y desuscribirte cuando el componente se destruye para evitar fugas de memoria.

    ```Typescript
    import { Component, OnInit, OnDestroy } from '@angular/core';
    import { ActivatedRoute, ParamMap } from '@angular/router';
    import { Subscription } from 'rxjs'; // Necesario para gestionar la suscripción

    @Component({
      selector: 'app-example-detail',
      template: `<p>ID del elemento: {{ itemId }}</p>`
    })
    export class ExampleDetail implements OnInit, OnDestroy {
      itemId: string | null = null;
      private routeSubscription: Subscription | undefined;

      constructor(private route: ActivatedRoute) {}

      ngOnInit(): void {
        // Suscribirse a paramMap para reaccionar a cambios en los parámetros de ruta
        this.routeSubscription = this.route.paramMap.subscribe((params: ParamMap) => {
          this.itemId = params.get('id'); // 'id' debe coincidir con el nombre del parámetro en tu ruta (:id)
          console.log('ID del elemento (desde paramMap):', this.itemId);
          // Aquí cargarías los detalles del elemento usando this.itemId
        });
      }

      ngOnDestroy(): void {
        // Es crucial desuscribirse para evitar fugas de memoria
        if (this.routeSubscription) {
          this.routeSubscription.unsubscribe();
        }
      }
    }
    ```

2. **Usando el `snapshot.paramMap` (Sincrónico)**:  
    El snapshot proporciona acceso sincrónico a los parámetros de la ruta en el momento en que el componente se inicializa. Es útil cuando sabes que los parámetros de la ruta no cambiarán mientras el componente está activo (es decir, el componente siempre se destruirá y recreará si la ruta cambia). No requiere suscripción ni desuscripción.

    ```Typescript
    import { Component, OnInit } from '@angular/core';
    import { ActivatedRoute } from '@angular/router';

    @Component({
      selector: 'app-example-detail-snapshot',
      template: `<p>ID del elemento: {{ itemId }}</p>`
    })
    export class ExampleDetailSnapshot implements OnInit {
      itemId: string | null = null;

      constructor(private route: ActivatedRoute) {}

      ngOnInit(): void {
        // Acceder a los parámetros directamente desde el snapshot
        this.itemId = this.route.snapshot.paramMap.get('id');
        console.log('ID del elemento (desde snapshot):', this.itemId);
        // Aquí cargarías los detalles del elemento usando this.itemId
      }
    }
    ```

**Cuándo usar `ActivatedRoute` (en comparación con `input()`)**:

- **Para código legado**: Si estás trabajando con una base de código Angular más antigua que aún no ha adoptado `input()`.
- Para acceder a otros datos de la ruta: `ActivatedRoute` no solo proporciona `paramMap`, sino también `queryParamMap` (parámetros de consulta), `fragment` (fragmentos de URL), `data` (datos estáticos definidos en la ruta) y `url` (segmentos de URL). Si necesitas acceder a cualquiera de estos, `ActivatedRoute` es la herramienta.
- **Escenarios complejos de enrutamiento**: Para escenarios donde la lógica de la ruta es muy dinámica o cuando se interactúa con rutas hijas y padres de formas específicas.

Para la mayoría de los casos de uso donde solo necesitas un parámetro de ruta simple, la función `input()` es más limpia y moderna en Angular 20+.

## 3. Refactorizando el Frontend de Ofertas Inmobiliarias

### 3.1. Crear el `PropertyService`

#### 3.1.1. Genera el servicio `property`

- Abre tu terminal en la raíz de tu proyecto `real-estate-frontend`.
- Ejecuta el comando para generar un servicio dentro de la carpeta `src/app/features/properties/services/`:

  ```Terminal
  ng generate service features/properties/services/property
  ```

- Esto creará `src/app/features/properties/services/property.service.ts` y `src/app/features/properties/services/property.service.spec.ts`.

#### 3.1.2. Implementa la lógica en `src/app/features/properties/services/property.service.ts`

- Mueve tu array de datos simulados de propiedades a este servicio.
- Asegúrate de que `propertiesData` sea una Signal (`signal<Property[]>`).
- Crea un método `getAllProperties()` que devuelva una `Promise<Property[]>` (simulando una llamada asíncrona).
- Crea un método `getPropertyById(id: number)` que devuelva una `Promise<Property | undefined>`.
- **Asegúrate de que los datos simulados coincidan con la nueva estructura del modelo `Property`**.

```Typescript
import { Injectable, signal } from '@angular/core';
import { Property } from '../models/property.model'; // Asegúrate de que la ruta sea correcta

@Injectable({
  providedIn: 'root' // Este servicio será un singleton en toda la aplicación
})
export class PropertyService {
  // Los datos de las propiedades se gestionan internamente como una Signal
  private propertiesData = signal<Property[]>([
    {
      id: 1,
      address: 'Calle 10 # 15-20',
      city: 'Bogotá',
      price: 250000,
      bedrooms: 3,
      bathrooms: 2,
      imageUrl: 'https://placehold.co/600x400/E0E0E0/333333?text=Apartamento+Bogota',
      description: 'Moderno apartamento con 3 habitaciones, 2 baños y balcón con vista a la ciudad. Cerca de transporte público y comercios.'
    },
    {
      id: 2,
      address: 'Carrera 50 # 25-10',
      city: 'Medellín',
      price: 450000,
      bedrooms: 4,
      bathrooms: 3,
      imageUrl: 'https://placehold.co/600x400/D0D0D0/444444?text=Casa+Medellin',
      description: 'Hermosa casa de 4 habitaciones, 3 baños, amplio jardín y piscina. Ideal para familias. Ubicada en un barrio tranquilo.'
    },
    {
      id: 3,
      address: 'Avenida 3N # 40-50',
      city: 'Cali',
      price: 120000,
      bedrooms: 0,
      bathrooms: 0,
      imageUrl: 'https://placehold.co/600x400/C0C0C0/555555?text=Lote+Cali',
      description: 'Terreno plano de 500m² con excelente ubicación. Ideal para desarrollo residencial o comercial. Todos los servicios.'
    },
    {
      id: 4,
      address: 'Calle 72 # 58-30',
      city: 'Barranquilla',
      price: 180000,
      bedrooms: 0,
      bathrooms: 1,
      imageUrl: 'https://placehold.co/600x400/B0B0B0/666666?text=Oficina+Barranquilla',
      description: 'Espacio de oficina moderno con vistas panorámicas de la ciudad. Ideal para startups o empresas en crecimiento. Conexión a internet de alta velocidad.'
    },
    {
      id: 5,
      address: 'Bocagrande, Carrera 1 # 10-100',
      city: 'Cartagena',
      price: 320000,
      bedrooms: 2,
      bathrooms: 2,
      imageUrl: 'https://placehold.co/600x400/A0A0A0/777777?text=Apartamento+Cartagena',
      description: 'Acogedor apartamento de 2 habitaciones con impresionantes vistas al océano. Acceso directo a la playa y áreas comunes.'
    }
  ]);

  // Exponer las propiedades como una Signal de solo lectura
  properties = this.propertiesData.asReadonly();

  constructor() { }

  /**
   * Simula la obtención de todas las propiedades de una API.
  * @returns Una Promesa que resuelve con un array de Property.
  */
  getAllProperties(): Promise<Property[]> {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve(this.propertiesData()); // Devuelve el valor actual de la Signal
      }, 500); // Simula un retardo de red
    });
  }

  /**
   * Simula la obtención de una propiedad específica por su ID.
  * @param id El ID de la propiedad a buscar.
  * @returns Una Promesa que resuelve con la Property encontrada o undefined si no existe.
  */
  getPropertyById(id: number): Promise<Property | undefined> {
    return new Promise(resolve => {
      setTimeout(() => {
        const property = this.propertiesData().find(p => p.id === id);
        resolve(property);
      }, 300); // Simula un retardo de red
    });
  }

  // Puedes añadir más métodos para añadir, actualizar o eliminar propiedades si es necesario
  // addProperty(property: Property): void { ... }
  // updateProperty(property: Property): void { ... }
}
```

### 3.2. Configurar el Enrutamiento para `real-estate-frontend` con Feature Routes

#### 3.2.1. Crea el archivo de rutas de la característica `src/app/features/properties/properties.routes.ts`

Este archivo contendrá las rutas específicas de la funcionalidad de propiedades.

```Typescript
import { Routes } from '@angular/router';
import { PropertyList } from './components/property-list/property-list';
import { PropertyDetail } from './components/property-detail/property-detail';

export const PROPERTIES_ROUTES: Routes = [
  { path: '', component: PropertyList }, // Ruta base para /properties
  { path: ':id', component: PropertyDetail }, // Ruta para /properties/:id
];
```

#### 3.2.2. Modifica el archivo de rutas principal `src/app/app.routes.ts` para usar `loadChildren`

Ahora, el path `properties` cargará perezosamente las rutas definidas en `properties.routes.ts`.

```Typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', redirectTo: '/properties', pathMatch: 'full' }, // Redirige la raíz a la lista de propiedades
  {
    path: 'properties', // Cuando la URL sea /properties, carga perezosamente las rutas de propiedades
    loadChildren: () => import('./features/properties/properties.routes').then(m => m.PROPERTIES_ROUTES)
  },
  { path: '**', redirectTo: '/properties' } // Ruta comodín para cualquier otra URL no definida
];
```

#### 3.2.3. Configura `src/app/app.config.ts` para usar el enrutador y `withComponentInputBinding()`

Esta configuración no cambia, ya que `provideRouter` maneja tanto la carga eager como la perezosa.

```Typescript
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router'; // Importa withComponentInputBinding

import { routes } from './app.routes'; // Importa tus rutas definidas

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withComponentInputBinding()) // Habilita el binding de inputs para parámetros de ruta
  ]
};
```

### 3.3. Crear el Componente `PropertyDetail`

Este componente mostrará los detalles de una propiedad específica, recibiendo el ID vía `input()`.

#### 3.3.1. Genera el componente `property-detail`

- Abre tu terminal en la raíz de tu proyecto `real-estate-frontend`.
- Ejecuta el comando:

  ```Typescript
  ng generate component features/properties/components/property-detail
  ```

- Asegúrate de que los archivos se creen en la estructura deseada (ej., `property-detail.ts`, `property-detail.html`, `property-detail.css`).

#### 3.3.2. Implementa la lógica en `src/app/features/properties/components/property-detail/property-detail.ts`

- Importa `input`, `signal`, `effect`, `Router`.
- Importa `Property` y `PropertyService`.
- Define `propertyId = input<string | undefined>()` para recibir el parámetro.
- Inyecta `PropertyService` y `Router`.
- Usa un `effect` para cargar los detalles de la propiedad cuando `propertyId` cambie (usando el `input()`).
- Usa Signals para `propertyDetails`, `loading` y `error`.
- Implementa un método `goBack()` para navegar de vuelta a la lista.

```Typescript
import { Component, OnInit, input, signal, effect, OnDestroy } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Router } from '@angular/router';
import { Property } from '../../models/property.model';
import { PropertyService } from '../../services/property.service';

@Component({
  selector: 'app-property-detail',
  imports: [CommonModule],
  templateUrl: './property-detail.html',
  styleUrl: './property-detail.css'
})
export class PropertyDetail {
  // --- Forma moderna (Angular 20+): Recibir el ID de la ruta como un input (RECOMENDADO) ---
  // El nombre 'id' debe coincidir con el parámetro de ruta definido en properties.routes.ts (:id).
  // Requiere `withComponentInputBinding()` en app.config.ts.
  propertyId = input<string | undefined>();
  numericPropertyId = signal<number | undefined>(undefined);

  propertyDetails = signal<Property | undefined>(undefined); // Signal para los detalles de la propiedad

  loading = signal<boolean>(true); // Signal para el estado de carga
  error = signal<string | null>(null); // Signal para mensajes de error

constructor(
    private router: Router, 
    private propertyService: PropertyService,
  ) {
    // --- Uso de 'input()' para reaccionar al cambio del ID de la ruta ---
    // Este efecto se ejecutará cada vez que el valor de `propertyId` cambie.
    effect(() => {
      const idString = this.propertyId();
      if (idString) {
        const idNum = parseInt(idString, 10);
        if (!isNaN(idNum)) {
          this.numericPropertyId.set(idNum); // Actualiza la Signal numérica
          this.loadPropertyDetails(idNum); // Carga los detalles usando el ID del input
        } else {
          this.error.set('ID de propiedad inválido.');
          this.loading.set(false);
        }
      } else {
        this.error.set('ID de propiedad no proporcionado.');
        this.loading.set(false);
      }
    }, { allowSignalWrites: true }); // Permitir escrituras de Signals dentro de este efecto (para numericPropertyId, error, loading)
  }

  private loadPropertyDetails(id: number): void {
    this.loading.set(true);
    this.error.set(null);
    this.propertyDetails.set(undefined);

    this.propertyService.getPropertyById(id)
      .then(property => {
        this.propertyDetails.set(property);
        this.loading.set(false);
        if (!property) {
          this.error.set('Propiedad no encontrada.');
        }
      })
      .catch(err => {
        console.error('Error al cargar los detalles de la propiedad:', err);
        this.error.set('Error al cargar los detalles de la propiedad.');
        this.loading.set(false);
      });
  }

  /**
   * Navega de vuelta a la lista de propiedades.
  */
  goBack(): void {
    this.router.navigate(['/properties']); // Navegación programática
  }
}
```

#### 3.3.3. Diseña la plantilla `src/app/features/properties/components/property-detail/property-detail.html` con Tailwind CSS

- Muestra un mensaje de carga o error.
- Si la propiedad está disponible, muestra todos sus detalles.
- Añade un botón "Volver a la lista de propiedades".
- **Asegúrate de que los campos mostrados (`address`, `city`) coincidan con el nuevo modelo y elimina los campos que ya no existen**.

```HTML
<div class="container mx-auto p-8 bg-gray-50 min-h-screen flex flex-col items-center">
  <div class="w-full max-w-4xl bg-white rounded-xl shadow-lg p-8">
    <button
      (click)="goBack()"
      class="mb-6 flex items-center px-4 py-2 bg-gray-200 text-gray-800 rounded-lg hover:bg-gray-300 transition-colors duration-300 font-semibold"
    >
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
        <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
      </svg>
      Volver a la lista de propiedades
    </button>

    @if (loading()) {
      <div class="flex justify-center items-center h-64">
        <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
        <p class="ml-4 text-2xl text-blue-600">Cargando detalles de la propiedad...</p>
      </div>
    } @else if (error()) {
      <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
        <p class="font-bold mb-2">¡Error!</p>
        <p>{{ error() }}</p>
      </div>
    } @else if (propertyDetails()) {
      <div class="flex flex-col md:flex-row gap-8">
        <div class="md:w-1/2">
          <img
            [src]="propertyDetails()!.imageUrl"
            [alt]="'Imagen de ' + propertyDetails()!.address"
            class="w-full h-auto rounded-xl shadow-md object-cover"
            onerror="this.onerror=null;this.src='https://placehold.co/600x450/CCCCCC/666666?text=Property+Detail';"
          />
        </div>
        <div class="md:w-1/2">
          <h1 class="text-4xl font-extrabold text-gray-900 mb-4">{{ propertyDetails()!.address }}</h1>
          <p class="text-gray-600 text-xl mb-6">{{ propertyDetails()!.city }}</p>
          <p class="text-gray-700 text-lg mb-4">{{ propertyDetails()!.description }}</p>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
            <div class="bg-blue-50 p-4 rounded-lg shadow-sm">
              <p class="text-blue-800 font-semibold">Precio:</p>
              <p class="text-blue-700 text-lg">${{ propertyDetails()!.price | number }}</p>
            </div>
            <div class="bg-green-50 p-4 rounded-lg shadow-sm">
              <p class="text-green-800 font-semibold">Habitaciones:</p>
              <p class="text-green-700 text-lg">{{ propertyDetails()!.bedrooms }}</p>
            </div>
            <div class="bg-red-50 p-4 rounded-lg shadow-sm">
              <p class="text-red-800 font-semibold">Baños:</p>
              <p class="text-red-700 text-lg">{{ propertyDetails()!.bathrooms }}</p>
            </div>
          </div>
        </div>
      </div>
    } @else {
      <div class="text-center p-8 text-2xl text-gray-500 bg-gray-100 border border-gray-300 rounded-lg shadow-md">
        <p>No se pudieron cargar los detalles de la propiedad.</p>
      </div>
    }
  </div>
</div>
```

### 3.4. Actualizar el Componente `PropertyList`

#### 3.4.1. Actualiza el componente

Este componente (o el componente que lista las propiedades) debe ser actualizado para usar el `PropertyService` y para navegar a la página de detalles.

1. Abre `src/app/features/properties/components/property-list/property-list.ts` (o el nombre de tu componente de lista).
2. Importa `PropertyService` y `Router`.
3. Inyecta `PropertyService` y `Router` en el constructor.
4. Elimina los datos simulados locales del componente.
5. Modifica `ngOnInit` para usar `this.propertyService.getAllProperties()`.
6. Añade un método `handleViewPropertyDetails(propertyId: number)` que use `this.router.navigate(['/properties', propertyId])`.
7. Cambia `filterLocation` a `filterCity` para que coincida con el nuevo modelo.

```Typescript
import { Component, OnInit, signal, computed, effect } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Property } from '../../models/property.model';
// Importa el servicio y el enrutador
import { PropertyService } from '../../services/property.service';
import { Router } from '@angular/router';
 import { PropertyCard } from '../property-card/property-card';

@Component({
  selector: 'app-property-list',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule,
    PropertyCard
  ],
  templateUrl: './property-list.html',
  styleUrl: './property-list.css'
})
export class PropertyList implements OnInit {
  // filteredProperties es una computed Signal que depende de properties del servicio
  filterCity = signal<string>(''); // Cambiado de filterLocation a filterCity
  loading = signal<boolean>(true);
  error = signal<string | null>(null);

  // Accede a las propiedades directamente desde el servicio
  filteredProperties = computed(() => {
    const currentFilter = this.filterCity().toLowerCase(); // Usa filterCity
    const allProperties = this.propertyService.properties(); // Accede a la Signal de propiedades del servicio
    if (!currentFilter) {
      return allProperties;
    } else {
      return allProperties.filter(property =>
        property.city.toLowerCase().includes(currentFilter) // Filtra por city
      );
    }
  });

  constructor(
    private propertyService: PropertyService, // Inyecta el PropertyService
    private router: Router // Inyecta el Router para navegación
  ) {
    effect(() => {
      console.log(`El filtro de ciudad ha cambiado a: "${this.filterCity()}"`); // Usa filterCity
    });

    // Observa los cambios en la Signal 'properties' del servicio para actualizar el estado de carga/error
    effect(() => {
      if (this.propertyService.properties().length > 0) {
        this.loading.set(false);
        this.error.set(null);
      }
    }, { allowSignalWrites: true });
  }

  ngOnInit(): void {
    this.loading.set(true);
    this.error.set(null);

    // Carga las propiedades usando el servicio
    this.propertyService.getAllProperties()
      .then(properties => {
        // La Signal 'properties' del servicio se actualiza internamente,
        // y filteredProperties reaccionará a eso.
        this.loading.set(false);
      })
      .catch(err => {
        console.error('Error al cargar las propiedades:', err);
        this.error.set('Error al cargar las propiedades desde el servicio.');
        this.loading.set(false);
      });
  }

  onFilterInputChange(event: Event): void {
    const inputElement = event.target as HTMLInputElement;
    this.filterCity.set(inputElement.value); // Usa filterCity
  }

  /**

- Navega a la página de detalles de una propiedad.
- @param propertyId El ID de la propiedad a ver.
   */
  handleViewPropertyDetails(propertyId: number): void {
    console.log(`Navegando a detalles de la propiedad con ID: ${propertyId}`);
    this.router.navigate(['/properties', propertyId]); // Navegación programática a la ruta de detalles
  }
}
```

#### 3.4.2. Actualiza la plantilla `src/app/features/properties/components/property-list/property-list.html`

- Añade un botón o un enlace en cada tarjeta de propiedad para "Ver Detalles" que llame a `handleViewPropertyDetails(property.id)`.
- Asegúrate de que los campos mostrados (`address`, `city`) y el campo de filtro (`filterCity`) coincidan con el nuevo modelo.

```HTML
<div class="container mx-auto p-8 bg-gray-50 min-h-screen">
  <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12">Propiedades Disponibles</h1>

  <div class="mb-8 flex justify-center">
    <input
      type="text"
      placeholder="Filtrar por ciudad..."
      [ngModel]="filterCity()"
      (ngModelChange)="filterCity.set($event)"
      class="p-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 w-full max-w-md text-lg"
    >
  </div>

  @if (loading()) {
    <div class="flex justify-center items-center h-64">
      <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
      <p class="ml-4 text-2xl text-blue-600">Cargando propiedades...</p>
    </div>
  } @else if (error()) {
    <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
      <p class="font-bold mb-2">¡Error!</p>
      <p>{{ error() }}</p>
    </div>
  } @else if (filteredProperties().length === 0) {
    <div class="text-center p-8 text-2xl text-gray-500 bg-gray-100 border border-gray-300 rounded-lg shadow-md">
      <p>No se encontraron propiedades que coincidan con el filtro.</p>
    </div>
  } @else {
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      @for (property of filteredProperties(); track property.id) {
        <div class="bg-white rounded-xl shadow-lg overflow-hidden flex flex-col transform transition-transform duration-300 hover:scale-105">
          <img
            [src]="property.imageUrl"
            [alt]="'Imagen de ' + property.address"
            class="w-full h-48 object-cover"
            onerror="this.onerror=null;this.src='<https://placehold.co/600x400/CCCCCC/666666?text=Property>';"
          />
          <div class="p-6 flex flex-col flex-grow">
            <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ property.address }}</h2>
            <p class="text-gray-600 text-lg mb-3">{{ property.city }}</p>
            <p class="text-blue-600 font-extrabold text-3xl mb-4">${{ property.price | number }}</p>

            <div class="flex items-center text-gray-700 text-base mb-4">
              <span class="flex items-center mr-4">
                <svg class="w-5 h-5 mr-1 text-gray-500" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M10 2a3 3 0 00-3 3v.586l-1.707 1.707A1 1 0 005 9v5a2 2 0 002 2h6a2 2 0 002-2V9a1 1 0 00-.293-.707L13 5.586V5a3 3 0 00-3-3zm-1 3a1 1 0 012 0v.586l1.707 1.707A1 1 0 0113 9v5a1 1 0 01-1 1H8a1 1 0 01-1-1V9a1 1 0 01.293-.707L9 5.586V5z"></path></svg>
                {{ property.bedrooms }} Habitaciones
              </span>
              <span class="flex items-center">
                <svg class="w-5 h-5 mr-1 text-gray-500" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M10 2a3 3 0 00-3 3v.586l-1.707 1.707A1 1 0 005 9v5a2 2 0 002 2h6a2 2 0 002-2V9a1 1 0 00-.293-.707L13 5.586V5a3 3 0 00-3-3zm-1 3a1 1 0 012 0v.586l1.707 1.707A1 1 0 0113 9v5a1 1 0 01-1 1H8a1 1 0 01-1-1V9a1 1 0 01.293-.707L9 5.586V5z"></path></svg>
                {{ property.bathrooms }} Baños
              </span>
            </div>

            <p class="text-gray-700 text-sm flex-grow mb-4">{{ property.description.substring(0, 100) }}...</p>

            <div class="mt-auto">
              <button
                (click)="handleViewPropertyDetails(property.id)"
                class="w-full bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md"
              >
                Ver Detalles
              </button>
            </div>
          </div>
        </div>
      }
    </div>
  }
</div>
```

### 3.5. Actualizar `App` en `real-estate-frontend`

1. Abre `src/app/app.ts`.
2. Asegúrate de que `RouterOutlet` esté importado y añadido al array `imports`.
3. Abre `src/app/app.html`.
4. Reemplaza el selector de tu lista de propiedades (ej., `<app-property-list></app-property-list>`) con `<router-outlet></router-outlet>`. El enrutador se encargará de cargar PropertyListComponent cuando la ruta sea /properties.

```Typescript
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router'; 
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [
    RouterOutlet // RouterOutlet es necesario para que las rutas funcionen
  ],
  templateUrl: './app.html',
  styleUrl: './app.css'
})
export class AppComponent {
  title = 'real-estate-frontend';
}
```

```HTML
<!-- Aquí se renderizarán los componentes de las rutas -->
<router-outlet></router-outlet>
```

### 3.6. Verificar y Probar `real-estate-frontend`

1. Asegúrate de que tu servidor de desarrollo de Angular esté corriendo (`ng serve`).
2. Abre tu navegador en `http://localhost:4200/`.
3. Deberías ver la lista de propiedades.
4. Prueba el filtro por ciudad.
5. Haz clic en el botón "Ver Detalles". Deberías ser redirigido a una nueva vista que muestra los detalles de la propiedad seleccionada.
6. Usa el botón "Volver a la lista de propiedades" en la vista de detalles.
7. Intenta acceder directamente a una URL como `http://localhost:4200/properties/1` para ver los detalles de la propiedad con ID 1.
