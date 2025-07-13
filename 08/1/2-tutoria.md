# Tutoría: Introducción a Angular, CLI, Componentes y Estructura Moderna

## 1. Revisión del Entorno de Desarrollo

- **Preguntas al grupo**:
  - "¿Alguien tuvo problemas con la instalación de Node.js o Angular CLI?" (Ofrecer soporte rápido si es necesario).
  - "¿Qué comandos usamos para verificar las versiones de Node, npm y Angular CLI?" (Pedir a los estudiantes que los recuerden y los ejecuten si es posible).
  - "¿Qué extensiones de VS Code les parecieron más útiles y por qué?"
- **Demostración rápida (si es necesario)**: Mostrar cómo verificar las versiones, o cómo solucionar el problema de `Set-ExecutionPolicy` en PowerShell si fue un punto de fricción.

## 2. Debate sobre SPAs vs. MPAs

- **Discusión abierta**:
  - "¿Cuál es la principal diferencia entre una SPA y una aplicación web tradicional (MPA)?"
  - "Mencionen una ventaja y una desventaja de las SPAs."
  - "¿Por qué Angular es una buena opción para construir SPAs?"
  - "¿Cómo se relaciona el proyecto de ofertas inmobiliarias que creamos con el concepto de SPA?" (Enfocarse en la carga inicial y las actualizaciones dinámicas).

## 3. Fundamentos de Angular en `PropertyList`

- **Análisis de código en vivo**: Abrir `src/app/features/properties/components/property-list/property-list.ts` y `property-list.html`.
  - **Componente Standalone**:
    - "¿Qué hace que `PropertyList` sea un componente standalone?" (Esperar mención del array `imports`).
    - "¿Por qué importamos `CommonModule` y `FormsModule` aquí?" (Para `pipes` y `ngModel`).
  - **Plantillas y Data Binding**:
    - **Señalar ejemplos de interpolación `{{ }}`**: "Dónde estamos mostrando datos del componente en el HTML?"
    - **Señalar ejemplos de property binding `[ ]`**: "¿Cómo estamos vinculando la URL de la imagen o el atributo `alt`?"
    - **Señalar ejemplos de event binding `( )`**: "¿Qué sucede cuando el usuario escribe en el campo de filtro o hace clic en el botón 'Buscar'?"
    - **Señalar two-way data binding `[(ngModel)]`**: "¿Cómo logramos que el valor del input y la propiedad `filterCity` se mantengan sincronizados?"
  - **Nueva Sintaxis de Control de Flujo (`@if`, `@for`)**:
    - "¿Pueden identificar los bloques `@if` y `@for` en la plantilla?"
    - "¿Cuál es la importancia del bloque `track` en `@for`?" (Clave para el rendimiento y la identificación de elementos).
    - "¿Cuándo se muestra el bloque `@empty`?" (Cuando la colección está vacía).

## 4. TypeScript en Acción

- **Revisión de `property.model.ts`**:
  - "¿Qué es una `interface` en TypeScript y para qué la usamos en `property.model.ts`?" (Definir la forma de los datos).
  - "¿Qué beneficio obtenemos al tipar `properties: Property[]` en `PropertyList`?" (Detección temprana de errores, autocompletado).
- **Preguntas de escenario**:
  - "Si accidentalmente escribo `property.adress` (con una 's' en lugar de 'dd') en la plantilla, ¿qué me diría TypeScript?"
  - "¿Cómo ayuda TypeScript a la colaboración en un equipo grande?"

## 5. Integración y Uso de Tailwind CSS

- **Revisión de la configuración de Tailwind CSS**:
  - "¿Cuáles son los pasos clave para integrar Tailwind CSS manualmente en un proyecto Angular?" (Instalar paquetes, crear `.postcssrc.json`, añadir directivas en `styles.css`).
  - "¿Por qué incluimos @tailwind base; @tailwind components; @tailwind utilities; en styles.css?"
- **Identificación en la plantilla**:
  - Pedir a los estudiantes que abran **property-list.html** y señalen algunas clases de Tailwind que hayan usado y expliquen brevemente qué hacen (ej., `container`, `mx-auto`, `p-8`, `grid`, `gap-8`, `shadow-lg`, `text-indigo-600`).
  - "¿Cómo se diferencia el enfoque de Tailwind de un framework CSS tradicional como Bootstrap?"

## 6. El Ciclo de Vida: `ngOnInit`

- **Discusión**:
  - "¿Cuál es el propósito principal del método `ngOnInit`()?" (Inicialización después de que los inputs están disponibles).
  - "¿Por qué cargamos los datos simulados dentro de `ngOnInit` y no en el constructor de `PropertyList`?" (El constructor es para inyección de dependencias, `ngOnInit` para lógica que depende de la inicialización del componente).
