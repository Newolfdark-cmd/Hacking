---
Maquina: dockerlabs
SO: Linux
Explotación: Análisis de imágenes y web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - web
  - file_upload
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sVC -Pn 172.17.0.2
```

![[Pasted image 20241105083851.png]]

Y si investigamos un poco la web nos encontramos lo siguiente ->

![[Pasted image 20241105092138.png]]

Si hacemos un análisis de los subdirectorios podemos ver los siguientes ->

```bash
gobuster dir -u http://172.17.0.2/ -w ~/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

![[Pasted image 20241105092212.png]]

El que me llama más la atención es el de machine.php que parece una plataforma como para poder subir un .zip

![[Pasted image 20241105092306.png]]

Digo lo de .zip porque si intentas subir un archivo con otro formato... pues no deja :c

Pero podemos probar con otros formatos de PHP a ver si no tienen todos, desde hacktricks tenemos los siguientes -> **PHP**: _.php_, _.php2_, _.php3_, ._php4_, ._php5_, ._php6_, ._php7_, .phps, ._phps_, ._pht_, ._phtm, .phtml_, ._pgif_, _.shtml, .htaccess, .phar, .inc, .hphp, .ctp, .module_

![[Pasted image 20241105193124.png]]

Como se puede apreciar en la imagen, parece que el .phar si que lo acepta, así que podemos subir una reverse shell.

![[Pasted image 20241105193223.png]]

Aquí tenemos nuestra reverse shell subida, con tan solo un click en el archivo ya nos manda petición de conexión a nuestro nc (he utilizado la reverse shell de pentestmonkey).

![[Pasted image 20241105193357.png]]

ya estamos dentro del servidor, pero no tenemos una sesión de tty, así que toca hacer un upgrade de shell.

![[Pasted image 20241105193525.png]]

Con la shell upgradeada nos disponemos a ver que comandos podemos ejecutar como root, parece que tenemos acceso a grep y cut. Con estos permisos podemos llegar a leer cualquier archivo

Vamos con un ejemplo ->

```bash
cat /etc/shadow
```

![[Pasted image 20241105193714.png]]

Nos da permission denied, pues no tenemos permisos.

```bash
LFILE=/etc/shadow
sudo cut -d "" -f1 "$LFILE"
```

![[Pasted image 20241105193825.png]]

Repetimos el proceso para /etc/passwd/

![[Pasted image 20241105194151.png]]



Con esto tenemos el hash de dbadmin -> "$y$j9T$uBRiDXLmynAwhL7ienFp.0$BtPUY8FedeKsMf3YkuItmPBy/8H/RFcpztS2wQHmMy3"

Dejo aquí también una tabla para saber el formato de cifrado de /etc/shadow ->

![[Pasted image 20241105194344.png]]

Pero bueno, esto va a ser más sencillo que hacer fuerta bruta a un yescript...

Si indagamos un poco en los subdirectorios podemos encontrar lo siguiente ->

![[Pasted image 20241105195619.png]]

Y como podemos leer todos los archivos...

```bash
LFILE=/root/clave.txt
sudo cut -d "" -f1 "$LFILE"
```

![[Pasted image 20241105200024.png]]

Ahora probamos a utilizar la contraseña yyyyy

![[Pasted image 20241105200104.png]]