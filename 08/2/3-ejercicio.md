# Ejercicio Autoguiado: Gestión de Cursos Universitarios (Continuación de la clase 1)

Este ejercicio te permitirá aplicar los conceptos de Signals y comunicación entre componentes en tu proyecto existente `university-courses-frontend`.

## Objetivos del Ejercicio

1. Utilizar Signals para la gestión del estado de la lista de cursos y el filtro.
2. Crear un componente hijo `CourseCardSignal` que muestre los detalles de un curso y se comunique con el padre usando `input()` y `output()`.
3. (Opcional, para comparación) Crear un `CourseCardTraditional` usando `@Input()` y `@Output()`.

## 1. Verificar/Definir el Modelo de Datos para un Curso

Asegúrate de que tu archivo `src/app/features/courses/models/course.model.ts` tenga la siguiente interfaz:

```Typescript
export interface Course {
  id: number;
  name: string;
  code: string;
  credits: number;
  department: string;
  description: string;
  enrolledStudents: number;
  maxStudents: number;
  imageUrl: string; // Usar URLs de placeholder como https://placehold.co/400x300/CCCCCC/666666?text=Course+Image
}
```

## 2. (Opcional, para comparación): Crear el Componente Hijo `CourseCardTraditional` (Usando `@Input()`, `@Output` y `EventEmitter`)

Este componente representará una sola tarjeta de curso utilizando el enfoque tradicional. Si ya lo tienes de la Clase 1, puedes saltar la generación y solo verificar el código.

1. **Genera el componente `course-card-traditional`**:
    - Abre tu terminal en la raíz de tu proyecto `university-courses-frontend`.
    - Ejecuta el comando para generar un componente standalone dentro de la carpeta `src/app/features/courses/components/`:

    ```Terminal
    ng generate component features/courses/components/course-card-traditional
    ```

2. **Implementa la lógica en `src/app/features/courses/components/course-card-traditional/course-card-traditional.ts`**:
    - Importa `Input`, `Output`, `EventEmitter` y la interfaz `Course`.
    - Declara un `@Input() course!: Course;` para recibir el objeto `Course` del padre.
    - Declara dos `@Output()`:
      - `enrollCourse = new EventEmitter<number>();` (para cuando se intente inscribir en el curso).
      - `viewDetails = new EventEmitter<number>();` (para cuando se haga clic en "Ver Detalles").
    - Crea métodos `onEnrollClick()` y `onViewDetailsClick()` que emitan los eventos correspondientes con el `id` del curso.
    - Añade una propiedad computada para verificar si el curso está lleno (aunque no es una Signal, puedes calcularlo en un getter).

    ```Typescript
    import { Component, Input, Output, EventEmitter } from '@angular/core';
    import { CommonModule } from '@angular/common'; // Para ngClass
    import { Course } from '../../models/course.model'; // Importa la interfaz Course

    @Component({
      selector: 'app-course-card-traditional',
      imports: [CommonModule],
      templateUrl: './course-card-traditional.html',
      styleUrl: './course-card-traditional.css'
    })
    export class CourseCardTraditional {
      @Input() course!: Course; // Recibe un objeto Course del componente padre

      @Output() enrollCourse = new EventEmitter<number>(); // Emite el ID del curso para inscripción
      @Output() viewDetails = new EventEmitter<number>(); // Emite el ID del curso para ver detalles

      // Getter para saber si el curso está lleno (enfoque tradicional)
      get isFull(): boolean {
        return this.course.enrolledStudents >= this.course.maxStudents;
      }

      onEnrollClick(): void {
        if (!this.isFull) {
          // En una aplicación real, usarías un modal o un Toastr, no alert()
          alert('Intentando inscribir al curso (Tradicional): ' + this.course.name);
          this.enrollCourse.emit(this.course.id);
        } else {
          alert('Este curso está lleno y no se puede inscribir (Tradicional).');
        }
      }

      onViewDetailsClick(): void {
        this.viewDetails.emit(this.course.id);
      }
    }
    ```

