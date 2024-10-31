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

