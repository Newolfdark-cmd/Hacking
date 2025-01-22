### Descripción

### Metodos:

##### Ping Sweeps:

**Poco** **Fiable**. Escaneo de red usada para descubrir hosts dentro de una red de IPs. La idea es mandar diferentes peticiones de ping y observar las respuestas  (windows puede bloquear por defecto esta técnica).

Comando básico para comprobar el estado de un host:
``ping ip 

Comando para mandar cierto número de paquetes:
``ping -c 5(paquetes) ip

Escanear todos los hosts de una red (no hay que fiarse mucho de ping):
``ping -b -c 1 10.10.25.0(red)

Escanear todos los hosts de una red con fping (no hay mucha dfierencia pero puede ayudar):
``fping -a -g 10.10.25.0(red) 2>/dev/null

#### Nmap:

Herramienta fudamental de seguridad que muchas de sus funcionalidades consiste en la detección de hosts.

Escaneo de hosts con nmap pero haciendo uso de ping:
``nmap -sn ip(o red)

Escaneo de hosts con nmap haciendo uso de ping y de un archivo con todas las ip (incluido rangos):
``nmap -sn -iL file

Escaneo mediante uso de paquetes synk que son mas precisos:
``nmap -sn -PS(puertos) target