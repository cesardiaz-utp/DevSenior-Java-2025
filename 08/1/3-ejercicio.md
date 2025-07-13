# Ejercicio Autoguiado: Gestión de Cursos Universitarios

## Objetivos del Ejercicio

Desarrollar un frontend básico para la gestión de cursos universitarios, reforzando la creación de proyectos standalone, la definición de modelos con TypeScript, la visualización de datos con `@for`, el uso de `ngOnInit` y la aplicación de estilos con Tailwind CSS.

## Instrucciones Detalladas

### 1. Crear un Nuevo Proyecto Angular

1. **Abre tu terminal** en la carpeta donde guardas tus proyectos.
2. Crea un nuevo proyecto Angular llamado `university-courses-frontend`.

    ```Terminal
    ng new university-courses-frontend
    ```

3. Navega a la carpeta del proyecto e inicia el servidor de desarrollo:

    ```Terminal
    cd university-courses-frontend

    ng serve
    ```

    Verifica que la aplicación base se ejecuta correctamente en `http://localhost:4200/`.

### 2. Integrar Tailwind CSS

1. Detén el servidor de desarrollo (`Ctrl+C` en la terminal).
2. **Instala Tailwind CSS y sus dependencias de PostCSS**:

    ```Terminal
    npm install tailwindcss @tailwindcss/postcss postcss
    ```

3. **Crea el archivo de configuración de PostCSS**: Crea un nuevo archivo llamado `.postcssrc.json` en la raíz de tu proyecto.

    ```JSON
    {
      "plugins": {
        "@tailwindcss/postcss": {}
      }
    }
    ```

4. **Añade las directivas de Tailwind a tu archivo CSS principal (`src/styles.css`)**:

    ```CSS
    @import "tailwindcss";
    ```

5. Inicia el servidor de desarrollo nuevamente (`ng serve`) y verifica que Tailwind esté integrado (puedes probar añadiendo una clase simple como `bg-blue-500` a un `div` en `app.html`).

### 3. Definir el Modelo de Datos para un Curso

1. Siguiendo la estructura de carpetas profesional, crea la siguiente ruta: `src/app/features/courses/models/`.
2. Dentro de `src/app/features/courses/models/`, crea un archivo llamado `course.model.ts`.
3. Define una interfaz TypeScript llamada `Course` con las siguientes propiedades:
    - `id: number`
    - `code: string` (ej. "CS101", "MA203")
    - `name: string` (ej. "Introducción a la Programación", "Cálculo Multivariable")
    - `credits: number`
    - `professor: string`
    - `description: string`
    - `imageUrl: string` (puedes usar `https://placehold.co/400x200/E0E0E0/333333?text=Curso+Universitario` como placeholder)

    ```Typescript
    export interface Course {
      id: number;
      code: string;
      name: string;
      credits: number;
      professor: string;
      description: string;
      imageUrl: string;
    }
    ```

### 4. Crear el Componente `CourseList`

1. Siguiendo la estructura de carpetas profesional, crea la siguiente ruta: `src/app/features/courses/components/`.
2. Genera un nuevo componente standalone llamado `course-list` dentro de esta carpeta:

    ```Terminal
    ng generate component features/courses/components/course-list
    ```

### 5. Implementar la Lógica en `CourseList`

