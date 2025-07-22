# Ejercicio Autoguiado: Gestión de Cursos Universitarios (Continuación de la clase 2)

Este ejercicio te permitirá aplicar los mismos conceptos de enrutamiento y servicios en tu proyecto `university-courses-frontend`, consolidando lo aprendido.

## Objetivos del Ejercicio

1. Crear un `CourseService` para centralizar la gestión de datos de los cursos.
2. Implementar el enrutamiento para navegar entre la lista de cursos y una vista de detalles de un curso específico, utilizando **Feature Routes** con `loadChildren`.
3. Utilizar **exclusivamente `input()`** para recibir el ID del curso en el componente de detalles.

## 1. Crear el `CourseService`

1. **Genera el servicio `course`**:
    - Abre tu terminal en la raíz de tu proyecto `university-courses-frontend`.
    - Ejecuta el comando para generar un servicio dentro de la carpeta `src/app/features/courses/services/`:

      ```Terminal
      ng generate service features/courses/services/course
      ```

2. **Implementa la lógica en `src/app/features/courses/services/course.service.ts`**:
    - Mueve el array `simulatedData` que tenías en `CourseList` a este servicio.
    - Asegúrate de que `coursesData` sea una Signal (`signal<Course[]>`).
    - Crea un método `getAllCourses()` que devuelva una `Promise<Course[]>` (simulando una llamada asíncrona).
    - Crea un método `getCourseById(id: number)` que devuelva una `Promise<Course | undefined>`.
    - Crea un método `enrollStudent(courseId: number)` que actualice la Signal `coursesData` de forma inmutable.

