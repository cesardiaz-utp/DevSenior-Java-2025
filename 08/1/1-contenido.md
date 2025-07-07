# Clase 1: Introducción a React con Vite, Hooks y Estructura Moderna

**¡Hola a todos!**
¡Bienvenidos a la primera clase del **Módulo 8**! Este módulo marca un giro emocionante hacia el desarrollo frontend, donde aprenderemos a construir interfaces de usuario dinámicas, eficientes y modulares utilizando **React JS**. Dejaremos la comodidad del backend para sumergirnos en la creación de experiencias interactivas y visualmente atractivas que interactuarán con las APIs que ya hemos dominado.

En esta clase, no solo exploraremos los fundamentos de React y la rapidez de Vite, sino que los aplicaremos paso a paso para construir un **frontend básico para listar cursos**, consumiendo datos de nuestra API de Spring Boot. A medida que avancemos en el desarrollo de este mini-proyecto, introduciremos y explicaremos cada concepto clave: desde la configuración del entorno, pasando por los componentes y el manejo de estado con Hooks, hasta la integración de estilos con Tailwind CSS y el tipado con TypeScript.

Prepárense para construir su primera aplicación React full stack, conectando lo que ya saben de Spring Boot con un frontend moderno.

## Objetivos de Aprendizaje

Al finalizar esta clase, serás capaz de:

- **Comprender el concepto de Single Page Applications (SPA)** y sus ventajas en el desarrollo web moderno.
- **Crear y configurar un proyecto React desde cero con Vite**, entendiendo sus beneficios para el desarrollo frontend.
- **Desarrollar componentes funcionales en React** utilizando JSX e interpolación de variables para construir interfaces dinámicas.
- **Gestionar el estado y los efectos secundarios** en componentes React con los Hooks `useState` y `useEffect`.
- **Integrar y aplicar estilos con Tailwind CSS** para un diseño responsivo y eficiente.
- **Consumir datos de APIs RESTful** utilizando la Fetch API y manejar la comunicación con un backend local.
- **Aplicar TypeScript para añadir tipado estático** a proyectos React, mejorando la robustez y mantenibilidad del código.
- **Conocer una estructura de carpetas profesional** para proyectos React, facilitando la escalabilidad y el mantenimiento.

## 1. Configuración del Entorno de Desarrollo para React y Vite

Antes de sumergirnos en la creación de nuestra primera aplicación React, es crucial tener nuestro entorno de desarrollo configurado correctamente. Necesitaremos instalar Node.js (que incluye npm) y un editor de código adecuado como Visual Studio Code.

### 1.1. Instalar Node.js y npm

