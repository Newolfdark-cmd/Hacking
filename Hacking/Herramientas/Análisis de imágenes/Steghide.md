---
Herramienta: Steghide
Comando: steghide
Categoría: Análisis de imágenes
fecha: 2024-10-17
tags:
  - herramienta
---

## Descripción 

**Steghide** es una herramienta de ocultación de datos que permite insertar datos ocultos dentro de archivos de imagen y audio. Utiliza técnicas de esteganografía para ocultar información sin alterar la apariencia del archivo original, lo que puede ser útil para la transferencia segura de datos.

## Ejemplos de Uso 

1. **Insertar un archivo en una imagen**: 

```bash 
steghide embed -cf imagen.jpg -ef archivo.txt
```

2. **Ver el contenido oculto sin extraerlo**:: 

```bash 
steghide info imagen.jpg
```

3. **Insertar un archivo y especificar una contraseña**:

```bash 
steghide embed -cf imagen.jpg -ef archivo.txt -p "tu_contraseña"
```

4. **Extraer un archivo oculto de una imagen**:

```bash 
steghide extract -sf imagen.jpg
```