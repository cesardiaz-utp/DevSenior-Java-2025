# Preguntas de Entrevista Técnica: Modulo 8 - Angular Moderno – Interfaces Dinámicas para Aplicaciones Full Stack

## Sección 1: Introducción a Angular, CLI, Componentes y Estructura Moderna

### 1. ¿Qué es una Single Page Application (SPA) y cuáles son sus principales ventajas en el desarrollo web moderno?

Una Single Page Application (SPA) es una aplicación web que carga una única página HTML y actualiza dinámicamente el contenido a medida que el usuario interactúa con ella, sin necesidad de recargar la página completa.

**Ventajas**:

- **Experiencia de Usuario (UX) Fluida**: Se siente más como una aplicación de escritorio, con transiciones rápidas y sin recargas completas.
- **Rendimiento Mejorado**: Después de la carga inicial, solo se intercambian datos, no la página completa, lo que reduce el tráfico de red.
- **Desarrollo Simplificado**: Permite una clara separación entre el frontend (Angular) y el backend (API), facilitando el desarrollo paralelo.

### 2. ¿Cuál es el rol principal de Angular CLI al iniciar y desarrollar un proyecto Angular?

Angular CLI (Command Line Interface) es una herramienta de línea de comandos que simplifica el desarrollo de Angular. Su rol principal es:

- **Generación de Proyectos**: Permite crear un nuevo proyecto Angular con una estructura base y configuraciones predeterminadas (`ng new`).
- **Generación de Elementos**: Genera componentes, servicios, módulos, directivas, pipes, etc., con el boilerplate necesario (`ng generate component`, `ng generate service`, etc.).
- **Servidor de Desarrollo**: Proporciona un servidor local con recarga en caliente (`ng serve`) para ver los cambios en tiempo real.
- **Construcción y Optimización**: Compila y optimiza la aplicación para producción (`ng build`), incluyendo `tree-shaking` y minificación.
- **Ejecución de Pruebas**: Permite ejecutar pruebas unitarias y de extremo a extremo.

### 3. ¿Cómo se crea un "Standalone Component" en Angular y cuál es una de sus principales características?

Se puede crear un "Standalone Component" usando Angular CLI:

```Terminal
ng generate component my-standalone-component 
```

Una de sus principales características es que **no requiere ser declarado en un `NgModule`**. En su lugar, importa directamente sus dependencias (otros componentes, directivas, pipes o módulos de Angular) a través de la propiedad `imports` en su decorador `@Component`.

### 4. ¿Para qué se utiliza el lifecycle hook `ngOnInit` en un componente Angular?

`ngOnInit` es un _lifecycle hook_ que se ejecuta una vez después de que Angular ha inicializado las propiedades de entrada (`@Input()`) del componente y ha realizado la primera ronda de detección de cambios. Se utiliza comúnmente para:

- Inicializar datos.
- Realizar llamadas a servicios para obtener datos iniciales.
- Configurar suscripciones a observables.

### 5. ¿Cuáles son los beneficios de usar Tailwind CSS para el estilado en un proyecto Angular?

- **Desarrollo Rápido**: Permite estilizar directamente en el HTML con clases de utilidad, reduciendo el cambio de contexto entre archivos.
- **Consistencia de Diseño**: Fomenta un diseño uniforme al usar un conjunto predefinido de utilidades.
- **Tamaño de CSS Reducido**: Purga las clases no utilizadas en producción, resultando en archivos CSS muy pequeños.
- **Diseño Responsivo Integrado**: Facilita la creación de layouts responsivos con prefijos de clases (ej., `md:`, `lg:`).
- **No más nombres de clases**: Elimina la necesidad de inventar y mantener nombres de clases semánticos.

### 6. ¿Por qué es importante aplicar TypeScript en proyectos Angular?

TypeScript es un superconjunto de JavaScript que añade tipado estático. Su importancia en Angular radica en:

