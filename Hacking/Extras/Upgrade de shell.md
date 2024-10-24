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

## Tratamiento TTy:

1. Poner la shell en espera con Ctrl-Z

-> Ctrl-Z

2. Examinar el $TERM para luego ponerlo en la otra shell.

```bash
 echo $TERM
 stty -a
```

Importante tomar nota del número de filas y columuas ejemplo -> (_“rows 37; columns 146”_)

3. Poner el valor del stty actual en raw para pasarselo al echo

```bash
stty raw -echo
```

4. Volver a la shell de antes

```bash
 fg
  reset
```

6. Modificar las variables

$ export SHELL=bash
$ export TERM=xterm256-color
$ stty rows 37 columns 146
$ bash -i

7. En caso de fallo poner las siguientes.

$ export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
$ export TERM=xterm
$ export SHELL=bash
$ cat /etc/profile; cat /etc/bashrc; cat ~/.bash_profile; cat ~/.bashrc; cat ~/.bash_logout; env; set
$ export PS1='[\u@\h \W]\$