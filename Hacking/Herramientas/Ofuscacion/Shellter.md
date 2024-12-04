---
Herramienta: 🔐 shellter
Comando: shellter
Categoría: ofuscacion
fecha: 2024-12-4
tags:
  - herramienta
  - ofuscacion
---

## Descripción 

**Shellter** es una herramienta avanzada de ofuscación utilizada para inyectar payloads en archivos ejecutables y evadir detecciones de antivirus (AV). Funciona mediante la manipulación de binarios y la introducción de shellcodes en secciones seguras del archivo objetivo. Es ideal para pruebas de penetración y red teaming. 

## Características clave 

-  **Inyección dinámica**: Modifica ejecutables para incluir payloads sin alterar su funcionalidad. 
- **Bypass de AV**: Diseñado para evitar detecciones de soluciones antivirus. 
- **Compatibilidad**: Soporta binarios de 32 y 64 bits. 
- **Modos interactivo y automático**: Permite configuraciones personalizadas o ejecuciones rápidas.

## Ejemplos de uso

Cuanto menor sea el ejecutable mejor para el tiempo de ofuscación.

**Ejemplo 1**

Ejecutamos la terminal interactiva con el comando shellter.

Con operation mode en A

PE Target: Path_to_file

Automáticamente hace un backup del archivo que se va a modificar y tras unos segundos/minutos saldrá un número de instrucciones utilizadas. Despues un panel de si quieres utilizar el modo sigilo (Stealth) y le vamos a decir que Si.

Tras eso pregunta si quieres usar un payload custom pero vamos a decir que no (L). Y ya tendrás tu primer archivo ofuscado.