1. Abre `src/app/features/courses/components/course-list/course-list.ts`.
2. Importa `OnInit`, `CommonModule` y `FormsModule` (para el filtro), y tu interfaz `Course`.
3. Haz que la clase `CourseList` implemente `OnInit`.
4. Declara una propiedad `courses: Course[] = []` para almacenar la lista de cursos.
5. Declara una propiedad `allCourses: Course[] = []` para guardar todos los cursos y permitir el filtrado.
6. Declara una propiedad `filterTerm: string = ''` para el campo de búsqueda.
7. Implementa el método `ngOnInit()` para cargar una lista de datos simulados de cursos. Asegúrate de que los datos simulados coincidan con la interfaz `Course`. Puedes simular un pequeño retraso con `setTimeout` para imitar una carga de red.
8. Crea un método `filterCourses()` que actualice la lista `courses` basándose en si `filterTerm` está vacío o si el `code` o `name` del curso incluye el `filterTerm`.

    ```Typescript
    import { Component, OnInit } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { FormsModule } from '@angular/forms';
    import { Course } from '../../models/course.model'; // Asegúrate de que la ruta sea correcta

    @Component({
      selector: 'app-course-list',
      imports: [CommonModule, FormsModule],
      templateUrl: './course-list.html',
      styleUrl: './course-list.css'
    })
    export class CourseList implements OnInit {
      courses: Course[] = [];
      allCourses: Course[] = [];
      filterTerm: string = '';
      loading: boolean = true; // Para simular carga

      constructor() { }

      ngOnInit(): void {
        this.loading = true;
        // Simular carga de datos
        setTimeout(() => {
          this.allCourses = [
            {
              id: 1,
              code: 'CS101',
              name: 'Introducción a la Programación',
              credits: 3,
              professor: 'Dr. Ana Pérez',
              description: 'Fundamentos de programación utilizando Python.',
              imageUrl: 'https://placehold.co/400x200/E0E0E0/333333?text=CS101'
            },
            {
              id: 2,
              code: 'MA203',
              name: 'Cálculo Multivariable',
              credits: 4,
              professor: 'Prof. Juan García',
              description: 'Estudio de funciones de varias variables, derivadas parciales e integrales múltiples.',
              imageUrl: 'https://placehold.co/400x200/D0D0D0/444444?text=MA203'
            },
            {
              id: 3,
              code: 'HI301',
              name: 'Historia Universal',
              credits: 3,
              professor: 'Dra. Laura Gómez',
              description: 'Recorrido por los eventos y civilizaciones más importantes de la historia.',
              imageUrl: 'https://placehold.co/400x200/C0C0C0/555555?text=HI301'
            },
            {
              id: 4,
              code: 'PH101',
              name: 'Filosofía Antigua',
              credits: 3,
              professor: 'Prof. Carlos Ruiz',
              description: 'Exploración de las principales corrientes filosóficas de la antigüedad.',
              imageUrl: 'https://placehold.co/400x200/B0B0B0/666666?text=PH101'
            },
            {
              id: 5,
              code: 'CS205',
              name: 'Estructuras de Datos',
              credits: 4,
              professor: 'Dr. Ana Pérez',
              description: 'Implementación y análisis de estructuras de datos fundamentales.',
              imageUrl: 'https://placehold.co/400x200/A0A0A0/777777?text=CS205'
            }
          ];
          this.courses = this.allCourses; // Inicialmente, mostrar todos los cursos
          this.loading = false;
        }, 1000);
      }

      filterCourses(): void {
        if (!this.filterTerm) {
          this.courses = this.allCourses;
        } else {
          this.courses = this.allCourses.filter(course =>
            course.name.toLowerCase().includes(this.filterTerm.toLowerCase()) ||
            course.code.toLowerCase().includes(this.filterTerm.toLowerCase())
          );
        }
      }
    }
    ```

### 6. Diseñar la Plantilla de `CourseList` con Tailwind CSS

