---
Maquina: buscalove
SO: Linux
Explotación: Análisis web (LFI) Y Fuerza Bruta
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - ssh
  - LFI
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sS -Pn 172.17.0.2
```

![[Pasted image 20241025094131.png]]

Tras pasar un gobuster para analizar los subdirectorios nos encontramos lo siguiente ->

```bash
gobuster dir -u http://172.18.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

![[Pasted image 20241025101558.png]]

Lamentablemente, si analizamos los subdirectorios de wordpress no encontramos nada del otro mundo... pero si que he encontrado un comentario un tanto sospechoso  -> 

![[Pasted image 20241025101704.png]]

```html
<!-- El desarollo de esta web esta en fase verde muy verde te dejo aqui la ventana abierta con mucho love para los curiosos que gustan de leer -->
```

Tras investigar arduamente la versión del apache, los subdirectorios con gobuster y mirando a ver si había algún otro mensaje oculto... Me temo que no hay nada más. Así que vamos a intentar un ataque por LFI.

```bash
wfuzz -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -u http://172.18.0.2/wordpress/index.php?FUZZ=../../../../../etc/passwd --hc 404 --hl 40
```

Este comando está probando varias palabras del diccionario para identificar si alguna permite la inclusión de archivos en el servidor objetivo, y específicamente intenta acceder a `/etc/passwd`.

![[Pasted image 20241025134139.png]]

Para que funcionó con la palabra love... Curioso.

![[Pasted image 20241025134111.png]]

Ahora ya hemos podido observar lo que contiene el /etc/passwd y gracias a eso sabemos que en el sistema hay dos usuarios posibles Pedro y Rosa.

Pues como tenemos un SSH y nombres de usuario... Que tocará? pues fuerza bruta amigos ;)

```bash
hydra -l rosa -P /home/alejandro/wordlists/rockyou.txt ssh://172.18.0.2
```

Y resuelta queeeeeeeeeeee

![[Pasted image 20241025135149.png]]

Con que la passwd de rosa es "lovebug" pues bueno saberlo para que entremos en el SSH.

![[Pasted image 20241025135243.png]]

Y ya estamos dentro, tocaría ver si rosa tiene algún que otro permiso con el clasico sudo -l.

![[Pasted image 20241025135429.png]]

Parece que Rosa es el ojo que todo lo ve, pero con esto no se me ocurre como escalar privs. Eso si, lo podemos ver todo...

![[Pasted image 20241025135530.png]]

Parece que root tiene un pequeño secretito... Vamos a leerlo a ver que pone.

![[Pasted image 20241025135604.png]]

Pero esto que ehhhh "4E 5A 58 57 43 59 33 46 4F 4A 32 47 43 34 54 42 4F 4E 58 58 47 32 49 4B" parece que están jugando al hundir la flota. Eso o es un código hexadecimal -> "NZXWCY3FOJ2GC4TBONXXG2IK" Que a su vez es un código en base32 -> "noacertarasosi"

Como hay otro usuario me hace sospechar que será su passwd. Vamos a probar a accede por ssh a Pedro y ver que tal... Y parece que funciona! Ahora a ver los permisos.

![[Pasted image 20241025140927.png]]

Oooooooo parece que tiene permisos para ejecutar env como root... Bueno pues gracias supongo ;)

![[Pasted image 20241025141014.png]]





