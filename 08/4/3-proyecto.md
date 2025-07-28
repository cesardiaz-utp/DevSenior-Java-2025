# Proyecto del Módulo 8: Sistema de Gestión de Campus Universitario (Frontend)

Este documento detalla los requisitos funcionales y técnicos para el desarrollo del frontend de un Sistema de Gestión de Campus Universitarios. Este proyecto es una continuación y expansión del trabajo realizado en el Módulo 7 (`university-courses-frontend`), transformando una aplicación de listado de cursos en un sistema integral para gestionar estudiantes, profesores, cursos, departamentos, horarios y calificaciones dentro de un entorno universitario.

El objetivo principal es construir una interfaz de usuario intuitiva y robusta utilizando las últimas características de Angular (v20), incluyendo componentes `standalone`, Signals, y formularios reactivos. Por ahora, la gestión de datos se realizará en memoria con persistencia a través de `localStorage`, simulando la interacción con un backend que se integrará en módulos posteriores.

## 1. Requisitos Funcionales

### 1.1. Gestión de Usuarios

Este módulo permitirá la administración de diferentes tipos de usuarios dentro del sistema.

- **Registro de Usuarios**:
  - Los usuarios (estudiantes, profesores, administradores) podrán registrarse en el sistema.
  - Se requerirán campos como nombre, apellido, email, contraseña, y tipo de rol (estudiante, profesor, administrador).
  - Validación de formato de email y fortaleza de contraseña.
- **Inicio de Sesión (Autenticación)**:
  - Los usuarios podrán iniciar sesión con su email y contraseña.
  - Simulación de autenticación: Se verificará si el usuario existe en los datos locales y si la contraseña coincide.
  - Manejo de estados de sesión (usuario logueado/deslogueado).
- **Gestión de Perfiles**:
  - Los usuarios podrán ver y editar su propia información de perfil (excepto el tipo de rol).
  - Los administradores podrán ver y editar la información de cualquier usuario.
- **Listado de Usuarios (Solo Administradores)**:
  - Los administradores podrán ver una lista de todos los usuarios registrados.
  - Funcionalidad de búsqueda y filtrado por nombre, email o rol.
- **Creación/Edición de Usuarios (Solo Administradores)**:
  - Los administradores podrán añadir nuevos usuarios y editar los existentes, incluyendo la asignación de roles.
- **Eliminación de Usuarios (Solo Administradores)**:
  - Los administradores podrán eliminar usuarios del sistema.

### 1.2. Gestión de Cursos

- **Listado de Cursos**:
  - Visualización de todos los cursos disponibles con detalles clave (nombre, código, créditos, departamento, estudiantes inscritos/máximo).
  - Funcionalidad de búsqueda y filtrado por nombre, código o departamento.
  - Indicador visual para cursos llenos.
- **Detalles del Curso**:
  - Visualización de información detallada de un curso específico.
  - Opción para "Inscribirse" en un curso (si el usuario es estudiante y el curso no está lleno).
- **Creación/Edición de Cursos (Solo Administradores/Profesores)**:
  - Formulario para añadir nuevos cursos.
  - Formulario para editar cursos existentes.
  - Campos requeridos: nombre, código, créditos, departamento, descripción, URL de imagen, estudiantes inscritos (valor inicial 0), máximo de estudiantes.
  - Validaciones: código único, créditos en rango, estudiantes inscritos no mayor que máximo.
- **Eliminación de Cursos (Solo Administradores/Profesores)**:
  - Opción para eliminar un curso.

### 1.3. Gestión de Departamentos

- **Listado de Departamentos**:
  - Visualización de todos los departamentos con su nombre y una breve descripción.
  - Funcionalidad de búsqueda y filtrado.
- **Creación/Edición de Departamentos (Solo Administradores)**:
  - Formulario para añadir nuevos departamentos.
  - Formulario para editar departamentos existentes.
  - Campos requeridos: nombre, descripción.
- **Eliminación de Departamentos (Solo Administradores)**:
  - Opción para eliminar un departamento (con advertencia si tiene cursos asociados).

### 1.4. Gestión de Horarios (Básico)

- **Asignación de Horarios (Solo Administradores/Profesores)**:
  - Para cada curso, se podrá asignar un horario (ej., "Lunes 10:00-12:00", "Miércoles 14:00-16:00").
  - Se podrá seleccionar un profesor para impartir el curso.
  - Simulación: Por ahora, solo se guardará la cadena de texto del horario y el ID del profesor asignado.
- **Visualización de Horarios**:
  - Los estudiantes podrán ver los horarios de sus cursos inscritos.
  - Los profesores podrán ver los horarios de los cursos que imparten.
  - Los administradores podrán ver todos los horarios.

