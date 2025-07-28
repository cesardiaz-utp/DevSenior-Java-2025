# Clase 4: Formularios Reactivos, Gestión de Estado Compleja y Mejores Prácticas

**¡Hola a todos!**

¡Bienvenidos a la cuarta clase del **Módulo 8**! En la clase anterior, dimos un paso crucial al profundizar en la organización de nuestra aplicación mediante el uso de **Servicios** para encapsular la lógica de negocio y la gestión de datos, y exploramos el **Enrutamiento** para crear una navegación fluida y estructurada entre las diferentes vistas de nuestra aplicación. Esto sentó las bases para construir interfaces de usuario más complejas y modulares.

En esta clase, nos centraremos en la **interacción del usuario a través de formularios**, un pilar fundamental de casi cualquier aplicación web. Exploraremos los **Formularios Reactivos** de Angular, una forma poderosa y predecible de manejar la entrada de datos, incluyendo validaciones síncronas y asíncronas, lo que nos permitirá crear experiencias de usuario más dinámicas y con menos errores. Además, profundizaremos en la **gestión de estado compleja** utilizando la combinación de **Servicios** y **Signals**, un patrón que ya hemos comenzado a tocar, pero que ahora veremos en su máxima expresión para manejar la complejidad de aplicaciones a gran escala. También aprenderemos a **persistir datos en el cliente** usando `localStorage`, lo que nos permitirá guardar información importante entre sesiones del navegador, mejorando la experiencia del usuario. Finalmente, cerraremos con un conjunto de mejores prácticas para escribir código Angular optimizado y mantenible, asegurando que nuestras aplicaciones no solo funcionen bien, sino que también sean fáciles de entender, depurar y escalar en el futuro.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Diseñar y construir formularios reactivos** robustos y reutilizables en Angular, comprendiendo su arquitectura y cómo se vinculan con la plantilla.
- **Validar formularios de manera eficiente** utilizando validadores síncronos y asíncronos, incluyendo la creación de validadores personalizados para requisitos específicos.
- **Gestionar el estado complejo** de la aplicación mediante servicios y Signals, estableciendo una fuente única de verdad y promoviendo la inmutabilidad.
- **Persistir y recuperar el estado del cliente** utilizando mecanismos de almacenamiento local como `localStorage`, y entender sus implicaciones de seguridad y uso.
- **Aplicar mejores prácticas** para optimizar el rendimiento y la mantenibilidad del código Angular, desde la detección de cambios hasta la organización del proyecto.

## 1. Diseño y Construcción de Formularios Reactivos Reutilizables

Los formularios son la columna vertebral de la interacción del usuario en la mayoría de las aplicaciones web, permitiendo la entrada de datos, la configuración de preferencias y la ejecución de acciones. Angular ofrece dos enfoques para construir formularios: **Template-driven forms** y **Reactive forms**. Si bien los Template-driven forms pueden ser más sencillos para formularios muy básicos, en este curso nos centraremos exclusivamente en los Formularios Reactivos debido a su superioridad en términos de escalabilidad, previsibilidad, facilidad de prueba y control explícito sobre el estado del formulario, lo que los hace la elección ideal para formularios complejos y aplicaciones grandes que requieren validaciones dinámicas y manipulación programática.

### 1.1. ¿Por qué Formularios Reactivos? Una Mirada Más Profunda

La preferencia por los formularios reactivos en Angular se basa en varias ventajas clave:

- **Previsibilidad y Control Explícito**: A diferencia de los formularios dirigidos por plantillas, donde la lógica del formulario se infiere del HTML, los formularios reactivos se construyen programáticamente en el código TypeScript. Esto significa que tienes un control explícito sobre cada `FormControl`, `FormGroup` y `FormArray` desde el inicio. Puedes definir su estructura, valores iniciales y validadores directamente en tu clase de componente, lo que hace que el estado del formulario sea siempre predecible y fácil de seguir. Esta "declaración primero" en TypeScript facilita enormemente la depuración, ya que puedes inspeccionar el estado del formulario en cualquier momento en tu código.
- **Escalabilidad para Formularios Complejos**: Para formularios con muchas entradas, validaciones interdependientes, campos que aparecen o desaparecen dinámicamente, o formularios que requieren manipulación programática (como restablecer, precargar datos o deshabilitar campos condicionalmente), los formularios reactivos son mucho más manejables. Su API programática permite añadir o eliminar controles, cambiar validadores, y actualizar valores de forma sencilla y estructurada.
- **Facilidad de Prueba Unitaria**: Dado que toda la lógica del formulario (su estructura, valores y validaciones) reside en el código TypeScript, es mucho más fácil escribir pruebas unitarias para ellos. Puedes instanciar tu `FormGroup` en un entorno de prueba, simular cambios en los valores, y verificar el estado de validación sin necesidad de renderizar el componente en el DOM, lo que acelera el ciclo de desarrollo y mejora la calidad del software.
- **Reactividad y Observables**: Los formularios reactivos integran perfectamente con la librería RxJS. Cada `FormControl`, `FormGroup` y `FormArray` expone propiedades `valueChanges` y `statusChanges` que son `Observables`. Esto te permite reaccionar a los cambios en los valores o el estado de validación del formulario de manera asíncrona y declarativa. Por ejemplo, puedes deshabilitar un botón de envío hasta que el formulario sea válido, o realizar una validación asíncrona (como verificar la unicidad de un nombre de usuario en una API) solo después de que el usuario haya dejado de escribir por un momento. Esta reactividad se alinea perfectamente con el paradigma de Signals en Angular, permitiendo una gestión de estado fluida y eficiente.

### 1.2. Componentes Clave de los Formularios Reactivos: Los Bloques de Construcción

Los formularios reactivos se construyen a partir de tres bloques fundamentales, que son instancias de `AbstractControl`:

1. **FormControl**: Es la unidad más básica. Representa una entrada de formulario individual, como un campo de texto (`<input type="text">`), un checkbox (`<input type="checkbox">`), un radio button (`<input type="radio">`) o un select (`<select>`). Un `FormControl` gestiona el valor actual de esa entrada y su estado de validación (válido, inválido, tocado, sucio, pendiente). Cuando el valor de la entrada cambia, el `FormControl` emite un nuevo valor a través de su Observable `valueChanges`.
2. **FormGroup**: Sirve para agrupar múltiples instancias de `FormControl` (y/o otros `FormGroup` o `FormArray`) en una unidad lógica. Un `FormGroup` te permite gestionar el valor y el estado de validación de un conjunto de controles relacionados. Por ejemplo, un formulario de registro podría tener un `FormGroup` para la información personal (nombre, apellido) y otro `FormGroup` anidado para la información de contacto (email, teléfono). Si todos los controles dentro de un `FormGroup` son válidos, entonces el `FormGroup` en sí será válido.
3. **FormArray**: Agrupa una colección de instancias de `FormControl` o `FormGroup`. Es particularmente útil para formularios donde el número de entradas es dinámico y puede cambiar en tiempo de ejecución. Por ejemplo, si estás construyendo un formulario para añadir múltiples números de teléfono, una lista de habilidades, o los ítems de un pedido en un carrito de compras, un `FormArray` te permite añadir o eliminar controles de forma programática.

### 1.3. Habilitar Formularios Reactivos en tu Aplicación

Para que Angular reconozca y procese las directivas de formularios reactivos en tus plantillas, necesitas importar y proporcionar el módulo correspondiente en la configuración de tu aplicación. Para aplicaciones `standalone` (el enfoque moderno de Angular), esto se hace en `app.config.ts`:

```Typescript
// src/app/app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router';
import { provideHttpClient } from '@angular/common/http';
import { provideReactiveFormsModule } from '@angular/forms'; // ¡Importa esta función!

import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withComponentInputBinding()),
    provideHttpClient(),
    provideReactiveFormsModule() // ¡Provee las funcionalidades de formularios reactivos a toda la aplicación!
  ]
};
```

Además de la configuración global, en cada componente `standalone` donde vayas a usar formularios reactivos, debes importar `ReactiveFormsModule` (o los constructores de formulario individuales como `FormControl`, `FormGroup`, `FormArray` y `Validators` si solo necesitas esos elementos específicos y quieres un bundle más pequeño) y añadirlos al array `imports` de tu decorador `@Component`. `ReactiveFormsModule` exporta todas las directivas y clases necesarias para trabajar con formularios reactivos.

```Typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
// Importa ReactiveFormsModule para usar directivas como [formGroup], formControlName
// También puedes importar los constructores de formulario individualmente si lo prefieres
import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';

@Component({
  selector: 'app-property-form',
  standalone: true,
  imports: [
    CommonModule,
    ReactiveFormsModule // Importa ReactiveFormsModule para usar las directivas de formularios reactivos
  ],
  // ...
})
export class PropertyFormComponent {
  // ...
}
```

### 1.4. Creación de un `FormGroup`

La creación de un `FormGroup` se realiza en el código TypeScript de tu componente. Aquí es donde defines la estructura de tu formulario, especificando cada `FormControl`, su valor inicial y los validadores síncronos o asíncronos asociados.

```Typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';

@Component({
  selector: 'app-property-form',
  standalone: true,
  imports: [
    CommonModule,
    ReactiveFormsModule
  ],
  templateUrl: './property-form.component.html',
  styleUrl: './property-form.component.css'
})
export class PropertyFormComponent implements OnInit {
  // Define el FormGroup para el formulario de propiedad
  // Cada propiedad en este objeto representa un FormControl en el formulario
  propertyForm = new FormGroup({
    // 'address' es un FormControl. Su valor inicial es una cadena vacía.
    // 'Validators.required' es un validador síncrono que hace que el campo sea obligatorio.
    address: new FormControl('', Validators.required),
    city: new FormControl('', Validators.required),
    // 'price' tiene múltiples validadores: obligatorio y un valor mínimo de 1.
    price: new FormControl(0, [Validators.required, Validators.min(1)]),
    bedrooms: new FormControl(0, [Validators.required, Validators.min(0)]),
    bathrooms: new FormControl(0, [Validators.required, Validators.min(0)]),
    imageUrl: new FormControl('', Validators.required),
    description: new FormControl('', Validators.required)
  });

  constructor() { }

  ngOnInit(): void {
    // Los Observables `valueChanges` y `statusChanges` permiten la reactividad.
    // Puedes suscribirte a cambios en el valor del formulario completo.
    // Esto es útil para lógica que depende del estado general del formulario.
    this.propertyForm.valueChanges.subscribe(value => {
      console.log('Cambios en el formulario:', value);
      // Ejemplo: Podrías habilitar/deshabilitar otros campos basándote en estos cambios.
    });

    // O a cambios en un control específico.
    // Esto es útil para validaciones condicionales o para mostrar feedback en tiempo real.
    this.propertyForm.get('address')?.valueChanges.subscribe(address => {
      console.log('Dirección cambiada a:', address);
      // Ejemplo: Podrías hacer una búsqueda de autocompletado aquí.
    });
  }

  // Método que se ejecuta cuando el formulario se envía
  onSubmit(): void {
    // Verifica si el formulario es válido antes de procesar los datos
    if (this.propertyForm.valid) {
      console.log('Formulario válido, enviando datos:', this.propertyForm.value);
      // Aquí es donde normalmente enviarías los datos a un servicio para interactuar con una API.
      // Por ejemplo: this.propertyService.addProperty(this.propertyForm.value);
    } else {
      console.log('Formulario inválido. Por favor, revisa los errores.');
      // Opcional pero recomendado: Marcar explícitamente todos los controles como 'touched'
      // Esto fuerza a Angular a mostrar los mensajes de error de validación para todos los campos,
      // incluso si el usuario no los ha tocado.
      this.propertyForm.markAllAsTouched();
    }
  }
}
```

**Una nota sobre `FormBuilder`**: Para formularios más grandes o complejos, Angular proporciona la clase `FormBuilder`. `FormBuilder` es un servicio inyectable que simplifica la creación de instancias de `FormControl`, `FormGroup` y `FormArray` con una sintaxis más concisa.

```Typescript
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { inject } from '@angular/core'; // Para inyectar FormBuilder en funciones

// ... dentro de tu componente
export class PropertyFormComponent implements OnInit {
  private fb = inject(FormBuilder); // Inyectar FormBuilder

  propertyForm = this.fb.group({
    address: ['', Validators.required], // Valor inicial, luego validadores
    city: ['', Validators.required],
    price: [0, [Validators.required, Validators.min(1)]],
    bedrooms: [0, [Validators.required, Validators.min(0)]],
    bathrooms: [0, [Validators.required, Validators.min(0)]],
    imageUrl: ['', Validators.required],
    description: ['', Validators.required]
  });

  // ... resto del componente
}
```

`FormBuilder` es especialmente útil cuando necesitas crear `FormArray`s o `FormGroup`s anidados, ya que reduce la verbosidad.

### 1.5. Enlazar el `FormGroup` a la Plantilla HTML

Una vez que has definido tu `FormGroup` en el código TypeScript, necesitas vincularlo a tu plantilla HTML. Esto se logra utilizando las directivas de formularios reactivos de Angular:

- `[formGroup]`: Esta directiva se aplica al elemento `<form>` y lo vincula a la instancia de `FormGroup` que has creado en tu componente. Por ejemplo, `[formGroup]="propertyForm"`.
- `formControlName`: Esta directiva se aplica a cada elemento de entrada (`<input>`, `<textarea>`, `<select>`) y lo vincula a un `FormControl` específico dentro de tu `FormGroup`. El valor de `formControlName` debe coincidir con el nombre de la propiedad del `FormControl` en tu `FormGroup`. Por ejemplo, `formControlName="address"`.

```HTML
<div class="container mx-auto p-8 bg-gray-50 min-h-screen flex justify-center items-start">
  <div class="w-full max-w-2xl bg-white rounded-xl shadow-lg p-8">
    <h1 class="text-3xl font-bold text-gray-900 mb-6 text-center">Añadir/Editar Propiedad</h1>

    <!-- El formulario se vincula a la instancia 'propertyForm' definida en el componente TypeScript -->
    <form [formGroup]="propertyForm" (ngSubmit)="onSubmit()" class="space-y-6">
      <!-- Campo Dirección -->
      <div>
        <label for="address" class="block text-sm font-medium text-gray-700 mb-1">Dirección</label>
        <input
          id="address"
          type="text"
          formControlName="address" <!-- Vincula este input al FormControl 'address' -->
          class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
          placeholder="Ej. Calle Falsa 123"
        >
        <!-- Muestra el mensaje de error si el control 'address' es inválido y ha sido tocado (el usuario interactuó con él) -->
        @if (propertyForm.get('address')?.invalid && propertyForm.get('address')?.touched) {
          <p class="mt-1 text-sm text-red-600">La dirección es obligatoria.</p>
        }
      </div>

      <!-- Campo Ciudad -->
      <div>
        <label for="city" class="block text-sm font-medium text-gray-700 mb-1">Ciudad</label>
        <input
          id="city"
          type="text"
          formControlName="city" <!-- Vincula este input al FormControl 'city' -->
          class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
          placeholder="Ej. Bogotá"
        >
        @if (propertyForm.get('city')?.invalid && propertyForm.get('city')?.touched) {
          <p class="mt-1 text-sm text-red-600">La ciudad es obligatoria.</p>
        }
      </div>

      <!-- Campo Precio -->
      <div>
        <label for="price" class="block text-sm font-medium text-gray-700 mb-1">Precio ($)</label>
        <input
          id="price"
          type="number"
          formControlName="price" <!-- Vincula este input al FormControl 'price' -->
          class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
          placeholder="Ej. 350000"
        >
        @if (propertyForm.get('price')?.invalid && propertyForm.get('price')?.touched) {
          <!-- Accede a errores específicos usando propertyForm.get('controlName')?.errors?.['validatorKey'] -->
          @if (propertyForm.get('price')?.errors?.['required']) {
            <p class="mt-1 text-sm text-red-600">El precio es obligatorio.</p>
          }
          @if (propertyForm.get('price')?.errors?.['min']) {
            <p class="mt-1 text-sm text-red-600">El precio debe ser un número positivo.</p>
          }
        }
      </div>

      <!-- Campos Habitaciones y Baños (en una fila) -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="bedrooms" class="block text-sm font-medium text-gray-700 mb-1">Habitaciones</label>
          <input
            id="bedrooms"
            type="number"
            formControlName="bedrooms"
            class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
            placeholder="Ej. 3"
          >
          @if (propertyForm.get('bedrooms')?.invalid && propertyForm.get('bedrooms')?.touched) {
            @if (propertyForm.get('bedrooms')?.errors?.['required']) {
              <p class="mt-1 text-sm text-red-600">El número de habitaciones es obligatorio.</p>
            }
            @if (propertyForm.get('bedrooms')?.errors?.['min']) {
              <p class="mt-1 text-sm text-red-600">El número de habitaciones no puede ser negativo.</p>
            }
          }
        </div>
        <div>
          <label for="bathrooms" class="block text-sm font-medium text-gray-700 mb-1">Baños</label>
          <input
            id="bathrooms"
            type="number"
            formControlName="bathrooms"
            class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
            placeholder="Ej. 2"
          >
          @if (propertyForm.get('bathrooms')?.invalid && propertyForm.get('bathrooms')?.touched) {
            @if (propertyForm.get('bathrooms')?.errors?.['required']) {
              <p class="mt-1 text-sm text-red-600">El número de baños es obligatorio.</p>
            }
            @if (propertyForm.get('bathrooms')?.errors?.['min']) {
              <p class="mt-1 text-sm text-red-600">El número de baños no puede ser negativo.</p>
            }
          }
        </div>
      </div>

      <!-- Campo URL de Imagen -->
      <div>
        <label for="imageUrl" class="block text-sm font-medium text-gray-700 mb-1">URL de Imagen</label>
        <input
          id="imageUrl"
          type="text"
          formControlName="imageUrl"
          class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
          placeholder="Ej. https://ejemplo.com/imagen.jpg"
        >
        @if (propertyForm.get('imageUrl')?.invalid && propertyForm.get('imageUrl')?.touched) {
          <p class="mt-1 text-sm text-red-600">La URL de la imagen es obligatoria.</p>
        }
      </div>

      <!-- Campo Descripción -->
      <div>
        <label for="description" class="block text-sm font-medium text-gray-700 mb-1">Descripción</label>
        <textarea
          id="description"
          formControlName="description"
          rows="4"
          class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
          placeholder="Describe la propiedad..."
        ></textarea>
        @if (propertyForm.get('description')?.invalid && propertyForm.get('description')?.touched) {
          <p class="mt-1 text-sm text-red-600">La descripción es obligatoria.</p>
        }
      </div>

      <!-- Botón de Envío -->
      <div class="flex justify-end">
        <button
          type="submit"
          [disabled]="propertyForm.invalid" <!-- El botón se deshabilita si el formulario no es válido -->
          class="px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          Guardar Propiedad
        </button>
      </div>
    </form>
  </div>
</div>
```

## 2. Validación Eficiente de Formularios (Validadores Síncronos y Asíncronos)

La validación es un aspecto crítico de cualquier formulario, ya que asegura que los datos ingresados por el usuario sean correctos, completos y consistentes antes de ser procesados o enviados al servidor. Los formularios reactivos de Angular proporcionan un sistema de validación robusto y flexible, permitiéndonos aplicar validadores síncronos y asíncronos.

### 2.1. Estados de un `FormControl`

Cada `FormControl` (y por extensión, `FormGroup` y `FormArray`) tiene propiedades que reflejan su estado actual, las cuales son útiles para mostrar feedback al usuario:

- `valid`: `true` si el control pasa todas las validaciones.
- `invalid`: `true` si el control no pasa alguna validación.
- `pending`: `true` si el control está ejecutando una validación asíncrona.
- `disabled`: `true` si el control está deshabilitado.
- `enabled`: `true` si el control está habilitado.
- `pristine`: `true` si el usuario no ha modificado el valor del control.
- `dirty`: `true` si el usuario ha modificado el valor del control.
- `untouched`: `true` si el usuario no ha interactuado con el control (no ha recibido el foco y luego lo ha perdido).
- `touched`: `true` si el usuario ha interactuado con el control (ha recibido el foco y luego lo ha perdido).
- `errors`: Un objeto que contiene los errores de validación actuales, donde la clave es el nombre del validador y el valor es el objeto de error.