- **Detección de Errores Temprana**: Los errores relacionados con tipos se detectan en tiempo de compilación (antes de ejecutar el código), no en tiempo de ejecución, lo que facilita la depuración.
- **Mejor Mantenibilidad**: El código tipado es más fácil de entender y mantener, especialmente en proyectos grandes y con múltiples desarrolladores.
- **Autocompletado y Refactorización**: Mejora la experiencia de desarrollo al proporcionar autocompletado inteligente, navegación de código y refactorización más segura en IDEs.
- **Claridad del Código**: Los tipos hacen explícitas las expectativas sobre los datos, mejorando la legibilidad.

### 7. Describe una estructura de carpetas profesional para un proyecto Angular grande utilizando componentes standalone

Una estructura profesional se basa en la modularidad por características (_feature-driven_):

```Text
src/
├── app/
│   ├── app.config.ts           # Configuración global (providers, rutas raíz)
│   ├── app.ts                  # Componente raíz de la aplicación (layout principal)
│   ├── app.routes.ts           # Rutas principales con lazy loading de características
│   │
│   ├── core/                   # Servicios y componentes singleton (autenticación, manejo de errores, etc.)
│   │   ├── services/
│   │   └── guards/
│   │
│   ├── features/               # Carpeta para las características principales de la aplicación
│   │   ├── users/              # Característica de usuarios
│   │   │   ├── components/
│   │   │   ├── models/
│   │   │   ├── services/
│   │   │   └── users.routes.ts
│   │   ├── courses/            # Característica de cursos
│   │   │   ├── components/
│   │   │   ├── models/
│   │   │   ├── services/
│   │   │   └── courses.routes.ts
│   │   └── ... (otras características)
│   │
│   └── shared/                 # Componentes, directivas, pipes, modelos reutilizables
│       ├── components/
│       ├── pipes/
│       ├── directives/
│       └── models/
│
├── assets/                     # Archivos estáticos (imágenes, iconos)
├── environments/               # Configuraciones de entorno
└── main.ts                     # Punto de entrada de la aplicación
```

## Sección 2: Reactividad con Signals y Comunicación entre Componentes

### 8. Menciona tres _lifecycle hooks_ comunes en Angular y describe brevemente su propósito

1. `ngOnInit`: Se ejecuta una vez después de que Angular ha inicializado las propiedades de entrada del componente. Ideal para inicialización de datos y llamadas a servicios.
2. `ngOnChanges`: Se ejecuta cuando Angular detecta cambios en las propiedades de entrada (`@Input()`) del componente. Recibe un objeto `SimpleChanges` con los cambios actuales y anteriores.
3. `ngOnDestroy`: Se ejecuta justo antes de que Angular destruya el componente. Ideal para tareas de limpieza, como desuscribirse de observables para evitar fugas de memoria.

### 9. ¿Qué son las "Signals" en Angular y cómo mejoran la reactividad?

Las "Signals" son un nuevo sistema de reactividad en Angular que envuelven un valor y notifican a los consumidores cuando ese valor cambia. Mejoran la reactividad al permitir una **detección de cambios granular**. Cuando una Signal se actualiza, Angular sabe exactamente qué partes de la interfaz de usuario dependen de esa Signal y solo actualiza esas partes, en lugar de ejecutar una detección de cambios completa en todo el árbol de componentes, lo que optimiza el rendimiento.

### 10. Proporciona un ejemplo de cómo usar `signal()`, `computed()` y `effect()` juntos

```Typescript
import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-signal-example',
  standalone: true,
  template: `
    <p>Contador: {{ count() }}</p>
    <p>Doble del contador: {{ doubleCount() }}</p>
    <button (click)="increment()">Incrementar</button>
  `,
})
export class SignalExampleComponent {
  // signal(): Crea una Signal básica
  count = signal(0);

  // computed(): Crea una Signal que depende de otras Signals
  doubleCount = computed(() => this.count() * 2);

  constructor() {
    // effect(): Ejecuta un efecto secundario cuando una Signal cambia
    effect(() => {
      console.log(`El contador actual es: ${this.count()}`);
    });
  }

  increment() {
    this.count.update(currentCount => currentCount + 1); // Actualiza la Signal
  }
}
```

