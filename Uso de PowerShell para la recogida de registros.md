# PowerShell para la recogida de registros

## **Introducción:**

En esta serie, previsualizaremos algunos ejemplos que podrían utilizarse para crear automatización para la recolección y análisis de registros, junto con la orquestación utilizando Task Scheduler, para simular una respuesta básica de seguridad en una máquina virtual de Windows.

Crear un script de PowerShell como el que vamos a crear a continuación implica reunir registros de eventos específicos desde el registro de seguridad de Windows y exportarlos a un archivo XML para su posterior análisis. El siguiente guión está diseñado para recoger eventos para un solo día, centrándose en eventos exitosos de sesión (Event ID 4624). Sin embargo, puede ajustar el ID de Evento o añadir más para recoger diferentes registros de seguridad.

## **Objetivos de aprendizaje:**

- Recoger registros de seguridad de un sistema de Windows con PowerShell.

## **Aguilar:**

### Paso 1: Abrir tu IDE

Comience abriendo su editor de script preferido. PowerShell ISE es una buena opción para principiantes, mientras que el código de estudio visual con la extensión de PowerShell es excelente para usuarios más avanzados.

### Paso 2: Configuración de script

Al principio de su script, defina las variables para la ubicación de registro (el registro de seguridad), el directorio de salida donde desea guardar los registros recopilados, y la fecha de uso como parte del nombre del archivo de los registros exportados.

```plaintext
# Define the output directory path
$outputPath = "C:\\SecurityLogs"

# Check if the output directory exists; if not, create it
if (-not (Test-Path -Path $outputPath)) {
    New-Item -ItemType Directory -Path $outputPath
}

# Get the current date in YYYYMMDD format for the filename
$currentDate = Get-Date -Format "yyyyMMdd"
```

### Paso 3: Recogida y Registros de Exportaciones

Uso `Get-WinEvent`con un filtro para el registro de seguridad y ID de evento 4624 (eventos de inicio de sesión exitosos). Ajusta el `StartTime`y `EndTime`si desea recoger troncos por un período distinto al día actual. A continuación, exporte los registros recogidos a un archivo XML en el directorio de salida especificado.  
  
**Nota:** Estamos utilizando `Get-WinEvent`en vez de `Get-EventLog`debido a que este último está desactualizado y potencialmente no está disponible en futuras versiones de Windows PowerShell. Según [Microsoft](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-eventlog?view=powershell-5.1), `_Get-EventLog_`_utiliza una API Win32 que se desprecaba. Los resultados pueden no ser exactos._

```plaintext
# Define the start and end times for today
$startTime = (Get-Date).Date
$endTime = (Get-Date).Date.AddDays(1).AddSeconds(-1)

# Hashtable for filtering events
$filterHashtable = @{
    LogName   = "Security"
    ID        = 4624
    StartTime = $startTime
    EndTime   = $endTime
}

# Collect the logs and export them to XML
$events = Get-WinEvent -FilterHashtable $filterHashtable
if ($events) {
    $events | Export-CliXml -Path "$outputPath\\$currentDate-SecurityLogs.xml"
    Write-Host "Security logs for successful logins on $currentDate have been collected and saved to: $outputPath" -ForegroundColor Green
} else {
    Write-Host "No security logs for successful logins found for $currentDate." -ForegroundColor Yellow
}

```

![](https://i.imgur.com/q5SkOJK.png)

### Paso 4: Salvar y ejecutar el guión

Guarda tu script con el `.ps1`extensión, por ejemplo, `collect-securitylogs.ps1`.

Para ejecutar el script, abra PowerShell como administrador, navegue al directorio que contiene el script y luego ejecutarlo escribiendo `.\collect-securitylogs.ps1`. Recuerde, ejecutar scripts pueden requerir cambiar la política de ejecución de su sistema (por ejemplo, `Set-ExecutionPolicy RemoteSigned`).

![](https://i.imgur.com/tXtlez5.png)

![](https://i.imgur.com/3K0PGUM.png)

Como usted ve arriba de un archivo titulado `script.log`se crea en el `C:\`camino. Esto es para ilustrar las entradas de registro. El archivo principal, siendo el XML, se ubicará aquí `C:\SecurityLogs`.

![](https://i.imgur.com/izrDldq.png)

### **Notas:**

**Seguridad y permisos:** Ejecutar este guión requiere privilegios de administrador porque accede al registro de seguridad.

**Política** de **ejecución:** Si encuentras un error relacionado con las políticas de ejecución de script, es posible que tengas que ajustarlos. Sea cauteloso y asegúrese de entender las implicaciones de seguridad.

**Personalización:** El script se puede personalizar para recoger diferentes tipos de eventos cambiando el ID de Evento en el hashtable o ajustando el plazo para la recogida de registro.

## **Resumen:**

Este script proporciona un enfoque fundamental para la recolección de datos de seguridad log, que pueden ser adaptados y expandidos en base a requisitos específicos o incluyen estrategias de filtrado y gestión de registros más sofisticadas.
