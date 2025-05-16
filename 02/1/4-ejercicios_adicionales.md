# Ejercicios adicionales

## Ejercicio 1: Modelando una Cuenta Bancaria Simple

Crea una clase llamada `CuentaBancaria` para representar una cuenta de ahorros básica.

- **Atributos (Privados)**:
  - `numeroCuenta` (String) - Identificador único de la cuenta.
  - `nombreTitular` (String) - Nombre del propietario de la cuenta.
  - `saldo` (double) - El monto actual de dinero en la cuenta.
- **Constructores**:
  - Un constructor que reciba el `numeroCuenta` y el `nombreTitular`, inicializando el saldo en 0.0.
  - Un constructor que reciba el `numeroCuenta`, el `nombreTitular` y un `saldoInicial`.
- **Métodos (Públicos)**:
  - `getNumeroCuenta()`: Retorna el número de cuenta.
  - `getNombreTitular()`: Retorna el nombre del titular.
  - `getSaldo()`: Retorna el saldo actual.
  - `depositar(double monto)`: Recibe un monto y lo suma al saldo. Debe validar que el monto sea positivo.
  - `retirar(double monto)`: Recibe un monto y lo resta del saldo. Debe validar que el monto sea positivo y que haya suficiente saldo disponible.
  - `mostrarInformacion()`: Imprime en consola la información completa de la cuenta (número, titular, saldo).

## Ejercicio 2: Representando un Coche

Diseña una clase llamada `Coche` para modelar un vehículo.

- **Atributos (Privados)**:
  - `marca` (String)
  - `modelo` (String)
  - `añoFabricacion` (int) - Ojo con la letra 'ñ'.
  - `velocidadActual` (double) - Inicialmente 0.0.
  - `motorEncendido` (boolean) - Inicialmente false.
- **Constructores**:
  - Un constructor que reciba `marca`, `modelo` y `añoFabricacion`.
  - Un constructor que reciba solo `marca` y `modelo`, asignando un año por defecto (ej. 2025).
- **Métodos (Públicos)**:
  - `getMarca()`, `getModelo()`, `getAñoFabricacion()`, `getVelocidadActual()`, `isMotorEncendido()`: Métodos de acceso para los atributos.
  - `encenderMotor()`: Cambia el estado de `motorEncendido` a `true`. Solo debe hacerlo si ya no está encendido.
  - `apagarMotor()`: Cambia el estado de `motorEncendido` a `false`. Solo debe hacerlo si ya está encendido y la `velocidadActual` es 0.
  - `acelerar(double incremento)`: Aumenta la `velocidadActual`. Solo debe hacerlo si el motor está encendido y el incremento es positivo.
  - `frenar(double decremento)`: Disminuye la `velocidadActual`. Solo debe hacerlo si el motor está encendido, el decremento es positivo, y la velocidad no se vuelve negativa.
  - `mostrarEstado()`: Imprime en consola la información relevante del coche (marca, modelo, año, velocidad actual, si el motor está encendido).

## Ejercicio 3: Creando una Tarea Pendiente

Modela una clase llamada `Tarea` para gestionar elementos de una lista de tareas pendientes.

- **Atributos (Privados)**:
  - `descripcion` (String) - La descripción de la tarea.
  -`prioridad` (int) - Un número que indica la prioridad (ej. 1=Alta, 2=Media, 3=Baja).
  - `completada` (boolean) - Indica si la tarea ha sido marcada como completada (inicialmente `false`).
- **Constructores**:
  - Un constructor que reciba la `descripcion` y asigne una prioridad por defecto (ej. 2 - Media).
  - Un constructor que reciba la `descripcion` y la `prioridad`. Debe validar que la prioridad esté en un rango válido (ej. 1 a 3).
- **Métodos (Públicos)**:
  - `getDescripcion()`, `getPrioridad()`, `isCompletada()`: Métodos de acceso.
  - `setPrioridad(int nuevaPrioridad)`: Permite cambiar la prioridad, validando que esté en el rango correcto.
  - `marcarComoCompletada()`: Cambia el estado de completada a `true`.
  - `mostrarEstado()`: Imprime en consola la descripción de la tarea, su prioridad y si está completada o no.

## Ejercicio 4: Diseñando un Punto en un Plano 2D

Crea una clase llamada `Punto` para representar un punto en un plano cartesiano (con coordenadas X e Y).

- **Atributos (Privados)**:
  - `x` (double) - Coordenada en el eje X.
  - `y` (double) - Coordenada en el eje Y.
- **Constructores**:
  - Un constructor que cree un punto en el origen (0,0).
  - Un constructor que reciba las coordenadas `x` e `y`.
- **Métodos (Públicos)**:
  - `getX()`, `getY()`: Métodos de acceso para las coordenadas.
  - `setX(double nuevoX)`, `setY(double nuevoY)`: Métodos para modificar las coordenadas.
  - `mover(double deltaX, double deltaY)`: Recibe desplazamientos en X y Y y actualiza las coordenadas del punto sumando estos valores.
  - `calcularDistanciaDesdeOrigen()`: Calcula y retorna la distancia del punto al origen (0,0) usando la fórmula de distancia $\sqrt{x^2 + y^2}$. (Requiere `Math.sqrt()` y `Math.pow()` o simplemente `x*x + y*y`).
  - `mostrarCoordenadas()`: Imprime en consola las coordenadas del punto en formato (x, y).