### 11. ¿Cómo se comunican datos de un componente padre a un componente hijo en Angular, utilizando tanto `@Input()` como la nueva función `input()`?

#### Usando `@Input()` (tradicional)

- **Hijo**:

  ```Typescript
  import { Component, Input } from '@angular/core';
  
  @Component({ /* ... */ })
  export class ChildComponent {
    @Input() message: string = '';
  }
  ```

- **Padre**:

  ```HTML
  <app-child message="'Hola desde el padre'"></app-child>
  ```

#### Usando `input()` (Angular 16+)

- **Hijo**:

  ```Typescript
  import { Component, input } from '@angular/core';

  @Component({ /* ... */ })
  export class ChildComponent {
    message = input<string>(''); // Define una propiedad de entrada reactiva
  }
  ```

- **Padre**:

  ```HTML
  <app-child message="'Hola desde el padre'"></app-child>
  ```

### 12. ¿Cómo se comunican eventos de un componente hijo a un componente padre en Angular, utilizando `@Output()` con `EventEmitter` y la nueva función `output()`?

#### Usando `@Output()` y `EventEmitter` (tradicional)

- **Hijo**:

  ```Typescript
  import { Component, Output, EventEmitter } from '@angular/core';

  @Component({ /* ... */ })
  export class ChildComponent {
    @Output() buttonClicked = new EventEmitter<string>();

    onClick() {
      this.buttonClicked.emit('Botón presionado en el hijo');
    }
  }
  ```

- **Padre**:

  ```HTML
  <app-child (buttonClicked)="handleChildEvent($event)"></app-child>
  ```

  ```Typescript
  handleChildEvent(message: string) {
    console.log(message);
  }
  ```

#### Usando `output()` (Angular 16+)

- **Hijo**:

  ```Typescript
  import { Component, output } from '@angular/core';

  @Component({ /* ... */ })
  export class ChildComponent {
    buttonClicked = output<string>(); // Define una salida de evento

    onClick() {
      this.buttonClicked.emit('Botón presionado en el hijo');
    }
  }
  ```

- **Padre**:

  ```HTML
  <app-child (buttonClicked)="handleChildEvent($event)"></app-child>
  ```

  ```Typescript
  handleChildEvent(message: string) {
    console.log(message);
  }
  ```

## Sección 3: Navegación (Routing) y Servicios

### 13. ¿Cuál es el propósito principal de un "Servicio" en Angular y por qué se utiliza la Inyección de Dependencias para proporcionarlos?

El propósito principal de un "Servicio" en Angular es **centralizar la lógica de negocio, la gestión de datos y las funcionalidades compartidas** que no están directamente relacionadas con la interfaz de usuario. Ayudan a mantener los componentes "delgados" y enfocados en la presentación.

La **Inyección de Dependencias (DI)** se utiliza para proporcionar servicios porque:

- **Reusabilidad**: Permite que los servicios sean reutilizables en múltiples componentes sin tener que crearlos manualmente cada vez.
- **Mantenibilidad**: Facilita la modificación y el mantenimiento de los servicios, ya que los componentes no tienen una dependencia directa y rígida de ellos.
- **Testabilidad**: Hace que el código sea más fácil de probar unitariamente, ya que se pueden "inyectar" versiones simuladas (mocks) de los servicios durante las pruebas.
- **Gestión del Ciclo de Vida**: El sistema de DI de Angular gestiona la creación y destrucción de las instancias de los servicios.

### 14.  ¿Cómo se crea y se registra un servicio en Angular para que esté disponible en toda la aplicación?

1. **Crear el servicio**:

    ```Terminal
    ng generate service services/my-data
    ```

2. **Registrar el servicio**:

    Por defecto, Angular CLI añade `providedIn: 'root'` al decorador `@Injectable()` del servicio. Esto lo registra como un "singleton" a nivel de la aplicación, lo que significa que hay una única instancia del servicio disponible en toda la aplicación.

    ```Typescript
    import { Injectable } from '@angular/core';

    @Injectable({
      providedIn: 'root' // Esto lo registra globalmente
    })
    export class MyDataService {
      // ...
    }
    ```

