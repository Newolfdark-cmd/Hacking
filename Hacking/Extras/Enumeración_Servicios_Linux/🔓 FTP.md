## 📌 ¿Qué es FTP?
**FTP (File Transfer Protocol)** es un protocolo de red utilizado para la transferencia de archivos entre sistemas. Puede operar en modo **activo** o **pasivo** y, si está mal configurado, puede permitir accesos anónimos o filtraciones de información sensible.

## 🔌 Puertos en los que funciona
- **TCP 21** → Control de sesión FTP.
- **TCP 20** → Transferencia de datos (modo activo).

---

## 🔍 Enumeración y análisis de FTP

**Escaneo de todos los scripts de FTP con Nmap:**
```bash
nmap -p 21 --script ftp* ip
```

**Escaneo de vulnerabilidades en FTP:**
```bash
nmap -p 21 --script vuln ip
```

**Listar archivos accesibles en un servidor FTP sin autenticación:**
```bash
nmap --script ftp-anon -p 21 ip
```