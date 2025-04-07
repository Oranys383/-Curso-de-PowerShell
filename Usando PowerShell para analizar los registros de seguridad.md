# PowerShell para Analizar los registros de seguridad

## **Introducción:**

Este laboratorio asumirá que ha completado el "Using PowerShell for Log Collection". Ahora en este laboratorio, vamos a demostrar un script de PowerShell que le permite analizar los registros de seguridad recogidos por el `collect-securitylogs.ps1`Escote. Por simplicidad, nombraremos el nuevo guión `analyze-securitylogs.ps1`. Para facilitar la orquestación, vamos a utilizar el Programador de tareas.

Esta guía se centrará en un script simplificado que resume los eventos de inicio de sesión de los archivos XML. El resumen podría incluir contando intentos exitosos y fallidos de inicio de sesión

## **Objetivos de aprendizaje:**

- Cree un script para realizar el análisis de registro con PowerShell.
- Entender los conceptos de orquestación mediante la creación de un Programador de tareas.

## **Aguilar:**

### Paso 1: Creación de tu Guión

Abra su editor de script preferido, como PowerShell ISE o Visual Studio Code, y comience con la configuración básica de su script:

1. **Especifique el Directorio de Entradas:** Defina la ruta donde se almacenan los archivos de registro XML.
2. Comprueba **los archivos XML** : Verifique que existan archivos XML en el directorio especificado.

```plaintext
# Define the directory containing the XML log files
$inputPath = "C:\\SecurityLogs"

# Verify the directory exists and contains XML files
if (-not (Test-Path -Path $inputPath)) {
    Write-Host "The specified input path does not exist." -ForegroundColor Red
    exit
}

$xmlFiles = Get-ChildItem -Path $inputPath -Filter "*.xml"
if ($xmlFiles.Count -eq 0) {
    Write-Host "No XML files found for analysis." -ForegroundColor Yellow
    exit
}

```

### Paso 2: Analizando los registros

Para cada archivo XML encontrado, cargue el archivo y luego analice para resumir los intentos de inicio de sesión. Para simplificarlo, los lo permite contar el número de accesos exitosos (Event ID 4624).

```plaintext
# Initialize a counter for successful logins
$successfulLoginCount = 0

foreach ($file in $xmlFiles) {
    [xml]$xmlContent = Get-Content -Path $file.FullName

# Count successful login events where Event ID is 4624
$successfulLoginCount += ($xmlContent.Objs.Obj | Where-Object { $_.Props.I32.N -eq "Id" -and $_.Props.I32."#text" -eq "4624" }).count


# Display the summary of successful logins
Write-Host "Total successful logins: $successfulLoginCount" -ForegroundColor Green

```

![](https://i.imgur.com/jbYRcZX.png)

### Paso 3: Guardar y probar su guión

Guarde su script con un nombre significativo, como `analyze-securitylogs.ps1`.

Pruebe el script en PowerShell y ejecutelo como Administrador para asegurarse de que lee correctamente los archivos XML y resume los datos como se esperaba.

![](https://i.imgur.com/thh5QNZ.png)

### Paso 4: Orquestación con programador de tareas

Para automatizar la ejecución de sus scripts ( `collect-securitylogs.ps1`y `analyze-securitylogs.ps1`), puede utilizar Windows Task Scheduler:

**Abre el Programador de tareas** y crea una nueva tarea.

![](https://i.imgur.com/Qt5lFRz.png)

**General Tab** : Dele a su tarea un nombre y confiéquelo para ejecutar con los privilegios más altos.

![](https://i.imgur.com/sbZ9nSM.png)

**Triggers Tab:** Configura cuando quieres que el script se ejecute (por ejemplo, diariamente en un momento específico).

![](https://i.imgur.com/IHyu0e4.png)

**Actions Tab:** Crear una nueva acción para iniciar un programa, y establecer el programa/script en `powershell.exe`, y agregue la ruta completa a su script en el campo de Agregar argumentos, prefijado con `File`. Por ejemplo: `File "C:\PathToYourScript\analyze-securitylogs.ps1"`.

![](https://i.imgur.com/T07E7yu.png)

![](https://i.imgur.com/RqVoDjN.png)

**Condiciones y Configuración Tabs** : Ajuste estas configuraciones según sea necesario para cumplir con sus requisitos y configuración del sistema específicos.

### Notas:

Recuerde probar bien sus scripts antes de automatizarlos con Task Scheduler para asegurarse de que se realizan como se esperaba en su entorno.

## **Resumen:**

Este enfoque simplificado se centra en resumir los inicios de sesión exitosos de los registros de seguridad recogidos. Dependiendo de sus necesidades, podría expandir el script para analizar diferentes tipos de eventos, buscar patrones o generar informes más detallados.