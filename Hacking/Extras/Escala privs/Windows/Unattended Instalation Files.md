
Proceso de instalación automática de windows para grandes despliegues, este proceso incluye una serie de usuarios y contraseñas dentro de sus archivos de configuración.

Los dicheros que suelen incluir estos usuarios son (es posible que estén en base64):

- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Autounattend.xml

Desde PowerUp podemos observar estos ficheros con el comando:

```powershell
powershell -ep bypass
. .\PowerUp.ps1
Invoke-PivescAudit
```

![[Pasted image 20250214093816.png]]

Una vez sabemos que existen estos ficheros podemos ver su contenido.

![[Pasted image 20250214094025.png]]

Se puede observar como la contraseña está cifrada en base64.

![[Pasted image 20250214094101.png]]
