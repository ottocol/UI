# Ejercicio de vistas en iOS: controles básicos (1,5 puntos)

El objetivo es practicar con los controles básicos de la interfaz de usuario en iOS creando un "panel de control" ficticio para una supuesta nave espacial al estilo del que aparece en la figura.

![](images/panel_control.png)

Puedes ayudarte de la [documentación de referencia de UIKit](https://developer.apple.com/documentation/uikit/views_and_controls) para ver los métodos y propiedades de los componentes:

- **(0,25 puntos)** Campo de texto: Al escribir en él y pulsar intro en el teclado *onscreen* ,debe concatenarse lo escrito al campo de texto de varias líneas (un *text view*)

> Si quieres que aparezca el teclado *onscreen* en el simulador debes desactivar la opción de menú de `I/O > Keyboard > Connect Hardware Keyboard` (que por defecto aparece activada) ya que si lo está se usará el teclado físico del Mac pero no aparecerá el teclado en pantalla.

- **(0,5 puntos)** *Slider*: 
    + Debes fijar el valor inicial en 0 y el final en 100 (son propiedades del objeto). **Fíjalas por código** en el `viewDidLoad` del controller, consulta la documentación para ver el nombre. Necesitarás un *outlet* para acceder al *slider* y poder cambiar sus propiedades por código.
    + Al cambiar su valor debe aparecer en un *label* al lado cuál es el valor actual. Al igual que los botones, para procesar un cambio en un slider puedes crear un *action* con Ctrl+Arrastrar
- **(0,25 puntos)** Al pulsar el botón ¡Emergencia! debe aparecer un *action sheet* con tres opciones: "nave salvavidas", "hiperespacio" o "autodestrucción" (como ves, una opción a la que deberías ponerle `style: .destructive`). Las opciones simplemente imprimirán un mensaje en la consola con `print` (por ejemplo "¡¡Lanzada nave salvavidas!!).
- **(0,5 puntos)** Añade un *picker* para elegir el destino del viaje de entre una serie de destinos prefijados (por ejemplo "Saturno", "M13", "Nebulosa de Orión",...). Cada vez que cambie el valor seleccionado en el *picker* debe aparecer un mensaje en la consola indicándolo y mostrando el valor actual.