### 2.2. Validadores Síncronos: Verificación Inmediata

Los validadores síncronos se ejecutan de forma inmediata cada vez que el valor del control cambia. Son funciones puras que reciben un `AbstractControl` (que puede ser un `FormControl`, `FormGroup` o `FormArray`) y devuelven un objeto de error (`ValidationErrors`) si el control es inválido, o `null` si es válido. Angular proporciona una serie de validadores incorporados en la clase `Validators`, que cubren los casos de uso más comunes:

- `Validators.required`: Asegura que el campo no esté vacío.
- `Validators.minLength(min)`: El valor de la cadena debe tener al menos `min` caracteres.
- `Validators.maxLength(max)`: El valor de la cadena debe tener como máximo `max` caracteres.
- `Validators.pattern(regex)`: El valor debe coincidir con una expresión regular específica. Útil para formatos de email, números de teléfono, etc.
- `Validators.min(value)`: Para campos numéricos, el valor debe ser mayor o igual a `value`.
- `Validators.max(value)`: Para campos numéricos, el valor debe ser menor o igual a `value`.
- `Validators.email`: Valida que el valor sea una dirección de correo electrónico con un formato básico.

Puedes aplicar múltiples validadores a un `FormControl` pasándolos como un array al segundo argumento del constructor de `FormControl`:

```Typescript
new FormControl(
  '', // Valor inicial
  [
    Validators.required, // El campo es obligatorio
    Validators.minLength(5), // Mínimo 5 caracteres
    Validators.maxLength(50), // Máximo 50 caracteres
    Validators.pattern(/^[a-zA-Z ]*$/) // Solo letras y espacios
  ]
);
```

```Typescript
new FormControl(
  null, // Valor inicial nulo
  [
    Validators.required,
    Validators.min(100), // Valor mínimo 100
    Validators.max(1000) // Valor máximo 1000
  ]
);
```

### 2.3. Validadores Personalizados Síncronos: Lógica de Negocio Específica

Cuando los validadores incorporados no son suficientes, puedes crear tus propios validadores síncronos. Estos son simplemente funciones que implementan la interfaz `ValidatorFn`. Reciben un `AbstractControl` y deben devolver un objeto `ValidationErrors` (que es un objeto donde las claves son los nombres de los errores, ej. `{ 'miErrorPersonalizado': true }`) si la validación falla, o `null` si es exitosa.

#### Ejemplo de validador personalizado para un rango de precios específico

```Typescript
import { AbstractControl, ValidatorFn, ValidationErrors } from '@angular/forms';

/**
 * Validador personalizado que verifica si un precio está dentro de un rango dado.
 * @param min El precio mínimo permitido.
 * @param max El precio máximo permitido.
 * @returns Una función validadora.
 */
export function priceRangeValidator(min: number, max: number): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const price = control.value;

    // Si el valor es nulo o no es un número, y no es un campo requerido, se considera válido aquí.
    // Si es requerido, Validators.required lo manejará.
    if (price === null || isNaN(price)) {
      return null;
    }

    // Si el precio está fuera del rango, devuelve un objeto de error
    if (price < min || price > max) {
      return {
        priceRange: {
          min: min,
          max: max,
          actual: price,
          message: `El precio debe estar entre $${min} y $${max}.`
        }
      };
    }
    // Si el precio está dentro del rango, devuelve null (válido)
    return null;
  };
}
```

Uso en un `FormControl` en tu componente:

```Typescript
new FormControl(0, [Validators.required, priceRangeValidator(10000, 1000000)]);
```

#### Otro ejemplo: validador para que dos campos coincidan (ej. contraseña y confirmar contraseña)

```Typescript
export function matchPasswordValidator(controlName: string, matchingControlName: string): ValidatorFn {
  return (formGroup: AbstractControl): ValidationErrors | null => {
    const control = formGroup.get(controlName);
    const matchingControl = formGroup.get(matchingControlName);

    if (!control || !matchingControl) {
      return null; // Si los controles no existen, no se puede validar
    }

    // Si el control de confirmación tiene errores previos de otro validador, no sobrescribir
    if (matchingControl.errors && !matchingControl.errors['mismatch']) {
      return null;
    }

    // Establecer error si las contraseñas no coinciden
    if (control.value !== matchingControl.value) {
      matchingControl.setErrors({ mismatch: true });
      return { mismatch: true };
    } else {
      matchingControl.setErrors(null); // Limpiar error si coinciden
      return null;
    }
  };
}
```

Uso en un `FormGroup`:

```Typescript
this.userForm = new FormGroup({
  password: new FormControl('', Validators.required),
  confirmPassword: new FormControl('', Validators.required)
}, { validators: matchPasswordValidator('password', 'confirmPassword') });
```

### 2.4. Validadores Asíncronos: Verificación con Operaciones Asíncronas

Los validadores asíncronos son necesarios para validaciones que no pueden resolverse de forma inmediata, como verificar la unicidad de un nombre de usuario o un email en una base de datos a través de una llamada a una API. A diferencia de los síncronos, devuelven una `Promise<ValidationErrors | null>` o un `Observable<ValidationErrors | null>`. Angular esperará a que esta Promesa u Observable se resuelva antes de determinar el estado de validez del control. Durante este tiempo, el control tendrá el estado `pending`.

#### Ejemplo de validador asíncrono (simulando una llamada a API para verificar unicidad de dirección)

```Typescript
import { AbstractControl, AsyncValidatorFn, ValidationErrors } from '@angular/forms';
import { Observable, of, timer } from 'rxjs'; // Importa timer
import { map, catchError, switchMap } from 'rxjs/operators'; // Importa switchMap
import { PropertyService } from '../services/property.service'; // Asume que tienes un servicio

/**
 * Validador asíncrono para verificar la unicidad de una dirección.
 * Simula una llamada a la API.
 * @param propertyService El servicio para interactuar con las propiedades.
 * @param currentPropertyId Opcional. El ID de la propiedad actual si estamos en modo edición,
 * para permitir que la dirección original del elemento sea "única" para sí misma.
 * @returns Una función validadora asíncrona.
 */
export function uniqueAddressValidator(propertyService: PropertyService, currentPropertyId?: number): AsyncValidatorFn {
  return (control: AbstractControl): Promise<ValidationErrors | null> | Observable<ValidationErrors | null> => {
    if (!control.value) {
      return of(null); // Si el campo está vacío, es válido (si 'required' no está presente)
    }

    // Utilizamos `timer` para introducir un debounce, evitando llamadas excesivas a la API
    // mientras el usuario está escribiendo. `switchMap` cancela la petición anterior
    // si una nueva se inicia antes de que la anterior termine.
    return timer(500).pipe( // Espera 500ms después de la última pulsación
      switchMap(() => {
        const address = control.value.toLowerCase();
        // Simulación de la llamada a la API:
        // En un caso real, propertyService.checkAddressUnique(address, currentPropertyId)
        // devolvería un Observable<boolean> o Promise<boolean>
        // Por ejemplo:
        // return propertyService.checkAddressUnique(address, currentPropertyId).pipe(
        //   map(isUnique => (isUnique ? null : { uniqueAddress: true })),
        //   catchError(() => of(null)) // Manejar errores de la API como 'válido' para no bloquear el formulario
        // );

        // Para fines de demostración, simulemos que "calle 123" no es única
        if (address === 'calle 123' && currentPropertyId !== 1) { // Asume que la propiedad con ID 1 tiene "calle 123"
          return of({ uniqueAddress: true }); // Devuelve un error si no es único
        }
        return of(null); // Es único
      }),
      catchError(() => of(null)) // Manejar errores de la llamada API como válido para no bloquear el formulario
    );
  };
}
```

Uso en un `FormControl` (en el componente):

```Typescript
import { inject } from '@angular/core';

// En el constructor o ngOnInit, necesitas el inyector para el servicio
const propertyService = inject(PropertyService);
this.propertyForm = new FormGroup({
  address: new FormControl('', {
    validators: Validators.required,
    asyncValidators: uniqueAddressValidator(propertyService), // Pasa el servicio
    updateOn: 'blur' // O 'change' si quieres validación en tiempo real
  }),
  // ...otros controles
});
```

#### Consideraciones importantes al usar validadores asíncronos

- `updateOn`: Por defecto, los validadores se ejecutan en cada evento `change` (síncronos) o `blur` (asíncronos). Para validadores asíncronos que hacen llamadas a la API, es altamente recomendable usar `updateOn: 'blur'` (cuando el campo pierde el foco) o `updateOn: 'submit'` para evitar un exceso de peticiones mientras el usuario escribe. Puedes configurar esto en el tercer argumento del constructor de `FormControl` o `FormGroup`.
- **Estado `pending`**: Mientras el validador asíncrono está en ejecución, el control tendrá la propiedad `pending` en `true`. Puedes usar esto en tu plantilla para mostrar un indicador de carga (ej., "Verificando unicidad...").
- **Manejo de errores**: Es crucial que tu validador asíncrono maneje los errores de la API (usando `catchError` en el pipe de RxJS) y devuelva un `Observable` que emita `null` (para que la validación no falle por un error de red) o un error específico si la API devuelve un mensaje de error de validación.

### 2.5. Mostrar Errores de Validación en la Plantilla HTML

Para proporcionar una buena experiencia de usuario, debes mostrar mensajes de error claros cuando los campos del formulario son inválidos. Puedes acceder al estado de validación de un control de formulario (`invalid`, `touched`, `dirty`, `errors`) en tu plantilla utilizando la sintaxis `controlName?.invalid` o `controlName?.errors?.['validatorKey']`.

