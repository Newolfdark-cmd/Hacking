## 📌 ¿Qué es Finger?
**Finger** es un protocolo utilizado para obtener información sobre los usuarios de un sistema remoto. Se usaba antiguamente en sistemas Unix para consultar datos como el nombre real, la última conexión y el directorio de inicio.  
Si el servicio está expuesto, puede permitir la enumeración de usuarios y facilitar ataques de fuerza bruta.

## 🔌 Puertos en los que funciona
- **TCP 79** → Finger.

---

## 🔍 Enumeración de Finger

**Conexión directa al servicio para consultar un usuario específico:**
```bash
finger user@ip
```

**Escaneo de usuarios con Metasploit:**
```bash
msfconsole
use auxiliary/scanner/finger/finger_user
set RHOSTS ip
exploit
```

**Enumeración masiva de usuarios con `finger-user-enum`:**
```bash
./finger-user-enum pl -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t ip
```

