---
Maquina: firsthacking
SO: Linux
Explotación: Vulnerabilidad
Escala_privs: No
Dificultad: Muy fácil
tags:
  - maquina
  - vulnerabilidad
  - linux
---
## Reconocimiento:

1. **Explorar los puerto abiertos del docker**: 

```bash 
sudo nmap -p- --open --min-rate 5000 -sS -n -Pn -sV 172.17.0.2
```

Importante el -sV pues el servicio FTP tiene una vulnerabilidad y para conocer su versión hay que poner este parámetro.

![[Pasted image 20241020113629.png]]

Si buscamos por Internet sobre la versión del FTP podemos dar con un CVE que se encarga de explotar esta versión.

![[Pasted image 20241020113744.png]]

## Explotación:

Si ejecutamos el script de la CVE podremos acceder al FTP vulnerable:

![[Pasted image 20241020113851.png]]