```HTML
<div>
  <label for="username">Nombre de Usuario</label>
  <input id="username" type="text" formControlName="username">
  <!-- El bloque @if se muestra si el control es inválido y el usuario ya lo ha tocado -->
  @if (propertyForm.get('username')?.invalid && propertyForm.get('username')?.touched) {
    <!-- Condición para el error 'required' -->
    @if (propertyForm.get('username')?.errors?.['required']) {
      <p class="text-red-500 text-sm mt-1">El nombre de usuario es obligatorio.</p>
    }
    <!-- Condición para el error 'minlength' -->
    @if (propertyForm.get('username')?.errors?.['minlength']) {
      <p class="text-red-500 text-sm mt-1">
        El nombre de usuario debe tener al menos {{ propertyForm.get('username')?.errors?.['minlength']?.requiredLength }} caracteres.
      </p>
    }
    <!-- Condición para el error 'uniqueAddress' (de un validador asíncrono) -->
    @if (propertyForm.get('username')?.errors?.['uniqueAddress']) {
      <p class="text-red-500 text-sm mt-1">Este nombre de usuario ya está en uso.</p>
    }
  }
  <!-- Muestra un mensaje de "Verificando..." mientras la validación asíncrona está pendiente -->
  @if (propertyForm.get('username')?.pending) {
    <p class="text-blue-500 text-sm mt-1">Verificando unicidad...</p>
  }
</div>
```

Puedes considerar crear un componente de "control de formulario" reutilizable o una directiva para encapsular la lógica de visualización de errores y reducir la duplicación en tus plantillas.

## 3. Gestión Avanzada de Estado con Servicios y Signals

En aplicaciones complejas, el manejo del estado puede volverse un desafío significativo. A medida que la aplicación crece, los datos necesitan ser compartidos entre múltiples componentes, y las actualizaciones de estado deben ser consistentes y predecibles. Si el estado se dispersa entre los componentes, puede llevar a problemas como la "prop drilling" (pasar props a través de muchos niveles de componentes), inconsistencias de datos y dificultades para depurar. Los **Servicios** combinados con **Signals** (y `computed`/`effect`) ofrecen una solución potente y moderna para la gestión de estado en Angular, proporcionando un flujo de datos reactivo, centralizado y fácil de entender.

### 3.1. Principios de Gestión de Estado Centralizada: La Base de la Escalabilidad

Adoptar un patrón de gestión de estado centralizada se basa en varios principios fundamentales:

- **Fuente Única de Verdad (Single Source of Truth - SSOT)**: Para cada segmento particular del estado de la aplicación, debe haber un único lugar donde se almacene y se gestione. Por ejemplo, un `PropertyService` sería la SSOT para todos los datos relacionados con las propiedades inmobiliarias, mientras que un `UserService` lo sería para el usuario autenticado y su información. Esto elimina la ambigüedad sobre dónde se encuentran los datos y cómo se modifican.
- **Inmutabilidad**: Cuando se actualiza el estado, siempre se debe crear una _nueva instancia_ del objeto o array que contiene los datos, en lugar de mutar el existente directamente. Este principio es crucial por varias razones:
  - **Detección de Cambios**: Angular (especialmente con `OnPush` o Signals) optimiza la detección de cambios al comparar referencias de objetos. Si mutas un objeto existente, Angular podría no detectar el cambio, lo que llevaría a que la interfaz de usuario no se actualice. Al crear una nueva instancia, la referencia cambia, y Angular (o las Signals) detectan el cambio de manera eficiente.
  - **Previsibilidad**: La inmutabilidad hace que el flujo de datos sea más predecible, ya que el estado anterior no puede ser modificado por error por diferentes partes de la aplicación.
  - **Facilidad de Depuración**: Es más fácil rastrear los cambios de estado cuando cada modificación resulta en una nueva versión del estado.
- **Reactividad**: Los componentes deben reaccionar a los cambios en el estado del servicio de forma automática. Aquí es donde las Signals brillan. Un servicio expone Signals de solo lectura, y los componentes que las consumen se actualizan automáticamente cuando el valor de la Signal cambia, sin necesidad de suscripciones manuales o gestión de `ChangeDetectionRef`.
- **Separación de Preocupaciones**: Este es un principio fundamental de la arquitectura de Angular. Los servicios son responsables de gestionar el estado, la lógica de negocio, la interacción con APIs y la persistencia de datos. Los componentes, por otro lado, solo se preocupan por consumir ese estado, mostrarlo en la interfaz de usuario y emitir eventos de usuario que el servicio puede procesar. Esta clara división hace que cada parte de la aplicación sea más modular, fácil de entender y de probar.

### 3.2. Ejemplo: Un Servicio de Carrito de Compras con Signals y Persistencia

Para ilustrar la gestión avanzada de estado, consideremos un `CartService` que gestiona los ítems en un carrito de compras. Este servicio no solo mantendrá el estado del carrito, sino que también lo persistirá en `localStorage` para que los ítems no se pierdan al recargar la página.

```Typescript
import { Injectable, signal, computed, effect } from '@angular/core';

export interface CartItem {
  productId: number;
  name: string;
  price: number;
  quantity: number;
}

@Injectable({
  providedIn: 'root' // El servicio es un singleton a nivel de aplicación
})
export class CartService {
  // Signal privada para el estado interno del carrito.
  // Es privada para que el acceso directo desde fuera del servicio sea solo a través de métodos controlados.
  private _cartItems = signal<CartItem[]>([]);

  // Signal pública de solo lectura para que los componentes la consuman.
  // Los componentes pueden leer el estado del carrito pero no modificarlo directamente.
  cartItems = this._cartItems.asReadonly();

  // Signal computada para el total del carrito.
  // Se recalcula automáticamente cada vez que _cartItems cambia.
  cartTotal = computed(() =>
    this._cartItems().reduce((total, item) => total + (item.price * item.quantity), 0)
  );

  // Signal computada para el número total de ítems distintos en el carrito
  itemCount = computed(() => this._cartItems().length);

  constructor() {
    // Al inicializar el servicio, intentamos cargar el carrito desde localStorage.
    // Esto asegura que el estado persista entre recargas de página.
    const savedCart = localStorage.getItem('cart');
    if (savedCart) {
      try {
        // Parseamos la cadena JSON a un objeto y actualizamos la Signal
        this._cartItems.set(JSON.parse(savedCart));
        console.log('Carrito cargado desde localStorage.');
      } catch (e) {
        console.error('Error al parsear el carrito de localStorage:', e);
        // Si hay un error de parseo (ej., datos corruptos), limpiamos localStorage.
        localStorage.removeItem('cart');
      }
    }

    // Un efecto que se ejecuta cada vez que _cartItems cambia.
    // Este efecto es responsable de persistir el estado del carrito en localStorage.
    // 'allowSignalWrites: true' es necesario si el efecto va a modificar otras signals,
    // pero aquí solo estamos guardando en localStorage, lo cual es un efecto secundario.
    effect(() => {
      this.saveCartToLocalStorage();
      console.log('Carrito actualizado y guardado en localStorage.');
    });
  }

  /**
   * Añade un producto al carrito. Si ya existe, incrementa la cantidad.
   * @param item El producto a añadir (con productId, name, price).
   */
  addItem(item: { productId: number; name: string; price: number }): void {
    this._cartItems.update(currentItems => {
      const existingItem = currentItems.find(i => i.productId === item.productId);
      if (existingItem) {
        // Si el ítem ya existe, actualiza su cantidad de forma inmutable
        return currentItems.map(i =>
          i.productId === item.productId ? { ...i, quantity: i.quantity + 1 } : i
        );
      } else {
        // Si es un nuevo ítem, añádelo de forma inmutable
        return [...currentItems, { ...item, quantity: 1 }];
      }
    });
    // La persistencia se maneja automáticamente por el efecto.
  }

  /**
   * Reduce la cantidad de un producto en el carrito, o lo elimina si la cantidad llega a cero.
   * @param productId El ID del producto a modificar.
   */
  decreaseItemQuantity(productId: number): void {
    this._cartItems.update(currentItems => {
      return currentItems.flatMap(item => {
        if (item.productId === productId) {
          if (item.quantity > 1) {
            return [{ ...item, quantity: item.quantity - 1 }]; // Decrementar cantidad
          } else {
            return []; // Eliminar ítem si la cantidad es 1
          }
        }
        return [item]; // Mantener otros ítems sin cambios
      });
    });
  }

  /**
   * Elimina un producto completamente del carrito.
   * @param productId El ID del producto a eliminar.
   */
  removeItem(productId: number): void {
    this._cartItems.update(currentItems =>
      currentItems.filter(item => item.productId !== productId)
    );
    // La persistencia se maneja automáticamente por el efecto.
  }

  /**
   * Vacía completamente el carrito.
   */
  clearCart(): void {
    this._cartItems.set([]);
    // La persistencia se maneja automáticamente por el efecto.
  }

  // Método privado para guardar el estado actual del carrito en localStorage
  private saveCartToLocalStorage(): void {
    localStorage.setItem('cart', JSON.stringify(this._cartItems()));
  }
}
```

En este ejemplo, el `CartService` encapsula toda la lógica del carrito, incluyendo la adición, eliminación y actualización de ítems, así como el cálculo del total y la persistencia. Los componentes que necesiten interactuar con el carrito solo necesitan inyectar `CartService` y:

- Acceder a `cartItems()` y `cartTotal()` para mostrar el estado actual del carrito en la interfaz de usuario.
- Llamar a métodos como `addItem()`, `removeItem()` o `clearCart()` para modificar el estado del carrito.

Las Signals se encargan de la reactividad: cualquier componente que esté usando `cartItems()` o `cartTotal()` se actualizará automáticamente cuando el estado interno del carrito cambie, sin que el componente tenga que preocuparse por suscripciones o detección de cambios manual.

## 4. Persistencia y Recuperación del Estado del Cliente (Almacenamiento Local, de Sesión y Cookies)

La persistencia del estado del cliente es la capacidad de una aplicación web para guardar datos en el navegador del usuario, de modo que estos datos no se pierdan cuando el usuario cierra la pestaña, el navegador o recarga la página. Esto es fundamental para mejorar la experiencia del usuario, ya que evita la necesidad de reintroducir información o reconfigurar preferencias en cada visita. Las APIs de almacenamiento web, `localStorage` y `sessionStorage`, junto con las `Cookies`, son las herramientas estándar para lograr esto.

