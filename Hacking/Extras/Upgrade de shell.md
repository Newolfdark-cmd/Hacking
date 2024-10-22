---
Funcion: Mejorar shell
SO: Linux
tags:
  - extra
  - shell
  - linux
---

## Descripción 

A veces cuando conseguimos el acceso a root del sistema o incluso acceso al sistema obtenemos una shell poco vistosa o poco interactiva. Para mejorar la shell podemos realizar varios procedimientos.

## Forma 1:

Si la shell tiene Python instalado podemos probar con los comandos:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'

o

python3 -c 'import pty; pty.spawn("/bin/bash")'

```

## Forma 2:

Upgrade de shell a bash:

```bash
SHELL=/bin/bash script -q /dev/null
```
