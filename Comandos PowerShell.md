# Hoja de charco de PowerShell

1. Cómo funciona PowerShell
2. Recursos de PowerShell
3. Hoja de charco de PowerShell

# Guía de referencia de PowerShell

_**PowerShell para las tareas post-expansión**_

**Realice un barrido de ping de red:**

```plaintext
PS C:\\> 1..255 | % {echo "192.168.1.$_"; ping -n 1 -w 100 192.168.1.$_ | Select-String ttl}
```

**Ejecute un escaneo de babor:**

```plaintext
PS C:\\> 1..1024 | % {echo ((new-object Net.Sockets.TcpClient).Connect("192.168.1.1",$_)) "Port $_ is open!"} 2>$null
```

**Descargar un archivo a través de HTTP:**

```plaintext
PS C:\\> (New-Object System.Net.WebClient).DownloadFile("http://192.168.1.1/file.exe", "file.exe")
```

**Buscar archivos por nombre:**

```plaintext
PS C:\\> Get-ChildItem "C:\Users\" -recurse -include *credentials*.txt
```

**Lista de actualizaciones instaladas:**

```plaintext
PS C:\\> Get-HotFix
```

**Access registro Windows:**

```plaintext
PS C:\\> cd HKLM:\
PS HKLM:\> ls
```

**Lista de programas de inicio del registro:**

```plaintext
PS C:\\> Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\run
```

**Recodificar una cadena a Base64:**

```plaintext
PS C:\\> [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("Hello PowerShell!"))
```

**Administrar las reglas de firewall de Windows:**

```plaintext
PS C:\\> Get-NetFirewallRule –all
PS C:\\> New-NetFirewallRule -Action Allow -DisplayName AllowAccess -RemoteAddress 192.168.1.25
```

_**Cmdlets de uso frecuente (y atajos)**_

**Artículos de directorio de la lista (ls, dir, gci):**

```plaintext
PS C:\\> Get-ChildItem
```

**Copie un archivo (cp, copia, cpi):**

```plaintext
PS C:\\> Copy-Item src.txt dst.txt
```

**Mover un archivo (mv, move, mi):**

```plaintext
PS C:\\> Move-Item src.txt dst.txt
```

**Búsqueda de texto en un archivo:**

```plaintext
PS C:\\> Select-String –path c:\users\*.txt –pattern secret
PS C:\\> ls -r c:\users -file | % {Select-String -path $_ -pattern secret}
```

**Visualización del contenido del archivo (gato, tipo, gc):**

```plaintext
PS C:\\> Get-Content file.txt
```

**Mostrar directorio actual (pwd, gl):**

```plaintext
PS C:\\> Get-Location
```

**Lista de procesos de ejecución (ps, GPS):**

```plaintext
PS C:\\> Get-Process
```

**Servicios de la lista:**

```plaintext
PS C:\\> Get-Service
```

**Salida de comando de formato (Formato-Lista):**

```plaintext
PS C:\\> ls | Format-List –property name
```

**Salida de paginato:**

```plaintext
PS C:\\> ls –r | Out-Host -paging
```

**Obtener SHA1 hash de un archivo:**

```plaintext
PS C:\\> Get-FileHash -Algorithm SHA1 file.txt
```

**Salida de comandos de exportación a CSV:**

```plaintext
PS C:\\> Get-Process | Export-Csv processes.csv
```

_**Descubriendo Cmdlets**_

**Lista de los cmdlets:**

```plaintext
PS C:\\> Get-Command
```

**Filtra cmdlets por verbo:**

```plaintext
PS C:\\> Get-Command Set*
PS C:\\> Get-Command –Verb Set
```

**Filtra cmdlets por sustantivo:**

```plaintext
PS C:\\> Get-Command *Process
PS C:\\> Get-Command –Noun process
```

_**Obtención de asistencia**_

**Ayuda básica:**

```plaintext
PS C:\\> Get-Help
```

**Documentación de Cmdlet:**

```plaintext
PS C:\\> Get-Help <cmdlet>
```

**Ayuda detallada:**

```plaintext
PS C:\\> Get-Help <cmdlet> -detailed
```

**Ejemplos:**

```plaintext
PS C:\\> Get-Help <cmdlet> -examples
```

**Ayuda completa:**

```plaintext
PS C:\\> Get-Help <cmdlet> -full
```

**Ayuda en línea (si está disponible):**

```plaintext
PS C:\\> Get-Help <cmdlet> -online
```

_**Atajos de Cmdlet**_

**Enumere todos los alias:**

```plaintext
PS C:\\> Get-Alias
```

**Expandir un alias a su mando completo:**

```plaintext
PS C:\\> alias <unknown alias>
PS C:\\> alias gcm
```

_**Mejora de la eficiencia en PowerShell**_

**Comandos autocompletos:**

```plaintext
PS C:\\> get-child<TAB>
PS C:\\> Get-ChildItem
```

**Acortar parámetros:**

```plaintext
PS C:\\> ls –recurse
PS C:\\> ls -r
```

_**Conceptos esenciales de PowerShell**_

**Ayuda y ejemplos:**

```plaintext
PS C:\\> Get-Help [cmdlet] -examples
PS C:\\> help [cmdlet] -examples
```

**Lista de comandos:**

```plaintext
PS C:\\> Get-Command
PS C:\\> gcm *[string]*
```

**Propiedades y métodos:**

```plaintext
PS C:\\> Get-Member
PS C:\\> [cmdlet] | gm
```

**Manejo de artículos de tubería:**

```plaintext
PS C:\\> ForEach-Object { $_ }
PS C:\\> [cmdlet] | % { [cmdlet] $_ }
```

**Búsqueda en cadena:**

```plaintext
PS C:\\> Select-String
PS C:\\> sls –path [file] –pattern [string]
```

_**Pipelining, bucles y variables**_

**Salida de tuberías cmdlet:**

```plaintext
PS C:\\> Get-Process | Format-List –property name
```

**Bucle a través de elementos en el oleoducto:**

```plaintext
PS C:\\> ls *.txt | ForEach-Object {cat $_}
```

**Filtro con el objeto de dónde:**

```plaintext
PS C:\\> Get-Process | Where-Object {$_.name –eq "notepad"}
```

**Generar rangos de números y bucle:**

```plaintext
PS C:\\> 1..10
PS C:\\> 1..10 | % {echo "Hello!"}
```

**Crear y lista variables:**

```plaintext
PS C:\\> $number = 42
PS C:\\> ls variable:
```

**Equipo ejemplo:**

```plaintext
PS C:\\> dir | group
```