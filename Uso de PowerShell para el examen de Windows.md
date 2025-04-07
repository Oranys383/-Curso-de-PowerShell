# PowerShell para el examen de Windows

### **Introducción**

En este laboratorio final, vamos a aprender a aplicar diferentes comandos de PowerShell para revisar un sistema de Windows para que se den los signos de un posible compromiso. Dada la naturaleza maliciosa del laboratorio, no podré compartir la VM, pero espero que puedas estimular un entorno similar o seguir a lo largo de otra máquina.

**Descargo de responsabilidad:** Windows Defender está apagado ya que estamos tratando con archivos de malware que han comprometido al host. Si planeas replicar este laboratorio, hazlo en una máquina sucia o VM en la nube.

### **Requisitos**

Estoy usando una VM de Windows tradicional a través de Amazon Web Services (AWS).

Tipo de Instancia: **Agrandés x86-64 2vCPUs - 8GB de RAM**

Sistema operativo: **Windows**

Espacio disco para discocerticos: **50 GB**

![](https://i.imgur.com/v446nCF.png)

### **Aguiinos**

Abra PowerShell como Administrador. La diferencia entre dirigir PowerShell como administrador y dirigirla sin privilegios administrativos radica en el nivel de acceso y control sobre los recursos del sistema. Las sesiones administrativas de PowerShell ofrecen privilegios elevados y un acceso más amplio a la configuración del sistema, mientras que las sesiones no administrativas son limitadas en sus capacidades y se limitan a operaciones específicas para el usuario.

![](https://i.imgur.com/Gkvfvhe.png)

Se le presentará un aviso de **Control** de **Cuenta de Usuario** **(UAC**). Adelante, introduzca su contraseña respectiva para proceder.

![](https://i.imgur.com/q5v1mcp.png)

Usando PowerShell, vamos a realizar una investigación en vivo del sistema para identificar, caracterizar y eliminar el malware usando PowerShell.

Vamos a ejecutar el comando [**Obtener Procesos**](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process?view=powershell-7.4). Esto proporcionará los procesos en una computadora local.

![](https://i.imgur.com/TSRk7xs.png)

Como puedes ver, el Get-Process nos proporciona varias columnas. Ahora, no tienes que ser un experto, pero este es un buen conocimiento para poner en tus notas:

1. **Mangos:** El número de manijas que el proceso ha abierto. Las manijas son referencias a recursos del sistema operativo como archivos, claves de registro o conexiones de red.
2. **NPM(K**): Memoria nopagada (en kilobytes) asignada al proceso. La memoria no pagada se refiere a la memoria que no se puede mover al archivo de página en el disco.
3. **PM(K**): Memoria de la página (en kilobytes) asignada al proceso. La memoria de Paged se puede mover al archivo de página en el disco cuando no está en uso.
4. **WS(K**): Conjunto de trabajo (en kilobytes) del proceso. El conjunto de trabajo es el conjunto de páginas de memoria que actualmente residen en memoria física y se asignan al proceso.
5. **VM(M**): Tamaño de la memoria virtual (en megabytes) del proceso. Esto representa la cantidad total de memoria virtual asignada al proceso.
6. **CPU(s**): La cantidad de tiempo de CPU (en segundos) que el proceso ha utilizado.
7. **Id:** El identificador único del proceso.
8. **ProcessName:** El nombre del proceso.

Me voy a centrar en el ProcessName. Con el mando anterior, voy a ejecutarlo contra un solo proceso.

![](https://i.imgur.com/y5g50vv.png)

Como puedes ver, si ejecutas el comando contra un solo proceso, solo obtendrás información al respecto. Puede obtener más información del proceso añadiendo un **carácter de tubería,** a continuación, el Select-Object -Property. Este comando proviene de la [**Seleje**](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.4) cmdlet, que puede permitirle seleccionar objetos únicos o un número especificado de objetos

Vamos a ejecutar el comando de abajo:

![](https://i.imgur.com/SKGdmoR.png)

Como puede ver, la salida es MUY detallada. Tuve que redactarlo para verlos. Lo que pasa con este comando es que es extremadamente difícil de analizar. Creo que concluiremos sobre este proceso específico. Vamos a ejecutarlo contra todo el proceso en la máquina con propiedades específicas.

![](https://i.imgur.com/Yvjmbvq.png)

Como puedes ver, sólo aparecen tres columnas: **Camino, Nombre, Id**. Estos son los especificados en el comando después del argumento **de la -Propiedad**. Vamos a jugar con otro comando: **Dónde se opone**. Este comando nos permite seleccionar objetos de una colección basada en sus valores de propiedad. Voy a ejecutar este comando para recuperar todos los procesos que comienzan con el nombre _nginx_.

![](https://i.imgur.com/Mr621nK.png)

Como pueden ver aquí, hemos encontrado dos procesos con ese nombre. Intente ejecutar este comando Dónde-Object comando para coincidir con cualquier ruta donde la palabra _temp_ está presente, incluyendo un comodín.

![](https://i.imgur.com/zsYC59C.png)

Como usted puede ver aquí: tenemos un archivo llamado **calcache.exe** ubicado en el . _Temp_ folder.igute sospechoso.

Vamos a ejecutar el comando **Get-FileHash** para recibir el hash SHA256 del archivo **calcache.exe**

![](https://i.imgur.com/pClHxlH.png)

Subamos el hash a VirusTotal

![](https://i.imgur.com/PeCX2Ug.png)

Como puede ver, este archivo es bastante malicioso, según los resultados VirusTotal. Esto concluirá nuestra parte de este examen. Desde aquí, puede sumergirse en otros aspectos como comprobar el registro, eliminar el malware y mucho más.

**Contenido de bonificación: BTLO - Mitsu**

![](https://i.imgur.com/tGZOlRz.png)

![](https://i.imgur.com/CytQrQQ.png)

Te han llamado para investigar la actividad sospechosa en una computadora. Debido a un desafortunado accidente que involucra miel, la única herramienta que puede usar es PowerShell. Usted apunta a identificar y verificar programas potencialmente maliciosos y examinarlos usando comandos de PowerShell.  
  
Este será un laboratorio **PAID** en nuestra plataforma [BTLO](https://blueteamlabs.online/home/investigations). Utilizando las lecciones clave en este curso, este laboratorio pondrá a prueba su habilidad en _el uso de_ _PowerShell para el examen de Windows_.  
  
