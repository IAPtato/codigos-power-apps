# codigos-power-apps
codigo que he utilizado para creacion de aplicaciones en power apps
__________________________________________________________________________________________________________
#con esto creamos un codigo de acceso 
If(
    TextInput1.Text = "Justin3000",
    Navigate(verRegistros, ScreenTransition.UnCoverRight),
    Notify("Contraseña incorrecta. Intenta nuevamente.", NotificationType.Error)
)
And 
Reset(TextInput1)
__________________________________________________________________________________________________________

# con este codigo ordenamos de forma descendente y filtramos por coincidencia de texto 
SortByColumns(
    If(
        Dropdown1.Selected.Value = "Todos",
        'Parametros Para Listados Items',
        Filter('Parametros Para Listados Items', 'Serie de Maquina'.Value = Dropdown1.Selected.Value)
    ),
    "ID",
    SortOrder.Descending
)
___________________________________________________________________________________________________________