3. Diseña la plantilla `src/app/features/courses/components/course-card-traditional/course-card-traditional.html` con Tailwind CSS:
    - Muestra el nombre, código, créditos, departamento, descripción, estudiantes inscritos/máximos.
    - Añade un botón "Inscribirse" que esté deshabilitado si `isFull` es `true`.
    - Añade un botón "Ver Detalles".
    - Usa la `imageUrl` para la imagen del curso.

    ```HTML
    <div class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 overflow-hidden">
      <img
        [src]="course.imageUrl"
        [alt]="course.name"
        class="w-full h-48 object-cover rounded-t-xl"
        onerror="this.onerror=null;this.src='https://placehold.co/400x300/CCCCCC/666666?text=Course+Image';"
      />
      <div class="p-6">
        <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ course.name }} ({{ course.code }})</h2>
        <p class="text-gray-700 text-sm mb-4 line-clamp-3">{{ course.description }}</p>
        <div class="flex flex-col gap-2 mb-4">
          <span class="px-3 py-1 rounded-full text-sm font-semibold bg-blue-100 text-blue-800">
            Créditos: {{ course.credits }}
          </span>
          <span class="px-3 py-1 rounded-full text-sm font-semibold bg-purple-100 text-purple-800">
            Departamento: {{ course.department }}
          </span>
          <span class="px-3 py-1 rounded-full text-sm font-semibold"
                [ngClass]="{'bg-green-100 text-green-800': !isFull, 'bg-red-100 text-red-800': isFull}">
            Estudiantes: {{ course.enrolledStudents }} / {{ course.maxStudents }}
          </span>
        </div>
        <div class="flex gap-4 mt-6">
          <button
            class="flex-1 bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md hover:shadow-lg"
            [disabled]="isFull"
            [ngClass]="{'opacity-50 cursor-not-allowed': isFull}"
            (click)="onEnrollClick()"
          >
            {{ isFull ? 'Curso Lleno (Tradicional)' : 'Inscribirse (Tradicional)' }}
          </button>
          <button
            class="flex-1 bg-gray-200 text-gray-800 py-3 px-4 rounded-lg font-semibold hover:bg-gray-300 transition-colors duration-300 shadow-md hover:shadow-lg"
            (click)="onViewDetailsClick()"
          >
            Ver Detalles (Tradicional)
          </button>
        </div>
      </div>
    </div>
    ```

## 3. Crear el Componente Hijo CourseCardSignal (Usando `input()` y `output()`)

Este componente representará una sola tarjeta de curso utilizando el nuevo enfoque de Signals.

1. **Genera el componente course-card-signal**:
    - Abre tu terminal en la raíz de tu proyecto `university-courses-frontend`.
    - Ejecuta el comando:

      ```Terminal
      ng generate component features/courses/components/course-card-signal
      ```

