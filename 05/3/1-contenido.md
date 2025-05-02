# Principios SOLID y su Aplicación Práctica en Java

## ¿Qué son los Principios SOLID y Por Qué Son Importantes?

SOLID es un acrónimo que representa cinco principios fundamentales para el diseño de software:

1. **S** - Single Responsibility Principle (Principio de Responsabilidad Única)
2. **O** - Open/Closed Principle (Principio Abierto/Cerrado)
3. **L** - Liskov Substitution Principle (Principio de Sustitución de Liskov)
4. **I** - Interface Segregation Principle (Principio de Segregación de Interfaz)
5. **D** - Dependency Inversion Principle (Principio de Inversión de Dependencia)

Seguir estos principios ayuda a construir software con las siguientes cualidades:

- **Mantenible:** Más fácil de corregir y actualizar.
- **Escalable:** Permite añadir nuevas características con menos esfuerzo.
- **Flexible:** Adaptable a cambios futuros.
- **Comprensible:** Diseño claro y fácil de entender.
- **Testable:** Facilita la escritura de pruebas unitarias.

## Principios SOLID en Detalle

### 1. Single Responsibility Principle (SRP)

**Definición:** Una clase debe tener **una, y solo una, razón para cambiar**. Esto significa que debe tener una única responsabilidad bien definida.

**Ejemplo de Violación:**

```java
public class BadReportGenerator {
    public String generateReportData() {
        // Generar datos del informe
        return "Datos del Informe";
    }

    public void saveToFile(String reportData, String filename) {
        // Guardar en archivo
    }

    public void sendEmail(String reportData, String emailAddress) {
        // Enviar por email
    }
}
```

**Refactorización:**

```java
public class ReportDataGenerator {
    public String generateReportData() {
        return "Datos del Informe";
    }
}

public class ReportSaver {
    public void saveToFile(String reportData, String filename) {
        // Guardar en archivo
    }
}

public class ReportEmailSender {
    public void sendEmail(String reportData, String emailAddress) {
        // Enviar por email
    }
}
```

### 2. Open/Closed Principle (OCP)

**Definición:** Las entidades de software deben estar **abiertas para extensión, pero cerradas para modificación**.

**Ejemplo de Violación:**

```java
public class BadAreaCalculator {
    public double calculateTotalArea(List<Object> shapes) {
        double totalArea = 0;
        for (Object shape : shapes) {
            if (shape instanceof Rectangle) {
                totalArea += ((Rectangle) shape).getArea();
            } else if (shape instanceof Circle) {
                totalArea += ((Circle) shape).getArea();
            }
        }
        return totalArea;
    }
}
```

**Refactorización:**

```java
public interface Shape {
    double calculateArea();
}

public class Rectangle implements Shape {
    @Override
    public double calculateArea() {
        return width * height;
    }
}

public class Circle implements Shape {
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class GoodAreaCalculator {
    public double calculateTotalArea(List<Shape> shapes) {
        return shapes.stream().mapToDouble(Shape::calculateArea).sum();
    }
}
```

### 3. Liskov Substitution Principle (LSP)

**Definición:** Las subclases deben ser sustituibles por sus clases base sin alterar el comportamiento esperado.

**Ejemplo de Violación:**

```java
public class Rectangle {
    public void setWidth(int width) { ... }
    public void setHeight(int height) { ... }
}

public class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        super.setWidth(width);
        super.setHeight(width);
    }
}
```

**Solución:** Evitar herencia directa si no cumple el contrato esperado.

### 4. Interface Segregation Principle (ISP)

**Definición:** Los clientes no deben ser forzados a depender de interfaces que no usan.

**Ejemplo de Violación:**

```java
interface BadRobot {
    void run();
    void fly();
    void drill();
}
```

**Refactorización:**

```java
interface Movable {
    void run();
}

interface Flying {
    void fly();
}

interface Drilling {
    void drill();
}
```

### 5. Dependency Inversion Principle (DIP)

**Definición:** Los módulos de alto nivel no deben depender de módulos de bajo nivel, ambos deben depender de abstracciones.

**Ejemplo de Violación:**

```java
public class BadOrderProcessor {
    private EmailNotificationService notificationService = new EmailNotificationService();
}
```

**Refactorización:**

```java
public interface NotificationService {
    void send(String recipient, String message);
}

public class GoodOrderProcessor {
    private final NotificationService notificationService;

    public GoodOrderProcessor(NotificationService notificationService) {
        this.notificationService = notificationService;
    }
}
```
