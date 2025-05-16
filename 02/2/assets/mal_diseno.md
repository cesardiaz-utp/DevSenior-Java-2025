# Casos de Mal Diseño en POO (Herencia y Abstracción)

Estos ejemplos ilustran situaciones que debes evitar al diseñar tus clases y jerarquías, ya que pueden llevar a código difícil de mantener, entender y extender.

## Caso 1: Herencia por Reutilización de Código (Rompiendo la Relación "_Es Un Tipo De_")

- **Escenario**: Tienes una clase `BaseDatos` con métodos para conectar, desconectar y ejecutar consultas. Luego, creas una clase `UtilidadesReportes` que hereda de `BaseDatos` simplemente para poder usar los métodos de conexión y desconexión, aunque conceptualmente, una "Utilidad de Reportes" _no es un tipo de_ "Base de Datos".
- **Código de Ejemplo (Esquema)**:

    ```Java
    class BaseDatos {
        public void conectar() { /* ... */ }
        public void desconectar() { /* ... */ }
        public void ejecutarConsulta(String sql) { /* ... */ }
    }

    // MAL DISEÑO: UtilidadesReportes NO ES UNA BaseDatos
    class UtilidadesReportes extends BaseDatos {
        public void generarReporte(String tipo) {
            // Usa métodos heredados sin ser realmente una BD
            conectar();
            ejecutarConsulta("SELECT * FROM ...");
            desconectar();
            // ... lógica del reporte ...
        }
    }
    ```

- **Problema**: Rompe el principio fundamental de la herencia ("_es un tipo de_"). Si la clase **BaseDatos** cambia o añade métodos que no tienen sentido para **UtilidadesReportes**, esta última se ve afectada innecesariamente. La relación lógica no existe.
- **Mejor Práctica**: Usar composición en lugar de herencia. La clase **UtilidadesReportes** debería tener una **BaseDatos** (como un atributo) y usar una instancia de ella para realizar las operaciones necesarias.

    ```Java
    // MEJOR DISEÑO: UtilidadesReportes TIENE UNA BaseDatos
    class UtilidadesReportes {
        private BaseDatos bd; // Composición

        public UtilidadesReportes(BaseDatos bd) {
            this.bd = bd;
        }

        public void generarReporte(String tipo) {
            // Usa la instancia de BaseDatos
            bd.conectar();
            bd.ejecutarConsulta("SELECT * FROM ...");
            bd.desconectar();
            // ... lógica del reporte ...
        }
    }
    ```

## Caso 2: Duplicación de Código en Jerarquías Potenciales

- **Escenario**: Tienes clases `Circulo`, `Cuadrado` y `Triangulo`. Cada una tiene un método `calcularArea()` y `calcularPerimetro()`, pero la implementación es completamente independiente en cada clase, y no hay una superclase común `FiguraGeometrica` que defina estos métodos (quizás como abstractos) o atributos comunes (como color).
- **Código de Ejemplo (Esquema)**:

    ```Java
    class Circulo {
        private double radio; // Atributo propio
        // ... constructor, getters/setters ...
        public double calcularArea() { /* ... */ return Math.PI * radio * radio; }
        public double calcularPerimetro() { /* ... */ return 2 * Math.PI * radio; }
    }

    class Cuadrado {
        private double lado; // Atributo propio
        // ... constructor, getters/setters ...
        public double calcularArea() { /* ... */ return lado * lado; }
        public double calcularPerimetro() { /* ... */ return 4 * lado; }
    }

    // Y así para Triangulo, etc.
    ```

- **Problema**: Aunque la implementación de `calcularArea()` y `calcularPerimetro()` es diferente para cada figura, el _concepto_ de tener un área y un perímetro es común. No aprovechar la herencia o la abstracción (clase abstracta `FiguraGeometrica` o interfaz `Calculable`) lleva a código menos organizado y dificulta tratar diferentes figuras de manera polimórfica. Si quisieras, por ejemplo, calcular el área total de un conjunto mixto de figuras, sería más complicado sin una superclase o interfaz común.
- **Mejor Práctica**: Crear una superclase abstracta FiguraGeometrica con métodos abstractos calcularArea() y calcularPerimetro(), o una interfaz Calculable con esos métodos.

    ```Java
    // MEJOR DISEÑO: Usando clase abstracta o interfaz
    public abstract class FiguraGeometrica { // O public interface Calculable { ... }
        // Atributos comunes si los hay, ej. color
        // Métodos concretos comunes si los hay
        public abstract double calcularArea(); // Método abstracto
        public abstract double calcularPerimetro(); // Método abstracto
    }

    class Circulo extends FiguraGeometrica { // O implements Calculable
        // ... atributos propios ...
        @Override public double calcularArea() { /* ... */ }
        @Override public double calcularPerimetro() { /* ... */ }
    }
    // Y así para las otras figuras
    ```

## Caso 3: Romper el Encapsulamiento a Través de Miembros No Privados (Aunque sea Legal)

- **Escenario**: Tienes una superclase `Configuracion` con un atributo `protected String valorConfig`. En una subclase `ConfiguracionEspecifica`, accedes y modificas `valorConfig` directamente usando `this.valorConfig = "nuevo"`; en lugar de usar un `protected` o `public` setter `setValorConfig(String valor)` que podría existir en la superclase y contener lógica de validación o notificación.
- **Código de Ejemplo (Esquema)**:

    ```Java
    class Configuracion {
        protected String valorConfig; // Atributo protegido

        // Posible setter con lógica (aunque sea protected o public)
        protected void setValorConfig(String valor) {
            // Alguna lógica de validación o procesamiento
            System.out.println("Validando/procesando valor antes de asignar...");
            this.valorConfig = valor;
        }

        public String getValorConfig() {
            return this.valorConfig; // O con lógica
        }
    }

    class ConfiguracionEspecifica extends Configuracion {
        public void modificarValorDirecto(String nuevoValor) {
            // MAL DISEÑO: Acceso directo al atributo protegido, saltando el setter
            this.valorConfig = nuevoValor;
            System.out.println("Valor modificado directamente en la subclase.");
        }

        public void modificarValorConSetter(String nuevoValor) {
            // MEJOR DISEÑO: Usando el setter (si existe y es accesible)
            setValorConfig(nuevoValor);
            System.out.println("Valor modificado usando el setter.");
        }
    }
    ```

- **Problema**: Aunque Java permite el acceso directo a miembros `protected` (y default) desde subclases (o clases en el mismo paquete), si la superclase provee un método (setter) para modificar ese miembro que contiene lógica importante (validación, notificaciones, efectos secundarios), saltarse ese método y acceder directamente rompe el encapsulamiento _efectivo_. La lógica importante se omite.
- **Mejor Práctica**: Siempre que exista un método (getter/setter/otro) en la superclase que sea la forma _intencionada_ de interactuar con un miembro, úsalo desde la subclase en lugar de acceder directamente al miembro, incluso si el modificador de acceso lo permite. Esto mantiene la lógica centralizada y coherente.
