# Tutoría: Formularios Reactivos, Gestión de Estado Compleja y Mejores Prácticas

Esta sección está diseñada para el profesor, ofreciendo una guía para reforzar los conceptos clave de la Clase 4 y asegurar que los estudiantes alcancen los objetivos de aprendizaje.

## 1. Repaso de Objetivos de Aprendizaje de la Clase

Comience la tutoría repasando los objetivos de la clase. Pregunte a los estudiantes si sienten que lograron cada uno de ellos. Esto ayuda a identificar áreas donde necesitan más apoyo.

- **Diseñar y construir formularios reactivos** robustos y reutilizables en Angular.
- **Validar formularios de manera eficiente** utilizando validadores síncronos y asíncronos.
- **Gestionar el estado complejo** de la aplicación mediante servicios y Signals.
- **Persistir y recuperar el estado del cliente** utilizando mecanismos de almacenamiento local.
- **Aplicar mejores prácticas** para optimizar el rendimiento y la mantenibilidad del código Angular.

## 2. Conceptos Clave para Reforzar y Estrategias de Explicación

- **Formularios Reactivos: Control Total y Previsibilidad**
  - **Refuerzo**: Enfatice la diferencia con los formularios `Template-driven`. Los reactivos son "código primero", lo que los hace más robustos para formularios complejos.
  - **Componentes Clave**: `FormControl`, `FormGroup`, `FormArray`. Utilice un diagrama para mostrar cómo se anidan.
  - `ReactiveFormsModule`: Asegúrese de que esté importado en `app.config.ts` y en los `imports` de los componentes `standalone`.
  - `valueChanges` y `statusChanges`: Muestre cómo suscribirse a ellos para depuración o lógica dinámica.
  - `FormBuilder`: Presente `FormBuilder` como una herramienta conveniente para crear formularios reactivos de forma más concisa, especialmente para estructuras anidadas o arrays.
- **Validación de Formularios: Asegurando la Calidad de los Datos**
  - **Estados del Control**: Repase los estados clave de un `FormControl` (`valid`, `invalid`, `touched`, `dirty`, `pending`) y cómo usarlos para feedback visual.
  - **Validadores Síncronos (`Validators`)**: Repase los más comunes (`required`, `min`, `max`, `pattern`, `minLength`, `maxLength`).
  - **Validadores Personalizados Síncronos**: Explique cómo crear funciones simples que devuelven un objeto de error. Proporcione ejemplos de validadores a nivel de `FormControl` y a nivel de `FormGroup` (como `matchPasswordValidator`).
  - **Validadores Asíncronos**: Esta es la parte más compleja.
    - **Cuándo usarlos**: Cuando la validación requiere una operación asíncrona (ej., llamada a API para unicidad).
    - **Cómo funcionan**: Devuelven un `Promise` o `Observable`. Angular espera a que se resuelvan.
    - **`debounceTime` y `switchMap` (o `take(1)`)**: Explique su importancia para optimizar las llamadas a la API y evitar condiciones de carrera o peticiones excesivas.
    - `updateOn`: Discuta cuándo usar `'blur'` o `'submit'` para validadores asíncronos.
  - **Mostrar Errores en la Plantilla**: Reafirme el uso de `control.invalid`, `control.touched`, `control.errors?.['key']` para una buena experiencia de usuario. Muestre el estado `pending` para validadores asíncronos.
- **Gestión Avanzada de Estado con Servicios y Signals**:
  - **Refuerzo**: Reitere el concepto de "Fuente Única de Verdad" y la importancia de la inmutabilidad al actualizar Signals.
  - **Patrones**: Muestre cómo un servicio puede exponer Signals de solo lectura (`asReadonly()`) y métodos para mutar el estado de forma controlada.
  - **`computed` y `effect`**: Cómo se utilizan para derivar estado o realizar efectos secundarios reactivamente (como la persistencia en `localStorage`). Enfatice que `effect` es para efectos secundarios y no debe cambiar directamente otras Signals (a menos que se use `allowSignalWrites` con precaución).
- **Persistencia de Estado del Cliente (`localStorage` / `sessionStorage` / `Cookies`)**:
  - **Cuándo usar cada uno**: Explique las diferencias clave en persistencia, alcance y capacidad entre `localStorage`, `sessionStorage` y `Cookies`.
  - **Limitaciones**: Solo cadenas para `localStorage`/`sessionStorage`, necesidad de `JSON.stringify()` y `JSON.parse()`. Tamaño limitado para `Cookies`.
  - **Integración**: Muestre el patrón de cargar en el constructor del servicio y guardar en cada actualización del estado (idealmente usando un `effect` para automatizarlo para `localStorage`/`sessionStorage`).
  - **Consideraciones de Seguridad: ADVERTENCIA CRÍTICA**: Nunca almacene información sensible (contraseñas, tokens JWT sin cifrar, datos de tarjetas de crédito) en `localStorage` o `sessionStorage` directamente debido a las vulnerabilidades de XSS. Explique cómo el flag `HttpOnly` en las cookies mitiga esto para la autenticación.
