
## 📌 ¿Qué es SNMP?
**SNMP (Simple Network Management Protocol)** es un protocolo utilizado para administrar y monitorear dispositivos de red como routers, switches y servidores. Funciona mediante la consulta de información estructurada en una **MIB (Management Information Base)**, donde se almacenan datos sobre la configuración y el estado del sistema.

## 🔌 Puertos en los que funciona
- **UDP 161** → Para consultas SNMP.
- **UDP 162** → Para recibir traps (alertas enviadas por dispositivos administrados).

---

## 🔍 Escaneo SNMP con Nmap
SNMP funciona sobre **UDP**, por lo que se debe especificar en los escaneos.

### **Detectar si SNMP está activo**
```bash
nmap -sU -p 161 ip
```

### **Enumeración de directorios SNMP**
``nmap -sU -p 161 --script=snmp-brute ip

**Función**: Intenta enumerar comunidades SNMP conocidas o débiles.

### **Obtener la versión de SNMP**
``nmap -sU -sV -p 161 ip

## **📡 Extracción de información SNMP**

### **Consultar información detallada de SNMP**
``snmpwalk -v version -c folder ip

### ****Guardar toda la información obtenida con Nmap****
``nmap -sU -p 161 --script snmp-* ip > snmp_info

## **🚨 Ataques SNMP**

### ****Ataque de fuerza bruta a SNMP****
```bash
hydra -l administrator -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ip smb
```