### 1.5. Gestión de Calificaciones (Básico)

- **Registro de Calificaciones (Solo Profesores)**:
  - Los profesores podrán seleccionar uno de sus cursos.
  - Para cada estudiante inscrito en ese curso, podrán introducir una calificación (ej., 0-100, A-F).
  - Simulación: Por ahora, solo se guardará la calificación como un número o cadena.
- **Visualización de Calificaciones (Estudiantes)**:
  - Los estudiantes podrán ver sus calificaciones para los cursos en los que están inscritos.

## 2. Requisitos Técnicos (Frontend)

El frontend se desarrollará utilizando Angular (v20) con un enfoque en la modernidad, rendimiento y mantenibilidad.

### 2.1. Arquitectura

- **Angular Standalone Components**: Todos los componentes, directivas y pipes se crearán como `standalone`, eliminando la necesidad de `NgModules` y promoviendo un mejor tree-shaking.
- **Servicios para Lógica de Negocio y Estado**:
  - Se utilizarán servicios (`@Injectable({ providedIn: 'root' })`) para encapsular la lógica de negocio, la gestión de datos y la persistencia.
  - Cada entidad principal (Usuarios, Cursos, Departamentos, etc.) tendrá su propio servicio.
- **Gestión de Estado con Signals**:
  - El estado de la aplicación (listas de usuarios, cursos, etc., y estados de carga/error) se gestionará utilizando `Signals` de Angular.
  - Los servicios expondrán `Signals` de solo lectura (`.asReadonly()`) para que los componentes las consuman de forma reactiva.
  - Se utilizarán `computed` Signals para derivar datos del estado (ej., cursos filtrados, total de estudiantes).
  - Se utilizarán `effects` para realizar efectos secundarios reactivos, como la persistencia en `localStorage`.

### 2.2. Estilado y UI/UX

- **Tailwind CSS**: El estilado de la aplicación se realizará exclusivamente con Tailwind CSS para un desarrollo rápido y un diseño responsive.
- **Diseño Responsive**: La interfaz de usuario debe ser completamente responsive y adaptable a diferentes tamaños de pantalla (móvil, tablet, escritorio) utilizando las utilidades de Tailwind.
- **Experiencia de Usuario (UX)**:
  - Navegación intuitiva y clara.
  - Feedback visual para acciones de usuario (ej., estados de carga, mensajes de éxito/error).
  - Formularios con validación en tiempo real y mensajes de error claros.
  - Uso de iconos relevantes para mejorar la comprensión visual.

### 2.3. Enrutamiento

- **Angular Router**: Se utilizará el enrutador de Angular para gestionar la navegación entre las diferentes vistas de la aplicación.
- **Lazy Loading**: Las rutas de las características principales (ej., `/users`, `/courses`, `/departments`) se cargarán de forma perezosa (`loadChildren` o `loadComponent`) para optimizar el tiempo de carga inicial de la aplicación.
- **Parámetros de Ruta**: Se utilizarán parámetros de ruta para la edición y visualización de detalles (ej., `/courses/edit/:id`, `/users/:id`).

### 2.4. Formularios

- **Formularios Reactivos**: Todos los formularios para la entrada de datos (creación/edición de usuarios, cursos, departamentos) se construirán utilizando Formularios Reactivos de Angular.
- **Validación Síncrona**: Se implementarán validaciones síncronas utilizando Validators incorporados (ej., `required`, `minlength`, `maxlength`, `pattern`, `min`, `max`) y validadores personalizados cuando sea necesario (ej., `matchPassword`, `uniqueCode`).
- **Validación Asíncrona (Simulada)**: Aunque no habrá `HttpClient` en este módulo, se puede simular un validador asíncrono con un `setTimeout` para demostrar el concepto (ej., verificar la unicidad de un email/código de curso).
- `FormBuilder`: Se utilizará `FormBuilder` para simplificar la creación de `FormGroups` y `FormArrays` complejos.

### 2.5. Persistencia de Datos (Local)

- `localStorage`: Todos los datos de la aplicación (usuarios, cursos, departamentos, etc.) se persistirán en el `localStorage` del navegador.
- **Sincronización con Signals**: Los servicios utilizarán `effect`s de Signals para guardar automáticamente el estado actualizado en `localStorage` cada vez que las Signals de datos cambien.
- **Carga Inicial**: Al iniciar la aplicación, los servicios intentarán cargar los datos desde `localStorage` para restaurar el estado de la sesión anterior.
- **Manejo de JSON**: Los objetos y arrays se serializarán a JSON (`JSON.stringify()`) antes de guardarlos y se deserializarán (`JSON.parse()`) al recuperarlos.

### 2.6. Rendimiento y Mantenibilidad

