# codigos-power-apps
codigo que he utilizado para creacion de aplicaciones en power apps
________________________________________________________________________________________________________________________________________
#con esto creamos un codigo de acceso 
If(
    TextInput1.Text = "Justin3000",
    Navigate(verRegistros, ScreenTransition.UnCoverRight),
    Notify("Contraseña incorrecta. Intenta nuevamente.", NotificationType.Error)
)
And 
Reset(TextInput1)
_________________________________________________________________________________________________________________________________________

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
___________________________________________________________________________________________________________________________________________
#con este codigo si tienes conexion a internet carga los datos directamente y si no los guarda en una bd sin conexion 

Set(FechaInicioCompleta, 
    DateAdd(
        DateAdd(DateValue3.SelectedDate, Value(HourValue3.Selected.Value), TimeUnit.Hours), 
        Value(MinuteValue3.Selected.Value), TimeUnit.Minutes
    )
);

Set(FechaFinCompleta, 
    DateAdd(
        DateAdd(DateValue4.SelectedDate, Value(HourValue4.Selected.Value), TimeUnit.Hours), 
        Value(MinuteValue4.Selected.Value), TimeUnit.Minutes
    )
);

If(
    IsBlank(DataCardValue41.Selected.Value) ||  //autorizado
    IsBlank(DataCardValue13.Selected.Value) ||  // Vehículo
    IsBlank(DataCardValue14.Selected.Value) ||  // Nombre
    IsBlank(DataCardValue15.Selected.Value) ||  // Motivo
    IsBlank(DataCardValue4.Text) ||             // Origen
    IsBlank(DataCardValue3.Text) ||             // Destino
    IsBlank(DataCardValue12.Selected.Value) ||  // Provincia
    IsBlank(FechaInicioCompleta) ||             // Fecha Inicio con Hora y Minutos
    IsBlank(DataCardValue6.Text) ||             // Km Inicio
    IsBlank(FechaFinCompleta) ||                // Fecha Fin con Hora y Minutos
    IsBlank(DataCardValue10.Text),              // Km Fin
    

    Notify("Por favor, completa todos los campos obligatorios.", NotificationType.Error),
    
    
    If(
        Connection.Connected,
        Patch(RegistroVehiculo, Defaults(RegistroVehiculo), 
            {
                IDUnico: DataCardValue8.Text,
                'Autorizado por:': DataCardValue41.Selected.Value, 
                Vehículo: DataCardValue13.Selected.Value,
                Nombre: DataCardValue14.Selected.Value,
                Motivo: DataCardValue15.Selected.Value,
                Origen: DataCardValue4.Text,
                Destino: DataCardValue3.Text,
                Provincia: DataCardValue12.Selected.Value,
                'Fecha Inicio': FechaInicioCompleta, // Ahora incluye Fecha + Hora + Minutos
                'Km Inicio': Value(DataCardValue6.Text),
                'Fecha Fin': FechaFinCompleta, // Ahora incluye Fecha + Hora + Minutos
                'Km Fin': Value(DataCardValue10.Text),
                'Combustible $': Value(DataCardValue7.Text),
                'Método de pago': DataCardValue11.Text,
                'Combustible Km': Value(DataCardValue16.Text),
                'Motivo/Observacion': DataCardValue17.Text
            }
        ),
        Collect( Dataversesincargar, {
                IDUnico: DataCardValue8.Text,
                'Autorizado por:': DataCardValue41.Selected.Value,
                Vehículo: DataCardValue13.Selected.Value,
                Nombre: DataCardValue14.Selected.Value,
                Motivo: DataCardValue15.Selected.Value,
                Origen: DataCardValue4.Text,
                Destino: DataCardValue3.Text,
                Provincia: DataCardValue12.Selected.Value,
                'Fecha Inicio': FechaInicioCompleta, // Ahora incluye Fecha + Hora + Minutos
                'Km Inicio': Value(DataCardValue6.Text),
                'Fecha Fin': FechaFinCompleta, // Ahora incluye Fecha + Hora + Minutos
                'Km Fin': Value(DataCardValue10.Text),
                'Combustible $': Value(DataCardValue7.Text),
                'Método de pago': DataCardValue11.Text,
                'Combustible Km': Value(DataCardValue16.Text),
                'Motivo/Observacion': DataCardValue17.Text
            });
        SaveData(Dataversesincargar, "Dataverseparacargar")
    );

    Navigate(Fin, ScreenTransition.Fade)
)
________________________________________________________________________________________________________________________________________
