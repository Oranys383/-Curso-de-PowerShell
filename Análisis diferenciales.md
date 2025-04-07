# PowerShell para análisis diferenciales

1. Cómo funciona PowerShell
2. Trabajando con PowerShell (Security)
3. PowerShell para análisis diferenciales

# Análisis diferencial w/ Pwsh

### **Introducción**

Hay muchas oportunidades para que un atacante despliegue malware en un sistema Windows. Como profesionales de la ciberseguridad, es importante capturar una base de referencia de información para usar en su evaluación, y luego comparar el entorno actual con la base de referencia conocida. Este proceso se conoce como _análisis diferencial_.

### **Análisis diferencial en acción: Creación de su base**

Creemos **tres** archivos de referencia para _servicios_, _tareas programadas_ y _usuarios locales_, y guardarlos en `C:\Users\BTLOTest\Desktop\baseline`carpeta (su ruta de archivo se verá diferente).

![](https://i.imgur.com/RsOkipa.png)

Para generar los tres archivos, ejecute los siguientes comandos a continuación. Estos comandos enumeran una lista de servicios, tareas programadas y usuarios locales.

```plaintext
Get-Service | Select-Object -ExpandProperty Name | Out-File "baseline/services.txt"
Get-ScheduledTask | Select-Object -ExpandProperty TaskName | Out-File "baseline/scheduledtasks.txt"
Get-LocalUser | Select-Object -ExpandProperty Name | Out-File "baseline/localusers.txt"
```

![](https://i.imgur.com/rnbMWXR.png)

Mover en la carpeta de referencia y ejecutar los siguientes comandos a continuación para asegurarse de que los archivos fueron generados:

```plaintext
Get-ChildItem
```

![](https://i.imgur.com/SqsQEmk.png)

Después de su sesión de PowerShell, ejecute los mismos comandos, pero en lugar de guardar la salida al directorio de referencia, guarde la salida al directorio actual, como se muestra aquí:

```plaintext
Get-Service | Select-Object -ExpandProperty Name | Out-File services.txt
Get-ScheduledTask | Select-Object -ExpandProperty TaskName | Out-File scheduledtasks.txt
Get-LocalUser | Select-Object -ExpandProperty Name | Out-File localusers.txt
```

**Descargo de responsabilidad:** _He introducido malware en mi máquina de Windows. Esto ayudará a cambiar la máquina rápidamente. No voy a compartir esto para proteger su sistema. Pero, por favor, siga la siguiente metodología._

Muy bien, ejecutaré el script arriba para generar la salida recién generada para poder compararla con la información de referencia.

![](https://i.imgur.com/cp8qYuE.png)

### **Servicios Análisis diferencial**

Vamos a ver por primera vez la lista de servicios. Comienza inspeccionando las primeras diez líneas de la `service.txt`Archivo de uso `Get-Content`, como se muestra aquí:

![](https://i.imgur.com/rw3yL1B.png)

Como puede ver, el archivo service.txt es una lista de nombres de servicio. Ahora, repita este comando sin el argumento _-Primero_, guardando el contenido del archivo en una variable llamada $servicesnow, como se muestra aquí:

![](https://i.imgur.com/AH2M6mp.png)

Repita el comando Get-Content, esta vez leyendo desde el archivo _basal-services.txt_, guardando los datos en una variable llamada $servicesbasline, como se muestra aquí:

![](https://i.imgur.com/1sinWTV.png)

Con ambas variables que representan los servicios ahora y la línea de base del servicio, podemos compararlas para identificar las diferencias usando el comando Compare-Object. Ejecute el comando Compare-Object con dos variables, como se muestra aquí:

![](https://i.imgur.com/ebC1Nax.png)

En la salida Compare-Object, verá un nuevo servicio: **Dynamics**. No estoy seguro de lo que hace este servicio, pero vamos a tomar nota de esto y continuar con los otros archivos.

### **Usuarios locales Análisis diferencial**

Repitemos este proceso. Utilice análisis diferencial para evaluar a los usuarios locales del sistema. De su actual sesión de PowerShell, ejecute el `localusers.txt`archivo en una variable llamada `$usersnow`con `Get-Content`Como se muestra aquí.

![](https://i.imgur.com/dnyNLLK.png)

A continuación, lea el archivo de referencia _basal/localusers.txt_ file en una variable llamada $userbaseline con Get-Content, como se muestra aquí.

![](https://i.imgur.com/sHdS9Zf.png)

Usemos el Compare-Object para comparar las dos variables para ver si se creó una nueva cuenta de usuario:

![](https://i.imgur.com/tJkUCDn.png)

Curiosamente, el malware añadió una nueva _dinámica_ llamada de usuario _con_ derechos de administración también.

### **Tareas programadas Análisis diferencial**

Cubrimos esto en detalle en la última sección, pero un refresco rápido no dolió. Las tareas programadas son trabajos que se ejecutan en Windows en un momento específico y con una frecuencia específica. El disparador puede ser el tiempo de reloj (corre este trabajo todos los días al mediodía), o puede estar basado en otros atributos, incluyendo eventos observados en el registro de eventos de Windows.

Para ver una lista de tareas programadas, puede utilizar el PowerShell `Get-ScheduledTask`comando. Pero, quiero ver si el **malware** añadió una tarea programada mediante el uso de análisis diferencial. Al igual que lo hicimos con los últimos dos archivos.txt, vamos a colocarlos en variables.

Leer el `scheduledtasks.txt`archivo en una variable llamada `$scheduledtasksnow`. A continuación, lea la versión basal del archivo en una variable llamada `$scheduledtasksbaseline`, como se muestra aquí:

![](https://i.imgur.com/MtFvSDL.png)

A continuación, vamos a comparar las dos variables usando Compare-Object, como se muestra aquí:

![](https://i.imgur.com/wfPxhgD.png)

La tarea programada añadida se llama _Microsoft eDynamics_.

Desde aquí, puede sumergirse más en las tareas programadas y determinar si es un comportamiento esperado para el sistema. Pero por ahora, concluiremos sobre el análisis dinámico con PowerShell.

![](https://i.imgur.com/gf231UL.png)

![](https://i.imgur.com/jCjXIKP.png)

### **Conclusión**

Las amenazas se pueden identificar en un sistema mediante la evaluación del entorno (como la búsqueda de comportamientos extraños), pero obtendrá la mejor **fidelidad** para el análisis mediante análisis. Recuerde, a veces usted no siempre obtendrá las fuentes de datos para la línea de base, por lo que debe ser flexible en cómo evalúa un sistema para identificar amenazas.