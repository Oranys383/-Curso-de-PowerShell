# PowerShell para la celebración de archivos

## **Introducción**

Este laboratorio será nuestra primera introducción, ya que es bastante fácil. Procederemos a aumentar en dificultades después de esta sección. En esta lección, aprenderás a usar el `FileLastWriteTime.ps1`script para recuperar el último tiempo de escritura de los archivos en un directorio especificado.

## **Objetivos de aprendizaje:**

- Crear un script para reunir tiempos de escritura de archivos con PowerShell.
- Actualice el script para ordenar los archivos por su último tiempo de escritura.

## **Aguiinos**

### Paso 1: Creación de tu Guión

El script será aproximadamente 7-9 líneas de código. Al igual que con este proyecto (y otros), puede ampliar y adaptar el código a su gusto. Vamos a desglosar esto a continuación:

```plaintext
# Get-FileLastWriteTime.ps1

param (
    [string]$directory
)

Get-ChildItem -Path $directory -File | Select-Object FullName, LastWriteTime
```

### **Explicación**

1. **Parámetros:** El script acepta un parámetro, `directory`, que es el directorio para buscar.
2. **Get-ChildItem** : Recupera todos los archivos en el directorio especificado.
3. **Seleccionar Objeto** : Selecciona y sale el nombre completo y la última hora de escritura de cada archivo

### Paso 2: Corre tu guión

Después de haber escrito su script, por favor ejecutarlo contra un directorio que frecuenta a menudo.

![](https://i.imgur.com/oFe6MsB.png)

Como se dio cuenta, tengo otros guiones de proyectos futuros que cubriremos pronto. Elegí esto retroactivamente para capturar la diferencia en **LastWriteTime**. Este comando lista el último tiempo de escritura de todos los archivos en el directorio especificado. Pero, por qué es importante? Bueno, dije que era un proyecto básico, pero escúchame: Típicamente, el **último tiempo** se refiere cuando un usuario abre y guarda un archivo, independientemente de si algún dato se cambia o se añade al archivo. Usted puede potencialmente utilizar estos scripts en casos forenses digitales o comprobar su sistema para cualquier cambio.

### Paso 3: Modificar su guión (Opcional)

Modifiquemos el script para ordenar los archivos por última vez cambiando esta porción del código

```plaintext
Get-ChildItem -Path $directory -File | Sort-Object LastWriteTime | Select-Object FullName, LastWriteTime
```

![](https://i.imgur.com/33Vdeu4.png)

Mira, el **LastWriteTime** está en orden cronológico para cada uno de los archivos.

## **Resumen**

Este laboratorio corto muestra la potencia de PowerShell cuando se trata de recoger ciertos atributos de los archivos, especialmente a través de todo un directorio. Seguiremos construyendo sobre nuestras habilidades en futuros laboratorios. Manténganse atentos.