- `ChangeDetectionStrategy.OnPush`: Se aplicará la estrategia de detección de cambios `OnPush` a los componentes donde sea apropiado para optimizar el rendimiento, especialmente en componentes que reciben `@Input()`s.
- **`trackBy` en `@for`**: Se utilizará la función `trackBy` en todos los bucles `@for` para mejorar el rendimiento de renderizado de listas grandes.
- **Organización del Código**:
  - Estructura de carpetas por característica (ej., `features/users`, `features/courses`, `features/departments`, `shared`).
  - Uso de archivos de barril (`index.ts`) para simplificar las importaciones.
  - Convenciones de nomenclatura consistentes siguiendo las guías de estilo de Angular.
- **Manejo de Errores**: Implementación de mecanismos de manejo de errores en los servicios y componentes para proporcionar feedback al usuario y depurar problemas.

### 2.7. Seguridad (Aspectos Frontend)

- **Validación de Entrada**: Fuerte validación del lado del cliente en todos los formularios para prevenir la entrada de datos maliciosos o incorrectos.
- **Simulación de Autenticación/Autorización**: Se simulará la lógica de autenticación y autorización (ej., mostrar/ocultar elementos de UI basados en el "rol" del usuario logueado, permitir/denegar acceso a rutas).

### 2.8. Accesibilidad

- Se considerarán los principios básicos de accesibilidad (ARIA attributes, semántica HTML) para asegurar que la aplicación sea utilizable por personas con discapacidades.

## 3. Estructura del Proyecto

La estructura del proyecto seguirá un enfoque modular y por características:

```text
src/
├── app/
│   ├── app.config.ts           # Configuración global de la aplicación (rutas, providers)
│   ├── app.component.ts        # Componente raíz
│   ├── app.routes.ts           # Rutas principales de la aplicación
│   └── core/                   # Módulo para servicios singleton (ej. AuthService)
│       └── services/
│           └── auth.service.ts
│   ├── features/               # Módulos por característica
│   │   ├── users/
│   │   │   ├── components/     # Componentes relacionados con usuarios (list, form, detail)
│   │   │   ├── models/         # Modelos de datos para usuarios
│   │   │   ├── services/       # Servicio de gestión de usuarios
│   │   │   └── users.routes.ts # Rutas específicas de usuarios
│   │   ├── courses/            # Módulo de cursos (continuación del Módulo 7)
│   │   │   ├── components/
│   │   │   ├── models/
│   │   │   ├── services/
│   │   │   └── courses.routes.ts
│   │   └── departments/        # Nuevo módulo para departamentos
│   │       ├── components/
│   │       ├── models/
│   │       ├── services/
│   │       └── departments.routes.ts
│   │   └── schedules/          # Nuevo módulo para horarios
│   │       ├── components/
│   │       ├── models/
│   │       ├── services/
│   │       └── schedules.routes.ts
│   │   └── grades/             # Nuevo módulo para calificaciones
│   │       ├── components/
│   │       ├── models/
│   │       ├── services/
│   │       └── grades.routes.ts
│   └── shared/                 # Componentes, pipes, directivas, modelos reutilizables
│       ├── components/         # Ej. Header, Footer, Modal, Card genérica
│       ├── pipes/
│       └── directives/
│
├── assets/                     # Archivos estáticos (imágenes, etc.)
├── environments/               # Configuraciones de entorno
└── main.ts                     # Punto de entrada de la aplicación
```

## 4. Consideraciones Futuras (Próximos Módulos)

Aunque para este módulo la gestión de datos será en memoria con `localStorage`, se tiene en cuenta que en módulos posteriores se integrará:

- **Integración con Backend (HttpClient)**: Se reemplazarán las operaciones en memoria por llamadas HTTP reales a una API RESTful (ej., `json-server` o un backend real) utilizando `HttpClient` de Angular.
- **Autenticación y Autorización Real**: Implementación de un flujo de autenticación JWT o OAuth con un backend real, incluyendo interceptores HTTP para añadir tokens y manejar errores de autenticación.
- **Librerías de UI**: Posible integración de librerías de componentes UI como Angular Material o PrimeNG para acelerar el desarrollo de la interfaz y mejorar la consistencia visual.
- **Pruebas Unitarias y E2E**: Implementación exhaustiva de pruebas unitarias con Jest/Karma y pruebas E2E con Cypress/Playwright.
- **Notificaciones**: Implementación de un sistema de notificaciones (ej., toasts) para feedback al usuario.
- **Internacionalización (i18n)**: Soporte para múltiples idiomas.

Este proyecto sentará las bases para construir una aplicación Angular robusta, escalable y mantenible, aplicando las mejores prácticas y las características más modernas del framework.
