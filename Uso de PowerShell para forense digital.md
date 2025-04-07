# PowerShell para los forenses digitales

## **Introducción:**

Este laboratorio se derivará del laboratorio **de citas de archivos** que hicimos antes en términos de recopilación de datos forenses de un sistema Windows en vivo. El script que crearemos a continuación cubrirá la recopilación de información sobre el sistema de destino, los usuarios, los procesos, las conexiones de red y los registros de eventos.

## **Objetivos de aprendizaje:**

- Crear un script introductorio para recopilar datos forenses básicos de un sistema Windows.
- Adapta tu guión para que se adapte a tu entorno.

## **Aguilar:**

### Paso 1: Creación de tu Guión

```plaintext
# This script collects basic forensic information from a Windows system

# Collect system information
Write-Output "Collecting system information..."
$systemInfo = Get-ComputerInfo
$systemInfo | Out-File -FilePath "C:\Forensics\SystemInfo.txt"

# Collect user information
Write-Output "Collecting user information..."
$userInfo = Get-LocalUser
$userInfo | Out-File -FilePath "C:\Forensics\UserInfo.txt"

# Collect running processes
Write-Output "Collecting running processes..."
$processes = Get-Process
$processes | Out-File -FilePath "C:\Forensics\Processes.txt"

# Collect network connections
Write-Output "Collecting network connections..."
$networkConnections = Get-NetTCPConnection
$networkConnections | Out-File -FilePath "C:\Forensics\NetworkConnections.txt"

# Collect event logs
Write-Output "Collecting event logs..."
$eventLogs = Get-EventLog -LogName System -Newest 100
$eventLogs | Out-File -FilePath "C:\Forensics\EventLogs.txt"

Write-Output "Forensic data collection completed."
```

### Paso 2: Entiéndalo y Tailor el Guión

**1. Configuración de Directorio Forense:**

Antes de ejecutar el script, cree un directorio donde se almacenen los datos forenses. Abra PowerShell como administrador y ejecute:

```plaintext
New-Item -Path "C:\" -Name "Forensics" -ItemType "Directory"
```

![](https://i.imgur.com/cUdAZwn.png)

Descargo de responsabilidad: En una escena de investigación forense real, **debe evitar cualquier alteración de la evidencia durante la recolección**. Aunque eso puede parecer imposible debido a la naturaleza de las cosas, le aconsejo que señale el _Out-File_ a un USB o a una unidad remota, si es posible. En este laboratorio, estaremos escribiendo la salida a la propia máquina para fines de prueba.

**2. Información del sistema:**

El `Get-ComputerInfo`cmdlet recopila información detallada sobre el ordenador, incluyendo versiones del sistema operativo, arquitectura y detalles de hardware. La salida se ahorra a `SystemInfo.txt`.

**3. Información del usuario:**

El `Get-LocalUser`cmdlet enumera todas las cuentas de usuario locales del sistema. Esta información es útil para identificar cuentas activas y potencialmente sospechosas. La salida se ahorra a `UserInfo.txt`.

**4. Procesos de ejecución:**

El `Get-Process`cmdlet captura información sobre todos los procesos de ejecución. Analizar los procesos en ejecución puede ayudar a identificar software malicioso o no autorizado. La salida se ahorra a `Processes.txt`.

**5. Conexiones en la Red:**

El `Get-NetTCPConnection`cmdlet enumera todas las conexiones TCP activas. Esto ayuda a identificar la actividad de la red inusual o sospechosa. La salida se ahorra a `NetworkConnections.txt`.

**6. Acostilla de Eventos:**

El `Get-EventLog`cmdlet recupera las últimas 100 entradas del registro de eventos del Sistema. Los registros de eventos son cruciales para identificar anomalías del sistema, fallas e incidentes de seguridad potenciales. La salida se ahorra a `EventLogs.txt`.

### Paso 3: Corre el guión

Antes de ejecutar el script, asegúrese de que está ejecutando PowerShell como administrador. Si aún no tienes que copiar el script a un archivo de .**.ps1,** hazlo ahora. Nombré el archivo. `ForensicCollection.ps1`. Por último, navega a donde salvó el guión y lo ejecutaste.

![](https://i.imgur.com/uBYhksw.png)

Bien el guión se ha completado. Echa un vistazo a la salida ubicada aquí C:Forensics.

![](https://i.imgur.com/VSNn66K.png)

Siéntete libre de abrir los archivos respectivos; te mostraré la salida mía. Para ser honesto, los encontré muy informativos mientras lo estaba pasando.

![](https://i.imgur.com/qEFnkvk.png)

![](https://i.imgur.com/GR3jFRg.png)

![](https://i.imgur.com/6LA1Kgs.png)

![](https://i.imgur.com/GSCWkAj.png)

![](https://i.imgur.com/dEYDpgJ.png)

_(Descargos: las salidas de las capturas de pantalla han sido redactadas para mayor claridad y espacio)_

### **Conclusión**

Este script básico de PowerShell proporciona un enfoque de base a la forense digital en los sistemas Windows. Al entender y ampliar realmente este script, puede desarrollar herramientas forenses más completas adaptadas a las necesidades específicas de investigación. Además, si la forense digital es su taza de té, echa un vistazo a nuestra [introducción GRATIS al Curso Forense Digital](https://elearning.securityblue.team/home/courses/free-courses/introduction-to-digital-forensics#description).