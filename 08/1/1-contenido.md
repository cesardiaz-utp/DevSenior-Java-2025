# Clase 1: Introducción a Angular, CLI, Componentes y Estructura Moderna

**Hola a todos!**
¡Bienvenidos a la primera clase del **Módulo 8**! En este módulo, daremos un giro emocionante hacia el desarrollo frontend, donde aprenderemos a construir interfaces de usuario dinámicas, eficientes y modulares utilizando el **framework Angular 20**. Nos sumergiremos en la creación de experiencias interactivas y visualmente atractivas que, en módulos posteriores, interactuarán con las APIs que ya hemos dominado.

En esta clase, no solo exploraremos los fundamentos de Angular y la potencia de Angular CLI, sino que los aplicaremos paso a paso para construir un **frontend básico para listar propiedades inmobiliarias**, utilizando datos simulados por ahora. A medida que avancemos en el desarrollo de este mini-proyecto, introduciremos y explicaremos cada concepto clave: desde la configuración del entorno y la creación del proyecto con Angular CLI, pasando por los componentes standalone, los ciclos de vida, hasta la integración de estilos con Tailwind CSS y el tipado con TypeScript. También haremos una primera mención a las **Signals** y el **flujo Zoneless**, que son pilares de la reactividad moderna en Angular.

Prepárense para construir su primera aplicación Angular, sentando las bases para un sistema de ofertas inmobiliarias completo.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Comprender el concepto de Single Page Applications (SPA)** y sus ventajas en el desarrollo web moderno.
- **Crear y configurar un proyecto Angular desde cero con Angular CLI**, entendiendo sus beneficios para el desarrollo frontend.
- **Desarrollar componentes utilizando el enfoque de componentes**.
- **Gestionar el ciclo de vida básico de los componentes con `ngOnInit`**.
- **Integrar y aplicar estilos con Tailwind CSS** para un diseño responsivo y eficiente.
- **Aplicar TypeScript para añadir tipado estático** a proyectos Angular, mejorando la robustez y mantenibilidad del código.
- **Conocer una estructura de carpetas profesional** para proyectos Angular con componentes standalone, facilitando la escalabilidad y el mantenimiento.

## 1. Configuración del Entorno de Desarrollo para Angular y Angular CLI

Antes de sumergirnos en la creación de nuestra primera aplicación Angular, es crucial tener nuestro entorno de desarrollo configurado correctamente. Necesitaremos instalar Node.js (que incluye npm) y un editor de código adecuado como Visual Studio Code.

### 1.1. Instalar Node.js y npm

**Node.js** es un entorno de ejecución de JavaScript del lado del servidor que también incluye `npm` (Node Package Manager), la herramienta estándar para gestionar las dependencias en proyectos JavaScript y Angular. Angular CLI se ejecuta sobre Node.js, por lo que es un requisito fundamental.

