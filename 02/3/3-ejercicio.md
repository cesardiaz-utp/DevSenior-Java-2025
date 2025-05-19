# Ejercicio Autónomo: Sistema Básico de Gestión Universitaria

Este ejercicio te guiará en la aplicación de los conceptos de modelado avanzado de clases, relaciones (Composición, Agregación) y el uso de Arrays.

## Objetivo

Diseñar e implementar un sistema simplificado para gestionar aspectos de una universidad, modelando las relaciones entre Estudiantes, Profesores, Cursos y Departamentos utilizando Arrays.

## Paso 1: Configuración del Proyecto

1. Abre tu entorno de desarrollo (VSCode).
2. Crea una nueva carpeta para este ejercicio, por ejemplo: java-course/module-02/class-03/university-system.
3. Abre esta carpeta en VSCode.

## Paso 2: Diseñando e Implementando la Clase `Student`

1. Crea un nuevo archivo Java llamado `Student.java`.
2. Declara la clase `public class Student`.
3. Define los atributos privados necesarios para un estudiante: `name` (String) y `studentId` (String).
4. Crea un constructor `public Student(String name, String studentId)` para inicializar estos atributos. Usa `this`.
5. Implementa métodos getter públicos para `name` y `studentId`.
6. (Opcional) Implementa un método `displayInfo()` que imprima el nombre y ID del estudiante en español.

```Java
public class Student {
    // Atributos privados
    private String name;
    private String studentId;

    // Constructor
    public Student(String name, String studentId) {
        this.name = name;
        this.studentId = studentId;
    }

    // Getters
    public String getName() {
        return this.name;
    }

    public String getStudentId() {
        return this.studentId;
    }

    // Método para mostrar información (Opcional)
    public void displayInfo() {
        System.out.println("Nombre del estudiante: " + this.name);
        System.out.println("ID del estudiante: " + this.studentId);
    }
}
```

## Paso 3: Diseñando e Implementando la Clase `Professor`

1. Crea un nuevo archivo Java llamado `Professor.java`.
2. Declara la clase `public class Professor`.
3. Define los atributos privados: `name` (String) y `professorId` (String).
4. **Relación con Departamento (Agregación)**: Un profesor pertenece a un departamento. Añade un atributo privado `private Department department;`. Esta es una relación de Asociación/Agregación (el profesor puede existir sin el departamento).
5. Crea un constructor `public Professor(String name, String professorId, Department department)` para inicializar estos atributos. Usa `this`.
6. Implementa métodos getter públicos para `name`, `professorId` y `department`.
7. (Opcional) Implementa un método `displayInfo()` que imprima el nombre, ID y el nombre del departamento al que pertenece (usando el getter del departamento).

```Java
// Necesitarás la clase Department, la crearemos después.
// Por ahora, puedes dejar el atributo Department y el parámetro del constructor.

public class Professor {
    // Atributos privados
    private String name;
    private String professorId;
    private Department department; // Relación de Agregación: Profesor pertenece a un Departamento

    // Constructor
    public Professor(String name, String professorId, Department department) {
        this.name = name;
        this.professorId = professorId;
        this.department = department; // Se recibe un objeto Department existente
    }

    // Getters
    public String getName() {
        return this.name;
    }

    public String getProfessorId() {
        return this.professorId;
    }

    public Department getDepartment() {
        return this.department;
    }

    // Método para mostrar información (Opcional)
    public void displayInfo() {
        System.out.println("Nombre del profesor: " + this.name);
        System.out.println("ID del profesor: " + this.professorId);
        if (this.department != null) {
            System.out.println("Departamento: " + this.department.getName()); // Asumiendo que Department tiene getName()
        } else {
            System.out.println("Departamento: No asignado");
        }
    }
}
```

## Paso 4: Diseñando e Implementando la Clase `Department`

1. Crea un nuevo archivo Java llamado `Department.java`.
2. Declara la clase `public class Department`.
3. Define atributos privados: `name` (String).
4. **Relación con Profesores (Agregación - con Array)**: Un departamento tiene varios profesores. Añade un atributo privado `private Professor[] professors;`.
    - **Decisión de Diseño**: Los profesores existen independientemente del departamento. La relación es **Agregación**. El Array de profesores se **pasará al constructor** del departamento.
    - Añade un atributo `private int professorCount = 0;` para llevar el conteo de profesores.
