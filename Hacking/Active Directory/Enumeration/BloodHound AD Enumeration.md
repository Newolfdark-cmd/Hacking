
Herramienta que permite recoger información sobre AD. 

Ejemplo de funcionamiento de la herramienta:

La herramienta se ubica en el path ``C:\Tools\BloodHound`` importante para recoger información la parte de la herramienta ubicada en ``C:\Tools\BloodHound\resources\app\Collectors``

```PowerShell

#Desde el path C:\Tools\BloodHound\resources\app\Collectors
powershell -ep bypass
. .\SharpHound.ps1
Invoke-Bloodhound -CollectionMethod All
```

Una vez termine nos dejará un .zip con todos los datos.

![[Pasted image 20250318090640.png]]

Este zip es muy importante para ponerlo en la interfaz gráfica. Para ello vamos a ejecutar el binario de BloodHount ubicado en ``C:\Tools\BloodHound\BloodHound``

Una vez dentro de la aplicación hay que subir el .zip con todos los datos del AD.


![[Pasted image 20250318091220.png]]

Una vez ha cargado los datos podemos acceder a varios datos de gran interés y con vista de gráfico.

Por ejemplo, podemos ver todos los dominios de administrador y ver todos sus datos, como la última vez que cambio su passwd.

También podemos ver todos los usuarios de Kerberos.

![[Pasted image 20250318091404.png]]

Es muy importante la parte de "Dangerous Privileges" ya que nos informa sobre los usuarios que tienen permisos que podrían ser dañinos.

![[Pasted image 20250318091640.png]]

