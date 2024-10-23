---
Herramienta: 🐍 Hydra
Comando: hydra
Categoría: Fuerza Bruta
fecha: 2024-10-17
tags:
  - herramienta
---

## Descripción 

**Hydra** es una herramienta popular para realizar ataques de diccionario y fuerza bruta contra varios protocolos y servicios de red HTTP, FTP, SSH.

## Ejemplos de Uso 

1. **Atacar un Servicio SSH**: 

```bash 
hydra -l usuario -P passwords.txt ssh://ip_del_objetivo
```

2. **Atacar un Servicio FTP**:: 

```bash 
hydra -l usuario -P passwords.txt ftp://ip_del_objetivo
```

3. **Atacar un Servicio HTTP**:

```bash 
hydra -l usuario -P passwords.txt -s 80 -f -vV ip_del_objetivo http-get /ruta_del_formulario
```

4. **Usar un archivo de usuarios y contraseñas**:

```bash 
hydra -L usuarios.txt -P passwords.txt ssh://ip_del_objetivo
```