### 15. Explica la diferencia entre navegación declarativa y navegación programática en Angular Routing

- **Navegación Declarativa (`routerLink`)**: Se realiza directamente en la plantilla HTML utilizando la directiva `routerLink`. Es ideal para enlaces estáticos o cuando la navegación es una acción directa del usuario (ej., un clic en un menú).

    ```HTML
    <a routerLink="/courses">Ver Cursos</a>
    <a [routerLink]="['/users', userId]">Ver Perfil</a>
    ```

- **Navegación Programática (`Router` service)**: Se realiza en el código TypeScript utilizando el servicio `Router`. Es necesaria cuando la navegación depende de alguna lógica de negocio, una condición, o después de una acción asíncrona (ej., después de un login exitoso o el envío de un formulario).

    ```Typescript
    import { Router } from '@angular/router';

    constructor(private router: Router) {}

    navigateToCourses() {
      this.router.navigateByUrl('/courses');
    }

    navigateToUserProfile(userId: string) {
      this.router.navigate(['/users', userId]);
    }
    ```

### 16. ¿Cómo se reciben parámetros de ruta (ej., un ID de usuario en `/users/:id`) en un componente Angular, utilizando tanto la función `input()` (Angular 20+) como `ActivatedRoute` (tradicional)?

#### Usando input() (Angular 20+)

Se define una propiedad de entrada con el mismo nombre que el parámetro de ruta.

```Typescript
import { Routes } from '@angular/router';
import { UserDetailComponent } from './user-detail/user-detail.component';

export const USERS_ROUTES: Routes = [
  { path: ':id', component: UserDetailComponent }
];
```

```Typescript
import { Component, input } from '@angular/core';

@Component({ /* ... */ })
export class UserDetailComponent {
  userId = input<string>('id'); // El nombre 'id' debe coincidir con el parámetro de ruta
  // Ahora puedes usar this.userId() en la plantilla o en el código
}
```

#### Usando `ActivatedRoute` (tradicional)

Se inyecta `ActivatedRoute` y se accede a los parámetros a través de su propiedad `snapshot.paramMap` (para acceso síncrono) o `paramMap` (para un `Observable` reactivo).

```Typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Subscription } from 'rxjs';

@Component({ /* ... */ })
export class UserDetailComponent implements OnInit {
  userId: string | null = null;
  private routeSub: Subscription | undefined;

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    // Acceso síncrono (para rutas que no cambian el parámetro sin recargar el componente)
    this.userId = this.route.snapshot.paramMap.get('id');
    console.log('ID (snapshot):', this.userId);

    // Acceso reactivo (para rutas que pueden cambiar el parámetro sin destruir el componente)
    this.routeSub = this.route.paramMap.subscribe(params => {
      this.userId = params.get('id');
      console.log('ID (observable):', this.userId);
    });
  }

  ngOnDestroy() {
    this.routeSub?.unsubscribe(); // Importante desuscribirse
  }
}
```

### 17. Describe los dos mecanismos principales para configurar el enrutamiento en Angular: tradicional (carga eager) y `lazy loading`. ¿Cuándo se usaría cada uno?

1. **Carga Tradicional (Eager Loading)**:
    - **Mecanismo**: Todas las rutas y sus módulos asociados se cargan al inicio de la aplicación. Esto se configura importando directamente los módulos de rutas en el `app.routes.ts` (o `app-routing.module.ts` en versiones anteriores con NgModules).

    - **Ejemplo**:

      ```Typescript
      import { Routes } from '@angular/router';
      import { HomeComponent } from './home/home.component';
      import { AboutComponent } from './about/about.component';

      export const routes: Routes = [
        { path: '', component: HomeComponent },
        { path: 'about', component: AboutComponent },
      ];
      ```

    - **Cuándo usarlo**: Para aplicaciones pequeñas o secciones de la aplicación que son críticas y deben estar disponibles inmediatamente. La ventaja es que no hay retraso al navegar a estas rutas por primera vez, pero el tiempo de carga inicial de la aplicación puede ser mayor.

