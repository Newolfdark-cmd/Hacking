---
Herramienta: 📸 Exiftool
Comando: exiftool
Categoría: Análisis de imágenes
fecha: 2024-10-17
tags:
  - herramienta
---
## Descripción 

**ExifTool** es una herramienta poderosa para leer, escribir y editar metadatos en una amplia variedad de formatos de archivos, incluidos imágenes, audio y video. Permite extraer información como la fecha de creación, la cámara utilizada, y mucho más. 

## Ejemplos de Uso 

1. **Extraer todos los metadatos de una imagen**: 

```bash 
exiftool nombre_imagen.jpg
```

2. **Ver información específica (por ejemplo, el modelo de la cámara)**:: 

```bash 
exiftool -Model nombre_imagen.jpg
```

2. **Eliminar todos los metadatos de una imagen**:

```bash 
exiftool -all= nombre_imagen.jpg
```