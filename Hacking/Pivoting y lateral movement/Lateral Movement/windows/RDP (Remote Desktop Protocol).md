
Herramienta muy famosa utilizada para conectarse a otro dispositivo windows de la red de forma remota. Tiene la particularidad de que funciona también con interfaz gráfica.


Ejemplo:

Tenemos dos máquinas victima: 10.4.26.240 y 10.4.19.71

Si relizamos un nmap podemos observar que uno de los hosts tiene un vulenrabilidad muy conocida. Está ejecutando un BadBlue.

![[Pasted image 20250228092535.png]]

Nota: Para este tipo de vulnerabilidad utilizar el módulo windows/http/badblue_passthru de metasploit.

![[Pasted image 20250228093027.png]]

Como se puede apreciar el módulo de metasploit nos consigue una sesión de meterpreter dentro de la víctima.

Vamos a intentar buscar credenciales dentro del sistema.

![[Pasted image 20250228093425.png]]

En un principio tanto el archivo Production-Server.rdg como el RDCMan parece sospechosos de que tienen que ver con la configuración de un RDP.

![[Pasted image 20250228094006.png]]

Si nos fijamos podemos observar una especia de contraseña pero parece cifrada.

Para poder descifrar esta contraseña vamos a subir la herramienta SharpDPAPI a la máquina victima e intentar ejecutarla.

![[Pasted image 20250228095546.png]]

![[Pasted image 20250228095916.png]]

Parece que ha encontrado el archivo que queríamos descifrar pero nos dice que la contraseña requiere de una contraseña maestra.

Vamos a intentar otra cosa, desde meterpreter vamos a cargar el módulo de kiwi.

![[Pasted image 20250228100026.png]]

```bash
kiwi_cmd sekurlsa::dpapi
```

Y parece que kiwi nos puede conseguir la master key

![[Pasted image 20250228100231.png]]


Importante guardarnos los valores del GUID y del sha1(key).

![[Pasted image 20250228100409.png]]

![[Pasted image 20250228100604.png]]

Ahora vamos a intentar pasar la herramienta del SharpDPAPI pero dándole el parámetro de lo que hemos anotado anteriormente.

![[Pasted image 20250228100747.png]]

Parece que ahora si que ha podido descifrar el passwd.

Pues ya podemos intentar conectarnos mediante el RDP al otro server, a ver si son sus credenciales.

Para hacerlo utilizamos el comando:

```bash
xfreerdp /u:administrator /p:harry_123321 /v:10.4.19.71
```

![[Pasted image 20250228100919.png]]

Y vemos que hay conexión al sistema víctima.

