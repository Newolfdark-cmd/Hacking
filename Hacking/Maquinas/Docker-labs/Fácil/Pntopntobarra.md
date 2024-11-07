---
Maquina: library
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
  - LFI
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241107085059.png]]

Nos encontramos con un puerto 22 ssh y un puerto 80 http. Vamos a explorar primero el http a ver si nos encontramos algo interesante..

![[Pasted image 20241107085216.png]]

Parece que hay dos botones, si pulsamos el primero se desplegará el virus y la máquina se auto destruye y si pulsamos el segundo nos dice que no sabe donde está el archivo... Eslorando el código fuente me he topado con algo bastante interesante.



![[Pasted image 20241107085321.png]]

Si os fijáis cuando se pulsa el segundo botón se accede a una url donde se le pide directamente el archivo del sistema... Esto pinta a vulnerabilidad LFi de manual.

Vamos a probar a ver si tenemos suerte y nos da el /etc/hosts ->

![[Pasted image 20241107085414.png]]

Fijaos que como cambiando el valor de la imagen por un truquito para que nos devuelva el /etc/hosts y funciona a la perfección.

Como podemos observar nos encontramos el usuario con nombre "nico", como también tenemos el puerto de ssh abierto podemos intentar ver si podemos acceder a su clave id_rsa

![[Pasted image 20241107093139.png]]

Parece que si que se nos muestra, con lo que ya podemos iniciar sesión en el ssh mediante el id_rsa.

```bash
nano id_rsa #Poner el id_rsa con el formato que toca
chmod 600 id_rsa
ssh -i id_rsa nico@172.17.0.2
```

Con esto ya deberíamos poder iniciar sesión en la máquina como nico ->

![[Pasted image 20241107094049.png]]

Ahora tocaría pasar a la escalada de privs, para ello vamos a utilizar el clasico sudo -l ->

![[Pasted image 20241107094127.png]]

Como vemos tenemos permisos para env, vamos a gtfobins para ver que podemos realizar ->

```bash
sudo env /bin/sh
```

Y listo ya seríamos root.

![[Pasted image 20241107094230.png]]

Otra máquina para la colección ;).
