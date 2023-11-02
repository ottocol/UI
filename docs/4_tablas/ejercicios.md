# Ejercicios de tablas (2,5 puntos)

Vamos a crear una pequeña aplicación en la que se muestre una tabla con una lista de datos y se puedan insertar y eliminar filas

### Crear la interfaz (0,5 puntos)

Crea otra pantalla, al estilo de la que aparece en la figura. Tiene que tener los siguientes componentes:

- Un *text field*
- Un *button* con el texto "Insertar"
- Un *table view*

En el panel derecho de Xcode, selecciona lad propiedades adecuadas para que la tabla sea dinámica con un solo prototipo.

**Importante**: no te olvides de darle un identificador al prototipo, o `cell identifier`.


En cuanto al *layout*:

- El campo de texto debe aparecer junto al botón (lo más sencillo es que pongas ambos en un *stack view* centrado) y a una distancia de 20 puntos de la parte superior de la pantalla
- La parte superior de la tabla debe estar a una distancia de 20 puntos de la parte inferior del *stack view* y los otros tres bordes deben estar a 20 puntos de los bordes de la *safe area*.

![](images/interfaz_ejer_tablas.png).

### Mostrar datos en la tabla (1 punto)

- Crea una clase Swift llamada `DSTabla` que va a hacer de *datasource* de la tabla seleccionándola. Recuerda que el `DSTabla` debe heredar de `NSObject`e implementar el protocolo `UITableViewDataSource`
- Conecta el `DSTabla` con la propiedad `dataSource` de la tabla
- El `DSTabla` va a almacenar los datos en una propiedad `lista` de tipo array de `String` (puedes usar los nombres que aparecen en el ejemplo u otros datos cualesquiera, es indiferente) 

```swift
//en la clase DSTabla, definimos e inicializamos la propiedad
var lista = ["Daenerys Targaryen", "Jon Nieve", "Cersei Lannister", "Eddard Stark"]
```

- Ahora tendrás que implementar tú de manera adecuada los métodos:
	- `tableView(_:, numberOfRowsInSection:)` recuerda que debe devolver el número de filas de la tabla
	- `tableView(_:, cellForRowAt:)`: recuerda que debe devolver la celda para una fila determinada
   
**De nuevo importante**: en el método `dequeReusableCell` que necesitas llamar dentro del `cellForRowAt`, no te olvides de usar el mismo `cell identifier` que le has puesto antes al prototipo. 

### Insertar filas (0,5 puntos)

Haz que al pulsar sobre el botón insertar se añada el texto a la lista de datos en la última posición (método `append` del array) y luego añada visualmente la fila en la tabla (recuerda que es `insertRows` del `UITableView`).

Un parámetro del `insertRows` es el `IndexPath` de la celda. Puedes crear uno con el constructor `IndexPath(row:, section:)`, pasándole el número de fila y sección. En nuestra tabla solo hay una sección, así que sería la 0.

> Ten en cuenta que no necesitas el método `tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath)` ya que este lo llama iOS cuando pulsas en los iconos de eliminar o añadir celda, pero ahora lo estás haciendo con un botón propio. Solo necesitas insertar el dato en el array y la celda con `insertRows`.

### Delegate (0,5 puntos)

Vamos a hacer que se puedan "marcar" filas de la tabla. Para ello necesitas algún objeto que actúe de *delegate*. Crea para ello una clase `DelegateTabla` 

- Haz que esta clase herede de `NSObject` e implemente el protocolo `UITableViewDelegate`
- Implementa en esta clase un método similar al que hemos visto en clase que marca o desmarca una fila poniéndole un "checkmark", pero aquí cambiando el texto a color rojo (propiedad `textColor` a `UIColor.red`) y si está en rojo pasando a negro.

Ahora vamos a conectar el *delegate* con la tabla gráficamente, en vez de por código, así practicamos esta manera de hacerlo. Para ello:

1. Arrastrar un componente `Object` al árbol de componentes de la vista del *storyboard* con la pantalla del dispositivo (al árbol que aparece a la izquierda, no a la pantalla en sí).
2. Seleccionar el componente `Object`, y en el `Identity inspector` (cuarto icono del panel derecho de Xcode), escribir el nombre de la clase `DelegateTabla` en `Custom Class`.
3. Conectar tabla y delegate: seleccionamos la tabla con el ratón y vamos al `Connections inspector` (el último icono del panel derecho de Xcode). Arrastramos con el ratón (no hace falta `Ctrl`) desde el círculo que representa al `delegate` hasta el icono del objeto que representa a la clase `TablaDelegate`

Una vez hecho esto, comprueba que funciona correctamente.