2. **Implementa la lógica en `src/app/features/courses/components/course-card-signal/course-card-signal.ts`**:
    - Importa `input`, `output`, `computed` y la interfaz `Course`.
    - Declara un `input()` para recibir el objeto `course`.
    - Declara dos `output()`:
      - `enrollCourse = output<number>();`
      - `viewDetails = output<number>();`
    - Crea métodos `onEnrollClick()` y `onViewDetailsClick()` que emitan los eventos correspondientes con el id del curso.
    - Añade una Signal computada (`computed()`) para verificar si el curso está lleno.

    ```Typescript
    import { Component, input, output, computed } from '@angular/core';
    import { CommonModule } from '@angular/common'; // Para ngClass
    import { Course } from '../../models/course.model'; // Importa la interfaz Course

    @Component({
      selector: 'app-course-card-signal',
      standalone: true,
      imports: [CommonModule],
      templateUrl: './course-card-signal.html',
      styleUrl: './course-card-signal.css'
    })
    export class CourseCardSignal {
      // Recibe un objeto Course del componente padre usando input()
      // 'required' significa que el padre DEBE proporcionar este input.
      course = input.required<Course>();

      // Emite el ID del curso para inscripción usando output()
      enrollCourse = output<number>();
      // Emite el ID del curso para ver detalles usando output()
      viewDetails = output<number>();

      // Signal computada para saber si el curso está lleno.
      // Se recalcula automáticamente cuando 'course().enrolledStudents' o 'course().maxStudents' cambian.
      isFull = computed(() => this.course().enrolledStudents >= this.course().maxStudents);

      onEnrollClick(): void {
        // Accede al valor de la Signal 'isFull' llamándola como función
        if (!this.isFull()) {
          // Accede al valor de la Signal 'course' llamándola como función
          alert('Intentando inscribir al curso (Signals): ' + this.course().name);
          this.enrollCourse.emit(this.course().id);
        } else {
          // En una aplicación real, usarías un modal o un Toastr, no alert()
          alert('Este curso está lleno y no se puede inscribir (Signals).');
        }
      }

      onViewDetailsClick(): void {
        // Accede al valor de la Signal 'course' llamándola como función
        this.viewDetails.emit(this.course().id);
      }
    }
    ```

3. **Diseña la plantilla `src/app/features/courses/components/course-card-signal/course-card-signal.html` con Tailwind CSS**:
    - Es idéntica a la versión tradicional, pero las referencias a course ahora son funciones (`course()`).
    - Las referencias a `isFull` también son funciones (`isFull()`).
    - Ajusta los botones para que usen los métodos `onEnrollClick()` y `onViewDetailsClick()`.

    ```HTML
    <div class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 overflow-hidden">
      <img
        [src]="course().imageUrl"
        [alt]="course().name"
        class="w-full h-48 object-cover rounded-t-xl"
        onerror="this.onerror=null;this.src='https://placehold.co/400x300/CCCCCC/666666?text=Course+Image';"
      />
      <div class="p-6">
        <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ course().name }} ({{ course().code }})</h2>
        <p class="text-gray-700 text-sm mb-4 line-clamp-3">{{ course().description }}</p>
        <div class="flex flex-col gap-2 mb-4">
          <span class="px-3 py-1 rounded-full text-sm font-semibold bg-blue-100 text-blue-800">
            Créditos: {{ course().credits }}
          </span>
          <span class="px-3 py-1 rounded-full text-sm font-semibold bg-purple-100 text-purple-800">
            Departamento: {{ course().department }}
          </span>
          <span class="px-3 py-1 rounded-full text-sm font-semibold"
                [ngClass]="{'bg-green-100 text-green-800': !isFull(), 'bg-red-100 text-red-800': isFull()}">
            Estudiantes: {{ course().enrolledStudents }} / {{ course().maxStudents }}
          </span>
        </div>
        <div class="flex gap-4 mt-6">
          <button
            class="flex-1 bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md hover:shadow-lg"
            [disabled]="isFull()"
            [ngClass]="{'opacity-50 cursor-not-allowed': isFull()}"
            (click)="onEnrollClick()"
          >
            {{ isFull() ? 'Curso Lleno (Signals)' : 'Inscribirse (Signals)' }}
          </button>
          <button
            class="flex-1 bg-gray-200 text-gray-800 py-3 px-4 rounded-lg font-semibold hover:bg-gray-300 transition-colors duration-300 shadow-md hover:shadow-lg"
            (click)="onViewDetailsClick()"
          >
            Ver Detalles (Signals)
          </button>
        </div>
      </div>
    </div>
    ```

## 4. Modificar el Componente `CourseList` (Padre)

Ahora, actualizaremos el componente principal de la lista de cursos para usar Signals y para interactuar con el nuevo `CourseCardSignal`.

