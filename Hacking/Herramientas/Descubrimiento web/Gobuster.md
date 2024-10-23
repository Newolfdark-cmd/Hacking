---
Herramienta: 👻 gobuster
Comando: gobuster
Categoría: Descubrimiento web
fecha: 2024-10-21
tags:
  - herramienta
---
## Descripción 

**Gobuster** es una herramienta de fuerza bruta muy potente y con el objetivo de enumerar uris en sitios web, es decir, directorios y ficheros, subdominios dns y nombres de host virtuales en servidores web. **Importante** añadir -x y las extensiones que quieras buscar

1. **Identificar subdirectorios de una página web a partir de una wordlist**: 

```bash 
gobuster dir -u http://www.example.com/ -w /path/to/dictionary
```

2. **Enumeración de DNS**:

```bash 
$ gobuster dns -d example.com -w /path/to/dictionary -t 100
```

2. **Enumeración de ficheros**:

```bash 
gobuster dir -u http://www.example.com/ -w /path/to/dictionary -x .php,.txt,.py
```
