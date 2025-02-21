---
Herramienta: ⚙️ LinPEAS
Comando: linpeas.sh
Categoría: Escalación de privilegios
SO: Linux
tags:
  - ecalada_privs
  - linux
---

## Descripción

**LinPEAS** es un script de auditoría de seguridad diseñado para identificar posibles vectores de escalación de privilegios en sistemas Linux. Realiza un análisis exhaustivo de configuraciones, permisos, software instalado y otros aspectos del sistema que podrían explotarse para obtener mayores privilegios.

## Forma de uso

Para ejecutar LinPEAS en un sistema Linux al que se tiene acceso, sigue estos pasos:

### Paso 1: Descargar LinPEAS en el sistema objetivo

Para ejecutar LinPEAS, primero descárgalo en el sistema. Puedes transferirlo desde tu máquina usando `scp` o descargarlo directamente desde GitHub.

```bash
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh
```


