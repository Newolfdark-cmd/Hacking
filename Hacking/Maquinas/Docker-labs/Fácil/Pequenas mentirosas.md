---
Maquina: pequenas mentirosas
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
  - mysql
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sSV -Pn 172.17.0.2
```

![[Pasted image 20241121085037.png]]

Lo primero es lo primero, vamos a hacer un análisis web.

![[Pasted image 20241121085148.png]]

Super sospechosa esa A tan descarada jajaja. Tiene toda la pinta a ser un nombre de usuario.

Tras analizar mas en profundidad la web no logro ver nada... Pero con el nombre de usuario... podríamos intentar hacer fuerza bruta al ssh ->

```bash
hydra -l a -P ~/wordlists/rockyou.txt ssh://172.17.0.2 -f
```

![[Pasted image 20241121093752.png]]

Ya tenemos el usuario a con passwd secret.

![[Pasted image 20241121093818.png]]

Tras hacer una pequeña comprobación vemos que también está el usuario spencer... Vamos a ver nuestros permisos y nada ;) no tenemos permisos para sudo.

Al parecer un dato que no sabía es que los archivos asociados con servidores se almacenan en /srv/ y si accdemospodemos observar lo siguiente ->

![[Pasted image 20241121100604.png]]

Si leemos uno de los archivos nos podemos encontrar con lo siguiente ->

![[Pasted image 20241121101511.png]]

Y si probamos fuerza bruta obtenemos lo siguiente ->

![[Pasted image 20241121101605.png]]

Así que ya sabemos que el usuario spencer usa la pass password1.

![[Pasted image 20241121101649.png]]

Una vez dentro podemos ver que permisos tiene y ver si en gtfobins hay algun comando interesante...

```bash
sudo python3 -c 'import os; os.system("/bin/sh")'
```

![[Pasted image 20241121101744.png]]

Y ya seríamos root ;)