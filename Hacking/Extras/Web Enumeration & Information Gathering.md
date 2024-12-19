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

Whois es un protocolo utilizado para consultar bases de datos que contienen información registrada sobre nombres de dominio, direcciones IP y otros recursos de red. La información que puedes obtener incluye: - Datos de contacto del registrador. - Fechas de creación y expiración. - Estado del dominio. - Servidores DNS asociados.

Uso común: ``whois domain/ip

Otros ejemplos:
- ``whois example.com | grep "Registrar\|Expiry Date"
- ``whois 8.8.8.8
#### Host

Nos permite conocer las IPs de un determinado dominio.

Uso común: ``host domain

Otros ejemplos:
- ``host -t MX example.com # Registros de correo.
- ``host -t NS example.com # Servidores de nombres.
#### Whois domaintool.com

DomainTools es una plataforma web que ofrece información avanzada sobre dominios, con una interfaz más visual que `whois`. Permite consultar:

- Historial de dominios.
- Cambios de propietario.
- Servidores DNS anteriores.
- Relaciones con otros dominios.

#### Netcraft

Netcraft es una herramienta en línea que proporciona análisis detallados de dominios y sitios web. Ofrece:

- Información sobre tecnologías utilizadas.
- Historial de hosting.
- Posibles vulnerabilidades.
- Análisis de phishin

### Passive DNS Enumeration
#### DNSRecon

DNSRecon es una herramienta utilizada para realizar tareas relacionadas con la recolección de información DNS. Permite realizar consultas detalladas, identificar configuraciones erróneas o expuestas, y recopilar datos clave sobre un dominio.

**Funciones principales**:  
- Consultas DNS (A, AAAA, CNAME, MX, NS, SOA).  
- Fuerza bruta de subdominios.  
- Identificación de transferencias de zona (Zone Transfers).  
- Verificación de servidores DNS configurados incorrectamente.  
- Enumeración inversa de registros PTR.

**Uso común**: ``dnsrecon -d example.com

#### DNSDumpster 

DNSDumpster es una herramienta en línea para la recolección de información relacionada con DNS. Es particularmente útil para realizar un reconocimiento pasivo y visualizar la infraestructura de red de un dominio, incluyendo subdominios, registros DNS y direcciones IP. 

**Funciones principales**: 
- Identificación de registros DNS (A, MX, NS, TXT). 
- Descubrimiento de subdominios. 
- Visualización de la infraestructura de red (mapas gráficos). 
- Recolección de direcciones IP asociadas con el dominio. 
- Reconocimiento pasivo (sin enviar consultas directas al dominio objetivo).