5. Crea un constructor `public Department(String name, int maxProfessors)` que inicialice el nombre y **cree el array** `professors` con el tamaño `maxProfessors`.
6. Implementa getters para `name`.
7. Implementa un método `addProfessor(Professor professor)`:
    - Recibe un objeto `Professor`.
    - Verifica si hay espacio en el array (`professorCount < professors.length`).
    - Si hay espacio, añade el profesor a la posición `professorCount` del array y incrementa `professorCount`. Imprime un mensaje en español.
    - **Relación Bidireccional (Opcional pero recomendada)**: Si quieres que el profesor también sepa a qué departamento pertenece, necesitarías un setter setDepartment en la clase Professor y llamarlo aquí: `professor.setDepartment(this);`. (Para este ejercicio, nos enfocaremos en la relación unidireccional del Departamento al Profesor).
8. Implementa un método `displayProfessors()` que itere sobre el array `professors` (hasta `professorCount`) e imprima la información de cada profesor (usando el getter del profesor).

```Java
// Necesitarás la clase Professor

public class Department {
    // Atributos privados
    private String name;
    private Professor[] professors; // Relación de Agregación: Departamento tiene Profesores (Array pasado externamente)
    private int professorCount = 0; // Contador de profesores

    // Constructor
    public Department(String name, int maxProfessors) {
        this.name = name;
        // Agregación: El departamento crea su array para contener referencias a profesores existentes
        this.professors = new Professor[maxProfessors];
        System.out.println("Departamento '" + this.name + "' creado con capacidad para " + maxProfessors + " profesores.");
    }

    // Getter
    public String getName() {
        return this.name;
    }

    // Método para añadir un profesor
    public void addProfessor(Professor professor) {
        if (this.professorCount < this.professors.length) {
            this.professors[this.professorCount] = professor; // Añade al array
            this.professorCount++; // Incrementa el contador
            // Opcional: Si implementaste bidireccionalidad, llama a professor.setDepartment(this); aquí
            System.out.println("Profesor '" + professor.getName() + "' agregado al departamento '" + this.name + "'.");
        } else {
            System.out.println("ERROR: El departamento '" + this.name + "' está lleno. No se puede agregar a '" + professor.getName() + "'.");
        }
    }

    // Método para mostrar profesores
    public void displayProfessors() {
        System.out.println("Profesores en el departamento '" + this.name + "':");
        if (this.professorCount == 0) {
            System.out.println("  No hay profesores asignados.");
        } else {
            // Iterar sobre el array hasta el número de profesores agregados
            for (int i = 0; i < this.professorCount; i++) {
                 if (this.professors[i] != null) {
                    System.out.println("  - " + this.professors[i].getName() + " (ID: " + this.professors[i].getProfessorId() + ")");
                }
            }
        }
    }
}
```

## Paso 5: Diseñando e Implementando la Clase `Course`

1. Crea un nuevo archivo Java llamado `Course.java`.
2. Declara la clase `public class Course`.
3. Define atributos privados: `name` (String), `courseCode` (String), `maxCapacity` (int).
4. **Relación con Profesor (Asociación)**: Un curso es impartido por un profesor. Añade un atributo `privado private Professor professor;`.
5. **Relación con Estudiantes (Composición o Agregación - con Array)**: Un curso tiene varios estudiantes matriculados. Añade un atributo privado `private Student[] enrolledStudents;`.
    - **Decisión de Diseño**: ¿Los estudiantes "pertenecen" lógicamente solo a este curso, o pueden existir independientemente y matricularse en varios cursos? En un sistema universitario real, los estudiantes existen independientemente. Por lo tanto, la relación es **Agregación**. El Array de estudiantes se **pasará al constructor o se gestionará externamente**. Sin embargo, para simplificar la gestión del Array interno en este ejercicio, vamos a simular la **Composición** creando el array dentro del constructor del curso con la `maxCapacity` y añadiendo estudiantes uno por uno. Esto es una simplificación para practicar Composición con Arrays.
    - Añade un atributo `private int studentCount = 0;` para llevar el conteo de estudiantes matriculados.
