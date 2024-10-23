---
Herramienta: 🧶 Strings
Comando: steghide
Categoría: Análisis de imágenes
fecha: 2024-10-23
tags:
  - herramienta
---

## Descripción 

`strings` en Linux es un comando que se utiliza para buscar y extraer secuencias de caracteres legibles (cadenas) de archivos binarios. Es particularmente útil para analizar archivos que no son de texto (como ejecutables, bibliotecas compartidas o archivos de datos) y para recuperar información legible que puede estar incrustada en ellos.

## Ejemplos de Uso 

1. **Sacar cadenas de texto de una imagen**:

```bash
strings imagen | less
```

2. **Sacar cadenas de texto de una imagen e insertarlas en un diccionario**:

```bash
strings agua_ssh.jpg > diccionario.txt
```