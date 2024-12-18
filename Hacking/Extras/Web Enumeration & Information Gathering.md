---
Extra: 🌐 Web Enumeration & Information Gathering
fecha: 2024-12-18
tags:
  - extra
  - web
  - enumeracion
  - recon
---

## Web Enumeration & Information Gathering

### Descripción

La **enumeración y recopilación de información web** son pasos clave en cualquier fase de reconocimiento. Este proceso se enfoca en identificar detalles sobre un sitio web o aplicación, como directorios, subdominios, servicios, tecnologías utilizadas y posibles vulnerabilidades.

Herramientas como **Gobuster**, **WhatWeb**, **Dirb**, y **Sublist3r** facilitan esta tarea, permitiendo a los pentesters obtener una vista general de la superficie de ataque de una aplicación web antes de proceder con fases más agresivas, como la explotación.

---

### Objetivos comunes

1. **Identificar subdominios**:
   Herramientas como Sublist3r o Amass se utilizan para descubrir subdominios asociados con el dominio objetivo.

2. **Enumerar directorios y archivos**:
   Gobuster y Dirb permiten buscar rutas sensibles en servidores web a través de diccionarios.

3. **Recopilar tecnologías**:
   WhatWeb, Wappalyzer, o incluso inspecciones manuales ayudan a identificar servidores web, CMS, y frameworks.

4. **Analizar configuraciones DNS**:
   Se puede realizar con herramientas como `dig`, `nslookup` o `dnsenum`.

5. **Extraer metadatos**:
   Archivos accesibles como PDFs o imágenes a menudo contienen información sensible oculta.

---

### Guía de campo

#### Whois

Nos permite conocer diferente información sobre un dominio (update date, domain name, domain status, registry expiry date...)

Uso común: ``whois domain/ip

#### Host

Nos permite conocer las IPs de un determinado dominio.

Uso común: ``host domain

#### Whois domaintool.com

Plataforma online que nos permite conocer información sobre un determinado domino, más visual que desde consola.