1. Abre `src/app/features/courses/components/course-list/course-list.ts`.
2. Importa `signal`, `computed`, `effect` desde `@angular/core` y `CourseCardSignal`.
3. Cambia las propiedades a Signals y usa `computed` para el filtrado:

    ```Typescript
    import { Component, OnInit, signal, computed, effect } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { FormsModule } from '@angular/forms'; // Necesario para ngModel
    import { Course } from '../../models/course.model';
    // Importa el componente de tarjeta basado en Signals que vamos a usar
    import { CourseCardSignal } from '../course-card-signal/course-card-signal';
    // Opcional: Si quieres mantener el componente tradicional para referencia, impórtalo también
    import { CourseCardTraditionalComponent } from '../course-card-traditional/course-card-traditional';

    @Component({
      selector: 'app-course-list',
      standalone: true,
      imports: [
        CommonModule,
        FormsModule, // Importa FormsModule para [(ngModel)]
        CourseCardSignalComponent, // Añade CourseCardSignalComponent a los imports
        CourseCardTraditionalComponent // Lo mantenemos para que puedas alternar y comparar
      ],
      templateUrl: './course-list.html',
      styleUrl: './course-list.css'
    })
    export class CourseListComponent implements OnInit {
      // Signals para el estado de la aplicación
      allCourses = signal<Course[]>([]); // Contiene todos los cursos cargados
      filterDepartment = signal<string>(''); // Contiene el valor del input de filtro
      loading = signal<boolean>(true); // Indica si los datos están cargando
      error = signal<string | null>(null); // Contiene un mensaje de error si ocurre

      // Computed Signal: se recalcula automáticamente cuando allCourses o filterDepartment cambian
      filteredCourses = computed(() => {
        const currentFilter = this.filterDepartment().toLowerCase();
        if (!currentFilter) {
          return this.allCourses(); // Si no hay filtro, devuelve todos los cursos
        } else {
          // Filtra los cursos por departamento
          return this.allCourses().filter(course =>
            course.department.toLowerCase().includes(currentFilter)
          );
        }
      });

      constructor() {
        // Un efecto que se ejecuta cada vez que 'filterDepartment' cambia.
        // Útil para depuración o para efectos secundarios que dependen del filtro.
        effect(() => {
          console.log(`El filtro de departamento ha cambiado a: "${this.filterDepartment()}"`);
        });
      }

      ngOnInit(): void {
        this.loading.set(true); // Establece loading a true al inicio
        this.error.set(null); // Limpia cualquier error previo

        // Simulación de carga de datos asíncrona
        setTimeout(() => {
          const simulatedData: Course[] = [
            {
              id: 1,
              name: 'Cálculo I',
              code: 'MATH101',
              credits: 4,
              department: 'Matemáticas',
              description: 'Introducción a los conceptos fundamentales del cálculo diferencial e integral.',
              enrolledStudents: 25,
              maxStudents: 30,
              imageUrl: 'https://placehold.co/400x300/E0E0E0/333333?text=Calculo+I'
            },
            {
              id: 2,
              name: 'Programación Orientada a Objetos',
              code: 'CS201',
              credits: 3,
              department: 'Informática',
              description: 'Conceptos avanzados de diseño y desarrollo de software utilizando paradigmas de POO.',
              enrolledStudents: 40,
              maxStudents: 40, // Curso lleno
              imageUrl: 'https://placehold.co/400x300/D0D0D0/444444?text=POO'
            },
            {
              id: 3,
              name: 'Historia Antigua',
              code: 'HIST305',
              credits: 3,
              department: 'Historia',
              description: 'Estudio de las civilizaciones antiguas desde Mesopotamia hasta la caída de Roma.',
              enrolledStudents: 15,
              maxStudents: 20,
              imageUrl: 'https://placehold.co/400x300/C0C0C0/555555?text=Historia+Antigua'
            },
            {
              id: 4,
              name: 'Introducción a la Inteligencia Artificial',
              code: 'CS401',
              credits: 4,
              department: 'Informática',
              description: 'Fundamentos de la IA, aprendizaje automático y redes neuronales.',
              enrolledStudents: 35,
              maxStudents: 45,
              imageUrl: 'https://placehold.co/400x300/B0B0B0/666666?text=IA'
            },
            {
              id: 5,
              name: 'Física Cuántica',
              code: 'PHYS402',
              credits: 5,
              department: 'Física',
              description: 'Principios de la mecánica cuántica y sus aplicaciones.',
              enrolledStudents: 10,
              maxStudents: 10, // Curso lleno
              imageUrl: 'https://placehold.co/400x300/A0A0A0/777777?text=Fisica+Cuantica'
            },
            {
              id: 6,
              name: 'Economía Internacional',
              code: 'ECON301',
              credits: 3,
              department: 'Economía',
              description: 'Análisis de las relaciones económicas entre países y mercados globales.',
              enrolledStudents: 28,
              maxStudents: 30,
              imageUrl: 'https://placehold.co/400x300/909090/888888?text=Economia+Int'
            }
          ];
          this.allCourses.set(simulatedData); // Actualiza la Signal 'allCourses'
          this.loading.set(false); // Establece loading a false
        }, 1500);
      }

      // Método para actualizar la Signal 'filterDepartment' desde el input del filtro
      onFilterInputChange(event: Event): void {
        const inputElement = event.target as HTMLInputElement;
        this.filterDepartment.set(inputElement.value);
      }

      // Maneja el evento 'enrollCourse' emitido por el componente hijo
      handleEnrollCourse(courseId: number): void {
        // Actualiza la Signal 'allCourses' de forma inmutable
        this.allCourses.update(currentCourses =>
          currentCourses.map(course =>
            // Si el ID coincide y el curso no está lleno, incrementa los estudiantes inscritos
            course.id === courseId && course.enrolledStudents < course.maxStudents
              ? { ...course, enrolledStudents: course.enrolledStudents + 1 }
              : course
          )
        );
        console.log(`Inscrito en el curso con ID: ${courseId}`);
        // En una aplicación real, usarías un modal o un Toastr
        // alert(`Te has inscrito en el curso con ID: ${courseId}`); // Se usa en el componente hijo
      }

      // Maneja el evento 'viewDetails' emitido por el componente hijo
      handleViewCourseDetails(courseId: number): void {
        console.log(`Ver detalles del curso con ID: ${courseId}`);
        // Aquí podrías navegar a una página de detalles o abrir un modal
        alert(`Has hecho clic en 'Ver Detalles' para el curso con ID: ${courseId}`);
      }
    }
    ```

