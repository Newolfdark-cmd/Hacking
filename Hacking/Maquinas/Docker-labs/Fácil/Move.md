---
Maquina: Move
SO: Linux
Explotación: LFI
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
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241127144250.png]]

Primero que nada vemos que tiene un FTP abierto así que no nos cuesta nada preguntar si tiene el usuario anónimo activado ->

![[Pasted image 20241127145908.png]]

Parece que hay algo,y si nos adentramos en el directorio podemos observar que hay una base de datos,

![[Pasted image 20241127150553.png]]

Por otro lado si vemos el puerto 3000 está abierto y contiene un servicio de Grafana, con su panel de Login. Si probamos las credenciales por defecto admin/admin podemos llegar a acceder.

![[Pasted image 20241127150644.png]]

Si nos fijamos estamos en la versión 8.3

![[Pasted image 20241127152442.png]]

Y haciendo una búsqueda rápida podemos ver como hay algún exploit para esta versión.

![[Pasted image 20241127152520.png]]

Para poder utilizar el script basta con copiarlo a nuestra dirección y configurarlo, yo al final he utlizado uno en python.

![[Pasted image 20241127153513.png]]

Como podemos observar hay un usuario freddy que tiene acceso al sistema... También podemos observar una especie de contraseña...

![[Pasted image 20241127154948.png]]

```bash
hydra -l freddy -P ~/wordlists/rockyou.txt ssh://172.17.0.2 -t 64 -s 22
```

![[Pasted image 20241127155106.png]]

Parece que estamos dentro... Vamos a ver que permisos tenemos.

![[Pasted image 20241127155446.png]]

Parece que tenemos permisos de root para ejecutar con python el archivo /opt/maintenance.py

Si investigamos un poco más podemos ver su contenido y como somos propietarios del archivo con lo que lo podemos modificar.

![[Pasted image 20241127155534.png]]

Lo vamos a cambiar por lo siguiente...

```python
import os
import pty

def spawn_shell():
    # Spawnea una shell interactiva
    try:
        pty.spawn("/bin/bash")  # Cambia a "/bin/sh" si prefieres una shell diferente
    except Exception as e:
        print(f"Error al intentar spawnear la shell: {e}")

if __name__ == "__main__":
    spawn_shell()

```

Y listo, ya lo podemos ejecutar como sudo.

![[Pasted image 20241127155630.png]]

Ya tenemos el control de la máquina ;)