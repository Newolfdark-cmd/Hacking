---
Herramienta: 🌐 Nmap
Comando: nmap
Categoría: Escaneo de redes
fecha: 2024-10-21
tags:
  - herramienta
---
## Descripción 

**Nmap** es una herramienta de código abierto utilizada para descubrir hosts y servicios en una red. Realiza escaneos de puertos, detección de sistemas operativos, identificación de servicios y auditoría de seguridad de redes. **Importante** usar diferentes opciones según el propósito del escaneo.

1. **Escaneo básico de puertos abiertos**: 

```bash 
nmap -p 1-65535 www.example.com
```

2. **Detección de sistema operativo**:

```bash 
nmap -O www.example.com
```

3. **Escaneo rápido de una red**:

```bash 
nmap -sn 192.168.1.0/24`
```

4. **Detección de versiones de servicios**:

```bash 
nmap -sV www.example.com
```

5. **Escaneo de vulnerabilidades con scripts prediseñados (NSE)**:

```bash 
nmap --script=vuln www.example.com
```

### Tabla de Opciones de Nmap

| Opción  | Descripción                                           |
|---------|-------------------------------------------------------|
| `-sS`   | Escaneo SYN (stealth scan).                           |
| `-sT`   | Escaneo TCP completo (conexión completa).             |
| `-sU`   | Escaneo de puertos UDP.                               |
| `-sV`   | Detección de versiones de servicios.                  |
| `-O`    | Detección del sistema operativo.                      |
| `-A`    | Escaneo avanzado: incluye detección de SO, versión, traceroute, etc. |
| `-sP`   | Ping Scan: detecta hosts en una red.                  |
| `-sn`   | Escaneo de red sin ping (host discovery).             |
| `-sC`   | Usa scripts NSE predeterminados.                      |
| `-p`    | Escaneo de puertos específicos (ej: `-p 80,443`).     |
| `-Pn`   | Desactiva el ping, asumiendo que los hosts están activos. |
| `-T0` a `-T5` | Ajuste de velocidad del escaneo (T0: más lento, T5: más rápido). |
| `-v`    | Modo detallado (verbose) para más información.        |
| `-A`    | Activar detección de sistema operativo y versión.     |
| `-oN`   | Guarda los resultados en formato legible en texto.    |
| `-oX`   | Guarda los resultados en formato XML.                 |
| `--script` | Ejecuta scripts NSE específicos para detección de vulnerabilidades. |
| `-F`    | Escaneo rápido (solo puertos comunes).                |
| `--traceroute` | Realiza traceroute después del escaneo.        |
| `--top-ports` | Escanea los puertos más comunes (ej: `--top-ports 1000`). |
| `-6`    | Escaneo de IPv6.                                      |
| `-iL`   | Escanear desde una lista de hosts.                    |
| `-oG`   | Exporta el resultado en formato Grepable.             |
| `--open`| Muestra solo puertos abiertos.                        |
| `--reason` | Muestra la razón por la que se concluye que un puerto está abierto, cerrado o filtrado. |

6. **Comprobar scripts de un servicio**:

```bash 
ls -la /usr/share/nmap/scripts/ | grep -e "servicio"
```


7. **Escaneo de vulnerabilidades con scripts automáticos (NSE)**:

```bash 
nmap -sS -sV -sC -p- ip
```