```Typescript
import { Injectable, signal } from '@angular/core';
import { Course } from '../models/course.model';

@Injectable({
  providedIn: 'root' // Este servicio será un singleton en toda la aplicación
})
export class CourseService {
  // Los datos de los cursos se gestionan internamente como una Signal
  private coursesData = signal<Course[]>([
    {
      id: 1,
      name: 'Cálculo I',
      code: 'MATH101',
      credits: 4,
      department: 'Matemáticas',
      description: 'Introducción a los conceptos fundamentales del cálculo diferencial e integral, incluyendo límites, derivadas e integrales.',
      enrolledStudents: 25,
      maxStudents: 30,
      imageUrl: '[https://placehold.co/400x300/E0E0E0/333333?text=Calculo+I](https://placehold.co/400x300/E0E0E0/333333?text=Calculo+I)'
    },
    {
      id: 2,
      name: 'Programación Orientada a Objetos',
      code: 'CS201',
      credits: 3,
      department: 'Informática',
      description: 'Conceptos avanzados de diseño y desarrollo de software utilizando paradigmas de POO con Java y Python.',
      enrolledStudents: 40,
      maxStudents: 40,
      imageUrl: '[https://placehold.co/400x300/D0D0D0/444444?text=POO](https://placehold.co/400x300/D0D0D0/444444?text=POO)'
    },
    {
      id: 3,
      name: 'Historia Antigua',
      code: 'HIST305',
      credits: 3,
      department: 'Historia',
      description: 'Estudio de las civilizaciones antiguas desde Mesopotamia, Egipto, Grecia y Roma hasta la caída del Imperio Romano.',
      enrolledStudents: 15,
      maxStudents: 20,
      imageUrl: '[https://placehold.co/400x300/C0C0C0/555555?text=Historia+Antigua](https://placehold.co/400x300/C0C0C0/555555?text=Historia+Antigua)'
    },
    {
      id: 4,
      name: 'Introducción a la Inteligencia Artificial',
      code: 'CS401',
      credits: 4,
      department: 'Informática',
      description: 'Fundamentos de la IA, aprendizaje automático, redes neuronales y procesamiento de lenguaje natural.',
      enrolledStudents: 35,
      maxStudents: 45,
      imageUrl: '[https://placehold.co/400x300/B0B0B0/666666?text=IA](https://placehold.co/400x300/B0B0B0/666666?text=IA)'
    },
    {
      id: 5,
      name: 'Física Cuántica',
      code: 'PHYS402',
      credits: 5,
      department: 'Física',
      description: 'Principios de la mecánica cuántica, dualidad onda-partícula, ecuaciones de Schrödinger y aplicaciones modernas.',
      enrolledStudents: 10,
      maxStudents: 10,
      imageUrl: '[https://placehold.co/400x300/A0A0A0/777777?text=Fisica+Cuantica](https://placehold.co/400x300/A0A0A0/777777?text=Fisica+Cuantica)'
    },
    {
      id: 6,
      name: 'Economía Internacional',
      code: 'ECON301',
      credits: 3,
      department: 'Economía',
      description: 'Análisis de las relaciones económicas entre países, comercio internacional, finanzas globales y políticas económicas.',
      enrolledStudents: 28,
      maxStudents: 30,
      imageUrl: '[https://placehold.co/400x300/909090/888888?text=Economia+Int](https://placehold.co/400x300/909090/888888?text=Economia+Int)'
    }
  ]);

  // Exponer los cursos como una Signal de solo lectura para que los componentes puedan suscribirse a cambios
  courses = this.coursesData.asReadonly();

  constructor() { }

  /**
   * Simula la obtención de todos los cursos de una API.
   * @returns Una Promesa que resuelve con un array de Course.
   */
  getAllCourses(): Promise<Course[]> {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve(this.coursesData()); // Devuelve el valor actual de la Signal de cursos
      }, 500); // Simula un retardo de red de 500ms
    });
  }

  /**
   * Simula la obtención de un curso específico por su ID.
   * @param id El ID del curso a buscar.
   * @returns Una Promesa que resuelve con el Course encontrado o undefined si no existe.
   */
  getCourseById(id: number): Promise<Course | undefined> {
    return new Promise(resolve => {
      setTimeout(() => {
        const course = this.coursesData().find(c => c.id === id);
        resolve(course);
      }, 300); // Simula un retardo de red de 300ms
    });
  }

  /**
   * Simula la inscripción de un estudiante en un curso.
   * Actualiza la Signal interna de cursos de forma inmutable.
   * @param courseId El ID del curso en el que se va a inscribir.
   */
  enrollStudent(courseId: number): void {
    this.coursesData.update(currentCourses =>
      currentCourses.map(course =>
        // Incrementa enrolledStudents si el ID coincide y hay espacio
        course.id === courseId && course.enrolledStudents < course.maxStudents
          ? { ...course, enrolledStudents: course.enrolledStudents + 1 }
          : course
      )
    );
    console.log(`[CourseService] Estudiante inscrito en el curso con ID: ${courseId}`);
  }
}
```

## 2. Configurar el Enrutamiento para `university-courses-frontend` con Feature Routes

1. **Crea el archivo de rutas de la característica `src/app/features/courses/courses.routes.ts`**.
    - Este archivo contendrá las rutas específicas de la funcionalidad de cursos.

    ```Typescript
    import { Routes } from '@angular/router';
    import { CourseList } from './components/course-list/course-list';
    import { CourseDetail } from './components/course-detail/course-detail';

    export const COURSES_ROUTES: Routes = [
      { path: '', component: CourseList }, // Ruta base para /courses
      { path: ':id', component: CourseDetail }, // Ruta para /courses/:id
    ];
    ```

2. Modifica el archivo de rutas principal `src/app/app.routes.ts` para usar `loadChildren`.
    - Ahora, el path `courses` cargará perezosamente las rutas definidas en `courses.routes.ts`.

    ```Typescript
    import { Routes } from '@angular/router';

    export const routes: Routes = [
      { path: '', redirectTo: '/courses', pathMatch: 'full' }, // Redirige la raíz a la lista de cursos
      {
        path: 'courses', // Cuando la URL sea /courses, carga perezosamente las rutas de cursos
        loadChildren: () => import('./features/courses/courses.routes').then(m => m.COURSES_ROUTES)
      },
      { path: '**', redirectTo: '/courses' } // Ruta comodín para cualquier otra URL no definida
    ];
    ```