4. Actualiza la plantilla `src/app/features/courses/components/course-list/course-list.html`:
    - Ahora, para acceder a los valores de las Signals en la plantilla, debes llamarlas como funciones (ej., `loading()`, `filteredCourses()`, `filterDepartment()`).
    - Ajusta el `[(ngModel)]` para que use el método `onFilterInputChange` en el evento `(input)` o `(ngModelChange)`.
    - **Importante**: Dentro del `@for`, usa el selector de `CourseCardSignal`. Si quieres ver el `CourseCardTraditional`, puedes cambiar el selector o añadir un botón para alternar entre ellos.

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen">
      <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12 drop-shadow-lg">Catálogo de Cursos Universitarios</h1>

      <!-- Sección de Filtro -->
      <div class="mb-8 flex flex-col sm:flex-row items-center justify-center gap-4 p-6 bg-white rounded-xl shadow-md">
        <input
          type="text"
          placeholder="Filtrar por departamento (ej. Informática)"
          class="flex-grow p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-lg"
          [ngModel]="filterDepartment()" <!-- Usa property binding para leer la Signal -->
          (input)="onFilterInputChange($event)" <!-- Usa event binding para actualizar la Signal -->
        />
        <button
          class="w-full sm:w-auto bg-indigo-600 text-white py-3 px-6 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md"
          (click)="filterDepartment.set('')" <!-- Resetea la Signal de filtro -->
        >
          Limpiar Filtro
        </button>
      </div>

      @if (loading()) { <!-- Accede al valor de la Signal 'loading' -->
        <div class="flex justify-center items-center h-64">
          <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
          <p class="ml-4 text-2xl text-blue-600">Cargando cursos...</p>
        </div>
      } @else if (error()) { <!-- Accede al valor de la Signal 'error' -->
        <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
          <p class="font-bold mb-2">¡Error al cargar los cursos!</p>
          <p>{{ error() }}</p> <!-- Muestra el mensaje de error de la Signal -->
        </div>
      } @else if (filteredCourses().length === 0 && !loading()) { <!-- Accede a las Signals -->
        <div class="text-center p-8 text-2xl text-gray-500 bg-yellow-50 border border-yellow-300 rounded-lg shadow-md">
          <p class="font-bold mb-2">No hay cursos disponibles que coincidan con tu búsqueda.</p>
          <p>Intenta con otro departamento o reinicia el filtro.</p>
        </div>
      } @else {
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
          @for (course of filteredCourses(); track course.id) { <!-- Itera sobre la Signal computada 'filteredCourses' -->
            <!-- Usamos el componente hijo basado en Signals -->
            <app-course-card-signal
              [course]="course" <!-- Pasa el objeto 'course' como input (Signal) -->
              (enrollCourse)="handleEnrollCourse($event)" <!-- Escucha el evento de inscripción -->
              (viewDetails)="handleViewCourseDetails($event)" <!-- Escucha el evento de ver detalles -->
            ></app-course-card-signal>

            <!-- Si quieres probar la versión tradicional, puedes descomentar y usarla en paralelo o alternar -->
            <!--
            <app-course-card-traditional
              [course]="course"
              (enrollCourse)="handleEnrollCourse($event)"
              (viewDetails)="handleViewCourseDetails($event)"
            ></app-course-card-traditional>
            -->
          } @empty {
            <!-- Este bloque @empty no debería activarse si el @else if anterior maneja el caso de lista vacía -->
          }
        </div>
      }
    </div>
    ```

## 5. Asegurar la Integración en `App`

Asegúrate de que tu `App` esté importando y utilizando `CourseList`.

1. Abre `src/app/app.ts`.
2. Verifica que `CourseList` esté importado y añadido al array imports del decorador `@Component`.

    ```Typescript
    import { Component } from '@angular/core';
    import { RouterOutlet } from '@angular/router';
    // Asegúrate de que esta importación sea correcta según la ruta de tu componente
    import { CourseList } from './features/courses/components/course-list/course-list';

    @Component({
      selector: 'app-root',
      standalone: true,
      imports: [
        RouterOutlet,
        CourseList // Asegúrate de que CourseList esté aquí
      ],
      templateUrl: './app.html',
      styleUrl: './app.css'
    })
    export class App {
      title = 'university-courses-frontend'; // Asegúrate de que el título sea correcto
    }
    ```

3. Abre `src/app/app.html`.
4. Asegúrate de que el contenido predeterminado haya sido reemplazado por el selector de tu `CourseList`:

    ```HTML
    <app-course-list></app-course-list>
    ```

## 6. Verificar y Probar

1. Asegúrate de que tu servidor de desarrollo de Angular esté corriendo (`ng serve`).
2. Abre tu navegador en `http://localhost:4200/`.
3. Deberías ver la aplicación de gestión de cursos.
4. Prueba el filtro por departamento (ej., escribe "Informática" o "Matemáticas").
5. Haz clic en el botón "Inscribirse (Signals)" para ver cómo aumenta el número de estudiantes (y cómo se deshabilita si el curso está lleno).
6. Haz clic en el botón "Ver Detalles (Signals)" para ver la comunicación de eventos.

¡Felicidades! Has refactorizado tu aplicación `university-courses-frontend` para utilizar las nuevas primitivas de reactividad de Angular (Signals) y has implementado la comunicación entre componentes de padre a hijo y de hijo a padre utilizando `input()` y `output()`. Esto te proporciona una base sólida para construir aplicaciones Angular más eficientes y mantenibles.
