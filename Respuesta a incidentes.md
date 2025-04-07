# PowerShell para la respuesta a incidentes

1. Cómo funciona PowerShell
2. Trabajando con PowerShell (Security)
3. PowerShell para la respuesta a incidentes

# Incidentes Respuesta w/ Pwsh

### **Introducción**

PowerShell puede proporcionar a los profesionales de la ciberseguridad la capacidad de recopilar información crítica durante la respuesta de incidentes. No es posible enumerar todos los puntos de datos significativos, pero la siguiente sección a continuación debe guiarle a buscar **artefactos clave**.

**Información del Sistema de recogida de la utilización** `**Get-CimInstance**` **y** `**Get-WmiObject**`**:**

Las computadoras almacenan muchos datos. Veamos dos comandos que pueden ayudarnos a analizar eficientemente:

- **Get-CimInstance:** Short for Common Information Model, es un estándar de código abierto orientado a objetos para recuperar, mostrar y administrar información informática, de red, de aplicación y servicio.
- **Get-WmiObject:** Short for Windows Management Instrumentation, es la implementación de Microsoft de CIM. Esto también ayuda a reunir información, pero se ha desprecated en PowerShell 3.0. Todavía funciona, pero no es tan rápido y eficiente como Get-CimInstance.

Lets hacer un ejercicio práctico rápido con ambos comandos a continuación:

```plaintext
Get-CimInstance -ClassName Win32_OperatingSystem
Get-WmiObject -Class Win32_OperatingSystem
```

![](https://i.imgur.com/JsyETM9.png)

Como puede ver los comandos WMI y CIM ofrecen la misma salida. Las diferencias están principalmente entre bastidores. Si estás interesado en los detalles minúsculos, mira este [blog de Progress](https://www.progress.com/blogs/get-ciminstance-vs-get-wmiobject-whats-the-difference).

**Identificar actualmente Usuarios con** `**Get-CimInstance -ClassName Win32_ComputerSystem**`**:**

Durante un incidente en vivo, usted puede encontrar cuentas locales (o nuevas cuentas) en una máquina comprometida. La información sobre ese aspecto puede ayudar a los investigadores a sacar mejores conclusiones sobre quién hizo qué. Afortunadamente, hay un comando CIM que nos ayuda a comprobar. Para recuperar la información **del** usuario **de la sesión**, ejecute el siguiente comando:

```plaintext
Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object Name, UserName
```

![](https://i.imgur.com/gIl2tax.png)

_Descargo de responsabilidad: Usted es libre de personalizar los comandos en estas lecciones a su gusto_

**Reunir procesos de ejecución usando** `**Get-Process**` **y comprensión de los detalles del proceso con** `**Get-CimInstance -ClassName Win32_Process**`**:**

Durante una investigación, es posible que tenga que realizar un **Análisis en Vivo**. Los procesos de funcionamiento son bastante volátiles, normalmente almacenados en RAM o caché. Afortunadamente, hay un comando PowerShell que le permite sacar esta información al terminal (y puede redirigir esto a un archivo de texto para **el análisis posterior**). En primer lugar, vamos a listar los procesos de ejecución usando el comando a continuación:

```plaintext
Get-Process
```

![](https://i.imgur.com/igKAbv5.png)

_La imagen ha sido redactada para mayor claridad_

Ahora, vamos a obtener información detallada del proceso:

```plaintext
Get-CimInstance -ClassName Win32_Process | Select-Object Name, ProcessId, ParentProcessId, CommandLine
```

![](https://i.imgur.com/pNMvkyx.png)

_La imagen ha sido redactada para mayor claridad_

Esta visión detallada es bastante verbosa. Por ejemplo, actualmente tengo **notepad.exe** en ejecución. Pero, mira la sección _CommandLine:_ te mostrará los archivos que utilizan ese programa. No lo vimos con el comando Get-Process. Mostrándole la diferencia.

![](https://i.imgur.com/j4KS6x8.png)

**Comprobación de la actividad de la red** `**Get-NetTCPConnection**` **y** `**Get-NetUDPEndpoint**`**:**

Otro factor importante en la respuesta a los incidentes es la actividad de la red. Mediante el seguimiento del tráfico de la red, puede identificar y mitigar rápidamente posibles brechas de seguridad o ciberataques. Es importante minimizar el daño y reducir el tiempo de inactividad con este enfoque proactivo. Vigilemos las conexiones de red con el comando de abajo:

```plaintext
Get-NetTCPConnection
Get-NetUDPEndpoint
```

![](https://i.imgur.com/PqE9hfZ.png)

_La imagen ha sido redactada para mayor claridad_

En esta salida, vemos varias columnas de información:

- **LocalDirección** : La dirección IP que aceptará conexiones para este oyente o se utiliza para la conexión de salida
- **LocalPort:** El número de puerto local utilizado para la conexión; si el estado es `Listen`, entonces este es el número de puerto TCP escuchado
- **Estado:** El estado de la conexión de red
- **OwningProcess** : El ID de proceso del proceso que hizo la conexión o está escuchando conexiones

![](https://i.imgur.com/xPNC0QL.png)

**Examen de las tareas programadas con** `**Get-ScheduledTask**` **y** `**Export-ScheduledTask**`**:**

Los hackers pueden ganar persistencia a través de muchos métodos: uno es las tareas de la Lista. Esta es una utilidad integrada de Windows que permite a los usuarios automatizar tareas. scripts y programas en momentos o eventos específicos. Por lo tanto, cuál es la línea entre las tareas programadas y un hacker manteniendo la persistencia? Mediante tareas de programación, el hacker asegura que su malware, muy probablemente, funciona a intervalos predefinidos, manteniendo una presencia constante en el sistema comprometido a pesar de reinicios.

Para enumerar las tareas programadas en la máquina, ejecute el comando a continuación:

```plaintext
Get-ScheduledTask
```

![](https://i.imgur.com/YXNoPgv.png)

_La imagen ha sido redactada para mayor claridad_

### **Conclusión**

Pasamos por muchos casos de uso con los que PowerShell puede ayudar, ya sea coleccionando artefactos, monitoreando conexiones de red o viendo procesos de ejecución. PowerShell puede ser utilizado por Blue Teamers, Incident Responders y Administradores de Sistemas para cazar la persistencia implantada en máquinas Windows.
