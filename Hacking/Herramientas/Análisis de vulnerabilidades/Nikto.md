---
Herramienta: 🔎 Nikto
Comando: nikto
Categoría: Análisis de vulnerabilidades web
SO: Linux
tags:
  - escaneo_web
  - vulnerabilidades
  - seguridad
  - herramienta
---

## Nikto

### Descripción

**Nikto** es una herramienta de escaneo web diseñada para detectar vulnerabilidades, configuraciones inseguras y archivos sensibles en servidores web. Es ideal para realizar pruebas de penetración iniciales y descubrir fallos básicos en aplicaciones web o configuraciones del servidor. 

Nikto soporta múltiples servidores web y protocolos, y es altamente personalizable mediante plantillas y configuraciones.

---

### Características principales

1. **Detección de vulnerabilidades**:
   - Identifica configuraciones inseguras, como listas de directorios abiertas.
   - Detecta scripts inseguros, vulnerabilidades conocidas y problemas de configuración SSL/TLS.

2. **Soporte multi-protocolo**:
   - Compatible con HTTP, HTTPS, y otros servicios web.

3. **Actualizaciones de base de datos**:
   - Su base de datos de vulnerabilidades se mantiene activa y se puede actualizar fácilmente.

4. **Integración con otras herramientas**:
   - Se puede utilizar junto con Nmap o Burp Suite para profundizar en los análisis.

---

### Ejemplos Prácticos

Podemos realizar un escaneo de una web mediante el comando:

``nikto -h http://host -o nikto.html -Format htm

