
Windows Remote Management (WinRM) es una herramienta que nos permite usar conexiones con Windows mediante protocolos http/https. Suele utilizar los peurtos 5985 y 5986.

WinRM ofrece varios mecanismos de autenticación:

- Mediante hash NTLM: Utilizado en espacios de trabajo o cuando no este disponible Kerberos y es menos seguro que este último.
- Autenticación básica: Se utiliza mediante contraseña y usuario. Es un método aún menos seguro que el hash NTLM y suele estar deshabilitado por defecto.

Herramientas para interactuar con WinRM:

- Evil-WinRM: Herramienta de código abierto utilizada por penetration testers para exploatación, traspaso de archivos y conseguir información.
- CrackMapExec: Más limitada para interactuar con WInRM pero tambien es capaz de utilizar algún módulo
- PowerShell Remoting: Es una capacidad que tiene PowerShell para poder ejecutar comandos de forma remota.

Que herramienta escoger:

- Para casos administrativos: PowerShell Remoting
- Para casos de pentest:
	- Usar Evil-WinRM para casos en los que haya que enfocarse en este servicio.
	- Usar CrackMapExec si se quiere conseguir asegurar las creds de usuarios o reconocimiento de red.

Ejemplo de Uso:

Ip víctima -> 10.4.24.110

Si hacemos un scan de nmap en el host podemos observar que tiene el puerto de WinRM abierto.

![[Pasted image 20250303085626.png]]

Como hemos comentado antes CrackMapExec nos puede ayudar a asegurar las creds de los usuarios. Vamos a probar que la contraseña que nos han otorgado sería la necesaria para el usuario "administrator".

![[Pasted image 20250303085847.png]]

Incluso podemos llegar a ejecutar comandos.

![[Pasted image 20250303085929.png]]

Vamos a probar con Evil-WInRM.

![[Pasted image 20250303090023.png]]

Evil-WinRM nos da una shell completamente interactiva, lo que lo hace la opción más cómoda.

También podemos llegar a cargar scripts desde nuestro host.

![[Pasted image 20250303090219.png]]

De esta manera podemos llegar a cargar el Mimikatz y como funciona todo en memoria no se modifican ni añaden archivos físicos al host víctima.

Incluso podemos hacer un dump de las credenciales.

![[Pasted image 20250303090350.png]]

```powershell
Invoke-Mimikatz -Command 'sekurlsa::logonpasswords'
```

![[Pasted image 20250303090407.png]]

 