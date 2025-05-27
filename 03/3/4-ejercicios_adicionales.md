# Ejercicios adicionales

La idea de este loe ejercicios es **identificar los casos de prueba** y proponer la implementación de **pruebas unitarias** para los ejercicios desarrollados en la clase anterior.

**Nota:** Para cada caso de prueba, se recomienda implementar pruebas unitarias utilizando JUnit 5 y Mockito si aplica. Las pruebas deben validar tanto los **casos exitosos** como los de **manejo de errores** y el **registro de eventos**.

## _Ejercicio 1: Sistema de Gestión de Recetas de Cocina_

_Este ejercicio se enfoca en crear una aplicación que permita gestionar una colección de recetas de cocina._

_**Requisitos Funcionales**:_

_1. **Registro de Recetas**: El sistema debe permitir registrar nuevas recetas, incluyendo información como el nombre, una lista de ingredientes (con cantidad y unidad), las instrucciones de preparación, y un número de porciones._  
_2. **Consulta de Recetas**: Los usuarios deben poder buscar y visualizar los detalles de una receta específica por su nombre._  
_3. **Actualización de Recetas**: Debe ser posible modificar las instrucciones o el número de porciones de una receta existente._  
_4. **Listado de Recetas**: El sistema debe poder mostrar una lista de todas las recetas registradas._  
_5. **Manejo de Errores**:_  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta registrar una receta con datos incompletos o inválidos (ej. nombre vacío, porciones negativas), el sistema debe informarlo y evitar el registro._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta buscar o actualizar una receta que no existe, el sistema debe notificar al usuario._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta añadir más recetas de las que el sistema puede almacenar, debe haber una notificación._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Las entradas del usuario no numéricas cuando se espera un número (ej. para porciones) deben ser manejadas._  
_6. **Registro de Eventos**: Todas las operaciones importantes (registro, búsqueda, actualización, fallos) deben ser registradas para auditoría y diagnóstico de problemas._

## _Ejercicio 2: Sistema de Reservas de Salas de Reuniones_

_Este ejercicio se centra en un sistema para gestionar la disponibilidad y las reservas de varias salas de reuniones en una oficina._

_**Requisitos Funcionales**:_

_1. **Definición de Salas**: El sistema debe contar con un conjunto predefinido de salas de reuniones, cada una con un identificador único, un nombre y una capacidad máxima de personas._  
_2. **Registro de Reservas**: Los usuarios deben poder crear una reserva para una sala específica en una fecha y hora determinadas, especificando el nombre de quien reserva y el número de asistentes. Las reservas tienen una duración fija (ej. 1 hora)._  
_3. **Verificación de Disponibilidad**: Antes de registrar una reserva, el sistema debe verificar que la sala esté disponible para la fecha y hora solicitadas, y que el número de asistentes no exceda la capacidad de la sala._  
_4. **Consulta de Reservas**: Debe ser posible ver todas las reservas para una sala específica o todas las reservas para una fecha dada._  
_5. **Cancelación de Reservas**: Los usuarios deben poder cancelar una reserva existente mediante su identificador._  
_6. **Manejo de Errores**:_  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta reservar una sala con datos inválidos (ej. fecha pasada, número de asistentes negativo, nombre de reservante vacío), el sistema debe informarlo._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si la sala no está disponible (ya reservada o capacidad excedida), el sistema debe notificar al usuario._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta buscar o cancelar una reserva o sala que no existe, el sistema debe indicarlo._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Las entradas del usuario no numéricas deben ser manejadas._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta crear más reservas de las que el sistema puede almacenar, debe haber una notificación._  
_7. **Registro de Eventos**: Todas las acciones del sistema (creación, verificación, cancelación de reservas, y cualquier error) deben ser registradas detalladamente._

## _Ejercicio 3: Sistema de Gestión de Libros de una Pequeña Biblioteca_

_Este ejercicio retoma el tema de los libros, pero con un enfoque diferente: una biblioteca con capacidades de préstamo y devolución._

_**Requisitos Funcionales**:_

_1. **Catálogo de Libros**: El sistema debe permitir registrar libros en el catálogo, incluyendo su título, autor, ISBN (identificador único) y la cantidad total de copias disponibles._  
_2. **Préstamo de Libros**: Un usuario debe poder prestar un libro especificando su ISBN. El sistema debe verificar que el libro exista y que haya al menos una copia disponible para prestar. Si el préstamo es exitoso, la cantidad de copias disponibles debe disminuir._  
_3. **Devolución de Libros**: Un usuario debe poder devolver un libro especificando su ISBN. El sistema debe verificar que el libro exista. Si la devolución es exitosa, la cantidad de copias disponibles debe aumentar._  
_4. **Consulta de Libros**: Los usuarios deben poder buscar un libro por su ISBN o título, y el sistema debe mostrar su información, incluyendo la cantidad de copias disponibles._  
_5. **Listado de Libros**: El sistema debe mostrar un listado completo de todos los libros en el catálogo._  
_6. **Manejo de Errores**:_  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta registrar un libro con datos incompletos o inválidos (ej. ISBN duplicado, cantidad de copias negativa), el sistema debe evitar el registro._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta prestar un libro que no existe o del cual no hay copias disponibles, el sistema debe informarlo._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta devolver un libro que no existe, el sistema debe notificarlo._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Las entradas del usuario no numéricas deben ser manejadas._  
_&nbsp;&nbsp;&nbsp;&nbsp;- Si se intenta añadir más libros al catálogo de los que el sistema puede almacenar, debe haber una notificación._  
_7. **Registro de Eventos**: Todas las operaciones (registro, préstamo, devolución, consulta, listado y cualquier error) deben ser registradas en un log._