### 4.1. `localStorage` vs. `sessionStorage`: ¿Cuál Elegir?

Ambas APIs permiten almacenar pares clave-valor en el navegador, pero difieren en su ciclo de vida y alcance:

#### `localStorage`: Almacenamiento Persistente y Compartido

`localStorage` es parte de la Web Storage API y proporciona una forma de almacenar pares clave-valor en el navegador del usuario.

- **Persistencia**: Los datos almacenados en `localStorage` **persisten indefinidamente**, incluso después de que el usuario cierra el navegador o el sistema operativo. No tienen fecha de caducidad.
- **Alcance**: Los datos son **accesibles desde todas las ventanas y pestañas** del mismo origen (dominio y protocolo). Esto significa que si abres múltiples pestañas de tu aplicación, todas compartirán los mismos datos de `localStorage`.
- **Capacidad**: Generalmente, `localStorage` permite almacenar alrededor de 5 MB a 10 MB de datos por origen, lo cual es considerablemente más que las cookies.
- **API Básica**:
  - `localStorage.setItem(key: string, value: string)`: Guarda un par clave-valor. Es crucial recordar que tanto la `key` como el `value` deben ser cadenas de texto. Si intentas guardar un objeto o un número directamente, se convertirán implícitamente a cadenas, lo que puede llevar a resultados inesperados al recuperarlos.
  - `localStorage.getItem(key: string)`: Recupera el valor asociado a una clave. Si la clave no existe, devuelve `null`.
  - `localStorage.removeItem(key: string)`: Elimina un par clave-valor.
  - localStorage.clear(): Elimina todos los pares clave-valor del `localStorage` del origen actual. **¡Úsalo con precaución!**
- **Manejo de Objetos y Arrays**: Dado que el almacenamiento web solo guarda cadenas, si necesitas almacenar objetos JavaScript (como un array de ítems de carrito o un objeto de configuración de usuario), debes serializarlos a una cadena JSON antes de guardarlos y deserializarlos al recuperarlos.
- **Casos de Uso Ideales**: Es ideal para almacenar preferencias de usuario (ej., tema oscuro/claro), tokens de autenticación (con **extrema precaución** y medidas de seguridad adicionales), configuraciones de la aplicación, o cualquier dato que necesite persistir entre sesiones del usuario.

#### `sessionStorage`:Almacenamiento Temporal por Sesión

`sessionStorage` también forma parte de la Web Storage API y es muy similar a localStorage, pero con una diferencia crucial en su ciclo de vida.

- **Persistencia**: Los datos almacenados en `sessionStorage` **solo persisten mientras la pestaña o ventana del navegador esté abierta**. Los datos se eliminan automáticamente cuando se cierra la pestaña/ventana (o cuando se recarga la página si es una recarga completa que no mantiene el estado de la sesión, como un cierre y reapertura del navegador). Si el usuario duplica una pestaña, la nueva pestaña hereda los datos de `sessionStorage` de la original.
- **Alcance**: Los datos son **accesibles solo desde la pestaña/ventana que los creó**. Si abres una nueva pestaña de la misma aplicación, tendrá su propia instancia de `sessionStorage`, aislada de las demás.
- **Capacidad**: Similar a `localStorage`, generalmente permite almacenar alrededor de 5 MB a 10 MB de datos por origen y por pestaña.
- **API Básica**: La API es idéntica a la de `localStorage` (`sessionStorage.setItem()`, `sessionStorage.getItem()`, etc.).
- **Casos de Uso Ideales**: Es ideal para almacenar datos temporales específicos de la sesión actual, como el estado de un formulario de varios pasos, información de navegación dentro de una sesión, o datos que no deben compartirse entre diferentes pestañas.

### 4.2. `Cookies`: Pequeños Pares Clave-Valor Enviados con Cada Petición

Las `Cookies` son el mecanismo de almacenamiento de estado más antiguo y fundamental en la web. Son pequeños fragmentos de datos que un servidor envía al navegador web del usuario, y el navegador los almacena y los devuelve al mismo servidor con cada solicitud HTTP subsiguiente.

- **Persistencia**: Las cookies pueden ser **de sesión** (se eliminan cuando se cierra el navegador) o **persistentes** (tienen una fecha de caducidad y se mantienen hasta esa fecha o hasta que se eliminan manualmente).
- **Alcance**: Las cookies están asociadas a un dominio y una ruta específicos. Se envían automáticamente con cada solicitud HTTP al servidor que las estableció (o a un subdominio si se configura así).
- **Capacidad**: Las cookies tienen una capacidad de almacenamiento muy limitada, típicamente alrededor de **4 KB por dominio**, y hay un límite en el número de cookies por dominio (generalmente 20-50).
- **API Básica**: Aunque se pueden manipular directamente a través de `document.cookie` en JavaScript, esta API es engorrosa y propensa a errores. La mayoría de las veces, las cookies se gestionan en el servidor o a través de librerías cliente que simplifican su uso.

```Typescript
// Ejemplo básico de lectura y escritura de cookies con document.cookie (no recomendado para uso complejo)
document.cookie = "username=John Doe; expires=Thu, 18 Dec 2025 12:00:00 UTC; path=/";
console.log(document.cookie); // "username=John Doe"
```

- **Características Clave**:
  - **Envío Automático**: Se envían automáticamente con cada solicitud HTTP al servidor, lo que las hace ideales para la gestión de sesiones (autenticación) y seguimiento de usuarios.
  - **`HttpOnly` Flag**: Una cookie marcada como `HttpOnly` no puede ser accedida por JavaScript del lado del cliente (`document.cookie`). Esto es una medida de seguridad crucial para prevenir ataques de Cross-Site Scripting (XSS), ya que un atacante no podría robar el token de sesión si está en una cookie `HttpOnly`.
  - **`Secure` Flag**: Una cookie marcada como `Secure` solo se envía al servidor a través de conexiones HTTPS cifradas. Esto protege la cookie de ser interceptada en tránsito.
  - **`SameSite` Attribute**: Controla cuándo las cookies se envían con solicitudes de origen cruzado, ayudando a mitigar ataques de Cross-Site Request Forgery (CSRF).
- **Casos de Uso Ideales**: Gestión de sesiones de usuario (autenticación, carrito de compras persistente), seguimiento de usuarios (aunque cada vez más restringido por la privacidad), personalización básica del sitio web (ej., recordar el idioma preferido).

### 4.3. Comparación de Mecanismos de Persistencia del Cliente

| **Característica** | `localStorage` | `sessionStorage` | `Cookies` |
| --- | --- | --- | --- |
| **Persistencia** | Indefinida (hasta borrado manual/programático) | Hasta que la pestaña/ventana se cierra | Sesión o persistente (con fecha de caducidad) |
| **Alcance** | Todas las pestañas/ventanas del mismo origen | Solo la pestaña/ventana que la creó | Dominio y ruta específicos (se envían al servidor) |
| **Capacidad** | 5-10 MB | 5-10 MB | ~4 KB por dominio |
| **Envío al Servidor** | No automático (solo vía JS) | No automático (solo vía JS) | Automático con cada solicitud HTTP al dominio |
| **Acceso JS** | Sí (`localStorage.getItem`/`setItem`) | Sí (`sessionStorage.getItem`/`setItem`) | Sí (`document.cookie`), pero engorroso y menos seguro |
| **Uso Principal** | Preferencias de usuario, caché de datos, estado de app persistente | Estado temporal de sesión, formularios multipaso | Gestión de sesiones (autenticación), seguimiento, personalización básica |
| **Seguridad** | Vulnerable a XSS | Vulnerable a XSS | Puede ser `HttpOnly` (protege contra XSS), `Secure`, `SameSite` |

### 4.4. Integración con Servicios y Signals para Persistencia Automática

La forma más limpia y eficiente de integrar `localStorage` o `sessionStorage` con la gestión de estado de tu aplicación Angular es a través de tus servicios, en combinación con Signals. El patrón general es el siguiente:

1. **Carga Inicial (en el `constructor` del servicio)**: Al inicializar el servicio, intenta cargar el estado desde el mecanismo de almacenamiento elegido (ej., `localStorage`). Si los datos existen y son válidos, úsalos para inicializar las Signals del servicio. Esto asegura que la aplicación "recuerde" el estado de la sesión anterior.
2. **Guardado Automático (con un `effect` de Signals)**: Utiliza un `effect` de Signals para observar los cambios en las Signals que representan el estado que deseas persistir. Cada vez que estas Signals cambien, el `effect` se ejecutará y guardará el estado actualizado en el almacenamiento (ej., `localStorage`). Esto automatiza la persistencia y asegura que el almacenamiento siempre esté sincronizado con el estado actual de la aplicación.

Como se demostró en el ejemplo detallado del `CartService` en la sección anterior, este patrón permite que la persistencia sea transparente para los componentes, los cuales solo interactúan con el servicio para leer y modificar el estado, sin preocuparse por los detalles de cómo se guarda o se carga. Para `Cookies`, la gestión suele ser más del lado del servidor o a través de interceptores HTTP en Angular para añadir/leer encabezados de cookies de forma segura.

## 5. Optimización del Rendimiento y Mantenibilidad en Angular

Escribir código funcional es solo el primer paso en el desarrollo de aplicaciones web. Para aplicaciones a gran escala o aquellas que buscan ofrecer la mejor experiencia de usuario, la optimización del rendimiento y la mantenibilidad son tan cruciales como la funcionalidad misma. Un código bien optimizado y fácil de mantener no solo mejora la velocidad y la fluidez de la aplicación, sino que también reduce los costos de desarrollo a largo plazo, facilita la incorporación de nuevos miembros al equipo y permite una evolución más ágil del producto.

### 5.1. Estrategia de Detección de Cambios `OnPush`

