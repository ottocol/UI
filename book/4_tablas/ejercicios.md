# Ejercicios de tablas

## Tabla estática

Crea una pantalla que sea una tabla estática, al estilo de la aplicación de *settings* de iOS, con las siguientes condiciones:
- Debe tener al menos tres secciones, con el número de filas que quieras en cada una.
- Algunas celdas deben tener iconos. Puedes por ejemplo usar [estos](https://www.iconfinder.com/iconsets/ios-7-icons), o cualquier otro que encuentres por la web
- Prueba diferentes tipos de *accesory* en las celdas (accesibles en las propiedades de la celda)
- Una de las celdas tendrá como título “Ver lista” y nos llevará a la segunda pantalla (ahora veremos cómo)

## Tabla dinámica 

### Crear la interfaz y la navegación entre pantallas

Crea otra pantalla, al estilo de la que aparece en la figura. Tiene que tener los siguientes componentes:
- Un *text field*
- Un par de *button*s
- Un *table view*

![](interfaz_ejer2_tablas.png)

> Aunque habría que usar *autolayout*, para simplificar el ejercicio puedes ignorar este aspecto.

Conecta la pantalla inicial con esta mediante un *segue*, asociado a la celda con título “ver lista”. Para crearlo simplemente haz `Ctrl`+ arrastrar desde la celda hasta la pantalla destino. 

Recuerda que para poder volver atrás necesitarás implementar un método en la pantalla inicial con la siguiente signatura (aunque puedes cambiar el nombre)

```swift
@IBAction func volverAtras(segue: UIStoryboardSegue) {
 
}
```

Ahora asocia el botón “atrás” con este método haciendo `Ctrl` + arrastrar del botón al icono “exit” que aparece en la parte superior del “view controller”, y eligiendo el método `volverAtras` (o como lo hayas llamado) en el menú *popup*.

Comprueba que la navegación funciona y que puedes moverte entre las dos pantallas

### Mostrar datos en la tabla

- Primero crea una nueva clase que hará de *view controller* de la segunda pantalla: menú  `File > New > File`, selecciona la plantilla de `Cocoa Touch Class` y llama a la nueva clase `ListaViewController`, asegurándote de que es subclase de `UIViewController`.
- Haz que la clase creada sea efectivamente el *controller* de la segunda pantalla seleccionando esta, yendo al `identity inspector` (icono ![](Captura%20de%20pantalla%202015-10-21%20a%20las%2014.35.59.png)) y poniendo `ListaViewController` como valor de la propiedad `Class`.
- Crea una clase Swift llamada `GestorTabla` que va a hacer de *datasource* de la tabla seleccionándola. **Haz que el `GestorTabla` implemente el protocolo `UITableViewDataSource`**
- Conecta el `GestorTabla` con la propiedad `dataSource` de la tabla
- El `GestorTabla` va a almacenar los datos en una propiedad `nombres` de tipo array de `String` 

```swift
//en la clase GestorTabla, definimos e inicializamos la propiedad
var nombres = ["Daenerys Targaryen", "Jon Nieve", "Cersei Lannister", "Eddard Stark"]
```

- Ahora tendrás que implementar tú de manera adecuada los métodos:
	- `tableView(_:, numberOfRowsInSection:)` recuerda que debe devolver el número de filas de la tabla
	- `tableView(_:, cellForRowAt:)`: recuerda que debe devolver la celda para una fila determinada

### Insertar filas

Vamos a hacer que se pueda editar la tabla. Para ello necesitas algún objeto que actúe de *delegate*. Vamos a cargarle la responsabilidad también al `GestorTabla`:

- Haz que el esta clase implemente también el protocolo `UITableViewDelegate `
- En el `connections inspector` conecta gráficamente el *Outlet* `delegate` al `GestorTabla`
- Implementa en esta clase un método `insertarCelda(enTabla:,enFila:,conTexto:) ` que al pasarle un `UITableView`, un número de fila y un texto añada el texto a la lista de nombres y además añada visualmente la fila en la tabla.

En la pantalla de lista, haz que cuando se escriba un texto en el campo de texto y se pulse el botón `Insertar` se llame al método `insertarCelda (enTabla:,enFila:,conTexto:) ` que has definido

### Eliminar filas

Añádele a la pantalla con la lista de nombres un botón “Poner/quitar modo edición”. Haz que cuando se pulse el botón la tabla se ponga en “modo edición” llamando a `setEditing(true,animated:true)` sobre la tabla. Si está en modo edición (`isEditing` es `true)` haz que pase a modo normal.

Para que se borre no solo visualmente la fila sino también el nombre del array tendrás que implementar el método `tableView(_:, commit:, forRowAt:) `. Puedes usar como base el código de los apuntes, pero no es necesario que tengas en cuenta el caso de insertar en este método, ya que lo has implementado antes de otra forma, solo el de borrar.
