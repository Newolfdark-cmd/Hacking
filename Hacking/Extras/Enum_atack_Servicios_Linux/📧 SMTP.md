
## 📌 ¿Qué es SMTP?
**SMTP (Simple Mail Transfer Protocol)** es un protocolo utilizado para el envío de correos electrónicos entre servidores. Se basa en una arquitectura cliente-servidor y puede permitir la enumeración de usuarios si está mal configurado.

## 🔌 Puertos en los que funciona
- **TCP 25** → Envío de correos (sin cifrado).
- **TCP 465** → SMTP sobre SSL/TLS.
- **TCP 587** → SMTP con STARTTLS (cifrado recomendado).

---

## 🔍 Enumeración SMTP con Nmap

### **Listar scripts de Nmap para SMTP**
```bash
ls -al /usr/share/nmap/scripts/ | grep -e "smtp-"
```

### **Con el script smtp-commands podemos ver si utiliza "VRFY" lo cual nos puede permitir hacer un scanner de usuarios**:
```bash
nmap --script smtp-commands -p puerto ip
```

### **Ejecutar todos los scripts SMTP de Nmap**:
```bash
nmap --script smtp-* -p 25 ip
```

## 📡 Enumeración de usuarios en SMTP

### **Escaneo de usuarios con `smtp-user-enum`**:
```bash
smtp-user-enum -M VRFY /usr/share/metasploit-framework/data/wordlists/common_users.txt
```