6. Crea un constructor `public Course(String name, String courseCode, int maxCapacity, Professor professor)` que inicialice los atributos y cree el array `enrolledStudents` con el tamaño `maxCapacity`.
7. Implementa getters para todos los atributos.
8. Implementa un método `enrollStudent(Student student)`:
    - Recibe un objeto `Student`.
    - Verifica si hay espacio en el array (`studentCount < maxCapacity`).
    - Si hay espacio, añade el estudiante a la posición `studentCount` del array y incrementa `studentCount`. Imprime un mensaje en español indicando si la matrícula fue exitosa o si el curso está lleno.
9. Implementa un método `displayEnrolledStudents()` que itere sobre el array `enrolledStudents` (hasta `studentCount`) e imprima la información de cada estudiante matriculado (usando el getter del estudiante).

```Java
// Necesitarás las clases Student y Professor

public class Course {
    // Atributos privados
    private String name;
    private String courseCode;
    private int maxCapacity;
    private Professor professor; // Relación de Asociación: Curso tiene un Profesor
    private Student[] enrolledStudents; // Relación (simulando Composición para Array): Curso tiene Estudiantes
    private int studentCount = 0; // Contador de estudiantes matriculados

    // Constructor
    public Course(String name, String courseCode, int maxCapacity, Professor professor) {
        this.name = name;
        this.courseCode = courseCode;
        this.maxCapacity = maxCapacity;
        this.professor = professor;
        // Composición (simulada con Array): El curso crea su array de estudiantes
        this.enrolledStudents = new Student[maxCapacity];
        System.out.println("Curso '" + this.name + "' creado con capacidad para " + maxCapacity + " estudiantes.");
    }

    // Getters
    public String getName() {
        return this.name;
    }

    public String getCourseCode() {
        return this.courseCode;
    }

    public int getMaxCapacity() {
        return this.maxCapacity;
    }

    public Professor getProfessor() {
        return this.professor;
    }

    public Student[] getEnrolledStudents() {
        // Buena práctica: devolver una copia del array o un elemento específico
        // Por simplicidad en este ejercicio, devolveremos la referencia directa,
        // pero ten en cuenta que esto rompe el encapsulamiento del array interno.
        return this.enrolledStudents;
    }

    public int getStudentCount() {
        return this.studentCount;
    }


    // Método para matricular un estudiante
    public void enrollStudent(Student student) {
        if (this.studentCount < this.maxCapacity) {
            this.enrolledStudents[this.studentCount] = student; // Añade al array
            this.studentCount++; // Incrementa el contador
            System.out.println("Estudiante '" + student.getName() + "' matriculado en '" + this.name + "'.");
        } else {
            System.out.println("ERROR: El curso '" + this.name + "' está lleno. No se puede matricular a '" + student.getName() + "'.");
        }
    }

    // Método para mostrar estudiantes matriculados
    public void displayEnrolledStudents() {
        System.out.println("Estudiantes matriculados en '" + this.name + "':");
        if (this.studentCount == 0) {
            System.out.println("  No hay estudiantes matriculados.");
        } else {
            // Iterar sobre el array hasta el número de estudiantes matriculados
            for (int i = 0; i < this.studentCount; i++) {
                // Asegurarse de que el elemento no sea null (aunque con studentCount no debería serlo aquí)
                if (this.enrolledStudents[i] != null) {
                     System.out.println("  - " + this.enrolledStudents[i].getName() + " (ID: " + this.enrolledStudents[i].getStudentId() + ")");
                }
            }
        }
    }
}
```

## Paso 6: Creando la Clase Principal `UniversityApp`

1. Crea un nuevo archivo Java llamado `UniversityApp.java`.
2. Declara la clase `public class UniversityApp` con el método `main`.

```Java
// Necesitarás todas las clases creadas: Student, Professor, Course, Department

public class UniversityApp {
    public static void main(String[] args) {
        // Aquí crearemos instancias y estableceremos relaciones
    }
}
```

## Paso 7: Creando Instancias y Estableciendo Relaciones en main

Dentro del método main de UniversityApp.java, crea objetos y establece las relaciones que diseñaste:

1. Crea algunos objetos `Student`.
2. Crea algunos objetos `Professor`.
3. Crea algunos objetos `Department`, especificando el tamaño máximo de profesores. Agrega los objetos `Professor` creados a los departamentos usando el método `addProfessor()`.
4. Crea algunos objetos `Course`, pasándoles el nombre, código, capacidad máxima y un objeto `Professor` (el profesor que impartirá el curso).
5. Matricula los objetos `Student` creados en los `Courses` usando el método `enrollStudent()`. Intenta matricular más estudiantes de la capacidad máxima para probar la validación.
6. Llama a los métodos `display...()` para mostrar la información y las relaciones:
    - Muestra los profesores en cada departamento.
    - Muestra los estudiantes matriculados en cada curso.