2. **Lazy Loading (Carga Perezosa)**:
    - **Mecanismo**: Los módulos de rutas (y sus componentes, servicios, etc.) se cargan solo cuando el usuario navega a una ruta específica asociada a ellos. Esto se logra usando la propiedad `loadComponent` o `loadChildren` en la configuración de la ruta. Angular crea un chunk de JavaScript separado para el módulo cargado perezosamente.

    - **Ejemplo**:

      ```Typescript
      import { Routes } from '@angular/router';

      export const routes: Routes = [
        { path: '', redirectTo: '/home', pathMatch: 'full' },
        { path: 'home', loadComponent: () => import('./home/home.component').then(m => m.HomeComponent) }, // Para Standalone Components
        { path: 'users', loadChildren: () => import('./features/users/users.routes').then(m => m.USERS_ROUTES) }, // Para Feature Modules
        { path: 'courses', loadChildren: () => import('./features/courses/courses.routes').then(m => m.COURSES_ROUTES) },
      ];
      ```

    - **Cuándo usarlo**: Para aplicaciones grandes con muchas funcionalidades, o secciones que no son visitadas frecuentemente. La ventaja principal es que reduce el tiempo de carga inicial de la aplicación, ya que solo se descarga el código necesario para la vista actual, mejorando el rendimiento y la experiencia del usuario.

### 18. ¿Qué es el "Feature Routing" en Angular y cuáles son sus principales beneficios en la organización de una aplicación?

"Feature Routing" (o enrutamiento por característica) es una estrategia de organización del código en Angular donde cada característica principal de la aplicación tiene su propio archivo de configuración de rutas (`.routes.ts` para Standalone Components o un `NgModule` de enrutamiento dedicado para NgModules tradicionales). Estas rutas de características se importan y se cargan perezosamente (o eager, pero lazy es lo común) en el archivo de rutas principal (`app.routes.ts`).

**Beneficios principales**:

- **Modularidad y Organización**: Mantiene las rutas relacionadas con una característica específica agrupadas, facilitando la comprensión y el mantenimiento del código.
- **Carga Perezosa (Lazy Loading)**: Permite implementar fácilmente el lazy loading para cada característica, lo que mejora significativamente el rendimiento inicial de la aplicación al cargar solo el código necesario.
- **Escalabilidad**: A medida que la aplicación crece, añadir nuevas características es más sencillo y no afecta la complejidad del archivo de rutas principal.
- **Desarrollo en Equipo**: Diferentes equipos o desarrolladores pueden trabajar en características separadas sin conflictos en la configuración global de rutas.
- **Reusabilidad**: Las características con su enrutamiento pueden ser más fácilmente reutilizadas en otras partes de la aplicación o incluso en otros proyectos.

## Sección 4: Formularios Reactivos, Gestión de Estado Compleja y Mejores Prácticas

### 19. ¿Cuáles son las tres clases principales que componen la arquitectura de los Formularios Reactivos en Angular y cuál es su función?

1. `FormControl`: Representa un control de formulario individual (ej., un campo de entrada, un checkbox). Gestiona el valor del control, su estado de validación (válido/inválido, tocado/no tocado, sucio/limpio) y sus validadores.
2. `FormGroup`: Agrupa múltiples instancias de `FormControl` (y/o `FormGroup` anidados) en una sola unidad. Gestiona el valor y el estado de validación de todo el grupo, agregando el estado de sus controles hijos.
3. `FormArray`: Agrupa múltiples instancias de `FormControl` o `FormGroup` en un array. Es útil para formularios donde el número de controles es dinámico (ej., una lista de ítems).

### 19. ¿Cómo se crea un validador personalizado síncrono en Angular para un campo de formulario reactivo? Proporciona un ejemplo que valide si un campo no contiene la palabra "admin"

Un validador personalizado es una función que recibe un `AbstractControl` (que puede ser un `FormControl`, `FormGroup` o `FormArray`) y devuelve `null` si es válido, o un objeto de error si es inválido.

```Typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';

// Validador personalizado
export function noAdminValidator(): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const value = control.value as string;
    if (value && value.toLowerCase().includes('admin')) {
      return { noAdmin: true }; // Retorna un objeto de error
    }
    return null; // Retorna null si es válido
  };
}

// Uso en un componente:
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { noAdminValidator } from './validators/no-admin.validator'; // Suponiendo que el validador está en un archivo separado

// ... en tu componente
constructor(private fb: FormBuilder) {}

myForm: FormGroup = this.fb.group({
  username: ['', [Validators.required, noAdminValidator()]]
});
```

### 20. ¿Cómo se gestiona el estado complejo de la aplicación utilizando servicios y Signals para promover una "fuente única de verdad" y la inmutabilidad?

- **Servicios como Fuente Única de Verdad**: Se crea un servicio (ej., `CoursesStoreService`) que es el único responsable de mantener el estado de una parte específica de la aplicación (ej., la lista de cursos).
- **Signals para el Estado**: Dentro de este servicio, el estado se almacena en `Signals` (ej., `courses = signal<Course[]>([]);`). Las Signals son ideales porque son reactivas y permiten actualizaciones inmutables.
- **Inmutabilidad**: Las operaciones que modifican el estado (ej., añadir, actualizar, eliminar un curso) deben crear nuevas referencias de los datos. Por ejemplo, al añadir un curso, se usa `this.courses.update(current => [...current, newCourse])` para crear un nuevo array, en lugar de mutar el array existente.
- **Acceso de Solo Lectura**: Los componentes consumen el estado a través de la Signal de solo lectura (ej., `courses()`), y las modificaciones se realizan a través de métodos públicos del servicio, asegurando que el estado solo se modifique de forma controlada.

### 21. Cuáles son las principales alternativas para persistir datos del lado del navegador y cuáles son sus características generales?

Existen varias opciones para almacenar datos del lado del cliente en el navegador, cada una con sus propias características, casos de uso y limitaciones:

1. `localStorage`:
    - **Persistencia**: Los datos persisten incluso después de cerrar el navegador o la pestaña. No tienen fecha de expiración.
    - **Alcance**: Los datos son accesibles solo dentro del mismo origen (dominio, protocolo y puerto).
    - **Capacidad**: Mayor capacidad de almacenamiento (típicamente 5-10 MB).
    - **Acceso**: Se accede a través de la interfaz `Window.localStorage`.
    - **Tipo de datos**: Almacena solo cadenas de texto. Los objetos deben ser serializados a JSON (`JSON.stringify()`) y deserializados (`JSON.parse()`).
    - **Seguridad**: Vulnerable a ataques XSS (Cross-Site Scripting), ya que cualquier JavaScript en la página puede leer y modificar los datos. No debe usarse para datos sensibles como tokens de autenticación.
2. `sessionStorage`:
    - **Persistencia**: Los datos persisten solo durante la sesión de la pestaña del navegador. Se eliminan cuando la pestaña o el navegador se cierran.
    - **Alcance**: Los datos son accesibles solo dentro de la misma pestaña y el mismo origen. Si se abre una nueva pestaña con la misma URL, tendrá su propio `sessionStorage` independiente.
    - **Capacidad**: Similar a `localStorage` (típicamente 5-10 MB).
    - **Acceso**: Se accede a través de la interfaz `Window.sessionStorage`.
    - **Tipo de datos**: Almacena solo cadenas de texto, similar a `localStorage`.
    - **Seguridad**: También vulnerable a ataques XSS.
