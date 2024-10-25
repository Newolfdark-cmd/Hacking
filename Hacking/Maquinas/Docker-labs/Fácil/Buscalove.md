---
Maquina: buscalove
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
nmap -p- --open --min-rate 5000 -sS -Pn 172.17.0.2
```

![[Pasted image 20241025094131.png]]

Tras pasar un gobuster para analizar los subdirectorios nos encontramos lo siguiente ->

```bash
gobuster dir -u http://172.18.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

![[Pasted image 20241025101558.png]]

Lamentablemente, si analizamos los subdirectorios de wordpress no encontramos nada del otro mundo... pero si que he encontrado un comentario un tanto sospechoso  -> 

![[Pasted image 20241025101704.png]]

```html
<!-- El desarollo de esta web esta en fase verde muy verde te dejo aqui la ventana abierta con mucho love para los curiosos que gustan de leer -->
```


