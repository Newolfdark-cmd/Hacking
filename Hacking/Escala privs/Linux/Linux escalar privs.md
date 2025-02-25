---
Post-exploit: 🪜 escalar privs
fecha: 2024-10-23
tags:
  - Procedimientos
---
## Formas de escalada:

1. **Uso de `sudo -l` para ver comandos ejecutables como sudo:**

Si el usuario actual tiene acceso limitado a `sudo`, el comando `sudo -l` permite ver qué comandos puede ejecutar con privilegios elevados. Esto incluye scripts o binarios que podrían no haber sido asegurados correctamente.

```bash
sudo -l
```

2. **Buscar archivos con el bit SUID activado:**

El bit SUID (Set User ID) permite que un programa se ejecute con los privilegios del propietario del archivo, que generalmente es root. Si encuentras un archivo SUID vulnerable, podrías explotar esa funcionalidad para escalar privilegios.

```bash
find / -perm -4000 -user root 2>/dev/null
```

3. **Buscar archivos con capacidades especiales:**

Linux permite asignar "capacidades" a archivos sin necesidad de usar SUID. Las capacidades son privilegios específicos que un archivo puede tener, como `CAP_NET_BIND_SERVICE` para abrir puertos. Estas capacidades pueden abrir vulnerabilidades si no se controlan adecuadamente.

```bash
getcap -r / 2>/dev/null
```

4. **Explotar tareas cron mal configuradas:**

Las tareas programadas con cron, especialmente aquellas que se ejecutan con privilegios elevados, son un objetivo interesante. Si un script o binario ejecutado por cron es modificable, puedes modificarlo para incluir comandos que se ejecuten con privilegios de root.

```bash
cat /etc/crontab
o cat /etc/cron.d/
o cat /var/spool/cron/crontabs/
```

5. **Abusar de variables de entorno mal gestionadas:**

Algunos binarios SUID pueden confiar en variables de entorno, como `PATH`, para ejecutar comandos. Si puedes modificar estas variables, es posible engañar al binario para que ejecute tu propio código con privilegios de root.

Por ejemplo, si un binario llama a un comando como `ls` sin especificar su ruta absoluta, puedes modificar el `PATH` para que apunte a un script malicioso que hayas creado.

```bash
export PATH=/mi/directorio/malicioso:$PATH
```

6. **Archivos `.bashrc` o `.profile` de root modificables:**

Si puedes editar los archivos de inicio de sesión del usuario root, como `.bashrc` o `.profile`, puedes añadir un comando malicioso que se ejecute automáticamente la próxima vez que root inicie sesión.

```bash
echo 'nmap --interactive' >> /root/.bashrc
```

7. **Explotación de un kernel vulnerable:**

Las versiones antiguas del kernel de Linux pueden contener vulnerabilidades que permiten escalar privilegios. Si encuentras que el kernel en uso es vulnerable, puedes descargar un exploit específico para esa versión.

Comando para ver la versión del kernel:

```bash
uname -a
```

8. **Abusar de servicios locales mal configurados:**

Servicios locales como MySQL o Apache pueden correr con privilegios elevados. Si encuentras credenciales o configuraciones débiles, podrías explotarlas para obtener acceso root.

```bash
ps aux | grep root
```

9. **Buscar archivos o directorios con permisos incorrectos:**

Si encuentras archivos importantes, como `/etc/passwd` o `/etc/shadow`, con permisos de escritura para usuarios normales, puedes modificarlos para crear un usuario con privilegios root.

```bash
ls -la /etc/passwd /etc/shadow
```