---
Herramienta: 🕵️ WPScan
Comando: wpscan
Categoría: Análisis de vulnerabilidades web
SO: Linux
tags:
  - escaneo_web
  - wordpress
  - vulnerabilidades
  - herramienta
---

## WPScan

### Descripción

**WPScan** es una herramienta especializada en escanear vulnerabilidades en sitios web basados en **WordPress**. Permite a los pentesters y administradores de sistemas identificar configuraciones inseguras, plugins desactualizados, temas vulnerables y otras brechas de seguridad específicas de WordPress.

WPScan es ampliamente reconocido por su base de datos actualizada de vulnerabilidades relacionadas con WordPress, lo que lo convierte en una herramienta esencial para asegurar y auditar estos sitios.

---

### Características principales

1. **Detección de usuarios**:
   Identifica nombres de usuario que podrían ser utilizados en ataques de fuerza bruta.

2. **Plugins y temas**:
   Escanea versiones de plugins y temas instalados para detectar vulnerabilidades conocidas.

3. **Base de datos de vulnerabilidades**:
   Utiliza la base de datos oficial de WPScan para correlacionar los resultados con vulnerabilidades conocidas (CVE).

4. **Fuerza bruta**:
   Puede realizar ataques de fuerza bruta para probar la robustez de contraseñas de usuarios.

---

### Ejemplo práctico

#### Escaneo básico.

```bash
wpscan --url http://ejemplo.com
```

#### Escaneo de plugins y temas.

```bash
wpscan --url http://ejemplo.com --enumerate p,t
```

#### Escaneo de plugins agresivamente.

```bash
wpscan --url http://ejemplo.com --enumerate p --plugins-detection agressive
```