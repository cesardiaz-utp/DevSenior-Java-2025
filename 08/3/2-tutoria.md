# Tutoría: Navegación (Routing) y Servicios

Comience la tutoría repasando los objetivos de la clase. Pregunte a los estudiantes si sienten que lograron cada uno de ellos. Esto ayuda a identificar áreas donde necesitan más apoyo.

## 1. Repaso de Objetivos de Aprendizaje de la Clase

Comience la tutoría repasando los objetivos de la clase. Pregunte a los estudiantes si sienten que lograron cada uno de ellos. Esto ayuda a identificar áreas donde necesitan más apoyo.

- Comprender el propósito y los beneficios de los Servicios en Angular.
- Crear y registrar servicios utilizando la Inyección de Dependencias.
- Utilizar servicios para centralizar la lógica de negocio y la gestión de datos (simulando una API).
- Comprender el concepto de enrutamiento en una Single Page Application (SPA).
- Configurar rutas en una aplicación Angular.
- Implementar navegación declarativa (`routerLink`) y programática (`Router` service).
- Recibir parámetros de ruta en componentes utilizando la función `input()` (Angular 20+).
- Aplicar las mejores prácticas para la arquitectura de una aplicación Angular.

## 2. Conceptos Clave para Reforzar y Estrategias de Explicación

- **Servicios: El Corazón de la Lógica de Negocio**
  - **Refuerzo**: Pregunte a los estudiantes: "¿Por qué no ponemos toda la lógica de obtención de datos directamente en el componente?" Guíelos hacia las respuestas de **separación de responsabilidades, reusabilidad, mantenibilidad y facilidad de prueba**.
  - **Estrategia**: Utilice analogías. Por ejemplo, un componente es como un "presentador" que solo se preocupa por mostrar y reaccionar, mientras que un servicio es el "experto" o "proveedor de datos" que sabe cómo obtener y manipular la información.
  - `@Injectable({ providedIn: 'root' })`: Enfatice que esto crea un _singleton_ global. Pregunte: "¿Cuándo usarían `providedIn: 'root'` y cuándo no?" (Respuesta: casi siempre `root` para servicios de datos/lógica global; si un servicio solo se necesita en un componente específico y su ciclo de vida debe estar ligado a él, se podría usar `providers` en el `@Component`, pero es menos común y a menudo menos óptimo).
- **Inyección de Dependencias (DI): El Poder de la Flexibilidad**
  - **Refuerzo**: Explique la DI como la forma en que Angular "entrega" los servicios a los componentes. No es el componente quien "busca" el servicio, sino Angular quien lo "proporciona".
  - `inject()` vs. Constructor:
    - **Constructor**: La forma clásica, clara para dependencias principales de una clase.
    - `inject()`: La forma moderna y flexible. Explique sus ventajas:
      - Uso fuera del constructor (ej. en inicializadores de propiedades, funciones auxiliares).
      - Mejor integración con Signals (`effect`, `computed`).
      - Pregunte: "¿Cuándo preferirían `inject()` sobre la inyección por constructor y viceversa?" (Generalmente, `inject()` para concisión si solo se necesita la instancia, constructor si hay lógica de inicialización que depende de las inyecciones).
- **Enrutamiento: Navegación Fluida en SPAs**
  - **Refuerzo**: Asegúrese de que comprendan que una SPA carga una sola página HTML y luego cambia el contenido dinámicamente. El enrutamiento de Angular es clave para esto.
  - **Componentes Clave**:
    - `Routes` array: La "tabla de contenidos" de la aplicación.
    - `RouterOutlet`: El "marco" donde se carga el contenido dinámico.
    - `routerLink`: Navegación declarativa (HTML).
    - `Router` service: Navegación programática (TypeScript).
  - **Lazy Loading (`loadChildren` y `loadComponent`)**:
    - **Importancia**: Reducción del tamaño del bundle inicial, mejora el tiempo de carga.
    - **Diferencia clave**: `loadChildren` para _conjuntos de rutas_ (Feature Routes, como `properties.routes.ts`), loadComponent para un solo componente standalone. Utilice un diagrama simple para ilustrar cómo `loadChildren` carga un "sub-árbol" de la aplicación.
- **Parámetros de Ruta: `input()` (Angular 20+)**
  - **Refuerzo**: Esta es la forma más limpia y moderna. Si `withComponentInputBinding()` está habilitado, los parámetros de la URL se convierten automáticamente en `inputs` del componente.
  - **Ventajas de `input()`**: Concisión, reactividad con Signals, menos boilerplate.
  - **`ActivatedRoute` (Legado/Casos Avanzados)**: Explique que sigue siendo válido y necesario para acceder a otros datos de la ruta (query params, fragmentos, data estática) o para código existente. Insista en la necesidad de desuscripción (`unsubscribe`) al usar `paramMap` (Observable) para evitar fugas de memoria.

## 3. Abordando Desafíos Comunes de los Estudiantes

- **Errores de Ruta**:
  - Verificar que `path` en `app.routes.ts` coincida con el `routerLink` o la navegación programática.
  - Asegurarse de que el `path` con parámetro (`:id`) esté correctamente definido.
  - Confirmar que `RouterOutlet` esté presente en `app.component.html`.
  - Revisar la configuración de `provideRouter` en `app.config.ts`.
- **Problemas con `input()` y parámetros de ruta**:
  - Recordar habilitar `withComponentInputBinding()` en `app.config.ts`.
  - Verificar que el nombre del `input()` en el componente coincida exactamente con el nombre del parámetro en la ruta (ej., `:id` en la ruta -> `id = input<string>()` en el componente).
  - Asegurarse de manejar la conversión de tipo (ej., `parseInt(idString, 10)`).
- **Gestión de Datos Asíncronos en Servicios**:
  - Recordar que los servicios simulan llamadas a API con `Promise` y `setTimeout`.
  - Enfatizar que los componentes deben manejar los estados de carga (`loading`) y error (`error`) al consumir estos servicios.
- Fugas de Memoria con `ActivatedRoute` (si se usa `paramMap`):
  - Si algún estudiante usó `ActivatedRoute.paramMap`, es crucial revisar que hayan implementado `ngOnDestroy` y `unsubscribe()`. Este es un punto importante para la calidad del código.

## 4. Actividades de Refuerzo o Discusión

- **Preguntas Abiertas**:
  - "Si tuvieran que añadir una nueva sección a la aplicación (ej., 'Usuarios'), ¿cómo organizarían las rutas y los servicios para esa sección?"
  - "¿Cuándo usarían `routerLink` y cuándo `router.navigate()`?"
  - "¿Qué ventajas ven en tener los datos en un servicio en lugar de directamente en el componente de lista?"
- **Debugging en Vivo**: Tome un error común (ej., ruta no encontrada, parámetro no recibido) y depúrelo en vivo con los estudiantes, mostrando cómo usar las herramientas de desarrollo del navegador.
- **Pequeño Reto de Codificación**: Pida a los estudiantes que añadan una ruta adicional a uno de los proyectos (ej., una página "Acerca de" simple) y que naveguen a ella.
- **Revisión de Código**: Pida a algunos estudiantes que compartan su código de servicio o de componente de detalle para una revisión rápida y constructiva, destacando buenas prácticas y sugiriendo mejoras.