3. Configura `app.config.ts` para usar el enrutador y `withComponentInputBinding()`.
    - Esta configuración no cambia.

    ```Typescript
    import { ApplicationConfig } from '@angular/core';
    import { provideRouter, withComponentInputBinding } from '@angular/router';

    import { routes } from './app.routes'; // Importa tus rutas definidas

    export const appConfig: ApplicationConfig = {
      providers: [
        provideRouter(routes, withComponentInputBinding()) // Habilita el binding de inputs para parámetros de ruta
      ]
    };
    ```

## 3. Crear el Componente `CourseDetail`

Este componente mostrará los detalles de un curso específico, recibiendo el ID vía `input()`.

1. **Genera el componente `course-detail`**:
    - Abre tu terminal en la raíz de tu proyecto `university-courses-frontend`.
    - Ejecuta el comando:

      ```Typescript
      ng generate component features/courses/components/course-detail
      ```

2. **Implementa la lógica en `src/app/features/courses/components/course-detail/course-detail.ts`**:
    - Importa `input`, `signal`, `effect`, `Router`.
    - Importa `Course` y `CourseService`.
    - Define `courseId = input<string | undefined>()` para recibir el parámetro.
    - Inyecta `CourseService` y `Router`.
    - Usa un effect para cargar los detalles del curso cuando `courseId` cambie `(usando el input())`.
    - Usa Signals para `courseDetails`, `loading` y `error`.
    - Implementa un método `goBack()` para navegar de vuelta a la lista.

    ```Typescript
    import { Component, OnInit, input, signal, effect, OnDestroy } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { Router } from '@angular/router';
    import { Course } from '../../models/course.model';
    import { CourseService } from '../../services/course.service';

    @Component({
      selector: 'app-course-detail',
      imports: [CommonModule],
      templateUrl: './course-detail.html',
      styleUrl: './course-detail.css'
    })
    export class CourseDetail {
      // --- Forma moderna (Angular 20+): Recibir el ID de la ruta como un input (RECOMENDADO) ---
      // El nombre 'id' debe coincidir con el parámetro de ruta definido en courses.routes.ts (:id).
      // Requiere `withComponentInputBinding()` en app.config.ts.
      courseId = input<string | undefined>();
      numericCourseId = signal<number | undefined>(undefined);

      courseDetails = signal<Course | undefined>(undefined); // Signal para los detalles del curso
      loading = signal<boolean>(true); // Signal para el estado de carga
      error = signal<string | null>(null); // Signal para mensajes de error


      constructor(
        private router: Router, // Para navegación programática
        private courseService: CourseService // Para obtener los datos del curso
      ) {
        // --- Uso de 'input()' para reaccionar al cambio del ID de la ruta ---
        // Este efecto se ejecutará cada vez que el valor de `courseId` cambie.
        effect(() => {
          const idString = this.courseId(); // Obtiene el valor actual de la Signal 'courseId'
          if (idString) {
            const idNum = parseInt(idString, 10);
            if (!isNaN(idNum)) {
              this.numericCourseId.set(idNum); // Actualiza la Signal numérica
              this.loadCourseDetails(idNum); // Carga los detalles usando el ID del input
            } else {
              this.error.set('ID de curso inválido.');
              this.loading.set(false);
            }
          } else {
            this.error.set('ID de curso no proporcionado.');
            this.loading.set(false);
          }
        }, { allowSignalWrites: true }); // Permitir escrituras de Signals dentro de este efecto (para numericCourseId, error, loading)
      }

      private loadCourseDetails(id: number): void {
        this.loading.set(true);
        this.error.set(null);
        this.courseDetails.set(undefined); // Limpiar detalles anteriores

        this.courseService.getCourseById(id)
          .then(course => {
            this.courseDetails.set(course); // Actualiza la Signal con los detalles del curso
            this.loading.set(false);
            if (!course) {
              this.error.set('Curso no encontrado.');
            }
          })
          .catch(err => {
            console.error('Error al cargar los detalles del curso:', err);
            this.error.set('Error al cargar los detalles del curso.');
            this.loading.set(false);
          });
      }

      /**
       * Navega de vuelta a la lista de cursos.
      */
      goBack(): void {
        this.router.navigate(['/courses']); // Navegación programática
      }
    }
    ```

