---
Maquina: Los40ladrones
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
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sSV -Pn 172.17.0.2
```

![[Pasted image 20241126201259.png]]

Con el puerto 80 y un servicio de Apache running vamos a ver si nos cuenta algo interesante en el navegador web.

![[Pasted image 20241126201341.png]]

En un principio nada del otro mundo, la simple página de inicio de Apache, vamos a investigar mas...

![[Pasted image 20241126202051.png]]

Parece que hemos encontrado un archivo un poco curioso dentro del Apache...

![[Pasted image 20241126201629.png]]

Vamos a intentar explorar un poco esos número como si fueran puertos...

```bash
nmap -p 7000,8000,9000 -sU 172.17.0.2
```

![[Pasted image 20241126202556.png]]

Parece que algo si que hay... Para resolver esta máquina hay que hacer una cosa muy específica y para ello necesitamos la herramienta knock

```bash
sudo apt update && sudo apt install -y knockd

yyyy

knock -v 172.17.0.2 7000 8000 9000
```

![[Pasted image 20241126203530.png]]

Una vez hemos llamado a todos los puertos podemos volver a realizar un escaneo de puertos...

![[Pasted image 20241126203601.png]]

Sorpresa, parace que ahora si que tenemos un SSH... y bueno, tenemos en el mensaje anterior un tal "toctoc" así que vamos a intentar acceder al SSH con ese nombre.

```bash
hydra -l a -P ~/wordlists/rockyou.txt ssh://172.17.0.2 -f
```

![[Pasted image 20241126204709.png]]

Tenemos el usuario toctoc con pass kittycat, pues vamos allá

![[Pasted image 20241126204853.png]]

Estos son los permisos que tenemos... pues vamos a ejecutar el bash a ver que pasa.

![[Pasted image 20241126205001.png]]

Supongo que el ahora no está es un guiño al toctoc, máquina resuelta ;)