3. `cookies`:
    - **Persistencia**: Los datos pueden tener una fecha de expiración (persistentes) o ser de sesión (se eliminan al cerrar el navegador).
    - **Alcance**: Pueden ser configuradas para un dominio específico, subdominios, y rutas. Se envían automáticamente con cada solicitud HTTP al servidor para el dominio correspondiente.
    - **Capacidad**: Muy pequeña (típicamente 4 KB por dominio).
    - **Acceso**: Se accede a través de `document.cookie` en JavaScript, o directamente por el servidor en las cabeceras HTTP.
    - **Tipo de datos**: Almacena solo cadenas de texto simples.
    - **Seguridad**:
      - **`HttpOnly` flag**: Si se establece, la cookie no puede ser accedida por JavaScript, mitigando ataques XSS.
      - **`Secure` flag**: La cookie solo se envía a través de conexiones HTTPS, protegiéndola de interceptaciones en tránsito.
      - **`SameSite` attribute**: Controla cuándo las cookies se envían en solicitudes de origen cruzado, mitigando ataques CSRF (Cross-Site Request Forgery).
      - A pesar de las mejoras de seguridad, aún pueden ser vulnerables si no se configuran correctamente, especialmente para datos sensibles.

### 22. ¿Cuándo sería el caso de uso ideal para `localStorage` y cuáles son sus principales implicaciones de seguridad?

- **Caso de Uso Ideal**:
  - Almacenar preferencias de usuario (tema oscuro/claro, idioma).
  - Guardar el estado de UI no crítico (ej., si un panel está colapsado o expandido).
  - Caché de datos pequeños y no sensibles que no cambian con frecuencia y que mejoran la experiencia de usuario al evitar llamadas repetidas al servidor (ej., una lista estática de categorías).
  - Mantener un carrito de compras simple antes de que el usuario inicie sesión.
- **Implicaciones de Seguridad**:
  - **Vulnerabilidad a XSS**: La principal preocupación es que `localStorage` es directamente accesible a través de JavaScript. Si un atacante logra inyectar código malicioso (XSS) en tu página, puede leer, modificar o eliminar cualquier dato almacenado en `localStorage`. Por esta razón, nunca se deben almacenar datos sensibles como tokens de autenticación (JWTs), contraseñas, o información personal confidencial directamente en `localStorage`.
  - **No se envía con peticiones HTTP**: A diferencia de las `cookies`, los datos en `localStorage` no se envían automáticamente con cada petición HTTP al servidor, lo que significa que no son directamente útiles para la autenticación basada en sesiones que depende de cookies.

### 23. ¿Cuál es la principal diferencia entre `localStorage` y `sessionStorage` en términos de persistencia y alcance? Proporciona un ejemplo de cuándo usarías `sessionStorage`

- **Persistencia**:
  - `localStorage`: Los datos persisten indefinidamente hasta que son explícitamente eliminados por la aplicación o el usuario. Sobreviven al cierre del navegador y a las recargas de página.
  - `sessionStorage`: Los datos solo persisten durante la vida útil de la pestaña o ventana del navegador. Se eliminan automáticamente cuando la pestaña se cierra (incluso si el navegador sigue abierto).
- **Alcance**:
  - `localStorage`: Los datos son compartidos entre todas las pestañas y ventanas del mismo origen (dominio, protocolo y puerto).
  - `sessionStorage`: Los datos son aislados por cada pestaña o ventana. Si abres la misma URL en una nueva pestaña, esa nueva pestaña tendrá su propio `sessionStorage` vacío e independiente.

- **Ejemplo de uso de `sessionStorage`**:
  - **Formularios multipaso**: Si tienes un formulario largo dividido en varios pasos y quieres guardar el progreso del usuario a medida que avanza, `sessionStorage` es ideal. Los datos se mantienen mientras el usuario está en el proceso de completar el formulario en esa pestaña, pero se limpian si cierra la pestaña, lo que es deseable para evitar datos incompletos o obsoletos.
  - **Estado temporal de la sesión**: Almacenar el estado de la UI específico de una sesión de navegación, como filtros de búsqueda aplicados a una tabla o el estado de un modal, que no necesitas que persista si el usuario cierra la pestaña y vuelve más tarde.

### 24. Explica el concepto de "cookies" en el contexto web, sus atributos de seguridad (`HttpOnly`, `Secure`, `SameSite`) y cuándo serían preferibles sobre `localStorage` o `sessionStorage`

Las **cookies** son pequeños fragmentos de datos que un servidor web envía al navegador del usuario y que el navegador almacena. Luego, el navegador envía estas cookies de vuelta al servidor con cada solicitud posterior al mismo dominio. Se utilizan principalmente para la gestión de sesiones, personalización del usuario y seguimiento.