Angular utiliza un mecanismo de detección de cambios para mantener la interfaz de usuario sincronizada con el estado de la aplicación. Por defecto, Angular usa la estrategia `Default`, que es muy robusta: verifica si hay cambios en todos los componentes de arriba hacia abajo cada vez que ocurre un evento (clic del usuario, temporizador, respuesta de una petición HTTP, etc.). Si bien esto es seguro, puede volverse ineficiente en aplicaciones con muchos componentes o actualizaciones frecuentes.

La estrategia `OnPush` es una optimización de rendimiento clave. Al aplicar `changeDetection: ChangeDetectionStrategy.OnPush` a un componente, le indicas a Angular que este componente solo debe ser verificado para cambios si:

- Uno de sus `@Input()` cambia (y el cambio debe ser por **referencia**, no por mutación interna del objeto).
- Un evento es disparado desde el propio componente o uno de sus hijos (ej., un `(click)` en un botón dentro de la plantilla del componente).
- Se ejecuta manualmente `ChangeDetectorRef.detectChanges()` (para forzar una verificación) o `ChangeDetectorRef.markForCheck()` (para marcar el componente y sus ancestros como "sucios" para la próxima verificación).
- Un `async` pipe en la plantilla emite un nuevo valor.

```Typescript
import { Component, ChangeDetectionStrategy, Input } from '@angular/core';

@Component({
  selector: 'app-my-component',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush, // Aplica la estrategia OnPush
  template: `
    <p>Nombre: {{ user.name }}</p>
    <button (click)="updateName()">Actualizar Nombre</button>
  `
})
export class MyComponent {
  @Input() user: { name: string } = { name: 'Invitado' };

  updateName(): void {
    // Esto NO activará la detección de cambios OnPush porque muta el objeto existente:
    // this.user.name = 'Nuevo Nombre';

    // Esto SÍ activará la detección de cambios OnPush porque crea una nueva referencia:
    this.user = { ...this.user, name: 'Nuevo Nombre' };
  }
}
```

**Relación con Signals**: Las Signals, introducidas en Angular 16, son una mejora fundamental en la reactividad y el rendimiento. Por su naturaleza, las Signals permiten que Angular realice actualizaciones de DOM mucho más granulares. Cuando el valor de una Signal cambia, Angular sabe exactamente qué `computed` Signals o `effects` dependen de ella, y, lo que es más importante, qué partes específicas del DOM necesitan ser actualizadas. Esto contrasta con el modelo de detección de cambios basado en zonas (Zone.js) que a menudo lleva a re-renderizaciones de componentes enteros, incluso si solo una pequeña parte de sus datos ha cambiado.  
Con Signals, la re-renderización se limita solo a las partes afectadas, lo que resulta en una interfaz de usuario más rápida y fluida, y un menor consumo de recursos. Por lo tanto, `OnPush` no es estrictamente necesario para que las Signals funcionen eficientemente. Sin embargo, sigue siendo una buena práctica usar `OnPush` en componentes que también tienen `@Input()` tradicionales o lógica mixta, ya que optimiza la detección de cambios para esas partes del componente.

### 5.2. Lazy Loading (Carga Perezosa)

El Lazy Loading es la técnica de optimización más efectiva para reducir el tamaño del bundle inicial de la aplicación. Al cargar módulos, componentes o rutas solo cuando son necesarios (es decir, cuando el usuario navega a ellos), se reduce la cantidad de código JavaScript que el navegador tiene que descargar y ejecutar al inicio. Esto se traduce en un tiempo de carga inicial significativamente más rápido y una mejor experiencia para el usuario, especialmente en dispositivos móviles o con conexiones a internet lentas.

### 5.3. Función `trackBy` para `@for`

Cuando renderizas listas grandes de elementos en la plantilla utilizando la instrucción de bucle `@for`, Angular por defecto recrea los elementos del DOM si el orden de los datos cambia o si se añaden/eliminan elementos en el medio de la lista. Esto puede ser ineficiente. La función `trackBy` le dice a Angular cómo identificar de forma única cada elemento en la lista, incluso si el orden cambia. Al usar `trackBy`, Angular solo recrea o actualiza los elementos del DOM que realmente han cambiado, en lugar de recrear toda la lista.

```HTML
<!-- Sin trackBy: Angular puede recrear todos los elementos si el orden cambia o si se insertan/eliminan elementos -->
<!-- <div *ngFor="let item of items">{{ item.name }}</div> -->

<!-- Con trackBy: Angular solo recrea/actualiza los elementos que realmente han cambiado -->
@for (item of items; track trackById(item)) {
  <div>{{ item.name }}</div>
}
```

```Typescript
// En el componente
export class MyComponent {
  items = [
    { id: 1, name: 'Elemento A' },
    { id: 2, name: 'Elemento B' },
    { id: 3, name: 'Elemento C' }
  ];

  // Función trackBy que usa un identificador único (ej. 'id' del objeto)
  trackById(index: number, item: any): number {
    return item.id; // Retorna el ID único de cada elemento
  }

  // Ejemplo de cómo un cambio de orden sin trackBy recrearía el DOM
  // y con trackBy solo movería los elementos existentes.
  shuffleItems(): void {
    this.items = [
      { id: 3, name: 'Elemento C' },
      { id: 1, name: 'Elemento A' },
      { id: 2, name: 'Elemento B' }
    ];
  }
}
```

### 5.4. Signals para una Reactividad Granular y Eficiente

Las Signals, introducidas en Angular 16, son una mejora fundamental en la reactividad y el rendimiento. Por su naturaleza, las Signals permiten que Angular realice actualizaciones de DOM mucho más granulares. Cuando el valor de una Signal cambia, Angular sabe exactamente qué `computed` Signals o `effects` dependen de ella, y, lo que es más importante, qué partes específicas del DOM necesitan ser actualizadas. Esto contrasta con el modelo de detección de cambios basado en zonas (Zone.js) que a menudo lleva a re-renderizaciones de componentes enteros, incluso si solo una pequeña parte de sus datos ha cambiado. Con Signals, la re-renderización se limita solo a las partes afectadas, lo que resulta en una interfaz de usuario más rápida y fluida, y un menor consumo de recursos.

### 5.5. Tree-shaking y Componentes Standalone

- **Tree-shaking**: Es un proceso de optimización del tamaño del bundle que se realiza durante la fase de compilación. Elimina el código no utilizado de tu aplicación. Si importas una librería completa pero solo usas una pequeña parte de ella, el tree-shaking se encarga de eliminar el resto del código que no se referencia.
- **Componentes Standalone y `providedIn: 'root'`**: El enfoque `standalone` en Angular, junto con la propiedad `providedIn: 'root'` para los servicios, promueve un mejor tree-shaking. Al hacer que las dependencias sean más explícitas y eliminando la necesidad de `NgModules` (que a veces podían arrastrar código no utilizado debido a sus importaciones), el compilador de Angular puede identificar y eliminar el código muerto de manera más efectiva, resultando en bundles de aplicación más pequeños y tiempos de carga más rápidos.

### 5.6. Organización del Código: La Clave para la Mantenibilidad a Largo Plazo

Una estructura de código bien organizada es fundamental para la mantenibilidad, la escalabilidad y la colaboración en equipos grandes.

- **Estructura de Carpetas por Característica**: Organiza tu código en carpetas lógicas basadas en las características o dominios de tu aplicación (ej., `features/properties`, `features/courses`, `features/auth`, `shared/components`, `shared/services`). Esto hace que sea fácil encontrar el código relacionado y entender la funcionalidad de cada parte de la aplicación.
- **Archivos de Barril (Barrel Files) (`index.ts`)**: Dentro de cada carpeta de característica, puedes crear un archivo `index.ts` (conocido como "archivo de barril"). Este archivo exporta todos los componentes, servicios, pipes, modelos, etc., de esa carpeta. Esto simplifica las importaciones en otros archivos, ya que puedes importar múltiples elementos desde una sola ruta, en lugar de tener que especificar rutas largas para cada archivo.

  ```Typescript
  // src/app/features/properties/index.ts
  export * from './components/property-list/property-list.component';
  export * from './components/property-detail/property-detail.component';
  export * from './components/property-form/property-form.component';
  export * from './services/property.service';
  export * from './models/property.model';
  export * from './properties.routes';
  ```

  Luego, en otro archivo:

  ```Typescript
  import { PropertyListComponent, PropertyService } from '../features/properties';
  ```