3. **Diseña la plantilla `src/app/features/courses/components/course-detail/course-detail.html` con Tailwind CSS**
    - Muestra un mensaje de carga o error.
    - Si el curso está disponible, muestra todos sus detalles.
    - Añade un botón "Volver a la lista de cursos".

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen flex flex-col items-center">
      <div class="w-full max-w-4xl bg-white rounded-xl shadow-lg p-8">
        <button
          (click)="goBack()"
          class="mb-6 flex items-center px-4 py-2 bg-gray-200 text-gray-800 rounded-lg hover:bg-gray-300 transition-colors duration-300 font-semibold"
        >
          <svg xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
          </svg>
          Volver a la lista de cursos
        </button>

        @if (loading()) {
          <div class="flex justify-center items-center h-64">
            <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
            <p class="ml-4 text-2xl text-blue-600">Cargando detalles del curso...</p>
          </div>
        } @else if (error()) {
          <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
            <p class="font-bold mb-2">¡Error!</p>
            <p>{{ error() }}</p>
          </div>
        } @else if (courseDetails()) {
          <div class="flex flex-col md:flex-row gap-8">
            <div class="md:w-1/2">
              <img
                [src]="courseDetails()!.imageUrl"
                [alt]="courseDetails()!.name"
                class="w-full h-auto rounded-xl shadow-md object-cover"
                onerror="this.onerror=null;this.src='[https://placehold.co/600x450/CCCCCC/666666?text=Course+Detail](https://placehold.co/600x450/CCCCCC/666666?text=Course+Detail)';"
              />
            </div>
            <div class="md:w-1/2">
              <h1 class="text-4xl font-extrabold text-gray-900 mb-4">{{ courseDetails()!.name }}</h1>
              <p class="text-gray-600 text-xl mb-6">{{ courseDetails()!.code }}</p>
              <p class="text-gray-700 text-lg mb-4">{{ courseDetails()!.description }}</p>

              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
                <div class="bg-blue-50 p-4 rounded-lg shadow-sm">
                  <p class="text-blue-800 font-semibold">Créditos:</p>
                  <p class="text-blue-700 text-lg">{{ courseDetails()!.credits }}</p>
                </div>
                <div class="bg-purple-50 p-4 rounded-lg shadow-sm">
                  <p class="text-purple-800 font-semibold">Departamento:</p>
                  <p class="text-purple-700 text-lg">{{ courseDetails()!.department }}</p>
                </div>
                <div class="bg-green-50 p-4 rounded-lg shadow-sm">
                  <p class="text-green-800 font-semibold">Estudiantes Inscritos:</p>
                  <p class="text-green-700 text-lg">{{ courseDetails()!.enrolledStudents }}</p>
                </div>
                <div class="bg-red-50 p-4 rounded-lg shadow-sm">
                  <p class="text-red-800 font-semibold">Cupo Máximo:</p>
                  <p class="text-red-700 text-lg">{{ courseDetails()!.maxStudents }}</p>
                </div>
              </div>

              <!-- Puedes añadir un botón de inscripción aquí también si lo deseas -->
              <!-- <button
                class="w-full bg-indigo-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors duration-300 shadow-md"
                [disabled]="courseDetails()!.enrolledStudents >= courseDetails()!.maxStudents"
              >
                Inscribirse
              </button> -->
            </div>
          </div>
        } @else {
          <div class="text-center p-8 text-2xl text-gray-500 bg-gray-100 border border-gray-300 rounded-lg shadow-md">
            <p>No se pudieron cargar los detalles del curso.</p>
          </div>
        }
      </div>
    </div>
    ```

## 4. Actualizar el Componente `CourseList`

1. Abre `src/app/features/courses/components/course-list/course-list.ts`.
2. Importa `CourseService` y `Router`.
3. Inyecta `CourseService` y `Router` en el constructor.
4. Elimina el `simulatedData` local del componente.
5. Modifica `ngOnInit` para usar `this.courseService.getAllCourses()`.
6. Modifica `handleEnrollCourse` para usar `this.courseService.enrollStudent()`.
7. Modifica `handleViewCourseDetails` para usar `this.router.navigate(['/courses', courseId])` para la navegación programática.

    ```Typescript
    import { Component, OnInit, signal, computed, effect } from '@angular/core';
    import { CommonModule } from '@angular/common'; 
    import { FormsModule } from '@angular/forms';
    import { Course } from '../../models/course.model';
    import { CourseCardSignal } from '../course-card-signal/course-card-signal';
    import { CourseService } from '../../services/course.service';
    import { Router } from '@angular/router';

    @Component({
      selector: 'app-course-list',
      imports: [
        CommonModule,
        FormsModule,
        CourseCardSignalComponent
      ],
      templateUrl: './course-list.html',
      styleUrl: './course-list.css'
    })
    export class CourseList implements OnInit {
      // filteredCourses es una computed Signal que depende de allCourses del servicio
      filterDepartment = signal<string>('');
      loading = signal<boolean>(true);
      error = signal<string | null>(null);

      // Accede a los cursos directamente desde el servicio
      // Usamos computed para que filteredCourses reaccione a los cambios en courses del servicio
      filteredCourses = computed(() => {
        const currentFilter = this.filterDepartment().toLowerCase();
        const allCourses = this.courseService.courses(); // Accede a la Signal de cursos del servicio
        if (!currentFilter) {
          return allCourses;
        } else {
          return allCourses.filter(course =>
            course.department.toLowerCase().includes(currentFilter)
          );
        }
      });

      constructor(
        private courseService: CourseService, // Inyecta el CourseService
        private router: Router // Inyecta el Router para navegación
      ) {
        effect(() => {
          console.log(`El filtro de departamento ha cambiado a: "${this.filterDepartment()}"`);
        });

        // Observa los cambios en la Signal 'courses' del servicio para actualizar el estado de carga/error
        effect(() => {
          if (this.courseService.courses().length > 0) {
            this.loading.set(false);
            this.error.set(null);
          }
        }, { allowSignalWrites: true });
      }

      ngOnInit(): void {
        this.loading.set(true);
        this.error.set(null);

        // Carga los cursos usando el servicio
        this.courseService.getAllCourses()
          .then(courses => {
            // La Signal 'allCourses' ya no se usa directamente aquí,
            // la data está en courseService.courses()
            this.loading.set(false);
          })
          .catch(err => {
            console.error('Error al cargar los cursos:', err);
            this.error.set('Error al cargar los cursos desde el servicio.');
            this.loading.set(false);
          });
      }

      onFilterInputChange(event: Event): void {
        const inputElement = event.target as HTMLInputElement;
        this.filterDepartment.set(inputElement.value);
      }

      handleEnrollCourse(courseId: number): void {
        this.courseService.enrollStudent(courseId); // Llama al método del servicio para inscribir
        console.log(`Inscrito en el curso con ID: ${courseId} (a través del servicio)`);
        // La alerta ya se maneja en el CourseCardSignalComponent
      }

      handleViewCourseDetails(courseId: number): void {
        console.log(`Navegando a detalles del curso con ID: ${courseId}`);
        this.router.navigate(['/courses', courseId]); // Navegación programática a la ruta de detalles
      }
    }
    ```

8. **Actualiza la `plantilla src/app/features/courses/components/course-list/course-list.html`**:
    - Asegúrate de que el botón "Ver Detalles (Signals)" en `CourseCardSignal` (o donde lo tengas) llame a `handleViewCourseDetails(course.id)`.

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen">
      <h1 class="text-5xl font-extrabold text-center text-gray-900 mb-12">Cursos Universitarios</h1>

      <div class="mb-8 flex justify-center">
        <input
          type="text"
          placeholder="Filtrar por departamento..."
          [ngModel]="filterDepartment()"
          (ngModelChange)="filterDepartment.set($event)"
          class="p-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 w-full max-w-md text-lg"
        >
      </div>

      @if (loading()) {
        <div class="flex justify-center items-center h-64">
          <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
          <p class="ml-4 text-2xl text-blue-600">Cargando cursos...</p>
        </div>
      } @else if (error()) {
        <div class="text-center p-8 text-2xl text-red-600 bg-red-100 border border-red-400 rounded-lg shadow-md">
          <p class="font-bold mb-2">¡Error!</p>
          <p>{{ error() }}</p>
        </div>
      } @else if (filteredCourses().length === 0) {
        <div class="text-center p-8 text-2xl text-gray-500 bg-gray-100 border border-gray-300 rounded-lg shadow-md">
          <p>No se encontraron cursos que coincidan con el filtro.</p>

        </div>
      } @else {
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          @for (course of filteredCourses(); track course.id) {
            <app-course-card-signal
              [course]="course"
              (enroll)="handleEnrollCourse($event)"
              (viewDetails)="handleViewCourseDetails($event)"
            ></app-course-card-signal>
          }
        </div>
      }
    </div>
    ```

