---
Herramienta: 🕶️ MyBBScan
Comando: mybbscan
Categoría: Análisis de vulnerabilidades web
SO: Linux
tags:
  - escaneo_web
  - mybb
  - vulnerabilidades
  - herramienta
---

## MyBBScan

### Descripción

**MyBBScan** es una herramienta diseñada para analizar sitios web basados en el sistema de foros **MyBB**. Su principal objetivo es identificar configuraciones inseguras, vulnerabilidades en plugins y temas, y enumerar información sensible relacionada con el sistema. 

Es especialmente útil para pentesters y administradores que deseen evaluar la seguridad de su implementación de MyBB y prevenir posibles ataques.

---

### Características principales

1. **Detección de versiones**:
   - Identifica la versión de MyBB utilizada en el sitio, lo que permite verificar si contiene vulnerabilidades conocidas.

2. **Enumeración de plugins**:
   - Busca plugins instalados y verifica si tienen vulnerabilidades conocidas.

3. **Escaneo de temas**:
   - Detecta temas instalados y analiza su seguridad.

4. **Explotación básica**:
   - Incluye la capacidad de realizar pruebas específicas contra vulnerabilidades conocidas en MyBB.

---

### Ejemplo práctico

#### Escaneo básico

```bash
mybbscan --url http://ejemplo.com
```

![[Pasted image 20250113085125.png]]
