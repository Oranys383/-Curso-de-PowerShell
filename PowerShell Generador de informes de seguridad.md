# Generador de informes de seguridad

### **Introducción:**

Este laboratorio mostrará un script de PowerShell que automatiza la recolección, análisis y análisis de registros de seguridad de múltiples fuentes. El guión recopilará registros, extraerá información relevante y generará un informe en el que se destacarán posibles incidentes de seguridad. Será similar a los anteriores laboratorios del curso.

**Descargo de responsabilidad:** Siéntete libre de modificar y ampliar este proyecto basado en tus necesidades y entorno específicos

### **Escritura completa:**

```plaintext
# Define the log sources to collect
$logSources = @(
    "System",
    "Application",
    "Security"
)

# Define the output directory for logs
$outputDir = "C:\SecurityLogs"
New-Item -ItemType Directory -Path $outputDir -Force

# Collect logs from each source
foreach ($source in $logSources) {
    $logs = Get-EventLog -LogName $source -Newest 1000
    $logs | Export-Csv -Path "$outputDir\$source.csv" -NoTypeInformation
}

# Collect Sysmon logs
$sysmonLogs = Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 1000
$sysmonLogs | Export-Csv -Path "$outputDir\Sysmon.csv" -NoTypeInformation

# Define a function to parse logs
function Parse-Log {
    param (
        [string]$logFile
    )
    
    $logData = Import-Csv -Path $logFile
    foreach ($entry in $logData) {
        $eventID = $entry.EventID
        $timeGenerated = $entry.TimeGenerated
        $message = $entry.Message
        
        Write-Output "EventID: $eventID, Time: $timeGenerated, Message: $message"
    }
}

# Define a function to analyze logs
function Analyze-Log {
    param (
        [string]$logFile
    )
    
    $logData = Import-Csv -Path $logFile
    foreach ($entry in $logData) {
        $eventID = $entry.EventID
        $timeGenerated = $entry.TimeGenerated
        $message = $entry.Message
        
        if ($eventID -in 4625, 4648, 4688, 4689, 4768) {
            Write-Output "Potential Security Incident: EventID $eventID at $timeGenerated - $message"
        }
    }
}

# Define a function to generate a report
function Generate-Report {
    param (
        [string]$reportFile
    )
    
    $html = @"
<!DOCTYPE html>
<html>
<head>
    <title>Security Log Analysis Report</title>
</head>
<body>
    <h1>Security Log Analysis Report</h1>
    <table border="1">
        <tr>
            <th>EventID</th>
            <th>Time</th>
            <th>Message</th>
        </tr>
"@

    $logFiles = Get-ChildItem -Path $outputDir -Filter *.csv
    foreach ($logFile in $logFiles) {
        $logData = Import-Csv -Path $logFile.FullName
        foreach ($entry in $logData) {
            $eventID = $entry.EventID
            $timeGenerated = $entry.TimeGenerated
            $message = $entry.Message
            
            if ($eventID -in 4625, 4648, 4688, 4689, 4768) {
                $html += "<tr><td>$eventID</td><td>$timeGenerated</td><td>$message</td></tr>"
            }
        }
    }

    $html += @"
    </table>
</body>
</html>
"@

    $html | Out-File -FilePath $reportFile
}

# Parse and analyze each log file
$logFiles = Get-ChildItem -Path $outputDir -Filter *.csv
foreach ($logFile in $logFiles) {
    Parse-Log -logFile $logFile.FullName
    Analyze-Log -logFile $logFile.FullName
}

# Generate the report
$reportFile = "C:\SecurityLogs\AnalysisReport.html"
Generate-Report -reportFile $reportFile
```

## **Aguilar:**

### Paso 1: Ejecute su guión

Vamos a ejecutar el guión...full-securitylogs.ps1. Nombré el guión _Full-securitylogs.ps1,_ si no estabas al tanto.

![](https://i.imgur.com/PqoU9lL.png)

El script debe tomar un minuto o dos para procesar. Después, encontrará la salida en esta ruta de archivo: C: SecurityLogs.

![](https://i.imgur.com/NOIU8TO.png)

Eche un vistazo al **AnalysisReport.html**

![](https://i.imgur.com/vvfWskz.png)

![](https://i.imgur.com/wEbaU7m.png)

El script también proporciona un archivo ***.xml** que se puede utilizar para otros propósitos también

![](https://i.imgur.com/44Mroz5.png)

Por último, con los archivos **restantes *.csv**, puede subirlos en [`Timeline Explorer`](https://ericzimmerman.github.io/#!index.md)para filtrarlos aún más.

![](https://i.imgur.com/sOLu4pN.png)

## **Resumen:**

Este laboratorio opcional era necesario para complementar nuestros dos últimos laboratorios anteriores: Colección de Log y "Analyze Security Logs". Siéntase libre de expandir este script para capturar informes en múltiples dispositivos.potencial a través de redes. Vamos a sumergirnos en nuestro laboratorio final usando PowerShell.