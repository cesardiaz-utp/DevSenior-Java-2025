# Tutoría: Presentación, revisión estructural y retroalimentación

## 1. Bienvenida y Repaso del Proyecto

- **Objetivo**: Recordar el propósito del proyecto y los conceptos clave que debían aplicar.
- Dar la bienvenida y felicitar a los estudiantes por completar el Proyecto Integrador.
- Recordar brevemente los objetivos del proyecto: aplicar e integrar POO, relaciones, Arrays, Enums, organización.
- Explicar la estructura de la tutoría: revisión de diseños, feedback, posibles mejoras manuales y publicación en GitHub.

## 2. Presentación del Diseño y Código

- **Objetivo**: Que los estudiantes expliquen su enfoque de diseño, cómo aplicaron los conceptos y muestren partes clave de su código.
- **Dinámica**: Pedir a 2-3 estudiantes (voluntarios o seleccionados) que presenten su diseño y muestren su código. Pueden compartir su diagrama de clases (si lo hicieron) o describir verbalmente su diseño. Luego, mostrarán partes relevantes de su implementación en VSCode, explicando cómo aplicaron herencia, encapsulamiento, relaciones con Arrays, Enums, etc.

## 3. Evaluación y Feedback Técnico

- **Objetivo**: Evaluar colectivamente (tutor y compañeros) si el diseño y la implementación cumplen con los requisitos y aplican correctamente los conceptos clave del módulo. Proporcionar retroalimentación constructiva.
- **Dinámica**: Después de cada presentación (o discutiendo ejemplos comunes identificados por el tutor al revisar los proyectos), abrir un espacio para feedback. Utilizar la lista de verificación/rubrica como guía.
  - Puntos clave para validar/discutir:
    - **Modelado de Clases**: ¿Las clases identificadas representan correctamente las entidades?
    - **Encapsulamiento**: ¿Los atributos son private? ¿Se usan getters/setters adecuadamente?
    - **Constructores**: ¿Se inicializan correctamente los objetos? ¿Se usa this?
    - **Herencia/Polimorfismo/Abstracción**: Si se aplicaron, ¿fue de forma correcta y apropiada? ¿Se demostró polimorfismo?
    - **Relaciones**: ¿Se modelaron correctamente (Asociación, Composición, Agregación)? ¿Se usaron Arrays para las colecciones? ¿La implementación de la relación coincide con el tipo (creación interna vs. paso externo del array)? ¿Se manejó la capacidad del Array y null?
    - **Enums**: ¿Se usaron para conjuntos fijos de valores? ¿Se usaron correctamente en las clases?
    - **Paquetes y Capas**: ¿El código está organizado en paquetes lógicos (model, service, app)? ¿La comunicación entre capas es adecuada?
    - **Validaciones Básicas y Manejo de Errores**: ¿Se validan las entradas o condiciones (ej. permisos de rol, capacidad del array)? ¿Se muestran mensajes de error claros en español?
    - **Buenas Prácticas**: ¿Nombres claros y descriptivos? ¿Código legible y bien indentado? ¿Métodos con responsabilidad única? ¿Comentarios/mensajes de salida en español? ¿Javadoc presente en clases/métodos públicos?
    - **Feedback (Tutor y Compañeros)**: Fomentar que los compañeros hagan preguntas y ofrezcan sugerencias constructivas. El tutor complementa el feedback, señalando áreas de mejora y reforzando los aciertos. Animar a los estudiantes a tomar notas del feedback recibido para aplicar mejoras después de la tutoría.

## 4. Refactorizaciones Manuales y Mejora del Código

- **Objetivo**: Señalar áreas concretas donde los estudiantes pueden mejorar su código y discutir posibles enfoques de refactorización manual.
- **Dinámica**: Basado en el feedback, el tutor puede mostrar ejemplos de cómo refactorizar manualmente algunas estructuras comunes que podrían haber aparecido (ej. extraer un método auxiliar para imprimir detalles, simplificar lógica condicional). Animar a los estudiantes a aplicar estas mejoras en su propio código después de la tutoría.

## 5. Subida del Proyecto a GitHub

- **Objetivo**: Introducir el control de versiones con Git y GitHub como práctica estándar en el desarrollo de software.
- **Dinámica**: Guiar a los estudiantes en los pasos básicos para subir su proyecto a GitHub:
  - ¿Qué es Git? ¿Qué es GitHub? ¿Por qué usarlos para sus proyectos?
  - Inicializar un repositorio Git local en la carpeta del proyecto (`git init`).
  - Añadir los archivos al "staging area" (`git add .`).
  - Realizar el primer commit (`git commit -m "Initial commit: Project structure and core classes"`). Enfatizar la importancia de mensajes de commit claros y concisos que describan los cambios.
  - Crear un repositorio en GitHub.
  - Conectar el repositorio local con el remoto (`git remote add origin <url_del_repo>`).
  - Subir los cambios (`git push -u origin main` o `master`).
  - **README Profesional**: Explicar la importancia del archivo README.md en la raíz del proyecto para que otros entiendan rápidamente de qué trata. ¿Qué debe contener? (Título del proyecto, descripción breve, cómo ejecutarlo, tecnologías usadas - Java, POO M2 concepts -, autor/es, licencia opcional).
  - **Commits Bien Documentados**: Recomendar realizar commits frecuentes con mensajes descriptivos a medida que añaden funcionalidades o refactorizan.

## 6. Cierre del Módulo y Próximos Pasos

- **Objetivo**: Concluir el Módulo 2 y preparar para el Módulo 3.
- Recapitular los grandes temas cubiertos en el Módulo 2 (todos los pilares de POO, relaciones, arrays, organización, buenas prácticas, proyecto integrador).
- Enfatizar que el Proyecto Integrador es la culminación práctica del módulo y que seguirán construyendo sobre esta base.
- Mencionar brevemente los temas del Módulo 3 (colecciones más avanzadas, manejo de excepciones, etc.).
- Responder preguntas finales.