```Java
public class UniversityApp {
    public static void main(String[] args) {
        // 1. Crear Estudiantes
        Student student1 = new Student("Ana Garcia", "S1001");
        Student student2 = new Student("Juan Perez", "S1002");
        Student student3 = new Student("Maria Lopez", "S1003");
        Student student4 = new Student("Carlos Ruiz", "S1004");

        // 2. Crear Departamentos (especificando capacidad de profesores)
        Department computingDept = new Department("Ciencias de la Computación", 5);
        Department physicsDept = new Department("Física", 3);

        // 3. Crear Profesores (asignándolos a un departamento)
        Professor prof1 = new Professor("Dr. Smith", "P2001", computingDept);
        Professor prof2 = new Professor("Dr. Jones", "P2002", computingDept);
        Professor prof3 = new Professor("Dr. Curie", "P2003", physicsDept);

        // 4. Asignar Profesores a Departamentos (Agregación)
        computingDept.addProfessor(prof1);
        computingDept.addProfessor(prof2);
        physicsDept.addProfessor(prof3);

        // Mostrar profesores por departamento
        computingDept.displayProfessors();
        System.out.println();
        physicsDept.displayProfessors();
        System.out.println();

        // 5. Crear Cursos (asignando un profesor y capacidad)
        Course introToProgramming = new Course("Introducción a la Programación", "CS101", 3, prof1); // Capacidad 3
        Course classicalMechanics = new Course("Mecánica Clásica", "PHY201", 2, prof3); // Capacidad 2

        // 6. Matricular Estudiantes en Cursos (Composición simulada con Array)
        introToProgramming.enrollStudent(student1);
        introToProgramming.enrollStudent(student2);
        introToProgramming.enrollStudent(student3); // Este debería llenar el curso
        introToProgramming.enrollStudent(student4); // Este debería mostrar el error de curso lleno

        classicalMechanics.enrollStudent(student1);
        classicalMechanics.enrollStudent(student4); // Este debería llenar el curso
        classicalMechanics.enrollStudent(student2); // Este debería mostrar el error de curso lleno


        // Mostrar estudiantes matriculados por curso
        System.out.println();
        introToProgramming.displayEnrolledStudents();
        System.out.println();
        classicalMechanics.displayEnrolledStudents();
        System.out.println();

        // Opcional: Demostrar relaciones bidireccionales si las implementaste
        // System.out.println("Departamento del Profesor " + prof1.getName() + ": " + prof1.getDepartment().getName());
    }
}
```

## Criterios de Evaluación (Autónoma)

Al completar este ejercicio, revisa tu propio código y ejecución considerando:

- ¿Creaste las clases `Student`, `Professor`, `Course`, `Department` y `UniversityApp`?
- ¿Los atributos en todas las clases son `private`?
- ¿Implementaste constructores adecuados para inicializar los objetos?
- ¿Modelaste la relación "Curso tiene Estudiantes" usando un `Student[]` creado **dentro** de la clase `Course` (Composición simulada)?
- ¿Modelaste la relación "Departamento tiene Profesores" usando un `Professor[]` **pasado al constructor** de `Department` (Agregación)?
- ¿Modelaste la relación "Curso es impartido por un Profesor" usando un atributo `Professor` en la clase `Course` (Asociación)?
- ¿Implementaste métodos para añadir elementos a los Arrays (`enrollStudent`, `addProfessor`) y manejaste la capacidad máxima?
- ¿Implementaste métodos para mostrar la información y los contenidos de los Arrays (`displayEnrolledStudents`, `displayProfessors`)?
- ¿El código está escrito en `inglés`?
- ¿Los comentarios y mensajes de salida en consola están en `español`?
¿Utilizaste **solo Arrays** para colecciones?
- ¿Tu código es legible, bien indentado y comentado?

Este ejercicio te dará una práctica completa en el modelado de sistemas con relaciones y el uso de Arrays para colecciones de objetos, aplicando los conceptos de Composición y Agregación.

¡Mucho éxito!
