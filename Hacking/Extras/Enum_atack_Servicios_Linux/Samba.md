## 📌 ¿Qué es Samba?
**Samba** es una implementación libre del protocolo **SMB (Server Message Block)** que permite compartir archivos e impresoras en redes Windows y Linux. Si está mal configurado, puede permitir el acceso anónimo o la enumeración de usuarios y recursos.

## 🔌 Puertos en los que funciona
- **TCP 139** → NetBIOS Session Service.
- **TCP 445** → SMB Direct sobre TCP.

---

## 🔍 Enumeración de Samba

**Detección del servicio SMB/Samba:**
```bash
nmap -sV -p 139,445 ip
```

**Escaneo de usuarios locales en el sistema:**
```bash
enum4linux -r ip | grep "Local User"
```

**Listado de usuarios con permisos en Samba:**
```bash
smbmap -H ip
```

**Enumeración de shares accesibles en Samba:**
```bash
nmap --script smb-enum-shares -p 445 ip
```

## 📡 Conexión anónima y ejecución de comandos

**Conexión anónima a un share SMB:**
```bash
smbclient -N \\\\ip\\share -U ""
```

**Acceso anónimo con terminal de comandos en Samba:**
```bash
rcpclient -U "" ip
```

Una vez dentro, se pueden ejecutar los siguientes comandos:

1. `enumdomusers` → Enumerar usuarios del dominio.
2. `enumdomgroups` → Listar grupos del dominio.
3. `enumdomains` → Mostrar los dominios accesibles.
4. `queryuse john` → Consultar las conexiones activas del usuario "john".