1. **Descargar Node.js**: Visita el sitio web oficial de Node.js: [https://nodejs.org/es/download/](https://nodejs.org/es/download/)
2. **Elegir Versión**: Se recomienda descargar la versión **LTS (Long Term Support)**, ya que es la más estable y recomendada para la mayoría de los usuarios.
3. **Instalar**: Sigue el asistente de instalación para tu sistema operativo (Windows, macOS, Linux). Asegúrate de que la opción "npm package manager" esté seleccionada durante la instalación.
4. **Verificar Instalación**: Abre tu Terminal (o Símbolo del Sistema en Windows) y ejecuta los siguientes comandos para verificar que Node.js y npm se instalaron correctamente:

```Terminal
node -v  

# Debería mostrar la versión de Node.js (ej. v20.x.x)
```

```Terminal
npm -v   

# Debería mostrar la versión de npm (ej. 10.x.x)
```

### Nota sobre PowerShell en Windows

Si al intentar ejecutar comandos como `npm -v` o `npm install -g @angular/cli` en PowerShell recibes un error relacionado con la política de ejecución (ej., "No se puede cargar el archivo... porque la ejecución de scripts está deshabilitada en este sistema."), necesitarás ajustar la política de ejecución de PowerShell.

**Solución:**
Abre PowerShell **como Administrador** y ejecuta el siguiente comando:

```Terminal
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Confirma la acción si se te pregunta. Esto permitirá que los scripts firmados se ejecuten y los scripts que descargues de Internet sean ejecutados si los firmas tú mismo. Una vez hecho esto, puedes cerrar PowerShell y abrir uno nuevo (normalmente) para que los cambios surtan efecto, y luego intentar la instalación de Angular CLI nuevamente.

### 1.2. Instalar Angular CLI

El **Angular CLI (Command Line Interface)** es una herramienta de línea de comandos que te ayuda a crear, desarrollar y mantener aplicaciones Angular. Facilita tareas como la creación de proyectos, componentes, servicios, módulos y la construcción de la aplicación para producción.

Para instalar Angular CLI globalmente en tu sistema, abre tu terminal y ejecuta:

```Terminal
npm install -g @angular/cli
```

Una vez instalado, puedes verificar la versión:

```Terminal
ng version
```

### 1.3. Configurar Visual Studio Code (VS Code)

**Visual Studio Code (VS Code)** es un editor de código fuente gratuito, ligero y potente desarrollado por Microsoft. Es extremadamente popular en la comunidad de desarrollo web debido a su excelente soporte para TypeScript, Angular, su amplio ecosistema de extensiones y su depurador integrado.

**Extensiones Recomendadas para Angular y TypeScript**: Una vez instalado VS Code, ábrelo y busca las siguientes extensiones en la sección "Extensions" (Ctrl+Shift+X o Cmd+Shift+X):

- **Angular Language Service**: Proporciona autocompletado, navegación de código y verificación de errores para plantillas Angular.
- **Prettier - Code formatter**: Formatea automáticamente tu código para mantener un estilo consistente.
- **ESLint**: Herramienta para identificar patrones problemáticos encontrados en el código JavaScript/TypeScript.
- **Tailwind CSS IntelliSense**: Ofrece autocompletado, resaltado de sintaxis y linting para clases de Tailwind CSS.

Con Node.js, npm, Angular CLI y VS Code configurados, tu entorno de desarrollo estará listo para construir aplicaciones Angular.

## 2. Introducción a las Single Page Applications (SPA)

Antes de sumergirnos en Angular, es fundamental entender el paradigma de desarrollo que Angular y otras bibliotecas/frameworks modernas de frontend habilitan: las **Single Page Applications (SPA)**.

### 2.1. ¿Qué son las Single Page Applications (SPA)?

Una **Single Page Application (SPA)**, o **Aplicación de Una Sola Página**, es un tipo de aplicación web que carga una única página HTML en el navegador del usuario y luego actualiza dinámicamente el contenido de esa página a medida que el usuario interactúa con la aplicación. A diferencia de las aplicaciones web tradicionales (Multi-Page Applications - MPA) que requieren una recarga completa de la página cada vez que el usuario navega a una nueva sección, una SPA evita estas recargas, proporcionando una experiencia de usuario más fluida y similar a una aplicación de escritorio o móvil.

### 2.2. ¿Cómo Funcionan las SPAs?

Cuando un usuario accede a una SPA, el navegador carga inicialmente un único archivo HTML, junto con todos los recursos necesarios (JavaScript, CSS). Una vez cargado, el JavaScript de la aplicación toma el control:

- **Renderizado Inicial**: El JavaScript renderiza la interfaz de usuario inicial.
- **Interacciones Dinámicas**: Cuando el usuario hace clic en un enlace o un botón, en lugar de solicitar una nueva página al servidor, el JavaScript intercepta el evento, actualiza el DOM (Document Object Model) de la página actual y, si es necesario, realiza llamadas asíncronas a una API (como nuestra API de Spring Boot en futuros módulos) para obtener o enviar datos.
- **Actualización de la URL**: Aunque la página no se recarga, la URL en la barra de direcciones del navegador puede actualizarse para reflejar la "vista" actual, permitiendo el uso de los botones de "atrás" y "adelante" del navegador, y la capacidad de compartir enlaces directos (deep linking).

### 2.3. Ventajas Clave de las SPAs

1. **Experiencia de Usuario Fluida y Rápida**: Al evitar las recargas completas de la página, las SPAs ofrecen una transición instantánea entre vistas, lo que resulta en una experiencia de usuario más rápida, interactiva y sin interrupciones, similar a una aplicación nativa.
2. **Rendimiento Mejorado**: Una vez que la página inicial se carga, solo los datos necesarios se intercambian con el servidor (a través de APIs), lo que reduce la cantidad de datos transferidas y el tiempo de espera. El frontend maneja gran parte del renderizado, aliviando la carga del servidor.
3. **Desarrollo Eficiente de APIs**: Las SPAs se basan en APIs RESTful (o GraphQL) para la comunicación con el backend. Esto fomenta una clara separación de responsabilidades entre el frontend y el backend, permitiendo que equipos diferentes trabajen de forma independiente y que la misma API pueda ser consumida por múltiples clientes (web, móvil, etc.).
4. **Despliegue Simplificado**: El frontend de una SPA es típicamente un conjunto de archivos estáticos (HTML, CSS, JavaScript) que pueden ser servidos desde un CDN (Content Delivery Network) o un servidor web simple, lo que simplifica el despliegue y la escalabilidad del frontend.
5. **Reutilización de Código**: Los frameworks como Angular fomentan la modularidad a través de componentes y servicios, lo que facilita la reutilización de elementos de UI y lógica en diferentes partes de la aplicación.

### 2.4. Desventajas y Consideraciones

- **SEO (Search Engine Optimization)**: Históricamente, las SPAs presentaban desafíos para los motores de búsqueda, ya que el contenido se carga dinámicamente. Sin embargo, los motores de búsqueda modernos han mejorado significativamente su capacidad para rastrear JavaScript, y técnicas como el Server-Side Rendering (SSR) o la pre-renderización pueden mitigar este problema.
- **Tiempo de Carga Inicial**: La carga inicial de una SPA puede ser más lenta que la de una MPA simple, ya que todo el JavaScript y CSS de la aplicación deben descargarse. Esto se optimiza con técnicas como el code splitting y la carga diferida.
- **Complejidad de Desarrollo**: Las SPAs pueden ser más complejas de desarrollar y depurar debido a la gestión del estado del lado del cliente y la interacción asíncrona.

### 2.5. Tecnologías Populares para el Desarrollo de SPAs

Existen varias bibliotecas y frameworks de JavaScript que son ampliamente utilizados para construir Single Page Applications, cada uno con sus propias fortalezas y enfoques:

- **Angular (de Google)**: Un framework completo y robusto para construir aplicaciones web a gran escala. Ofrece una estructura más opinionada con un conjunto de herramientas y convenciones predefinidas (CLI, TypeScript, RxJS, etc.), lo que lo hace adecuado para proyectos empresariales que requieren consistencia y escalabilidad.
- **React (de Meta)**: Una biblioteca declarativa y eficiente para construir interfaces de usuario. Se centra en la capa de vista y es muy popular por su flexibilidad, su ecosistema de componentes y el uso del Virtual DOM.
- **Vue.js (creado por Evan You)**: Un framework progresivo que es fácil de aprender y muy versátil.
- **Svelte (creado por Rich Harris)**: Un enfoque diferente donde el framework compila tu código en JavaScript vanilla en tiempo de compilación.
- **SolidJS**: Una biblioteca reactiva moderna que ofrece un rendimiento similar al de Svelte al compilar componentes, pero con una sintaxis JSX que se asemeja a React.

Angular es una de las herramientas más populares y eficientes para construir SPAs, proporcionando un modelo de componentes robusto, un sistema de módulos y una fuerte integración con TypeScript que optimiza el desarrollo y la escalabilidad.

## 3. Angular

Ahora que entendemos el concepto de SPA, veamos cómo Angular encaja en este panorama.

### 3.1. ¿Qué es Angular?

Angular es un **framework de desarrollo de aplicaciones web de código abierto** mantenido por Google. A diferencia de React (que es una biblioteca para la capa de vista), Angular es un _framework completo_ que proporciona una estructura y un conjunto de herramientas para construir aplicaciones web complejas, desde el frontend hasta la gestión de estado, enrutamiento y comunicación con el backend.

Su objetivo principal es facilitar la creación de aplicaciones de una sola página (SPAs) escalables y de alto rendimiento, promoviendo patrones de diseño como la arquitectura basada en componentes y la inyección de dependencias.

### 3.2. Características Clave de Angular

Las características centrales de Angular son las que lo hacen tan efectivo y una elección popular para el desarrollo frontend a gran escala:

1. **Basado en Componentes**:
    - **Descripción**: Angular organiza la UI en componentes. Un componente es una clase TypeScript con un decorador `@Component` que define su plantilla HTML, sus estilos CSS y su lógica. Los componentes se pueden anidar y reutilizar.
    - **Ventaja**: Fomenta la modularidad, la reutilización del código y una clara separación de preocupaciones, lo que facilita el desarrollo, las pruebas y el mantenimiento de aplicaciones grandes.
2. **TypeScript como Lenguaje Principal**:
    - **Descripción**: Angular está construido con TypeScript, un superset de JavaScript que añade tipado estático. Esto permite escribir código más robusto y escalable.
    - **Ventaja**: Mejora la detección de errores en tiempo de desarrollo, proporciona un mejor autocompletado y refactorización en IDEs, y facilita la colaboración en equipos grandes.
3. **Inyección de Dependencias (DI)**:
    - **Descripción**: Angular utiliza un sistema de inyección de dependencias para proporcionar instancias de clases (como servicios) a los componentes que las necesitan. Esto promueve la modularidad y la facilidad de prueba.
    - **Ventaja**: Permite que los componentes sean más limpios y enfocados en su propósito, delegando la lógica de negocio y la gestión de datos a los servicios.
4. **Enrutamiento (Routing)**:
    - **Descripción**: Angular incluye un potente módulo de enrutamiento que permite crear SPAs con múltiples "vistas" o "páginas" sin recargar la página completa. Las URLs cambian dinámicamente.
    - **Ventaja**: Proporciona una experiencia de usuario fluida y permite la navegación profunda (deep linking) y el uso de los botones de atrás/adelante del navegador.
5. **Formularios Reactivos y Basados en Plantillas**:
    - **Descripción**: Angular ofrece dos enfoques para manejar formularios: reactivos (programáticos, más robustos para formularios complejos) y basados en plantillas (más simples para formularios básicos).
    - **Ventaja**: Proporciona herramientas poderosas para la validación de entrada de usuario y la gestión del estado de los formularios.
6. **RxJS para Manejo de Eventos Asíncronos**:
    - **Descripción**: Angular utiliza la biblioteca RxJS (Reactive Extensions for JavaScript) para manejar eventos asíncronos y flujos de datos. Conceptos como `Observables` son fundamentales para la comunicación con APIs.
    - **Ventaja**: Simplifica la gestión de operaciones asíncronas complejas, como las llamadas HTTP, y permite un manejo de errores más robusto.
7. **Angular CLI**:
    - **Descripción**: Una herramienta de línea de comandos robusta que simplifica la creación de proyectos, la generación de código (componentes, servicios), la construcción y el despliegue de aplicaciones Angular.
    - **Ventaja**: Acelera el desarrollo, asegura la consistencia en la estructura del proyecto y automatiza muchas tareas repetitivas.

### 3.3. Elementos Clave para un Correcto Desarrollo de Aplicaciones con Angular

Para dominar Angular y construir aplicaciones robustas, es fundamental comprender y utilizar correctamente sus elementos clave:

1. **Componentes**: Los bloques de construcción fundamentales de la UI. Cada componente tiene un selector, una plantilla HTML y estilos opcionales.
2. **Módulos (`NgModule`)**: Tradicionalmente, organizaban la aplicación en bloques funcionales. Sin embargo, en Angular 20, el enfoque se ha desplazado hacia los **componentes standalone**, reduciendo la necesidad de `NgModules` para componentes individuales.
3. **Plantillas (Templates)**: Son el HTML que define la vista de un componente. Utilizan la sintaxis de plantillas de Angular para la vinculación de datos (data binding) y la lógica condicional/repetitiva.
    - **Interpolación (`{{ }}`)**: Muestra valores de propiedades de componentes en la plantilla.

      ```HTML
      <h1>{{ title }}</h1>
      <p>La fecha actual es: {{ currentDate | date:'mediumDate' }}</p>
      ```

    - **Property Binding (`[ ]`)**: Vincula una propiedad del DOM a una propiedad del componente.

      ```HTML
      <img [src]="imageUrl" [alt]="imageAltText">
      <button [disabled]="!isLoggedIn">Guardar</button>
      ```

    - **Event Binding (`( )`)**: Escucha eventos del DOM y ejecuta métodos del componente.

      ```HTML
      <button (click)="onClick()">Haz clic</button>
      <input (input)="onInputChange($event)">
      ```

    - **Two-Way Data Binding (`[( )]` - ngModel)**: Sincroniza datos entre la vista y el modelo (requiere `FormsModule`).

      ```HTML
      <input [(ngModel)]="userName">
      ```

    - **Directivas Estructurales (`*ngIf`, `*ngFor` y la nueva sintaxis `@if`, `@for`)**: Modifican la estructura del DOM (añaden/eliminan elementos).

      **Sintaxis Tradicional**:

      ```HTML
      <div *ngIf="isLoading">Cargando...</div>
      <ul>
        <li *ngFor="let item of items">{{ item.name }}</li>
      </ul>
      ```

      **Nueva Sintaxis de Control de Flujo (Angular 17+)**: Esta nueva sintaxis es más legible, más performante y no requiere importar `CommonModule` en componentes standalone para su uso.

      - `@if` y `@else` (Condicionales):

        ```HTML
        @if (isLoading) {
          <div>Cargando...</div>
        } @else if (error) {
          <div class="text-red-500">Error: {{ error }}</div>
        } @else {
          <div>Contenido cargado.</div>
        }
        ```

      - `@for` (Iteración):

        ```HTML
        <ul>
          @for (item of items; track item.id) {
            <li>{{ item.name }}</li>
          } @empty {
            <li>No hay elementos para mostrar.</li>
          }
        </ul>
        ```

        **Nota**: El bloque `track` es obligatorio en `@for` y es crucial para el rendimiento, ya que le dice a Angular cómo identificar de forma única cada elemento en la lista, permitiendo actualizaciones más eficientes del DOM. El bloque `@empty` es opcional y se muestra cuando la lista está vacía.

    - Directivas de Atributo (`[ngClass]`, `[ngStyle]`): Cambian la apariencia o el comportamiento de un elemento.

      ```HTML
      <div [ngClass]="{'active': isActive, 'highlight': isHighlighted}">Contenido</div>
      ```

4. **Servicios**: Clases que encapsulan lógica de negocio, manipulación de datos o comunicación con APIs. Se inyectan en componentes u otros servicios.
5. **Inyección de Dependencias**: El mecanismo de Angular para entregar servicios a los componentes que los necesitan, promoviendo un código modular y fácil de probar.
6. **Ciclo de Vida de los Componentes**: Métodos de _hook_ que Angular llama en diferentes momentos de la vida de un componente (ej., `ngOnInit` para inicialización, `ngOnDestroy` para limpieza).
7. **Enrutamiento**: Define cómo los usuarios navegan entre diferentes vistas de la aplicación.
8. **Pipes**: Transforman datos en la plantilla (ej., date, currency, uppercase).

Comprender estos conceptos y cómo interactúan es fundamental para desarrollar aplicaciones Angular de manera efectiva, modular y con buen rendimiento.

### 3.4. Signals y el Flujo Zoneless

#### **Signals (Señales)**

- **¿Qué son?** Las Signals son un nuevo sistema de reactividad basado en primitivas de bajo nivel que permiten a Angular rastrear de forma más granular cuándo los datos cambian y qué partes de la interfaz de usuario necesitan ser actualizadas. Son una forma de declarar valores reactivos que notifican a sus consumidores cuando cambian.
- **¿Por qué son importantes?** Tradicionalmente, Angular ha usado Zone.js para detectar cambios. Las Signals ofrecen un mecanismo más explícito y eficiente para la reactividad, lo que puede llevar a un mejor rendimiento y a una depuración más sencilla.
- **Conceptos Clave**:
  - `signal()`: Para crear un valor reactivo.
  - `computed()`: Para crear valores derivados que se actualizan automáticamente cuando sus dependencias cambian.
  - `effect()`: Para ejecutar efectos secundarios cuando un valor reactivo cambia.
- **Impacto**: Simplifican la gestión del estado reactivo en los componentes y servicios, haciendo que el código sea más predecible y fácil de optimizar. Veremos más sobre Signals en la Clase 2.

### 3.4. Flujo Zoneless (Detección de Cambios sin Zone.js)

- **¿Qué es Zone.js?** Históricamente, Zone.js ha sido la base del sistema de detección de cambios de Angular. Parchea APIs asíncronas del navegador (como `setTimeout`, `fetch`, eventos DOM) para notificar a Angular cuándo debe verificar cambios en toda la aplicación.
- **Limitaciones**: Aunque potente, Zone.js puede introducir sobrecarga de rendimiento en aplicaciones grandes y hacer la depuración más compleja.
- **El Camino Zoneless**: Con la introducción de Signals, Angular está evolucionando hacia un modelo de detección de cambios que puede funcionar sin **Zone.js**. Esto significa que los cambios en los Signals notifican directamente a Angular qué componentes específicos necesitan ser re-renderizados, en lugar de realizar una verificación global.
- **Beneficios**: Mayor granularidad en la detección de cambios, mejor rendimiento y una experiencia de depuración más clara.
- **Impacto**: Aunque la transición completa a Zoneless es un proceso gradual, el uso de Signals nos prepara para este futuro más eficiente. Hablaremos más sobre esto en la Clase 3.

## 4. Qué es TypeScript y por qué usarlo con Angular: Añadiendo Tipado al Proyecto

Angular está construido con **TypeScript**, lo que lo convierte en el lenguaje principal para el desarrollo de aplicaciones Angular.

**TypeScript** es un superset de JavaScript que añade tipado estático opcional al lenguaje. Desarrollado y mantenido por Microsoft, TypeScript se compila (o transpila) a JavaScript plano, lo que significa que cualquier navegador o entorno de ejecución de JavaScript puede entenderlo.

### 4.1. ¿Qué significa que TypeScript es un "Superset" de JavaScript?

La relación "superset" significa que **todo código JavaScript válido es también código TypeScript válido**. Puedes tomar cualquier archivo `.js` y renombrarlo a `.ts`, y TypeScript lo entenderá. Sin embargo, TypeScript añade características adicionales que JavaScript no tiene, principalmente el **tipado estático**. Estas características se eliminan durante el proceso de **transpilación** a JavaScript puro, de modo que el código resultante pueda ser ejecutado por cualquier navegador o entorno de Node.js.

### 4.2. Características Principales de TypeScript

TypeScript extiende JavaScript con las siguientes características clave:

1. **Tipado Estático (Static Typing)**: Esta es la característica más fundamental y el corazón de TypeScript. Permite definir los tipos de variables, parámetros de funciones, valores de retorno, y propiedades de objetos.
    - **Declaración de Tipos**: Puedes especificar el tipo de una variable usando dos puntos (`:`) seguido del tipo.

      ```Typescript
      let nombre: string = "Juan Pérez";
      let edad: number = 30;
      let esActivo: boolean = true;
      let listaNumeros: number[] = [1, 2, 3]; // Array de números
      let cualquierCosa: any = "Hola"; // Tipo 'any' desactiva la verificación de tipos (usar con precaución)
      ```

    - **Inferencia de Tipos**: TypeScript es lo suficientemente inteligente como para inferir el tipo de una variable si no lo especificas explícitamente, basándose en el valor inicial.

      ```Typescript
      let mensaje = "Hola Mundo"; // TypeScript infiere que 'mensaje' es de tipo 'string'
      mensaje = 123; // Esto causaría un error de tipo: Type 'number' is not assignable to type 'string'.
      ```

    - **Tipos de Unión (Union Types)**: Permiten que una variable tenga uno de varios tipos posibles.

      ```Typescript
      let id: string | number = "abc-123"; // Puede ser string o number
      id = 456; // Válido
      id = true; // Error
      ```

    - **Tipos Literales (Literal Types)**: Permiten que una variable solo tenga un valor específico de un conjunto predefinido.

      ```Typescript
      type DireccionCardinal = "Norte" | "Sur" | "Este" | "Oeste";
      let miDireccion: DireccionCardinal = "Norte";
      miDireccion = "Diagonal"; // Error
      ```

    - `void`, `null`, `undefined`, `never`: Tipos especiales para funciones que no retornan nada, valores nulos, indefinidos o para funciones que nunca retornan (ej., lanzan un error).

2. **Interfaces**: Las interfaces son una de las herramientas más poderosas de TypeScript para definir la "forma" de los objetos. Actúan como contratos que garantizan que un objeto tendrá ciertas propiedades con tipos específicos. Son ideales para definir la estructura de los datos que se reciben de APIs o que se pasan entre componentes.

    ```Typescript
    // Definición de una interfaz
    interface Usuario {
      id: number;
      nombre: string;
      email: string;
      edad?: number; // Propiedad opcional (puede existir o no)
      roles: string[];
    }

    // Usando la interfaz
    function mostrarUsuario(usuario: Usuario) {
      console.log(`ID: ${usuario.id}, Nombre: ${usuario.nombre}, Email: ${usuario.email}`);
      if (usuario.edad) {
        console.log(`Edad: ${usuario.edad}`);
      }
    }

    const usuario1: Usuario = {
      id: 1,
      nombre: "Ana García",
      email: "ana@example.com",
      roles: ["admin", "editor"]
    };

    const usuario2: Usuario = {
      id: 2,
      nombre: "Luis Martínez",
      email: "luis@example.com",
      edad: 25,
      roles: ["viewer"]
    };

    mostrarUsuario(usuario1);
    mostrarUsuario(usuario2);
    ```

3. **Clases y Programación Orientada a Objetos (POO) Mejorada**:
Aunque JavaScript moderno ya tiene clases, TypeScript las mejora con modificadores de acceso (`public`, `private`, `protected`), interfaces para clases, y propiedades de solo lectura.

    ```Typescript
    class Animal {
      private nombre: string; // Solo accesible dentro de la clase
      protected edad: number; // Accesible en la clase y subclases

      constructor(nombre: string, edad: number) {
        this.nombre = nombre;
        this.edad = edad;
      }

      public hacerSonido(sonido: string): void {
        console.log(`${this.nombre} hace ${sonido}`);
      }
    }

    class Perro extends Animal {
      constructor(nombre: string, edad: number) {
        super(nombre, edad);
      }

      ladrar(): void {
        console.log(`${this.nombre} (edad: ${this.edad}) ladra!`);
      }
    }

    const miPerro = new Perro("Buddy", 3);
    miPerro.hacerSonido("guau");
    miPerro.ladrar();
    console.log(miPerro.nombre); // Error: Property 'nombre' is private
    ```

4. **Enums (Enumeraciones)**: Permiten definir un conjunto de constantes con nombre, lo que mejora la legibilidad y previene errores al usar valores "mágicos" (números o cadenas sin significado claro).

    ```Typescript
    enum EstadoPedido {
      Pendiente,    // 0 por defecto
      Procesando,   // 1
      Enviado,      // 2
      Entregado,    // 3
      Cancelado     // 4
    }

    let miPedido: EstadoPedido = EstadoPedido.Procesando;

    if (miPedido === EstadoPedido.Procesando) {
      console.log("El pedido está siendo procesado.");
    }

    // También puedes asignar valores explícitos
    enum Colores {
      Rojo = "#FF0000",
      Verde = "#00FF00",
      Azul = "#0000FF"
    }

    let colorFondo: Colores = Colores.Azul;
    console.log(colorFondo); // Salida: #0000FF
    ```

5. **Genéricos (Generics)**: Permiten escribir componentes y funciones que funcionan con una variedad de tipos en lugar de un solo tipo. Esto proporciona flexibilidad y reutilización de código sin sacrificar la seguridad de tipos.

    ```Typescript
    // Función genérica que devuelve el mismo tipo que recibe
    function identidad<T>(arg: T): T {
      return arg;
    }

    let salidaString = identidad<string>("Hola Genéricos"); // salidaString es de tipo string
    let salidaNumero = identidad<number>(123); // salidaNumero es de tipo number

    console.log(salidaString);
    console.log(salidaNumero);

    // Interfaz genérica para una caja que puede contener cualquier tipo de ítem
    interface Caja<T> {
      contenido: T;
    }

    let cajaDeNumeros: Caja<number> = { contenido: 100 };
    let cajaDeTexto: Caja<string> = { contenido: "Libro" };
    ```

6. **Decoradores (Decorators)**: Son una característica experimental de TypeScript (pero ampliamente utilizada en frameworks como Angular) que permite añadir metadatos a clases, propiedades, métodos o parámetros. Se usan para configurar el comportamiento de las clases en tiempo de ejecución.

    En Angular, los decoradores como `@Component`, `@Injectable`, `@Input`, `@Output` son fundamentales para definir la estructura y el comportamiento de los componentes y servicios.

    ```Typescript
    // Ejemplo conceptual de un decorador (no ejecutable sin configuración de Angular/TS)
    // En Angular, se usan así:
    import { Component, OnInit } from '@angular/core';

    @Component({ // Decorador de clase
      selector: 'app-mi-componente',
      templateUrl: './mi-componente.html',
      styleUrls: ['./mi-componente.css']
    })
    export class MiComponente implements OnInit {
      @Input() // Decorador de propiedad
      titulo: string = '';

      constructor() { }

      ngOnInit(): void { }
    }
    ```

7. **Módulos (ES Modules)**: TypeScript soporta el sistema de módulos de ECMAScript (ES Modules) (`import`/`export`), lo que permite organizar el código en archivos separados y reutilizables, mejorando la modularidad del proyecto.

    ```Typescript
    // archivo: utils.ts
    export function sumar(a: number, b: number): number {
      return a + b;
    }

    // archivo: app.ts
    import { sumar } from './utils';
    console.log(sumar(5, 3)); // 8
    ```

### 4.3. ¿Por qué usar TypeScript en el desarrollo de aplicaciones frontend modernas con Angular?

La integración de TypeScript con Angular es una de sus mayores fortalezas y la razón por la que es la elección predeterminada para el desarrollo de Angular.

1. **Detección Temprana de Errores (Robustez)**:
    - **Ventaja**: Permite detectar errores comunes (errores de tipo, errores de ortografía de propiedades, acceso a propiedades inexistentes) en tiempo de desarrollo, antes de que el código se ejecute en el navegador. Esto reduce drásticamente los errores en producción y el tiempo de depuración.
    - **Implicación en Angular**: Cuando defines la interfaz `Property` para tus ofertas inmobiliarias, TypeScript te avisará si intentas acceder a `property.addresss` (con una 's' extra) o si pasas un número donde se espera una cadena.
2. **Mejor Mantenibilidad y Escalabilidad**:
    - **Ventaja**: A medida que las bases de código crecen y evolucionan, la refactorización y la comprensión del código se vuelven mucho más fáciles con tipos explícitos. Los tipos actúan como una forma de "documentación viva".
    - **Implicación en Angular**: Si modificas la estructura de un DTO o una propiedad en un componente, TypeScript te alertará automáticamente en todos los lugares donde ese tipo es usado incorrectamente, reduciendo el riesgo de romper otras partes de la aplicación. Esto es crucial en proyectos grandes con múltiples desarrolladores.
3. **Experiencia de Desarrollo Mejorada (DX - Developer Experience)**:
    - **Ventaja**: Ofrece un autocompletado más inteligente (IntelliSense), navegación de código, refactorización automática y detección temprana de errores directamente en tu editor de código (como VS Code).
    - **Implicación en Angular**: Al escribir componentes y servicios, sabrás exactamente qué propiedades y métodos están disponibles y de qué tipo son. Esto acelera el desarrollo, reduce la necesidad de consultar la documentación constantemente y hace que el proceso de codificación sea más fluido y menos propenso a errores.
4. **Colaboración en Equipo Simplificada**:
    - **Ventaja**: Proporciona un contrato claro sobre la forma de los datos y las interfaces. Esto facilita que los diferentes miembros del equipo colaboren en la misma base de código sin introducir conflictos de tipos inesperados.
    - **Implicación en Angular**: Los desarrolladores de frontend pueden trabajar con confianza sabiendo el formato exacto de los datos que esperan de las APIs de backend (usando las interfaces de TypeScript), y cómo deben interactuar con otros componentes y servicios. Se reduce la ambigüedad y se mejora la comunicación dentro del equipo.

## 5. Construcción del Frontend de Ofertas Inmobiliarias

Ahora pondremos en práctica todo lo aprendido para crear nuestro frontend de ofertas inmobiliarias, desde la configuración inicial hasta la visualización de datos simulados.

### 5.1. Instalación de Angular para un Nuevo Proyecto de Ofertas Inmobiliarias

Primero, crearemos la estructura base de nuestro proyecto Angular.

1. **Abre tu Terminal/Línea de Comandos**.
2. **Navega a la carpeta** donde deseas crear tu proyecto: `cd ruta/a/tu/carpeta/proyectos`
3. **Ejecuta el comando de creación de Angular CLI**:
  
    ```Terminal
    ng new real-estate-frontend
    ```

    Te va a realizar unas preguntas, las cuales responderás lo siguiente:

    ```Terminal
    ✔ Do you want to create a 'zoneless' application without zone.js (Developer Preview)? No
    ✔ Which stylesheet format would you like to use? CSS
    ✔ Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? No
    ```

    Despues de esto, generará los archivos base de la aplicación:

    ```Terminal
    CREATE real-estate-frontend/angular.json (2530 bytes)
    CREATE real-estate-frontend/package.json (1142 bytes)
    CREATE real-estate-frontend/README.md (1540 bytes)
    CREATE real-estate-frontend/tsconfig.json (1026 bytes)
    CREATE real-estate-frontend/.editorconfig (331 bytes)
    CREATE real-estate-frontend/.gitignore (629 bytes)
    CREATE real-estate-frontend/tsconfig.app.json (444 bytes)
    CREATE real-estate-frontend/tsconfig.spec.json (422 bytes)
    CREATE real-estate-frontend/.vscode/extensions.json (134 bytes)
    CREATE real-estate-frontend/.vscode/launch.json (490 bytes)
    CREATE real-estate-frontend/.vscode/tasks.json (980 bytes)
    CREATE real-estate-frontend/src/main.ts (228 bytes)
    CREATE real-estate-frontend/src/index.html (317 bytes)
    CREATE real-estate-frontend/src/styles.css (81 bytes)
    CREATE real-estate-frontend/src/app/app.spec.ts (701 bytes)
    CREATE real-estate-frontend/src/app/app.ts (314 bytes)
    CREATE real-estate-frontend/src/app/app.css (0 bytes)
    CREATE real-estate-frontend/src/app/app.html (20438 bytes)
    CREATE real-estate-frontend/src/app/app.config.ts (412 bytes)
    CREATE real-estate-frontend/src/app/app.routes.ts (80 bytes)
    CREATE real-estate-frontend/public/favicon.ico (15086 bytes)
    ✔ Packages installed successfully.
        Successfully initialized git.
    ```

4. **Ejecuta los comandos sugeridos**:
    - `cd real-estate-frontend`: Entra a la carpeta del proyecto.
    - `ng serve`: Inicia el servidor de desarrollo de Angular. Este comando mostrará una URL (ej., `http://localhost:4200/`) donde podrás ver tu aplicación Angular ejecutándose en el navegador.

### 5.2. Estructura Profesional de Carpetas con Componentes

Una vez que el proyecto base ha sido creado, es fundamental organizar nuestra estructura de carpetas para asegurar la mantenibilidad y escalabilidad de nuestra aplicación de ofertas inmobiliarias. En Angular 20, con el enfoque de **componentes standalone** como pilar, la organización se centra en la modularidad por características y la reducción de la dependencia de los `NgModules` para la declaración de componentes individuales.

Aquí se detalla una estructura de carpetas común y recomendada para un proyecto Angular moderno con componentes standalone, alineada con la Guía de Estilo de Angular y las últimas recomendaciones de nomenclatura:

```Text
real-estate-frontend/
├── node_modules/       # Dependencias del proyecto (gestionadas por npm)
├── public/             # Recursos estáticos (imágenes, iconos, fuentes)
│   └── images/
│       └── placeholder-property.jpg
│
├── src/
│   ├── app/                # Contiene los componentes y la configuración principal de la aplicación
│   │   ├── app.ts              # Componente raíz de la aplicación (standalone) - Anteriormente app.component.ts
│   │   ├── app.html            # Plantilla HTML del componente raíz - Anteriormente app.component.html
│   │   ├── app.css             # Estilos del componente raíz - Anteriormente app.component.css
│   │   ├── app.config.ts       # Archivo de configuración central para la aplicación (proveedores, rutas, etc.)
│   │   ├── app.routes.ts       # Definición de rutas para la aplicación (si se usa enrutamiento)
│   │   │
│   │   ├── features/           # Agrupa funcionalidades de alto nivel (por dominio de negocio)
│   │   │   │                   # Cada subcarpeta aquí representa una característica principal de la aplicación.
│   │   │   │                   # Los componentes dentro de estas carpetas son standalone y se importan directamente
│   │   │   │                   # en otros componentes standalone o en los archivos de ruta.
│   │   │   ├── properties/         # Característica: Módulo de propiedades inmobiliarias
│   │   │   │   ├── components/     # Componentes standalone específicos de la funcionalidad de propiedades
│   │   │   │   │   ├── property-list/    # Componente standalone para listar propiedades
│   │   │   │   │   │   ├── property-list.ts    # Archivo de lógica del componente (sin .component)
│   │   │   │   │   │   ├── property-list.html  # Plantilla HTML del componente (sin .component)
│   │   │   │   │   │   └── property-list.css   # Estilos del componente (sin .component)
│   │   │   │   │   ├── property-card/    # Componente standalone para mostrar una sola propiedad (reutilizable dentro de 'properties')
│   │   │   │   │   │   ├── property-card.ts    # Archivo de lógica del componente (sin .component)
│   │   │   │   │   │   ├── property-card.html  # Plantilla HTML del componente (sin .component)
│   │   │   │   │   │   └── property-card.css   # Estilos del componente (sin .component)
│   │   │   │   │   └── (otros componentes standalone relacionados con propiedades: detail, form, etc.)
│   │   │   │   ├── models/         # Interfaces de datos y tipos para propiedades (ej., `Property` interface)
│   │   │   │   │   └── property.ts
│   │   │   │   └── services/       # Servicios de lógica de negocio para propiedades (ej., `PropertyService`)
│   │   │   │       └── property.ts
│   │   │   └── (otras funcionalidades: auth, user-profile, etc. cada una con su propia estructura de components/models/services)
│   │   │
│   │   └── shared/             # Contiene componentes, pipes, directivas y servicios que son
│   │       │                   # genéricos y reutilizables en **toda** la aplicación.
│   │       │                   # Estos también son componentes standalone y se importan directamente.
│   │       ├── components/     # Componentes UI genéricos (ej. Button, Input, LoadingSpinner)
│   │       │   └── loading-spinner/
│   │       │       └── loading-spinner.ts    # Archivo de lógica del componente (sin .component)
│   │       ├── pipes/          # Pipes personalizados (ej. currency, date)
│   │       │   └── (ej. price-format.ts)
│   │       └── directives/     # Directivas personalizadas
│   │           └── (ej. highlight.ts)
│   │
│   ├── environments/       # Configuraciones específicas para diferentes entornos (dev, prod)
│   │   ├── environment.ts
│   │   └── environment.prod.ts
│   │
│   ├── index.html          # Archivo HTML principal de la aplicación
│   ├── main.ts             # Punto de entrada de la aplicación (bootstrap de la aplicación standalone)
|   └── styles.css          # Archivo CSS principal de la aplicación
│
├── .gitignore          # Archivos y carpetas a ignorar por Git
├── angular.json        # Archivo de configuración de Angular CLI para el proyecto
├── package.json        # Metadatos del proyecto y lista de dependencias
├── tsconfig.json       # Configuración de TypeScript
└── README.md           # Descripción del proyecto
```

- `src/app/`: Esta carpeta contiene el componente raíz (`app.ts`) que es el punto de entrada de la interfaz de usuario. También incluye `app.config.ts`, que es crucial en las aplicaciones standalone para configurar proveedores a nivel de aplicación, y `app.routes.ts` si se utiliza el enrutamiento. Los componentes aquí, incluido `App`, son standalone.
  - **Nota de Nomenclatura**: Los archivos del componente raíz ahora son `app.ts`, `app.html`, `app.css`.
- `src/assets/`: Destinada a almacenar recursos estáticos como imágenes, iconos y fuentes que la aplicación utilizará. Esta es la convención predeterminada de Angular CLI y la recomendada por la Guía de Estilo. No se requiere ninguna modificación en `angular.json` si se mantiene esta ruta.
- `src/environments/`: Contiene archivos para gestionar configuraciones específicas de diferentes entornos (por ejemplo, desarrollo, producción), permitiendo cambiar variables como URLs de API sin modificar el código fuente.
- `src/app/features/`: Esta es una carpeta clave para organizar la aplicación por **dominios de negocio o funcionalidades principales**. Cada subcarpeta dentro de `features` (como `properties`, `auth`, `user-profile`) representa una característica independiente.
  - Dentro de cada característica (ej., `properties`), encontrarás subcarpetas para `components`, `models`, y `services`.
  - Los componentes dentro de `features` son **componentes standalone**. Esto significa que se auto-importan las dependencias necesarias (otros componentes, pipes, directivas, `CommonModule`) directamente en su decorador `@Component` a través del array imports, sin necesidad de un `NgModule` que los declare o exporte.
  - **Nota importante sobre nombres de archivos de componentes**: Siguiendo las recomendaciones más recientes para Angular 20, los archivos de componentes ya **no necesitan** el sufijo `.component` (ej., `property-list.ts`, `property-card.ts`). Esto hace los nombres de archivo más concisos y directos, manteniendo la claridad gracias a la estructura de carpetas. Angular CLI aún puede generar con el sufijo por defecto, pero se recomienda renombrarlos manualmente después de la generación para seguir esta convención.
  - Este enfoque fomenta la cohesión y la independencia de las características.
- `src/app/shared/`: Esta carpeta está diseñada para componentes, pipes, directivas y servicios que son **genéricos y reutilizables en múltiples partes o funcionalidades de la aplicación**.
  - Al igual que los componentes en `features`, los elementos en `shared/components`, `shared/pipes`, etc., también son `standalone` y sus archivos no llevan el sufijo `.component`. Se importan directamente en cualquier componente standalone que los necesite.
  - El objetivo es evitar la duplicación de código y promover la consistencia en la interfaz de usuario y la lógica común.
- `src/styles/`: Contiene los archivos de estilos globales de la aplicación, como `styles.css`. Aquí es donde se suelen importar las directivas base de frameworks CSS como Tailwind CSS.
- `main.ts`: Es el punto de entrada principal de la aplicación. En Angular 20, este archivo se encarga de arrancar la aplicación utilizando la configuración definida en `app.config.ts`.

Esta estructura fomenta la modularidad, la separación de preocupaciones y facilita la búsqueda de archivos, lo que es vital a medida que tu aplicación crece, especialmente con el paradigma de componentes standalone que simplifica las dependencias.

### 5.3. Integración de Tailwind CSS en el Proyecto

Para darle a nuestro listado de propiedades un aspecto profesional sin escribir CSS personalizado, integraremos **Tailwind CSS**. Esto nos permitirá estilizar nuestros componentes (en este caso, la lista de propiedades y sus elementos) directamente usando clases de utilidad.

**Tailwind CSS** es un _framework_ CSS de utilidad que te permite construir diseños personalizados rápidamente, directamente en tu marcado HTML (o plantillas Angular). A diferencia de otros _frameworks_ CSS (como Bootstrap o Material UI) que vienen con componentes pre-diseñados, Tailwind te proporciona clases de bajo nivel que puedes componer para crear cualquier diseño.

#### Ventajas

- **Velocidad de Desarrollo**: Construye UIs complejas directamente en tu HTML.
- **Flexibilidad**: No te fuerza a seguir un diseño específico; puedes crear tu propio sistema.
- **Tamaño Mínimo de CSS**: Con PurgeCSS (integrado en el proceso de _build_ de Angular), solo el CSS que realmente usas en tu proyecto se incluye en el _build_ final.

#### Paso a paso para la integración con Angular CLI

1. **Instala Tailwind CSS, PostCSS y Autoprefixer**: Abre tu terminal en la raíz de tu proyecto Angular (ej., `real-estate-frontend`).

    ```Terminal
    npm install tailwindcss @tailwindcss/postcss postcss
    ```

2. **Crea el archivo de configuración de PostCSS**: Crea un nuevo archivo llamado `.postcssrc.json` en la raíz de tu proyecto. Este archivo le dirá a Angular que use PostCSS con el plugin de Tailwind.

    ```JSON
    {
      "plugins": {
        "@tailwindcss/postcss": {}
      }
    }
    ```

3. **Añade las directivas de Tailwind a tu CSS principal**: Abre tu archivo CSS principal (normalmente `src/styles.css`) y añade las directivas de Tailwind en la parte superior:

    ```CSS
    @import "tailwindcss";
    ```

¡Listo! Ahora las clases de Tailwind CSS que uses en tus componentes Angular aplicarán estilos visuales cuando inicies tu aplicación con `ng serve`.

### 5.4. Ciclo de Vida de Componentes: `ngOnInit`

Los componentes Angular tienen un ciclo de vida bien definido, y Angular proporciona "hooks" (métodos) que puedes implementar para ejecutar código en momentos específicos de ese ciclo. Uno de los hooks más comunes y fundamentales es `ngOnInit`.

- `ngOnInit`:
  - **Propósito**: Este método se ejecuta **una vez** después de que Angular ha inicializado todas las propiedades de entrada (`@Input()`) del componente y ha configurado sus vistas. Es el lugar ideal para realizar inicializaciones de datos, configurar suscripciones a servicios, o realizar cualquier lógica que dependa de que el componente esté completamente listo.
  - **Cuándo usarlo**: Cuando necesitas cargar datos iniciales para el componente, realizar configuraciones que dependen de los inputs, o inicializar lógica compleja.
  - **Implementación**: Tu componente debe implementar la interfaz `OnInit` y definir el método `ngOnInit()`.

  ```Typescript
  import { Component, OnInit } from '@angular/core';

  @Component({
    selector: 'app-my-component',
    standalone: true,
    template: `<p>Mi componente</p>`
  })
  export class MyComponent implements OnInit {
    constructor() {
      // El constructor es para inyección de dependencias y lógica muy ligera.
      // Evita lógica compleja que dependa de inputs aquí.
    }

    ngOnInit(): void {
      // Aquí es donde la lógica de inicialización debe ir.
      console.log('MyComponent ha sido inicializado. Aquí puedo cargar datos.');
      // Por ejemplo, aquí cargaríamos nuestras propiedades simuladas
      this.loadSimulatedProperties();
    }

    loadSimulatedProperties() {
      // Lógica para cargar propiedades (simulada por ahora)
    }
  }
  ```

### 5.5. Creación del Frontend Básico para Listar Propiedades Inmobiliarias

Finalmente, aplicaremos todo lo anterior para construir el listado de propiedades.

1. **Crea el modelo `Property`**:
    - Crea la carpeta `src/app/features/properties/models/`.
    - Dentro, crea `property.ts` y define la interfaz `Property`.

    ```Typescript
    export interface Property {
      id: number;
      address: string;
      city: string;
      price: number;
      bedrooms: number;
      bathrooms: number;
      imageUrl: string;
      description: string;
      // Añade más propiedades si lo deseas
    }
    ```

2. **Genera el componente `PropertyList` (y renombra los archivos)**:
    - Abre tu terminal en la raíz del proyecto.
    - Ejecuta el comando para generar un componente standalone dentro de la estructura `features/properties/components`.

    ```Terminal
    ng generate component features/properties/components/property-list
    ```

3. **Implementa la lógica en `PropertyList`**:
    - Abre `src/app/features/properties/components/property-list/property-list.ts`.
    - Importa `OnInit` y la interfaz `Property`.
    - Declara una propiedad `properties` que será un array de `Property`.
    - Implementa `ngOnInit` para cargar datos simulados.

    ```Typescript
    import { Component, OnInit } from '@angular/core';
    import { CommonModule } from '@angular/common'; // Necesario para pipes como currency
    import { Property } from '../../models/property'; // Importa la interfaz Property

    @Component({
      selector: 'app-property-list',
      imports: [CommonModule], // Importa CommonModule para usar pipes como currency
      templateUrl: './property-list.html', // Ruta actualizada (sin .component)
      styleUrl: './property-list.css' // Ruta actualizada (sin .component)
    })
    export class PropertyList implements OnInit {
      properties: Property[] = [];
      loading: boolean = true;
      error: string | null = null;

      constructor() { }

      ngOnInit(): void {
        this.loading = true;
        this.error = null;

        // Simulación de carga de datos con un retraso
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
          this.properties = simulatedData;
          this.loading = false;
        }, 1500); // Simula un retraso de 1.5 segundos
      }
    }
    ```

4. **Diseña la plantilla (`property-list.html`) con Tailwind CSS**:
    - Abre `src/app/features/properties/components/property-list/property-list.html`.
    - Utiliza clases de Tailwind para crear un diseño de cuadrícula para las propiedades. Este HTML ya usa la nueva sintaxis `@if` y `@for`.

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen">
      <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12 drop-shadow-lg">Ofertas Inmobiliarias</h1>

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
                <button class="mt-6 w-full bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md hover:shadow-lg">
                  Ver Detalles
                </button>
              </div>
            </div>
          } @empty {
            <!-- Este bloque @empty se mostrará si properties está vacío, pero ya tenemos un @else if para eso. -->
            <!-- Sin embargo, es buena práctica incluirlo si el @for es la única condición de visualización de la lista. -->
            <!-- En este caso, el @else if (properties.length === 0) ya lo maneja. -->
          }
        </div>
      }
    </div>
    ```

5. **Ajusta los estilos (`property-list.css` - opcional)**:

    Puedes dejar este archivo vacío por ahora, ya que usaremos principalmente Tailwind. Si necesitas estilos muy específicos que Tailwind no cubre, los añadirías aquí.

6. **Actualiza `app.ts` y `app.html` para mostrar `PropertyList`**:
    - Abre `src/app/app.ts`.
    - Importa `PropertyList` y añádelo al array `imports` del `@Component`.

    ```Typescript
    import { Component } from '@angular/core';
    import { RouterOutlet } from '@angular/router'; // Si estuviera el router
    import { PropertyList } from './features/properties/components/property-list/property-list'; // Importa tu componente

    @Component({
      selector: 'app-root',
      standalone: true,
      imports: [
        RouterOutlet, // Si estuviera el router
        PropertyList // Añade tu componente aquí
      ],
      templateUrl: './app.html', // Ruta actualizada
      styleUrl: './app.css' // Ruta actualizada
    })
    export class App {
      title = 'real-estate-frontend';
    }
    ```

    - Abre `src/app/app.html` (¡el archivo renombrado!).
    - Reemplaza el contenido predeterminado con el selector de tu `PropertyList`.

    ```HTML
    <app-property-list></app-property-list>
    ```

Ahora, si ejecutas `ng serve`, deberías ver un mensaje de "Cargando propiedades..." seguido de la lista simulada de propiedades inmobiliarias en un diseño atractivo con Tailwind CSS.
