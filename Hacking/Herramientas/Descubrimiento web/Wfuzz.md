---
Herramienta: 🔍 wfuzz
Comando: wfuzz
Categoría: Descubrimiento web
fecha: 2024-10-25
tags:
  - herramienta
---
## Descripción 

**Wfuzz** es una herramienta de fuerza bruta diseñada para realizar ataques de fuzzing en aplicaciones web, permitiendo la enumeración de recursos, inyección de parámetros, y búsqueda de vulnerabilidades. Es particularmente útil para pruebas de seguridad en aplicaciones web y permite un alto nivel de personalización.

1. **Enumerar directorios y ficheros en una página web a partir de una wordlist**: 

```bash 
wfuzz -w /path/to/dictionary -u http://www.example.com/FUZZ
```

2. **Inyección de parámetros en URL**:

```bash 
wfuzz -w /path/to/dictionary -u http://www.example.com/index.php?param=FUZZ
```

2. ****Buscar archivos específicos en una ruta****:

```bash 
wfuzz -w /path/to/dictionary -u http://www.example.com/dir/FUZZ --hc 404
```