![Node JS](https://www.smartsight.in/wp-content/uploads/2021/09/NodeJS-768x448.jpg)

**Node.js** es un entorno de ejecución de JavaScript del lado del servidor que también incluye `npm` (Node Package Manager), la herramienta estándar para gestionar las dependencias en proyectos JavaScript y React. Vite se ejecuta sobre Node.js, por lo que es un requisito fundamental.

1. **Descargar Node.js**: Visita el sitio web oficial de Node.js: [https://nodejs.org/es/download/](https://nodejs.org/es/download/)
2. **Elegir Versión**: Se recomienda descargar la versión **LTS (Long Term Support)**, ya que es la más estable y recomendada para la mayoría de los usuarios.
3. **Instalar**: Sigue el asistente de instalación para tu sistema operativo (Windows, macOS, Linux). Asegúrate de que la opción "npm package manager" esté seleccionada durante la instalación.
4. **Verificar Instalación**: Abre tu Terminal (o Símbolo del Sistema en Windows) y ejecuta los siguientes comandos para verificar que Node.js y npm se instalaron correctamente:

    ```bash
    node -v  # Debería mostrar la versión de Node.js (ej. v20.x.x)

    npm -v   # Debería mostrar la versión de npm (ej. 10.x.x)
    ```

### 1.2. Configurar Visual Studio Code (VS Code)

**Visual Studio Code (VS Code)** es un editor de código fuente gratuito, ligero y potente desarrollado por Microsoft. Es extremadamente popular en la comunidad de desarrollo web debido a su excelente soporte para JavaScript, React, TypeScript, su amplio ecosistema de extensiones y su depurador integrado.

**Extensiones Recomendadas para React y TypeScript**: Una vez instalado VS Code, ábrelo y busca las siguientes extensiones en la sección "Extensions" (Ctrl+Shift+X o Cmd+Shift+X):

- **ES7 React/Redux/GraphQL/React-Native snippets**: Proporciona fragmentos de código (snippets) útiles para escribir código React más rápido.
- **Prettier - Code formatter**: Formatea automáticamente tu código para mantener un estilo consistente.
- **ESLint**: Herramienta para identificar patrones problemáticos encontrados en el código JavaScript/React.
- **Tailwind CSS IntelliSense**: Ofrece autocompletado, resaltado de sintaxis y linting para clases de Tailwind CSS.

Con Node.js, npm y VS Code configurados, tu entorno de desarrollo estará listo para construir aplicaciones React con Vite.

## 2. Introducción a las Single Page Applications (SPA)

Antes de sumergirnos en React, es fundamental entender el paradigma de desarrollo que React y otras bibliotecas modernas de frontend habilitan: las **Single Page Applications (SPA)**.

### 2.1. ¿Qué son las Single Page Applications (SPA)?

Una **Single Page Application (SPA)**, o **Aplicación de Una Sola Página**, es un tipo de aplicación web que carga una única página HTML en el navegador del usuario y luego actualiza dinámicamente el contenido de esa página a medida que el usuario interactúa con la aplicación. A diferencia de las aplicaciones web tradicionales (**Multi-Page Applications - MPA**) que requieren una recarga completa de la página cada vez que el usuario navega a una nueva sección, una SPA evita estas recargas, proporcionando una experiencia de usuario más fluida y similar a una aplicación de escritorio o móvil.

### 2.2. ¿Cómo Funcionan las SPAs?

Cuando un usuario accede a una SPA, el navegador carga inicialmente un único archivo HTML, junto con todos los recursos necesarios (JavaScript, CSS). Una vez cargado, el JavaScript de la aplicación toma el control:

- **Renderizado Inicial**: El JavaScript renderiza la interfaz de usuario inicial.
- **Interacciones Dinámicas**: Cuando el usuario hace clic en un enlace o un botón, en lugar de solicitar una nueva página al servidor, el JavaScript intercepta el evento, actualiza el DOM (Document Object Model) de la página actual y, si es necesario, realiza llamadas asíncronas a una API (como nuestra API de Spring Boot) para obtener o enviar datos.
- **Actualización de la URL**: Aunque la página no se recarga, la URL en la barra de direcciones del navegador puede actualizarse para reflejar la "vista" actual, permitiendo el uso de los botones de "atrás" y "adelante" del navegador, y la capacidad de compartir enlaces directos (deep linking).

### 2.3. Ventajas Clave de las SPAs

1. **Experiencia de Usuario Fluida y Rápida**: Al evitar las recargas completas de la página, las SPAs ofrecen una transición instantánea entre vistas, lo que resulta en una experiencia de usuario más rápida, interactiva y sin interrupciones, similar a una aplicación nativa.
2. **Rendimiento Mejorado**: Una vez que la página inicial se carga, solo los datos necesarios se intercambian con el servidor (a través de APIs), lo que reduce la cantidad de datos transferidos y el tiempo de espera. El frontend maneja gran parte del renderizado, aliviando la carga del servidor.
3. **Desarrollo Eficiente de APIs**: Las SPAs se basan en APIs RESTful (o GraphQL) para la comunicación con el backend. Esto fomenta una clara separación de responsabilidades entre el frontend y el backend, permitiendo que equipos diferentes trabajen de forma independiente y que la misma API pueda ser consumida por múltiples clientes (web, móvil, etc.).
4. **Despliegue Simplificado**: El frontend de una SPA es típicamente un conjunto de archivos estáticos (HTML, CSS, JavaScript) que pueden ser servidos desde un CDN (Content Delivery Network) o un servidor web simple, lo que simplifica el despliegue y la escalabilidad del frontend.
5. **Reutilización de Código**: Las bibliotecas como React fomentan la modularidad a través de componentes, lo que facilita la reutilización de elementos de UI y lógica en diferentes partes de la aplicación.

### 2.4. Desventajas y Consideraciones

- **SEO (Search Engine Optimization)**: Históricamente, las SPAs presentaban desafíos para los motores de búsqueda, ya que el contenido se carga dinámicamente. Sin embargo, los motores de búsqueda modernos han mejorado significativamente su capacidad para rastrear JavaScript, y técnicas como el **Server-Side Rendering (SSR)** o la pre-renderización pueden mitigar este problema.
- **Tiempo de Carga Inicial**: La carga inicial de una SPA puede ser más lenta que la de una MPA simple, ya que todo el JavaScript y CSS de la aplicación deben descargarse. Esto se optimiza con técnicas como el _code splitting_ y la carga diferida.
- **Complejidad de Desarrollo**: Las SPAs pueden ser más complejas de desarrollar y depurar debido a la gestión del estado del lado del cliente y la interacción asíncrona.

### 2.5. Tecnologías Populares para el Desarrollo de SPAs

Existen varias bibliotecas y frameworks de JavaScript que son ampliamente utilizados para construir Single Page Applications, cada uno con sus propias fortalezas y enfoques:

- [**React (de Meta)**](https://react.dev/): Una biblioteca declarativa y eficiente para construir interfaces de usuario. Se centra en la capa de vista y es muy popular por su flexibilidad, su ecosistema de componentes y el uso del Virtual DOM. Es ideal para proyectos grandes y complejos donde se necesita un alto grado de personalización.
- [**Angular (de Google)**](https://angular.dev/): Un framework completo y robusto para construir aplicaciones web a gran escala. Ofrece una estructura más opinionada con un conjunto de herramientas y convenciones predefinidas (CLI, TypeScript, RxJS, etc.), lo que lo hace adecuado para proyectos empresariales que requieren consistencia y escalabilidad.
- [**Vue.js (creado por Evan You)**](https://vuejs.org/): Un framework progresivo que es fácil de aprender y muy versátil. Puede ser adoptado incrementalmente, desde añadir interactividad a una página existente hasta construir SPAs completas. Es conocido por su simplicidad, rendimiento y excelente documentación.
- [**Svelte (creado por Rich Harris)**](https://svelte.dev/): Un enfoque diferente donde el framework compila tu código en JavaScript vanilla en tiempo de compilación, lo que resulta en paquetes de código muy pequeños y un rendimiento excepcional en tiempo de ejecución. No hay un "runtime" del framework en el navegador.
- [**Ember.js**](https://emberjs.com/): Un framework altamente opinionado que sigue el principio de "convención sobre configuración". Es ideal para equipos que buscan una estructura predecible y una solución "todo incluido" para aplicaciones web ambiciosas y de larga duración.
- [**SolidJS**](https://www.solidjs.com/): Una biblioteca reactiva moderna que ofrece un rendimiento similar al de Svelte al compilar componentes, pero con una sintaxis JSX que se asemeja a React. Se enfoca en la reactividad fina y un rendimiento sin igual.

**ReactJS** es una de las herramientas más populares y eficientes para construir SPAs, proporcionando un modelo de componentes robusto y un Virtual DOM que optimiza las actualizaciones de la interfaz de usuario.

## 3. ¿Qué es ReactJS? Características y Elementos Clave

[![Aprende React js en 10 minutos](https://markdown-videos-api.jorgenkh.no/youtube/Mb37ily9WuY?width=640&height=360)](https://www.youtube.com/watch?v=Mb37ily9WuY)

### 3.1. ¿Qué es ReactJS?

![React JS](https://media.licdn.com/dms/image/v2/D4D12AQF26-NZ279EaA/article-cover_image-shrink_423_752/article-cover_image-shrink_423_752/0/1688018102545?e=1756944000&v=beta&t=DxVlGfDByQR9nCAt4jyNox58qCas4ruoKUzU5fNBn2Q)

**ReactJS** (comúnmente conocido como React) es una **biblioteca de JavaScript de código abierto** declarativa, eficiente y flexible para construir interfaces de usuario. No es un framework completo (como Angular o Vue.js), sino que se centra específicamente en la **capa de vista** de una aplicación (es decir, lo que el usuario ve). Fue desarrollado por Facebook (ahora Meta) y lanzado en 2013.

Su objetivo principal es hacer que la creación de UIs interactivas sea sencilla, permitiendo a los desarrolladores construir aplicaciones web complejas a partir de pequeñas piezas de código reutilizables y aisladas, llamadas **componentes**.

### 3.2. Características Clave de ReactJS

Las características centrales de React son las que lo hacen tan efectivo y una elección popular para el desarrollo frontend:

1. **Basado en Componentes**:
    - **Descripción**: React te permite construir interfaces de usuario dividiéndolas en piezas independientes, reutilizables y autónomas llamadas **componentes**. Cada componente encapsula su propia lógica, estado y UI.
    - **Ventaja**: Fomenta la modularidad, la reutilización del código y una clara separación de preocupaciones, lo que facilita el desarrollo, las pruebas y el mantenimiento de aplicaciones grandes.
2. **Enfoque Declarativo**:
    - **Descripción**: En lugar de manipular directamente el DOM (Document Object Model) del navegador (un enfoque imperativo donde especificas cómo cambiar la UI), React te permite **describir qué quieres que la UI sea** en un momento dado.
    - **Ventaja**: Te enfocas en el estado de tu aplicación y React se encarga de determinar las actualizaciones óptimas del DOM para que la UI coincida con ese estado. Esto simplifica enormemente la lógica de la UI y la hace más predecible.
3. **Flujo de Datos Unidireccional (One-Way Data Flow)**:
    - **Descripción**: Los datos en React fluyen en una sola dirección: de los componentes padres a los componentes hijos a través de las **props** (propiedades). Los componentes hijos no pueden modificar directamente los datos que reciben de sus padres; si necesitan comunicar cambios, deben "llamar de vuelta" a funciones pasadas como props por el padre.
    - **Ventaja**: Este patrón de flujo de datos hace que la aplicación sea más predecible y fácil de depurar, ya que es sencillo seguir de dónde provienen los datos y cómo se propagan los cambios.
4. **Virtual DOM (Document Object Model)**:
    - **Descripción**: React mantiene una representación ligera de la UI en memoria, llamada **Virtual DOM**. Cuando el estado de un componente cambia, React primero actualiza este Virtual DOM. Luego, compara el Virtual DOM actual con la versión anterior para identificar solo las diferencias y aplica únicamente los cambios necesarios al DOM real del navegador.
    - **Ventaja**: Las operaciones directas sobre el DOM real son costosas en términos de rendimiento. El Virtual DOM minimiza estas operaciones, lo que resulta en actualizaciones de UI muy rápidas y eficientes, mejorando significativamente la experiencia del usuario.

### 3.3. Elementos Clave para un Correcto Desarrollo de Aplicaciones con ReactJS

Para dominar React y construir aplicaciones robustas, es fundamental comprender y utilizar correctamente sus elementos clave:

1. **Componentes**: Unidades fundamentales de construcción, típicamente **funcionales** en React moderno, que aceptan `props` y devuelven JSX.
2. **JSX (JavaScript XML)**: Extensión de sintaxis para JavaScript que permite escribir estructuras de UI (HTML) directamente dentro del código JavaScript, transformadas a `React.createElement()` en tiempo de compilación.
3. **Interpolación de Variables en JSX**:
    En JSX, puedes incrustar cualquier expresión de JavaScript dentro de las llaves `{}`. Esto te permite mostrar variables, propiedades de objetos, resultados de funciones, o cualquier lógica JavaScript directamente en tu marcado.

    ```Typescript
    function Saludo({ nombre }) {
      const mensaje = "¡Bienvenido!";
      return (
        <div>
          <h1>{mensaje} {nombre}!</h1>
          <p>La fecha actual es: {new Date().toLocaleDateString()}</p>
          {/* Puedes usar expresiones más complejas, como ternarios */}
          <p>{nombre.length > 5 ? 'Tu nombre es largo.' : 'Tu nombre es corto.'}</p>
        </div>
      );
    }
    ```

    **Ventaja**: Permite que la UI sea dinámica y reactiva a los cambios de datos de manera sencilla y legible, mezclando la lógica JavaScript con la estructura de la interfaz.
4. **Estado (`useState` Hook)**: Permite a los componentes funcionales tener datos que pueden cambiar y provocar una re-renderización del componente.

    ```Javascript
    const [valor, setValor] = useState(valorInicial);
    ```

5. **Efectos Secundarios (`useEffect` Hook)**: Permite realizar operaciones fuera del ciclo de renderizado (fetching de datos, suscripciones, temporizadores).

    ```Javascript
    useEffect(callback, [dependencias]);
    ```

6. **Props (Propiedades)**: La forma principal de comunicar datos de un componente padre a un hijo. Las props son inmutables para el componente hijo.
7. **Renderizado Condicional**: Mostrar diferentes elementos de UI basados en condiciones lógicas (ej., `if`, operador ternario).
8. **Renderizado de Listas y `key`**: Mostrar colecciones de elementos. La propiedad `key` única es crucial para la optimización de rendimiento al actualizar el DOM.
9. **Manejo de Eventos**: Responder a interacciones del usuario (clics, entradas). Los manejadores se definen como atributos en JSX (ej., `onClick={handleClick}`).
10. **Composición de Componentes**: Construir interfaces complejas combinando componentes más pequeños y simples, creando una jerarquía clara.
11. **Estructura de Carpetas Lógica**: Organizar el código de manera intuitiva y escalable (ej., `components/`, `pages/`, `services/`, `hooks/`, `utils/`).
12. **Herramientas de Build Modernas (Vite)**: Compilan, optimizan y sirven tu código React (manejan transpilación de JSX, optimización, hot-reloading).

Comprender estos conceptos y cómo interactúan es fundamental para desarrollar aplicaciones React de manera efectiva, modular y con buen rendimiento.

## 4. Qué es TypeScript y por qué usarlo con React: Añadiendo Tipado al Proyecto

A medida que nuestro proyecto de cursos crece, podemos mejorar su robustez y mantenibilidad utilizando **TypeScript**.

![Typescript](https://miro.medium.com/v2/resize:fit:606/1*UIioHehyD5o_6ydf3w2fuw.png)

[**TypeScript**](https://www.typescriptlang.org/) es un superset de JavaScript que añade tipado estático opcional al lenguaje. Desarrollado y mantenido por Microsoft, TypeScript se compila (o transpila) a JavaScript plano, lo que significa que cualquier navegador o entorno de ejecución de JavaScript puede entenderlo.

[![¿Qué Es TypeScript, Para Qué Sirve y Por Qué Es Mejor Que JavaScript?](https://markdown-videos-api.jorgenkh.no/youtube/y9tu-vGYXCk?width=640&height=360)](https://www.youtube.com/watch?v=y9tu-vGYXCk)

### 4.1. ¿Por qué usar TypeScript en el desarrollo de aplicaciones frontend modernas con React?

1. **Tipado Estático**:
    - **Ventaja**: Permite detectar errores comunes en tiempo de desarrollo (antes de ejecutar el código) en lugar de en tiempo de ejecución. Esto incluye errores de tipo, errores de ortografía de propiedades, acceso a propiedades inexistentes, etc.
    - **Implicación en React**: Puedes definir los tipos de las `props` que un componente espera, el estado que maneja, y el formato de los datos que consumes de las APIs. Esto es invaluable en proyectos grandes y en equipos, ya que sirve como una forma de "documentación viva" y previene muchos errores de integración entre componentes.
2. **Mejor Mantenibilidad y Escalabilidad**:
    - **Ventaja**: A medida que las bases de código crecen, la refactorización y la comprensión del código se vuelven mucho más fáciles con tipos explícitos.
    - **Implicación en React**: Cuando modificas la estructura de un DTO o una `prop` en un componente, TypeScript te alertará automáticamente en todos los lugares donde ese tipo es usado incorrectamente, reduciendo el riesgo de romper otras partes de la aplicación.
3. **Experiencia de Desarrollo Mejorada (DX - Developer Experience)**:
    - **Ventaja**: Ofrece un autocompletado más inteligente (IntelliSense), navegación de código, refactorización automática y detección temprana de errores en IDEs como VS Code.
    - **Implicación en React**: Al escribir componentes, sabrás exactamente qué `props` están disponibles y de qué tipo son, o qué métodos están presentes en un objeto de datos. Esto acelera el desarrollo y reduce la necesidad de consultar la documentación constantemente.
4. **Colaboración en Equipo**:
    - **Ventaja**: Proporciona un contrato claro sobre la forma de los datos y las interfaces, lo que facilita que los diferentes miembros del equipo colaboren en la misma base de código sin introducir conflictos de tipos inesperados.
    - **Implicación en React**: Los desarrolladores de frontend pueden trabajar con confianza sabiendo el formato exacto de los datos que esperan de las APIs de backend (usando las interfaces de TypeScript), y cómo deben interactuar con otros componentes.

### 4.2. Ventajas clave de TypeScript con React

- **Menos Errores en Producción**: Muchos errores lógicos se capturan antes de que lleguen a los usuarios.
- **Refactorización Confiable**: Cambiar la estructura de datos es más seguro y rápido.
- **Código Auto-Documentado**: Los tipos actúan como una forma de documentación para los desarrolladores.
- **Mejora la Escalabilidad de Proyectos**: Facilita el crecimiento de la base de código y la incorporación de nuevos miembros al equipo.

Aunque añadir TypeScript implica una curva de aprendizaje inicial y un poco más de código para las definiciones de tipos, los beneficios a largo plazo en la mantenibilidad, la calidad del código y la experiencia de desarrollo lo hacen una elección muy valiosa para cualquier proyecto React serio.

## 5. ¿Por qué usar React y Vite en 2025?

En el dinámico mundo del desarrollo web, elegir las herramientas adecuadas es crucial para la eficiencia y el rendimiento de tus proyectos. React ha sido durante años la biblioteca de JavaScript dominante para construir interfaces de usuario. Sin embargo, la forma en que inicializamos y desarrollamos aplicaciones React ha evolucionado significativamente, con **Vite** emergiendo como la solución predilecta frente a herramientas más antiguas como Create React App (CRA).

![Vite + React](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3xdgj7v9vhogcr37ar7b.png)

**Vite (pronunciado /vit/ como "vite" en francés, que significa "rápido")**:
Vite es una herramienta de build de próxima generación para el desarrollo web frontend. Su principal ventaja radica en su velocidad asombrosa, tanto en el arranque del servidor de desarrollo como en las actualizaciones en caliente (Hot Module Replacement - **HMR**).

[![¿QUE ES VITE? EN 6 MINUTOS](https://markdown-videos-api.jorgenkh.no/youtube/rmYUlzfT24E?width=640&height=360)](https://www.youtube.com/watch?v=rmYUlzfT24E)

### Ventajas de Vite frente a Create React App (CRA)

- **Arranque Ultra-Rápido del Servidor de Desarrollo**:
  - **CRA**: Utiliza Webpack, que es un _bundler_ basado en JavaScript. Al iniciar el servidor, Webpack debe procesar y _bundlear_ toda la aplicación antes de servirla, lo que puede ser lento en proyectos grandes.
  - **Vite**: Aprovecha los módulos ES Nativos del navegador. Esto significa que el navegador solicita solo los módulos necesarios para la página actual, y Vite los sirve directamente sin un _bundling_ inicial pesado. El servidor de desarrollo arranca casi instantáneamente.
- **Actualizaciones en Caliente (HMR) Instantáneas**:
  - **CRA**: El HMR de Webpack, aunque efectivo, puede volverse lento a medida que la aplicación crece, ya que a menudo necesita re-compilar y re-bundlear secciones significativas del código.
  - **Vite**: Gracias a los módulos ES Nativos, Vite solo invalida y vuelve a cargar los módulos que han cambiado. Esto resulta en actualizaciones de código casi instantáneas en el navegador, mejorando drásticamente la experiencia de desarrollo.
- **Menos Configuración ("Zero-Config")**:
  - Ambas herramientas apuntan a una experiencia "zero-config", pero Vite suele requerir aún menos intervención para comenzar y es más fácil de configurar para casos de uso avanzados sin "ejectar" la configuración base.
  - Vite viene preconfigurado con sensible defaults para React y TypeScript, haciendo el inicio de proyectos muy sencillo.
- **Optimización de Build para Producción**:
  - Vite utiliza Rollup para el _bundling_ de producción, conocido por generar _bundles_ más pequeños y optimizados que Webpack en muchos escenarios. Esto se traduce en tiempos de carga más rápidos para los usuarios finales.

En resumen, Vite es la elección moderna para React porque ofrece una experiencia de desarrollo superior en términos de velocidad, eficiencia y facilidad de uso, sin comprometer las optimizaciones para producción.

## 6. Creación de nuestro Proyecto de Cursos

Ahora que entendemos las ventajas, vamos a crear nuestro proyecto de "Listado de Cursos" usando Vite.

### 6.1. Pasos para la instalación

1. Abre tu Terminal/Línea de Comandos.
2. **Navega a la carpeta** donde deseas crear tu proyecto:

    ```bash
    cd ruta/a/tu/carpeta/proyectos
    ```

3. Ejecuta el comando de creación de Vite:

    ```bash
    npm create vite@latest
    ```

4. Sigue las indicaciones de la terminal:
    - **Project name**: (Nombre del proyecto)
      - Ingresa el nombre de tu proyecto (ej., `campus-courses-frontend`).
    - **Select a framework**: (Selecciona un framework)
      - Usa las flechas para seleccionar `React`.
    - **Select a variant**: (Selecciona una variante)
      - Selecciona `TypeScript`.

    Una vez que la instalación base esté completa, verás un mensaje indicando cómo continuar:

    ```bash
    cd campus-courses-frontend
    npm install
    npm run dev
    ```

5. Ejecuta los comandos sugeridos:
    - `cd campus-courses-frontend`: Entra a la carpeta del proyecto.
    - `npm install`: Instala todas las dependencias del proyecto definidas en `package.json`.
    - `npm run dev`: Inicia el servidor de desarrollo de Vite. Este comando mostrará una URL (ej., `http://localhost:5173/`) donde podrás ver tu aplicación React ejecutándose en el navegador.

### 6.2. Estructura Profesional de Carpetas

Una vez que el proyecto base ha sido creado, lo primero que haremos es organizar nuestra estructura de carpetas para asegurar la mantenibilidad y escalabilidad de nuestra aplicación de cursos. Adoptar un enfoque modular y por responsabilidades es clave.

Aquí se detalla una estructura de carpetas común y recomendada, que aplicaremos a nuestro proyecto:

```text
campus-courses-frontend/
├── node_modules/       # Dependencias del proyecto (gestionadas por npm)
├── public/             # Archivos estáticos que no son procesados por Vite (ej., index.html, favicon.ico)
├── src/
│   ├── assets/             # Recursos estáticos (imágenes, iconos, fuentes) - Ej. logo.svg, empty-state.png
│   ├── components/         # Componentes reutilizables y atómicos
│   │   ├── ui/             # Componentes de UI genéricos (Button, Input, Card) - Ej. Card.tsx, Button.tsx
│   │   │   ├── Card.tsx
│   │   │   └── Button.tsx
│   │   ├── common/         # Componentes comunes a varias secciones (Navbar, Footer) - Ej. Header.tsx, LoadingSpinner.tsx
│   │   │   ├── Header.tsx
│   │   │   └── LoadingSpinner.tsx
│   │   └── course/         # Componentes específicos de la entidad 'curso' - Ej. CourseCard.tsx, CourseList.tsx
│   │       ├── CourseCard.tsx
│   │       └── CourseList.tsx
│   ├── config/             # Archivos de configuración (ej. constantes de API) - Ej. apiConstants.ts
│   │   └── apiConstants.ts
│   ├── hooks/              # Hooks personalizados y reutilizables - Ej. useFetch.ts
│   │   └── useFetch.ts
│   ├── layouts/            # Plantillas de diseño para diferentes secciones de la app - Ej. MainLayout.tsx
│   │   └── MainLayout.tsx
│   ├── pages/              # Componentes de "página" (vistas principales) - Ej. CoursesPage.tsx
│   │   ├── CoursesPage.tsx
│   │   └── HomePage.tsx
│   ├── services/           # Lógica para interactuar con APIs externas (funciones de fetch) - Ej. courseService.ts
│   │   └── courseService.ts
│   ├── routes/             # Definición de rutas de la aplicación (usando React Router u similar) - Ej. AppRoutes.tsx
│   │   └── AppRoutes.tsx
│   ├── styles/             # Archivos de estilos globales o específicos de componentes (si no usas solo Tailwind) - Ej. index.css
│   │   └── index.css
│   ├── utils/              # Utilidades, funciones helper genéricas (formateo de fechas, validadores) - Ej. dateFormatter.ts
│   │   └── dateFormatter.ts
│   ├── App.tsx             # Componente principal de la aplicación (o App.tsx)
│   └── main.tsx            # Punto de entrada de la aplicación (o main.tsx)
├── .gitignore          # Archivos y carpetas a ignorar por Git
├── package.json        # Metadatos del proyecto y lista de dependencias
├── vite.config.ts      # Configuración de Vite
├── tailwind.config.ts  # Configuración de Tailwind CSS (si lo usas)
├── postcss.config.ts   # Configuración de PostCSS (si lo usas)
├── README.md           # Descripción del proyecto
└── tsconfig.json       # Configuración de TypeScript (si lo usas)
```

**Explicación de las carpetas clave para nuestro proyecto de cursos**:

- `src/components/`: Aquí guardaremos pequeños y reutilizables componentes de UI. Para nuestro listado de cursos, crearemos una subcarpeta `course` para componentes específicos como `CourseCard.tsx` (que mostrará los detalles de un curso) y `CourseList.tsx` (que contendrá varias `CourseCard`s). También podemos tener ui para componentes más genéricos (como un componente `Card` base) o `common` para elementos de layout.
- `src/pages/`: Contendrá los componentes que representan vistas completas de nuestra aplicación, como `CoursesPage.tsx`, que será la página principal donde listaremos todos los cursos.
- `src/services/`: Aquí centralizaremos la lógica para interactuar con nuestra API de Spring Boot. Tendremos un `courseService.ts` con funciones para obtener los datos de los cursos.
- `src/assets/`: Para cualquier imagen o recurso estático que utilicemos.
- `src/utils/`: Para funciones de utilidad que podríamos necesitar, como formatear fechas o textos.
- `App.tsx`: El componente raíz de nuestra aplicación, que orquestará la renderización de `CoursesPage`.
- `main.tsx`: El punto de entrada principal donde se monta nuestra aplicación React en el DOM.

Esta estructura fomenta la modularidad, la separación de preocupaciones y facilita la búsqueda de archivos, lo que es vital a medida que tu aplicación crece.

## 7. Integración de Tailwind CSS en el Proyecto

Para darle a nuestro listado de cursos un aspecto profesional sin escribir CSS personalizado, integraremos **Tailwind CSS**. Esto nos permitirá estilizar nuestros componentes (`CourseCard`, `CoursesPage`) directamente usando clases de utilidad.

![Tailwind CSS](https://devonblog.com/wp-content/uploads/2022/06/tailwind-thumb.jpg)

[**Tailwind CSS**](https://tailwindcss.com/) es un _framework_ CSS de utilidad que te permite construir diseños personalizados rápidamente, directamente en tu marcado HTML (o TSX en React). A diferencia de otros _frameworks_ CSS (como Bootstrap o Material UI) que vienen con componentes pre-diseñados, Tailwind te proporciona clases de bajo nivel que puedes componer para crear cualquier diseño.

[![¿Qué es TailwindCSS?](https://markdown-videos-api.jorgenkh.no/youtube/2NuGuBpP5fo?width=640&height=360)](https://www.youtube.com/watch?v=2NuGuBpP5fo)

### 7.1. Ventajas

- **Velocidad de Desarrollo**: Construye UIs complejas directamente en tu JSX.
- **Flexibilidad**: No te fuerza a seguir un diseño específico; puedes crear tu propio sistema.
- **Tamaño Mínimo de CSS**: Con PurgeCSS (integrado en Vite), solo el CSS que realmente usas en tu proyecto se incluye en el _build_ final.

### 7.2. Paso a paso para la integración con Vite + React

1. **Instala Tailwind CSS, PostCSS y Autoprefixer**: Abre tu terminal en la raíz de tu proyecto React (ej., **campus-courses-frontend)**.

    ```bash
    npm install -D tailwindcss @tailwindcss/vite
    ```

2. **Add the @tailwindcss/vite plugin to your Vite configuration (`vite.config.ts`).**:

    ```Typescript
    import { defineConfig } from 'vite'
    import react from '@vitejs/plugin-react'
    import tailwindcss from '@tailwindcss/vite'

    export default defineConfig({
      plugins: [
        react(),
        tailwindcss(),
      ],
    })
    ```

3. **Añade las directivas de Tailwind a tu CSS principal**:

    Abre tu archivo CSS principal (normalmente `src/index.css`) y añade las directivas de Tailwind en la parte superior:

    ```css
    @import "tailwindcss";
    ```

    Asegúrate de que este index.css esté importado en tu `main.tsx`. Si no lo está, añádelo en `src/main.tsx`:

    ```Typescript
    import './index.css';
    ```

¡Listo! Ahora las clases de Tailwind CSS que viste en CourseCard.tsx y CoursesPage.tsx aplicarán estilos visuales cuando inicies tu aplicación con npm run dev.

## 8. Manejar el Estado y Carga de Datos Iniciales

Ahora que tenemos un componente para mostrar un curso, necesitamos una "página" que contenga la lógica para cargar y mostrar varios cursos. Aquí es donde entran en juego los Hooks fundamentales de React: `useState` para manejar el estado (ej., la lista de cursos, si está cargando, si hay un error) y `useEffect` para realizar efectos secundarios como la carga de datos desde una API.

### 8.1. `useState`: Manteniendo el Estado de Nuestra UI

`useState` es el Hook que te permite añadir estado a tus componentes funcionales. En nuestra `CoursesPage`, usaremos `useState` para:

- Almacenar la lista de cursos que recibiremos de la API.
- Indicar si los datos están todavía en proceso de carga (`loading`).
- Almacenar cualquier mensaje de error que pueda ocurrir durante la carga (`error`).

**Sintaxis**: `const [estado, setEstado] = useState(valorInicial);`

### 8.2. `useEffect`: Trayendo Datos al Montar la Página

`useEffect` nos permite realizar "efectos secundarios" en componentes funcionales. La carga de datos desde una API es el caso de uso más común para `useEffect`. Queremos que nuestros cursos se carguen una vez que la página de cursos se muestra al usuario.

**Sintaxis**: `useEffect(callback, [dependencias]);`

Si el array de dependencias está vacío (`[]`), el `callback` se ejecutará solo **una vez** después del primer renderizado del componente, lo cual es perfecto para la carga inicial de datos.

### 8.3. Paso a paso para crear un proyecto

#### 8.3.1. Paso a paso para `CoursesPage.tsx`

1. **Crea el archivo**: Dentro de la carpeta `src/pages`, crea un nuevo archivo llamado `CoursesPage.tsx`.
2. **Define el componente y usa Hooks**:

    ```Typescript
    import React, { useState, useEffect } from 'react';
    import CourseCard from '../components/course/CourseCard'; // Importa el componente de tarjeta

    // Definimos las interfaces para la estructura de nuestros datos
    interface Course {
      id: number;
      name: string;
      description: string;
      professor?: { id: number; name: string; }; // Asumimos un objeto profesor con id y nombre
      maxCapacity: number;
      currentEnrollment?: number;
    }

    interface PaginatedCourseResponse {
      content: Course[];
      totalPages: number;
      totalElements: number;
      // Puedes añadir más campos de paginación si tu API los devuelve
    }

    function CoursesPage() {
      // 1. Declaración de estados con useState
      const [courses, setCourses] = useState<Course[]>([]); // Estado para almacenar la lista de cursos
      const [loading, setLoading] = useState<boolean>(true); // Estado para controlar el indicador de carga
      const [error, setError] = useState<string | null>(null); // Estado para guardar mensajes de error

      // 2. useEffect para la carga inicial de datos
      useEffect(() => {
        // Función asíncrona para traer los cursos de la API
        const fetchCourses = async () => {
          try {
            setLoading(true); // Activamos el estado de carga antes de la petición
            // Aquí haremos la petición real a nuestra API de Spring Boot
            // Por ahora, usamos un placeholder para ilustrar
            // const response = await fetch('http://localhost:8080/api/courses'); // Esta será la URL real

            // Simulación de una petición de API con un retraso
            const response = await new Promise(resolve => setTimeout(() => {
                resolve({
                    ok: true,
                    json: () => Promise.resolve({
                        content: [ // Simulamos la estructura de paginación de Spring Data
                            { id: 1, name: 'Introducción a React', description: 'Fundamentos de React y Hooks.', professor: { name: 'Dr. Frontend' }, maxCapacity: 50, currentEnrollment: 30 },
                            { id: 2, name: 'Desarrollo de APIs REST con Spring Boot', description: 'Construcción de APIs robustas.', professor: { name: 'Dra. Backend' }, maxCapacity: 40, currentEnrollment: 38 },
                            { id: 3, name: 'Bases de Datos con PostgreSQL', description: 'Modelado y consulta de datos.', professor: { name: 'Prof. Data' }, maxCapacity: 60, currentEnrollment: 15 },
                            { id: 4, name: 'Seguridad en Aplicaciones Web', description: 'Conceptos de Spring Security y JWT.', professor: { name: 'Ing. Cyber' }, maxCapacity: 30, currentEnrollment: 30 }, // Curso lleno
                        ]
                    })
                });
            }, 1500)); // Simula un retraso de 1.5 segundos

            if (!response.ok) {
              throw new Error(`Error HTTP: ${response.statusText}`);
            }
            const data = await response.json();
            setCourses(data.content); // Actualiza el estado 'courses' con los datos recibidos
          } catch (err) {
            setError(err.message); // Si hay un error, actualiza el estado 'error'
            console.error("Error fetching courses:", err);
          } finally {
            setLoading(false); // Siempre desactiva el estado de carga al finalizar
          }
        };

        fetchCourses(); // Llama a la función para ejecutar la carga de datos

        // Opcional: Función de limpieza (se ejecuta cuando el componente se desmonta)
        // return () => { console.log("CoursesPage desmontado, limpiando efectos."); };
      }, []); // El array vacío [] asegura que este efecto se ejecute solo una vez al montar

      // Renderizado condicional basado en el estado
      if (loading) return <div className="text-center p-8 text-xl text-gray-700">Cargando cursos...</div>;
      if (error) return <div className="text-center p-8 text-xl text-red-600">Error al cargar cursos: {error}</div>;
      if (courses.length === 0) return <div className="text-center p-8 text-xl text-gray-500">No hay cursos disponibles.</div>;

      // Si todo va bien, renderiza la lista de cursos
      return (
        <div className="container mx-auto p-8">
          <h1 className="text-4xl font-extrabold text-center text-gray-900 mb-10">Cursos Disponibles</h1>
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            {courses.map(course => (
              <CourseCard
                key={course.id} // 'key' es crucial para listas en React para optimización
                title={course.name}
                description={course.description}
                professor={course.professor ? course.professor.name : 'N/A'}
                maxCapacity={course.maxCapacity}
                currentEnrollment={course.currentEnrollment || 0}
              />
            ))}
          </div>
        </div>
      );
    }

    export default CoursesPage;
    ```

3. **Actualiza `App.tsx` para mostrar `CoursesPage`**:
    - Abre `src/App.tsx`.
    - Importa `CoursesPage`.
    - Reemplaza el contenido predeterminado de `App.tsx` para renderizar `CoursesPage`.

    ```Typescript
    import React from 'react';
    import CoursesPage from './pages/CoursesPage';
    // import './index.css'; // Asegúrate de importar tu CSS principal con Tailwind si lo usas aquí

    function App() {
      return (
        <div className="min-h-screen bg-gray-100">
          <CoursesPage />
        </div>
      );
    }

    export default App;
    ```

    Ahora, si ejecutas npm run dev, deberías ver un mensaje de "Cargando cursos..." seguido de la lista simulada de cursos.

## Recursos y Referencias

- Curso programación Web básico del canal "Hola Mundo"
  - [Aprende HTML ahora!](https://youtu.be/MJkdaVFHrto)
  - [Aprende CSS ahora!](https://youtu.be/wZniZEbPAzk)
  - [Aprende Javascript ahora!](https://youtu.be/QoC4RxNIs5M)
- Curso programación Web del canal "Midulive"
  - [Curso COMPLETO de HTML](https://youtu.be/3nYLTiY5skU)
  - [CURSO DE CSS COMPLETO](https://youtu.be/TlJbu0BMLaY)
  - [Introducción a la programación con JavaScript](https://youtu.be/Z34BF9PCfYg)
- [Google - Aprende desarrollo web](https://web.dev/learn?hl=es-419)
- [TypeScript - Curso COMPLETO desde 0](https://www.youtube.com/watch?v=sBgcHD1JsL4)
- [Curso Tailwind CSS 4 desde cero](https://www.youtube.com/watch?v=R5EXap3vNDA)
- [Curso React JS en Españo](https://www.youtube.com/watch?v=BisJdN2LWEY)