- **Atributos de Seguridad Clave**:
  - `HttpOnly`: Cuando una cookie se establece con el atributo `HttpOnly`, no puede ser accedida por JavaScript a través de `document.cookie`. Esto es crucial para prevenir ataques de Cross-Site Scripting (XSS), ya que un script malicioso no podría robar la cookie de sesión del usuario.
  - `Secure`: Una cookie con el atributo `Secure` solo se enviará al servidor si la solicitud se realiza a través de una conexión HTTPS (segura). Esto protege la cookie de ser interceptada por atacantes en redes no seguras.
  - `SameSite`: Este atributo ayuda a mitigar los ataques de Cross-Site Request Forgery (CSRF). Controla cuándo las cookies se envían en solicitudes de origen cruzado:
    - `Strict`: La cookie solo se envía en solicitudes del mismo sitio.
    - `Lax`: La cookie se envía en solicitudes del mismo sitio y en navegaciones de nivel superior (GET requests) desde otros sitios.
    - `None`: La cookie se envía en todas las solicitudes de origen cruzado, pero requiere que la cookie también sea `Secure`.
- **Cuándo serían preferibles sobre `localStorage` o `sessionStorage`**:
  - **Autenticación y Gestión de Sesiones**: Las cookies son el mecanismo preferido para la gestión de sesiones y la autenticación basada en sesiones, ya que se envían automáticamente con cada solicitud HTTP al servidor. Con los atributos `HttpOnly` y `Secure`, ofrecen una capa de seguridad superior contra XSS y ataques de red en comparación con localStorage para tokens de sesión.
  - **Necesidad de ser enviadas al servidor**: Si los datos almacenados necesitan ser enviados al servidor con cada solicitud (ej., un ID de sesión, un token de autenticación), las cookies son la opción natural.
  - **Seguridad reforzada por el servidor**: El servidor puede controlar mejor la creación, expiración y atributos de seguridad de las cookies, lo que las hace más robustas para datos sensibles que deben ser gestionados por el backend.

### 25. ¿Cómo se optimiza el rendimiento de una aplicación Angular en listas grandes utilizando `ChangeDetectionStrategy.OnPush` y la función `trackBy`?

- `ChangeDetectionStrategy.OnPush`:
  - Se aplica a los componentes para que Angular solo ejecute la detección de cambios en ellos cuando sus propiedades `@Input()` cambien (por referencia), cuando se dispare un evento desde el componente o sus hijos, o cuando una `Signal` que consumen cambie.
  - Esto reduce significativamente el número de veces que Angular necesita verificar los cambios en el árbol de componentes, mejorando el rendimiento al evitar re-renders innecesarios.
- **Función `trackBy` en `@for` (o `*ngFor`)**:
  - Se utiliza en los bucles `@for` (o `*ngFor`) para proporcionar a Angular una forma de identificar de manera única cada elemento en una colección.
  - Cuando la lista de datos cambia, `trackBy` permite a Angular solo re-renderizar o manipular los elementos del DOM que realmente han cambiado, añadido o eliminado, en lugar de destruir y recrear todos los elementos. Esto es crucial para el rendimiento y la experiencia de usuario en listas grandes.

  ```HTML
  <ul>
    @for (item of items; track item.id) {
      <li>{{ item.name }}</li>
    }
  </ul>
  ```

---

**Consejo Final para la Entrevista**: Durante una entrevista técnica, especialmente en preguntas de resolución de problemas o diseño de arquitectura, no solo importa la respuesta correcta, sino también cómo llegas a ella.  
Explicar tu razonamiento, las alternativas que consideras, los pros y los contras de cada enfoque, y cómo depurarías un problema, demuestra tus habilidades de pensamiento crítico y resolución de problemas.  
Esto permite al entrevistador entender tu lógica, guiarte si te desvías, y evaluar tus habilidades de comunicación, que son tan importantes como tus conocimientos técnicos.

**¡Mucha suerte!**