- **Convenciones de Nomenclatura Consistentes**: Sigue las guías de estilo de Angular (ej., sufijos `Component`, `Service`, `Pipe`, `Directive`, `Model`, `Route)` y establece convenciones de nomenclatura claras para variables, funciones y clases. La consistencia mejora la legibilidad y la comprensión del código para todo el equipo.

### 5.7. Linting y Análisis Estático: Guardianes de la Calidad del Código

- Utiliza herramientas como **ESLint** (con la configuración recomendada de Angular y TypeScript) para hacer cumplir las guías de estilo, detectar errores comunes, posibles problemas de rendimiento y mantener la consistencia del código en todo el proyecto. El linting automático en el editor y en los pipelines de CI/CD es invaluable.
- El análisis estático de código puede identificar patrones problemáticos o vulnerabilidades antes de que el código se ejecute.

### 5.8. Pruebas: Asegurando la Robustez y Facilitando la Refactorización

- **Pruebas Unitarias**: Para componentes, servicios, pipes y directivas de forma aislada. Aseguran que cada unidad de código funcione correctamente por sí misma.
- **Pruebas de Integración**: Para verificar la interacción entre diferentes partes de la aplicación (ej., un componente que usa un servicio, o la interacción de un formulario con sus validadores).
- **Pruebas End-to-End (E2E)**: Para simular la interacción completa del usuario con la aplicación en un navegador real.

Las pruebas son cruciales para la mantenibilidad a largo plazo. Permiten realizar refactorizaciones con confianza, aseguran que los cambios no introduzcan regresiones y sirven como documentación viva del comportamiento esperado del código.

## 6. Refactorizando el Frontend de Ofertas Inmobiliarias

Vamos a crear un formulario para añadir/editar propiedades, utilizando formularios reactivos y conectándolo con el `PropertyService`.

### 6.1. Habilitar Formularios Reactivos en `app.config.ts`

1. Abre `src/app/app.config.ts`.
2. Asegúrate de importar `provideReactiveFormsModule` y añadirlo al array `providers`.

  ```Typescript
  import { ApplicationConfig } from '@angular/core';
  import { provideRouter, withComponentInputBinding } from '@angular/router';
  import { provideReactiveFormsModule } from '@angular/forms'; // ¡Asegúrate de que esta línea esté aquí!

  import { routes } from './app.routes';

  export const appConfig: ApplicationConfig = {
    providers: [
      provideRouter(routes, withComponentInputBinding()),
      provideReactiveFormsModule() // Esto hace que los formularios reactivos estén disponibles
    ]
  };
  ```

### 6.2. Crear el Componente `PropertyFormComponent`

1. **Genera el componente `property-form`**:
    - Abre tu terminal en la raíz de tu proyecto `real-estate-frontend`.
    - Ejecuta el comando:

      ```Terminal
      ng generate component features/properties/components/property-form
      ```

2. **Implementa la lógica en `src/app/features/properties/components/property-form/property-form.ts`**:
    - Importa `FormControl`, `FormGroup`, `ReactiveFormsModule`, `Validators` y `Router`.
    - Importa `PropertyService` y Property `model`.
    - Define el `propertyForm` con los `FormControl` y validadores síncronos como se muestra en la sección de contenidos.
    - Inyecta `PropertyService` y `Router`.
    - Implementa el método `onSubmit()` para enviar los datos a `PropertyService` (crear un nuevo método `addProperty` en el servicio).
    - Añade un método `goBack()` para navegar a la lista de propiedades.

    ```Typescript
    import { Component, OnInit, input } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';
    import { PropertyService } from '../../services/property.service';
    import { Property } from '../../models/property.model';
    import { Router } from '@angular/router';

    @Component({
      selector: 'app-property-form',
      standalone: true,
      imports: [
        CommonModule,
        ReactiveFormsModule // Importa ReactiveFormsModule
      ],
      templateUrl: './property-form.html',
      styleUrl: './property-form.css'
    })
    export class PropertyForm implements OnInit {
      // Opcional: para modo edición, si se pasa un ID de propiedad
      propertyId = input<string | undefined>();

      propertyForm = new FormGroup({
        id: new FormControl<number | null>(null), // Para modo edición, puede ser null al crear
        address: new FormControl('', Validators.required),
        city: new FormControl('', Validators.required),
        price: new FormControl<number | null>(null, [Validators.required, Validators.min(1)]),
        bedrooms: new FormControl<number | null>(null, [Validators.required, Validators.min(0)]),
        bathrooms: new FormControl<number | null>(null, [Validators.required, Validators.min(0)]),
        imageUrl: new FormControl('', [Validators.required, Validators.pattern(/^(http|https):\/\/[^ "]+$/)]), // Validador de URL
        description: new FormControl('', [Validators.required, Validators.minLength(20)]) // Mínimo 20 caracteres
      });

      isEditMode = false;
      loadingForm = false;
      formError: string | null = null;

      constructor(
        private propertyService: PropertyService,
        private router: Router
      ) { }

      ngOnInit(): void {
        if (this.propertyId()) {
          this.isEditMode = true;
          this.loadingForm = true;
          this.propertyService.fetchPropertyById(parseInt(this.propertyId()!, 10))
            .then(property => {
              if (property) {
                this.propertyForm.patchValue(property); // Carga los datos de la propiedad en el formulario
              } else {
                this.formError = 'Propiedad no encontrada para edición.';
              }
            })
            .catch(err => {
              this.formError = `Error al cargar propiedad: ${err.message}`;
            })
            .finally(() => {
              this.loadingForm = false;
            });
        }
      }

      async onSubmit(): Promise<void> {
        this.propertyForm.markAllAsTouched(); // Marca todos los controles como tocados para mostrar errores
        if (this.propertyForm.invalid) {
          console.warn('Formulario inválido. No se puede enviar.');
          return;
        }

        const newProperty: Property = this.propertyForm.value as Property;
        try {
          if (this.isEditMode && newProperty.id) {
            await this.propertyService.updateProperty(newProperty.id, newProperty);
            console.log('Propiedad actualizada con éxito:', newProperty);
            // Mostrar mensaje de éxito o redirigir
          } else {
            // Asigna un ID temporal si no estás usando una API que genere IDs
            newProperty.id = Math.floor(Math.random() * 10000) + 100;
            await this.propertyService.addProperty(newProperty);
            console.log('Propiedad añadida con éxito:', newProperty);
            // Mostrar mensaje de éxito o redirigir
          }
          this.router.navigate(['/properties']); // Redirige a la lista después de guardar
        } catch (err: any) {
          this.formError = `Error al guardar propiedad: ${err.message}`;
          console.error('Error al guardar propiedad:', err);
        }
      }

      goBack(): void {
        this.router.navigate(['/properties']);
      }
    }
    ```

3. **Diseña la plantilla `src/app/features/properties/components/property-form/property-form.html` con Tailwind CSS**:
    - Enlaza el `FormGroup` y los `FormControl`s.
    - Muestra mensajes de error de validación para cada campo.
    - Añade un botón de envío que esté deshabilitado si el formulario es inválido.
    - Añade un botón "Volver".

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen flex justify-center items-start">
      <div class="w-full max-w-2xl bg-white rounded-xl shadow-lg p-8">
        <button
          (click)="goBack()"
          class="mb-6 flex items-center px-4 py-2 bg-gray-200 text-gray-800 rounded-lg hover:bg-gray-300 transition-colors duration-300 font-semibold"
        >
          <svg xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
          </svg>
          Volver
        </button>

        <h1 class="text-3xl font-bold text-gray-900 mb-6 text-center">
          @if (isEditMode) { Editar Propiedad } @else { Añadir Nueva Propiedad }
        </h1>

        @if (loadingForm) {
          <div class="flex justify-center items-center h-64">
            <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
            <p class="ml-4 text-2xl text-blue-600">Cargando formulario...</p>
          </div>
        } @else if (formError) {
          <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
            <p class="font-bold mb-2">¡Error!</p>
            <p>{{ formError }}</p>
          </div>
        } @else {
          <form [formGroup]="propertyForm" (ngSubmit)="onSubmit()" class="space-y-6">
            <!-- Campo Dirección -->
            <div>
              <label for="address" class="block text-sm font-medium text-gray-700 mb-1">Dirección</label>
              <input
                id="address"
                type="text"
                formControlName="address"
                class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                placeholder="Ej. Calle Falsa 123"
              >
              @if (propertyForm.get('address')?.invalid && propertyForm.get('address')?.touched) {
                <p class="mt-1 text-sm text-red-600">La dirección es obligatoria.</p>
              }
            </div>

            <!-- Campo Ciudad -->
            <div>
              <label for="city" class="block text-sm font-medium text-gray-700 mb-1">Ciudad</label>
              <input
                id="city"
                type="text"
                formControlName="city"
                class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                placeholder="Ej. Bogotá"
              >
              @if (propertyForm.get('city')?.invalid && propertyForm.get('city')?.touched) {
                <p class="mt-1 text-sm text-red-600">La ciudad es obligatoria.</p>
              }
            </div>

            <!-- Campo Precio -->
            <div>
              <label for="price" class="block text-sm font-medium text-gray-700 mb-1">Precio ($)</label>
              <input
                id="price"
                type="number"
                formControlName="price"
                class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                placeholder="Ej. 350000"
              >
              @if (propertyForm.get('price')?.invalid && propertyForm.get('price')?.touched) {
                @if (propertyForm.get('price')?.errors?.['required']) {
                  <p class="mt-1 text-sm text-red-600">El precio es obligatorio.</p>
                }
                @if (propertyForm.get('price')?.errors?.['min']) {
                  <p class="mt-1 text-sm text-red-600">El precio debe ser un número positivo.</p>
                }
              }
            </div>

            <!-- Campos Habitaciones y Baños (en una fila) -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div>
                <label for="bedrooms" class="block text-sm font-medium text-gray-700 mb-1">Habitaciones</label>
                <input
                  id="bedrooms"
                  type="number"
                  formControlName="bedrooms"
                  class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                  placeholder="Ej. 3"
                >
                @if (propertyForm.get('bedrooms')?.invalid && propertyForm.get('bedrooms')?.touched) {
                  @if (propertyForm.get('bedrooms')?.errors?.['required']) {
                    <p class="mt-1 text-sm text-red-600">El número de habitaciones es obligatorio.</p>
                  }
                  @if (propertyForm.get('bedrooms')?.errors?.['min']) {
                    <p class="mt-1 text-sm text-red-600">El número de habitaciones no puede ser negativo.</p>
                  }
                }
              </div>
              <div>
                <label for="bathrooms" class="block text-sm font-medium text-gray-700 mb-1">Baños</label>
                <input
                  id="bathrooms"
                  type="number"
                  formControlName="bathrooms"
                  class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                  placeholder="Ej. 2"
                >
                @if (propertyForm.get('bathrooms')?.invalid && propertyForm.get('bathrooms')?.touched) {
                  @if (propertyForm.get('bathrooms')?.errors?.['required']) {
                    <p class="mt-1 text-sm text-red-600">El número de baños es obligatorio.</p>
                  }
                  @if (propertyForm.get('bathrooms')?.errors?.['min']) {
                    <p class="mt-1 text-sm text-red-600">El número de baños no puede ser negativo.</p>
                  }
                }
              </div>
            </div>

            <!-- Campo URL de Imagen -->
            <div>
              <label for="imageUrl" class="block text-sm font-medium text-gray-700 mb-1">URL de Imagen</label>
              <input
                id="imageUrl"
                type="text"
                formControlName="imageUrl"
                class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                placeholder="Ej. [https://ejemplo.com/imagen.jpg](https://ejemplo.com/imagen.jpg)"
              >
              @if (propertyForm.get('imageUrl')?.invalid && propertyForm.get('imageUrl')?.touched) {
                @if (propertyForm.get('imageUrl')?.errors?.['required']) {
                  <p class="mt-1 text-sm text-red-600">La URL de la imagen es obligatoria.</p>
                }
                @if (propertyForm.get('imageUrl')?.errors?.['pattern']) {
                  <p class="mt-1 text-sm text-red-600">Por favor, introduce una URL válida.</p>
                }
              }
            </div>

            <!-- Campo Descripción -->
            <div>
              <label for="description" class="block text-sm font-medium text-gray-700 mb-1">Descripción</label>
              <textarea
                id="description"
                formControlName="description"
                rows="4"
                class="mt-1 block w-full p-3 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 text-lg"
                placeholder="Describe la propiedad..."
              ></textarea>
              @if (propertyForm.get('description')?.invalid && propertyForm.get('description')?.touched) {
                @if (propertyForm.get('description')?.errors?.['required']) {
                  <p class="mt-1 text-sm text-red-600">La descripción es obligatoria.</p>
                }
                @if (propertyForm.get('description')?.errors?.['minlength']) {
                  <p class="mt-1 text-sm text-red-600">La descripción debe tener al menos {{ propertyForm.get('description')?.errors?.['minlength']?.requiredLength }} caracteres.</p>
                }
              }
            </div>

            <!-- Botón de Envío -->
            <div class="flex justify-end">
              <button
                type="submit"
                [disabled]="propertyForm.invalid"
                class="px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
              >
                @if (isEditMode) { Actualizar Propiedad } @else { Guardar Propiedad }
              </button>
            </div>
          </form>
        }
      </div>
    </div>
    ```

### 6.3. Actualizar `PropertyService`

1. Abre `src/app/features/properties/services/property.service.ts`.
2. Añade los métodos `addProperty(property: Property)` y `updateProperty(id: number, property: Property)` que actualicen los datos de las propiedades respectivamente.

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

      /**
       * Añade una nueva propiedad a la lista en memoria.
      * @param property La propiedad a añadir.
      */
      addProperty(property: Property): void {
        // Asigna un ID simple para la gestión en memoria
        const newId = Math.max(...this.propertiesData.map(p => p.id || 0)) + 1;
        const propertyToAdd = { ...property, id: newId };
        this.propertiesData.push(propertyToAdd);
      }

      /**
       * Actualiza una propiedad existente en la lista en memoria.
      * @param id El ID de la propiedad a actualizar.
      * @param updatedProperty Los datos actualizados de la propiedad.
      */
      updateProperty(id: number, updatedProperty: Property): void {
        const index = this.propertiesData.findIndex(p => p.id === id);
        if (index > -1) {
          this.propertiesData[index] = { ...updatedProperty, id: id };
        }
      }
    }
    ```

### 6.4. Actualizar Rutas y Navegación

1. Actualiza `src/app/features/properties/properties.routes.ts` añade una ruta para el nuevo formulario.

    ```Typescript
    import { Routes } from '@angular/router';

    export const PROPERTIES_ROUTES: Routes = [
      { 
        path: '',
        loadComponent: () => import('./components/property-list/property-list').then(m => m.PropertyList)
      },
      {
        path: ':id',
        loadComponent: () => import('./components/property-detail/property-detail').then(m => m.PropertyDetail)
      },
      {
        path: 'properties/add',
        loadComponent: () => import('./components/property-form/property-form').then(m => m.PropertyForm)
      },
      {
        path: 'properties/edit/:id',
        loadComponent: () => import('./components/property-form/property-form').then(m => m.PropertyForm)
      },
    ];
    ```

2. Actualiza `src/app/features/properties/components/property-list/property-list.html` para añadir un botón "Añadir Propiedad" y un botón "Editar" en cada tarjeta.

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen">
      <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12">Propiedades Disponibles</h1>

      <div class="mb-8 flex justify-between items-center">
        <input
          type="text"
          placeholder="Filtrar por ciudad..."
          [ngModel]="filterCity()"
          (ngModelChange)="filterCity.set($event)"
          class="p-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 w-full max-w-md text-lg"
        >
        <button
          (click)="navigateToAddProperty()"
          class="px-6 py-3 bg-green-600 text-white font-semibold rounded-lg shadow-md hover:bg-green-700 transition-colors duration-300"
        >
          Añadir Propiedad
        </button>
      </div>

      @if (propertyService.loading()) {
        <div class="flex justify-center items-center h-64">
          <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
          <p class="ml-4 text-2xl text-blue-600">Cargando propiedades...</p>
        </div>
      } @else if (propertyService.error()) {
        <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
          <p class="font-bold mb-2">¡Error!</p>
          <p>{{ propertyService.error() }}</p>
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
                onerror="this.onerror=null;this.src='[https://placehold.co/600x400/CCCCCC/666666?text=Property](https://placehold.co/600x400/CCCCCC/666666?text=Property)';"
              />
              <div class="p-6 flex flex-col flex-grow">
                <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ property.address }}</h2>
                <p class="text-gray-600 text-lg mb-3">{{ property.city }}</p>
                <p class="text-blue-600 font-extrabold text-3xl mb-4">${{ property.price | number }}</p>

                <div class="flex items-center text-gray-700 text-base mb-4">
                  <span class="flex items-center mr-4">
                    <svg class="w-5 h-5 mr-1 text-gray-500" fill="currentColor" viewBox="0 0 20 20" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path d="M10 2a3 3 0 00-3 3v.586l-1.707 1.707A1 1 0 005 9v5a2 2 0 002 2h6a2 2 0 002-2V9a1 1 0 00-.293-.707L13 5.586V5a3 3 0 00-3-3zm-1 3a1 1 0 012 0v.586l1.707 1.707A1 1 0 0113 9v5a1 1 0 01-1 1H8a1 1 0 01-1-1V9a1 1 0 01.293-.707L9 5.586V5z"></path></svg>
                    {{ property.bedrooms }} Habitaciones
                  </span>
                  <span class="flex items-center">
                    <svg class="w-5 h-5 mr-1 text-gray-500" fill="currentColor" viewBox="0 0 20 20" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path d="M10 2a3 3 0 00-3 3v.586l-1.707 1.707A1 1 0 005 9v5a2 2 0 002 2h6a2 2 0 002-2V9a1 1 0 00-.293-.707L13 5.586V5a3 3 0 00-3-3zm-1 3a1 1 0 012 0v.586l1.707 1.707A1 1 0 0113 9v5a1 1 0 01-1 1H8a1 1 0 01-1-1V9a1 1 0 01.293-.707L9 5.586V5z"></path></svg>
                    {{ property.bathrooms }} Baños
                  </span>
                </div>

                <p class="text-gray-700 text-sm flex-grow mb-4">{{ property.description.substring(0, 100) }}...</p>

                <div class="mt-auto flex justify-between gap-2">
                  <button
                    (click)="handleViewPropertyDetails(property.id)"
                    class="flex-1 bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md"
                  >
                    Ver Detalles
                  </button>
                  <button
                    (click)="navigateToEditProperty(property.id)"
                    class="flex-1 bg-yellow-500 text-white py-3 px-4 rounded-lg font-semibold hover:bg-yellow-600 transition-colors duration-300 shadow-md"
                  >
                    Editar
                  </button>
                </div>
              </div>
            </div>
          }
        </div>
      }
    </div>
    ```

3. Actualiza `src/app/features/properties/components/property-list/property-list.ts` para añadir los métodos de navegación.

    ```Typescript
    import { Component, OnInit, signal, computed, effect } from '@angular/core';
    import { CommonModule, DecimalPipe } from '@angular/common'; // Eliminar NgFor, NgIf si no se usan explícitamente con @for/@if
    import { FormsModule } from '@angular/forms';
    import { Property } from '../../models/property.model';
    import { PropertyService } from '../../services/property.service';
    import { Router } from '@angular/router';

    @Component({
      selector: 'app-property-list',
      imports: [
        CommonModule,
        FormsModule,
        DecimalPipe
      ],
      templateUrl: './property-list.component.html',
      styleUrl: './property-list.component.css'
    })
    export class PropertyListComponent implements OnInit {
      filterCity = signal<string>('');

      filteredProperties = computed(() => {
        const currentFilter = this.filterCity().toLowerCase();
        const allProperties = this.propertyService.properties();
        if (!currentFilter) {
          return allProperties;
        } else {
          return allProperties.filter(property =>
            property.city.toLowerCase().includes(currentFilter)
          );
        }
      });

      constructor(
        public propertyService: PropertyService,
        private router: Router
      ) {
        effect(() => {
          console.log(`El filtro de ciudad ha cambiado a: "${this.filterCity()}"`);
        });
      }

      ngOnInit(): void {
        this.propertyService.fetchAllProperties(); // Llama al método del servicio para cargar las propiedades en memoria
      }

      onFilterInputChange(event: Event): void {
        const inputElement = event.target as HTMLInputElement;
        this.filterCity.set(inputElement.value);
      }

      handleViewPropertyDetails(propertyId: number): void {
        this.router.navigate(['/properties', propertyId]);
      }

      navigateToAddProperty(): void {
        this.router.navigate(['/properties/add']);
      }

      navigateToEditProperty(propertyId: number): void {
        this.router.navigate(['/properties/edit', propertyId]);
      }
    }
    ```

### 6.5. Verificar y Probar `real-estate-frontend`

1. Asegúrate de que tu servidor de desarrollo de Angular esté corriendo (`ng serve`).
2. Abre tu navegador en `http://localhost:4200/`.
3. Haz clic en "Añadir Propiedad". Rellena el formulario, prueba las validaciones y envía. Deberías ver la nueva propiedad en la lista (se añade en memoria).
4. Haz clic en "Editar" en una tarjeta existente. El formulario debería precargarse con los datos de la propiedad. Modifica y guarda.
5. Prueba las validaciones síncronas (campos vacíos, números negativos, URL inválida).
6. Observa cómo los estados de carga y error se manejan a través del servicio (simulados con `setTimeout`).
7. **Importante**: Dado que la persistencia es en memoria para este ejercicio, las propiedades añadidas o modificadas no se guardarán al recargar la página. Esto es intencional para esta etapa, ya que `HttpClient` y la persistencia real se verán en el próximo módulo.