1. Abre `src/app/features/courses/components/course-list/course-list.html`.
2. Crea un diseño con Tailwind CSS para mostrar la lista de cursos. Incluye:
    - Un título principal.
    - Un campo de entrada para el filtro de búsqueda (`[(ngModel)]="filterTerm"` y `(input)="filterCourses()"`).
    - Un mensaje de carga (`@if (loading)`).
    - Un mensaje si no se encuentran cursos (`@else if (courses.length === 0)`).
    - Un contenedor para la cuadrícula de cursos.
    - Dentro de un bloque `@for (course of courses; track course.id)`, diseña una "tarjeta" para cada curso que muestre:
      - La imagen del curso.
      - El código y nombre del curso.
      - Los créditos.
      - El profesor.
      - Una breve descripción.
      - Un botón "Ver Detalles" (no funcional por ahora).

    ```HTML
    <div class="container mx-auto p-8 bg-gray-50 min-h-screen rounded-xl shadow-inner">
      <h1 class="text-5xl font-extrabold text-center text-blue-800 mb-12 drop-shadow-lg">Catálogo de Cursos Universitarios</h1>

      <!-- Sección de Filtro -->
      <div class="mb-8 flex flex-col sm:flex-row items-center justify-center gap-4 p-6 bg-white rounded-xl shadow-md">
        <input
          type="text"
          placeholder="Buscar cursos por nombre o código (ej. Programación, CS101)"
          class="flex-grow p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 text-lg"
          [(ngModel)]="filterTerm"
          (input)="filterCourses()"
        />
        <button
          class="w-full sm:w-auto bg-blue-600 text-white py-3 px-6 rounded-lg font-semibold hover:bg-blue-700 transition-colors duration-300 shadow-md"
          (click)="filterCourses()"
        >
          Filtrar Cursos
        </button>
      </div>

      @if (loading) {
        <div class="flex justify-center items-center h-64">
          <div class="animate-spin rounded-full h-20 w-20 border-b-4 border-blue-500"></div>
          <p class="ml-4 text-2xl text-blue-600">Cargando cursos...</p>
        </div>
      } @else if (courses.length === 0 && !loading) {
        <div class="text-center p-8 text-2xl text-gray-500 bg-yellow-50 border border-yellow-300 rounded-lg shadow-md">
          <p class="font-bold mb-2">No se encontraron cursos que coincidan con tu búsqueda.</p>
          <p>Intenta con otro término o reinicia el filtro.</p>
        </div>
      } @else {
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
          @for (course of courses; track course.id) {
            <div class="bg-white rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 overflow-hidden flex flex-col">
              <img
                [src]="course.imageUrl"
                [alt]="course.name"
                class="w-full h-40 object-cover rounded-t-xl"
                onerror="this.onerror=null;this.src='https://placehold.co/400x200/CCCCCC/666666?text=No+Image';"
              />
              <div class="p-6 flex flex-col flex-grow">
                <h2 class="text-2xl font-bold text-gray-900 mb-2">{{ course.code }} - {{ course.name }}</h2>
                <p class="text-gray-700 text-sm mb-2">Profesor: <span class="font-semibold text-blue-700">{{ course.professor }}</span></p>
                <p class="text-gray-700 text-sm mb-4">Créditos: <span class="font-semibold">{{ course.credits }}</span></p>
                <p class="text-gray-600 text-base mb-4 line-clamp-3">{{ course.description }}</p>
                <button class="mt-auto w-full bg-blue-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-blue-700 transition-colors duration-300 shadow-md hover:shadow-lg">
                  Ver Detalles del Curso
                </button>
              </div>
            </div>
          } @empty {
            <!-- Este bloque @empty no se mostrará debido al @else if anterior -->
          }
        </div>
      }
    </div>
    ```

### 7. Integrar `CourseList` en `App`

1. Abre `src/app/app.ts`.
2. Importa `CourseList` y añádelo al array `imports` del decorador `@Component`.

    ```Typescript
    import { Component } from '@angular/core';
    import { RouterOutlet } from '@angular/router';
    import { CourseList } from './features/courses/components/course-list/course-list'; // Importa tu componente CourseList

    @Component({
      selector: 'app-root',
      standalone: true,
      imports: [
        RouterOutlet,
        CourseList // Añade CourseList aquí
      ],
      templateUrl: './app.html',
      styleUrl: './app.css'
    })
    export class AppComponent {
      title = 'university-courses-frontend';
    }
    ```

3. Abre `src/app/app.html`.
4. Reemplaza el contenido predeterminado con el selector de tu `CourseList`:

    ```HTML
    <app-course-list></app-course-list>
    ```

### 8. Verificar y Probar

1. Asegúrate de que tu servidor de desarrollo de Angular esté corriendo (`ng serve`).
2. Abre tu navegador en `http://localhost:4200/`.
3. Deberías ver el título del catálogo de cursos, un mensaje de carga temporal, y luego la lista simulada de cursos con un diseño atractivo de Tailwind CSS.
4. Prueba el campo de filtro escribiendo códigos de curso (ej., "CS101") o nombres (ej., "Historia").

¡Felicidades! Has completado tu primer ejercicio autoguiado en Angular, aplicando los conceptos de la Clase 1. Esto te da una base sólida para los temas más avanzados que veremos en las próximas clases. Si encuentras algún problema, revisa los pasos y compara tu código con los ejemplos proporcionados en la clase.
