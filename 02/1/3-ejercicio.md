# Ejercicio Autónomo: Modelando un Empleado

Este ejercicio te permitirá aplicar los conceptos fundamentales de la Programación Orientada a Objetos que viste en la Clase 1: crear una clase, definir sus atributos y métodos, usar constructores para crear objetos, y aplicar el encapsulamiento con private, getters y setters.

## Objetivos del Ejercicio

- Diseñar y crear una clase Java que modele una entidad del mundo real.
- Definir atributos (`private`) y métodos (`public`) para representar el estado y comportamiento de un objeto.
- Implementar constructores para inicializar objetos de diferentes maneras.
- Aplicar el encapsulamiento utilizando modificadores de acceso y métodos getter/setter.
- Crear múltiples objetos de la misma clase y manipular su estado a través de sus métodos públicos.
- Utilizar la palabra clave `this` correctamente en constructores y setters.

## Instrucciones Detalladas

### 1. Crear la Clase Empleado

Crea un nuevo archivo Java llamado `Empleado.java`. Este archivo no debe tener un método `main`.

### 2. Definir Atributos (Privados)

Dentro de la clase `Empleado`, declara los siguientes atributos para representar las características de un empleado. Haz que todos sean privados para aplicar el encapsulamiento.

- `nombreCompleto` (tipo `String`)
- `idEmpleado` (tipo `String`) - Considera que este ID es único y no debería poder cambiarse después de la creación.
- `salarioMensual` (tipo `double`)
- `departamento` (tipo `String`)

### 3. Implementar Constructores

- Crea al menos dos constructores para la clase `Empleado`:
  - Un constructor que reciba `nombreCompleto`, `idEmpleado` y `departamento` como parámetros, e inicialice el `salarioMensual` a un valor por defecto (ej. 0.0).
  - Un constructor que reciba `nombreCompleto`, `idEmpleado`, `salarioMensual` y `departamento` como parámetros, e inicialice todos los atributos.
- Utiliza la palabra clave `this` dentro de los constructores para referirte a los atributos de la instancia.

### 4. Implementar Getters y Setters

- Crea métodos públicos (**getters**) para permitir obtener el valor de cada atributo.
- Crea métodos públicos (**setters**) para permitir modificar el valor de `salarioMensual` y `departamento`.
- **Nota**: No crees un **setter** para `idEmpleado`, ya que dijimos que no debería poder cambiarse después de la creación.
- En el **setter** de `salarioMensual`, incluye una validación simple: si el salario ingresado es negativo, imprime un mensaje de error y no modifiques el atributo (o asígnale 0.0).

### 5. Implementar Otros Métodos de Comportamiento

- Crea un método público `mostrarInformacion()` que imprima en la consola toda la información del empleado de forma legible (nombre, ID, salario, departamento). Utiliza los **getters** dentro de este método para acceder a los atributos.
- Crea un método público `calcularSalarioAnual()` que calcule el salario anual (salario mensual * 12) y **retorne** ese valor (`double`).

### 6. Crear la Clase Principal con main

Crea un nuevo archivo Java llamado `EmpresaApp.java` (o similar). Este archivo **sí** debe tener el método main.

### 7. Crear y Manipular Objetos `Empleado` en `main`

- Dentro del método main de `EmpresaApp.java`, crea al menos tres objetos de la clase `Empleado` utilizando los diferentes constructores que definiste.
- Llama al método `mostrarInformacion()` para cada uno de los objetos creados para ver su estado inicial.
- Utiliza los métodos **setter** para modificar el salario o el departamento de alguno de los empleados.
- Vuelve a llamar a `mostrarInformacion()` para ver los cambios.
- Llama al método `calcularSalarioAnual()` para al menos un empleado, almacena el resultado en una variable y luego imprímelo en la consola.
- Intenta (y observa el error de compilación) acceder directamente a un atributo privado desde el `main` (ej. empleado1.nombreCompleto = "Nuevo Nombre";). Esto confirmará que el encapsulamiento funciona.

## Criterios de Evaluación (Autónoma)

Al completar este ejercicio, revisa tu propio código y ejecución considerando:

- ¿Creaste dos archivos Java separados (`Empleado.java` y `EmpresaApp.java`)?
- ¿Todos los atributos en `Empleado.java` son `private`?
- ¿Implementaste al menos dos constructores en `Empleado.java`?
- ¿Utilizaste `this` correctamente en los constructores y setters para referirte a los atributos?
- ¿Implementaste **getters** para todos los atributos y **setters** solo para `salarioMensual` y `departamento`?
- ¿Incluiste la validación para el `salarioMensual` en su **setter**?
- ¿Implementaste los métodos `mostrarInformacion()` y `calcularSalarioAnual()`?
- ¿Creaste múltiples objetos `Empleado` en el método main de `EmpresaApp.java`?
- ¿Interactuaste con los objetos solo a través de sus métodos públicos (constructores, getters, setters, otros métodos)?
- ¿El intento de acceder a un atributo privado desde `main` causó un error de compilación?
- ¿Tu código es legible y está bien comentado?

Este ejercicio te dará una práctica sólida modelando una entidad con POO y aplicando el encapsulamiento.

¡Mucho éxito!