## 5. Actualizar App en `university-courses-frontend`

1. Abre `src/app/app.ts`.
2. Asegúrate de que `RouterOutlet` esté importado y añadido al array `imports`.
3. Abre `src/app/app.html`.
4. Reemplaza el selector `<app-course-list></app-course-list>` con `<router-outlet></router-outlet>`. El enrutador se encargará de cargar `CourseList` cuando la ruta sea `/courses`.

```Typescript
import { Component  } from '@angular/core';
import { RouterOutlet } from '@angular/router'; // Asegúrate de importar RouterOutlet

@Component({
  selector: 'app-root',
  imports: [
    RouterOutlet 
  ],
  templateUrl: './app.html',
  styleUrl: './app.css'
})
export class App {
  title = 'university-courses-frontend';
}
```

```HTML
<!-- Aquí se renderizarán los componentes de las rutas -->
<router-outlet></router-outlet>
```

## 6. Verificar y Probar `university-courses-frontend`

1. Asegúrate de que tu servidor de desarrollo de Angular esté corriendo (`ng serve`).
2. Abre tu navegador en `http://localhost:4200/`.
3. Deberías ver la lista de cursos.
4. Prueba el filtro.
5. Haz clic en el botón "Inscribirse (Signals)". Verás que el número de estudiantes se actualiza.
6. Haz clic en el botón "Ver Detalles (Signals)". Deberías ser redirigido a una nueva vista que muestra los detalles del curso seleccionado.
7. Usa el botón "Volver a la lista de cursos" en la vista de detalles.
8. Intenta acceder directamente a una URL como `http://localhost:4200/courses/1` para ver los detalles del curso con ID 1.

---

¡Felicidades! Has implementado el enrutamiento con **Feature Routes** y has refactorizado tus aplicaciones para utilizar servicios, lo que las hace más modulares, escalables y fáciles de mantener. Además, has aplicado la forma moderna y recomendada de recibir parámetros de ruta con `input()` en Angular 20, y ahora también conoces la alternativa con `ActivatedRoute`. Ahora tus aplicaciones son verdaderas Single Page Applications con navegación entre vistas y una gestión de datos centralizada.
