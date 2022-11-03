## Concurrencia en iOS

Una *app* iOS por defecto se ejecuta en un único *thread*, el principal, en el que corre nuestro código y el que actualiza la interfaz de usuario, recibe los eventos generados por éste, etc. En caso de realizar una operación costosa en tiempo en este hilo, como por ejemplo cargar gran cantidad de datos de un servidor, se paralizaría la interfaz de usuario hasta que terminara la operación.

### APIs de concurrencia en iOS y Swift

Tanto iOS como OSX tienen varios APIs con distinto nivel de abstracción para trabajar con operaciones concurrentes:

- En el nivel más bajo estaría trabajar directamente con *threads*, representados por la clase del sistema `NSThread`. La mayoría de aplicaciones no necesitan la flexibilidad que nos proporciona trabajar a este nivel, o no merece la pena teniendo en cuenta lo complicado del código con respecto a las otras alternativas.
- En un nivel intermedio tenemos un _framework_ de Apple llamado *grand central dispatch* o GCD. Tiene un nivel de abstracción razonable para la mayoría de aplicaciones, de hecho en Internet podéis encontrar multitud de tutoriales y ejemplos que lo usan (podéis verlo por la llamada a una función llamada `dispatch_async`, que pone en marcha código concurrente).
- En el nivel más alto de abstracción están las *colas de operaciones* (aunque no es mucho mayor que GCD). Es el API que vamos a explicar aquí ya que es el más sencillo de usar.

Cada API usa internamente los otros de más bajo nivel. Es decir, GCD usa internamente *threads* y las colas de operaciones usan internamente GCD.

En el 2021 se introdujo el soporte para concurrencia de forma nativa en el propio lenguaje de desarrollo usado en iOS, en Swift. Se gestiona con construcciones como `async/await` y `Task`, similares a las de otros lenguajes como C# y Javascript. Al final del tema veremos brevemente estos mecanismos y las ventajas que aportan.


### Colas de operaciones

En una *cola de operaciones* podemos añadir trabajos concurrentes. Manejarlas a nivel básico es muy sencillo. Son instancias de `OperationQueue` y para añadir un trabajo a una solo hay que llamar a `addOperation()`. Hay diversas formas de pasar el código a ejecutar. La más cómoda es en forma de *clausura*. Por ejemplo:

```swift
let cola = OperationQueue();
cola.addOperation() {
    print("comienza operación 1...");
    sleep(5)
    print("...hecho 1");
}
cola.addOperation() {
    print("comienza operacion 2...")
    sleep(3)
    print("...hecho 2");
};
```

> NOTA: podemos ver el resultado del código anterior añadiéndolo por ejemplo a una aplicación iOS. Si usamos una aplicación de línea de comandos tendremos que añadir algo al código ya que si no el programa principal terminaría inmediatamente después del segundo `addOperation` y no se verían los mensajes en pantalla. Por ejemplo podemos llamar a `cola.waitUntilAllOperationsAreFinished()`  que como su propio nombre indica se espera hasta que todas las operaciones añadidas a la cola han terminado.

Si ejecutamos el código anterior veremos que aunque el código de la primera clausura comienza a ejecutarse primero, aun así termina después, es decir, ambas "tareas" se están ejecutando en paralelo y no secuencialmente. Por defecto este es el comportamiento de las colas de operaciones, aunque podemos definir dependencias entre tareas, de modo que se ejecute una solo cuando ha acabado otra determinada. Incluso podemos limitar el número de operaciones concurrentes que se pueden ejecutar en una cola.

### La cola de operaciones principal de una *app*

En aplicaciones iOS está predefinida lo que se llama la "cola de operaciones principal", que es la que ejecuta el código que actualiza la interfaz de usuario. Podemos acceder a ella con `OperationQueue.main`. Esta cola de operaciones no puede ejecutar operaciones concurrentes para evitar inconsistencias, que se podrían dar si dos tareas estuvieran modificando simultáneamente el mismo elemento de la interfaz. Podemos comprobar esto imprimiendo el valor de `OperationQueue.main.maxConcurrentOperationCount`, que veremos que vale 1, es decir, no hay operaciones concurrentes en esta cola.

Cuando necesitamos ejecutar una operación especialmente costosa en tiempo no es recomendable bloquear la interfaz de usuario, por lo que se suele crear una cola de operaciones aparte de la principal y ejecutar la operación en esta.  

Un problema adicional es que normalmente esta operación costosa debe actualizar la interfaz de usuario al finalizar, pero ningún hilo de ejecución que no sea el principal debe actualizar la interfaz de usuario, ya que lo contrario podría producir resultados inconsistentes. Esto lo podemos resolver accediendo a la cola de operaciones principal con `OperationQueue.main`. Por ejemplo:

```swift
let background = OperationQueue();
background.addOperation() {
    print("Comienzo mi duro trabajo...")
    sleep(4)
    print("...terminado!")
    print("pero yo NO DEBO tocar la interfaz")
    OperationQueue.main.addOperation() {
        print("Soy main. Desde aquí sí se puede actualizar la interfaz")
    }
}
```