- **Optimización del Rendimiento y Mantenibilidad**:
  - `ChangeDetectionStrategy.OnPush`: Explique cómo reduce las comprobaciones de Angular, pero requiere que las entradas sean inmutables. Discuta su relación con Signals (Signals ya son eficientes, pero `OnPush` sigue siendo útil para `Input`s tradicionales y componentes mixtos).
  - **`trackBy` para `@for`**: Demuestre su impacto en el rendimiento de listas grandes, evitando la recreación innecesaria del DOM.
  - **Signals**: Reitere que son inherentemente eficientes y cómo su granularidad mejora el rendimiento.
  - **Lazy Loading**: Recuérdeles su importancia para el tamaño del bundle inicial y los tiempos de carga.
  - **Tree-shaking y Standalone**: Cómo estas características modernas de Angular contribuyen a bundles más pequeños.
  - **Organización del Código**: Enfatice la importancia de una estructura de carpetas lógica por característica, el uso de archivos de barril (`index.ts`) y convenciones de nomenclatura consistentes para la mantenibilidad a largo plazo.
  - **Linting y Pruebas**: Brevemente, pero firmemente, reitere su valor para la calidad del código, la detección temprana de errores y la confianza en la refactorización.

## 3. Abordando Desafíos Comunes de los Estudiantes

- **Errores de `ReactiveFormsModule`**: Si los formularios no se vinculan, verificar la importación en `app.config.ts` y en el `imports` del componente.
- **Validaciones que no se activan**:
  - Asegurarse de que `formControlName` esté correcto.
  - Verificar que `control.touched` o `control.dirty` se estén usando correctamente en la plantilla.
  - Asegurarse de llamar a `form.markAllAsTouched()` en `onSubmit()` para mostrar errores en campos no tocados.
- **Validadores asíncronos que no funcionan**:
  - Verificar que el validador esté en el array `asyncValidators` y no en `validators`.
  - Asegurarse de que el Observable/Promise devuelva `null` o un objeto de error.
  - Revisar el `debounceTime` y `switchMap` (o `take(1)`) para entender el flujo.
  - Depurar la llamada a la API subyacente en la pestaña "Network" del navegador (si aplica en el ejercicio autoguiado).
- **Problemas de inmutabilidad con Signals**: Si los cambios no se reflejan, es probable que estén mutando un objeto/array en lugar de crear uno nuevo. Recuérdeles `map`, `filter`, `spread operator (...)` para operaciones inmutables.
- **Errores de `localStorage` / `sessionStorage`**: Asegurarse de usar `JSON.stringify()` y `JSON.parse()`. Revisar la pestaña "Application" -> "Local Storage" / "Session Storage" en las herramientas de desarrollo del navegador para verificar los datos guardados y los posibles errores de parseo en la consola.

## 4. Actividades de Refuerzo o Discusión

- **Preguntas Abiertas**:
  - "Si tuvieran que añadir un formulario de 'configuración de usuario' con varias opciones (tema, notificaciones, idioma), ¿cómo lo estructurarían con `FormGroup`s anidados?"
  - "¿Cómo manejarían la validación de un campo 'confirmar contraseña' que debe coincidir con el campo 'contraseña'?" (Validador personalizado a nivel de `FormGroup`).
  - "Si tuvieran un carrito de compras, ¿qué datos almacenarían en `localStorage` y por qué, y qué precauciones tomarían?"
  - "¿Qué herramientas usarían para asegurar la calidad del código en un proyecto Angular grande?"
- **Debugging en Vivo**: Tome un formulario con validaciones y demuestre cómo usar las herramientas de desarrollo del navegador para inspeccionar el estado del formulario (`value`, `status`, `errors`, `touched`, `dirty`, `pending`). Simule errores de red para mostrar el manejo de errores (en el ejercicio autoguiado).
- **Pequeño Reto de Codificación**: Pida a los estudiantes que añadan un validador personalizado (ej., un rango de créditos para un curso, o un validador de URL para la imagen) a uno de los formularios. O que implementen una funcionalidad de "borrar carrito" que también limpie `localStorage`.
- **Revisión de Código**: Pida a algunos estudiantes que compartan su PropertyFormComponent o CourseFormComponent y su Service correspondiente para una revisión, enfocándose en la estructura del formulario, las validaciones (síncronas y asíncronas), la integración con el servicio y la lógica de persistencia.

La clave de esta tutoría es la interacción. Anime a los estudiantes a hacer preguntas, a explicar sus soluciones y a debatir las diferentes aproximaciones a los problemas.
