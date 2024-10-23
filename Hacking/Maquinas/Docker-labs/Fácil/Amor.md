---
Maquina: amor
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

![[Pasted image 20241023192623.png]]

Apreciamos que hay una página web y un servicio de SSH, vamos primero a explorar el portal web a ver que nos ofrece.

![[Pasted image 20241023193103.png]]

Ya de primeras en uno de los mensajes apreciamos el nombre de "juan" destacado como que fue despedido (ya tenemos un posible nombre de usuario) y lo firma una tal "carlota" (otro posible vector para fuerza bruta).

Por si acaso le vamos a pasar un gobuster, a ver si detecta algo

```bash 
gobuster dir -u http://172.17.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

Por desgracia la información que nos da es muy escasa, tan solo un recurso al que no podemos acceder.

![[Pasted image 20241023193317.png]]

Toca pasar a la fuerza bruta...

```bash 
hydra -l carlota -P /home/alejandro/wordlists/rockyou.txt ssh://172.17.0.2
```

Y sorpresita...

![[Pasted image 20241023193936.png]]

y ¿podremos entrar?

![[Pasted image 20241023194042.png]]

Ya lo creo que si.

Muy bien ahora vamos a ver si podemos conseguir un vector de escala de privs. Primero vamos a lo básico, que permisos sudo tenemos?

```bash 
sudo -l
```

![[Pasted image 20241023194144.png]]

No iba a ser tan sencillo... Pero si que vamos a hacer un pequeño upgrade de shell, porque esta es horrible

```bash 
SHELL=/bin/bash script -q /dev/null
```

