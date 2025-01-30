## ¿Qué es NetBIOS?
**NetBIOS** (Network Basic Input/Output System) es una API que permite la comunicación entre dispositivos dentro de una red local (LAN). Originalmente diseñado para redes pequeñas, NetBIOS proporciona servicios básicos como:

- **Resolución de nombres**: Traduce nombres de dispositivos en direcciones IP.
- **Servicio de sesión**: Establece y mantiene conexiones entre dispositivos.
- **Servicio de datagramas**: Facilita el envío de datos sin conexión entre dispositivos.

En el contexto de **pentesting**, NetBIOS es relevante porque los servicios mal configurados pueden exponer información sensible, como nombres de dispositivos o usuarios.

## ¿Qué es SMB?
**SMB** (Server Message Block) es un protocolo utilizado para compartir archivos, impresoras y recursos entre dispositivos en una red. SMB opera sobre NetBIOS en redes antiguas, pero actualmente funciona principalmente sobre **TCP/IP**, usando el puerto 445.

### Características principales de SMB:
- Permite el acceso remoto a archivos y carpetas.
- Ofrece autenticación de usuarios para controlar el acceso a los recursos.
- Es utilizado por sistemas Windows y también compatible con otras plataformas (como Samba en Linux).

### Relevancia en Pentesting:
- Las versiones antiguas de SMB, como **SMBv1**, son vulnerables a ataques como **EternalBlue**, que fue explotado por el ransomware WannaCry.
- Servicios SMB expuestos pueden permitir la enumeración de usuarios, recursos compartidos y configuraciones de red.

## Puertos comunes
- **UDP 137**: Resolución de nombres NetBIOS.
- **UDP 138**: Datagramas NetBIOS.
- **TCP 139**: Sesión NetBIOS.
- **TCP 445**: SMB directo sobre TCP/IP.

## Herramientas útiles para analizar NetBIOS y SMB
- `nmap` con scripts específicos:
  - `nmap --script smb-enum-shares -p445 <IP>`
  - `nmap --script smb-os-discovery -p445 <IP>`
- `smbclient` para interactuar con recursos compartidos.
- `enum4linux` para enumerar información detallada de SMB.

## Enumeración del servicio

### `nbtscan red`
- **Descripción**: Escanea la red para descubrir nombres NetBIOS, direcciones IP y roles (como servidores o estaciones de trabajo).
- **Utilidad**: Nos permite identificar hosts activos en la red y sus nombres NetBIOS, que pueden incluir información relevante como el nombre del dominio.

### `nmblookup -A ip`
- **Descripción**: Consulta información NetBIOS de un host específico.
- **Utilidad**: Devuelve nombres de máquina, dominios y grupos de trabajo asociados con la dirección IP. Útil para identificar el entorno de red.

### `nmap -sV -p 139,445 ip`
- **Descripción**: Escanea los puertos 139 (NetBIOS) y 445 (SMB) e identifica los servicios y versiones en ejecución.
- **Utilidad**: Nos permite identificar la versión exacta de SMB que se ejecuta, lo que puede ser crucial para detectar vulnerabilidades conocidas.

### `nmap -p445 --script smb-protocols ip`
- **Descripción**: Utiliza un script de `nmap` para identificar qué versiones de SMB (como SMBv1, SMBv2, SMBv3) están habilitadas.
- **Utilidad**: Detectar el uso de SMBv1, que es vulnerable a múltiples exploits como EternalBlue.

### `nmap -p445 --script smb-security-mode ip`
- **Descripción**: Comprueba las políticas de seguridad SMB, como si permite autenticación débil o conexiones anónimas.
- **Utilidad**: Identifica configuraciones débiles que podrían ser aprovechadas para acceso no autorizado.

---

## Fuerza bruta y acceso anónimo

### `nmap -p445 --script smb-enum-users ip`
- **Descripción**: Enumera los usuarios disponibles en el servicio SMB utilizando un script de `nmap`.
- **Utilidad**: Revela usuarios potencialmente activos que pueden ser objetivo de ataques de fuerza bruta.

### `smbclient -L ip`
- **Descripción**: Lista los recursos compartidos disponibles en el host SMB. No se especifica contraseña para intentar un acceso anónimo.
- **Utilidad**: Comprueba si el servidor permite conexiones anónimas y revela recursos que pueden ser accedidos sin autenticación.

### `hydra -L users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ip smb`
- **Descripción**: Realiza un ataque de fuerza bruta sobre SMB utilizando una lista de usuarios (`users.txt`) y una lista de contraseñas.
- **Utilidad**: Identificar credenciales válidas mediante fuerza bruta.

### `psexec.py administrator@ip`
- **Descripción**: Herramienta de `Impacket` para ejecutar comandos de forma remota utilizando SMB. Requiere credenciales válidas.
- **Utilidad**: Acceder remotamente a un host como administrador, ideal para movimiento lateral y control total del sistema.

---

### Aplicaciones prácticas
- **Enumeración inicial**: Identificar hosts, servicios SMB activos, versiones y configuraciones débiles.
- **Explotación**: Acceso anónimo, fuerza bruta de credenciales o explotación de vulnerabilidades en versiones antiguas como SMBv1.
- **Post-explotación**: Usar credenciales para ganar control total con herramientas como `psexec.py`.

