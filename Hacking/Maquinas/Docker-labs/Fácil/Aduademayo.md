---
Maquina: aduademayo
SO: Linux
Explotación: Análisis de imágenes y web
Escala_privs: Permisos sudo
Dificultad: Fácil
tags:
  - maquina
  - web_discovery
  - analisis_imagen
  - linux
  - escala_privs
  - ssh
  - web
---
## Reconocimiento

1. **Explorar los puerto abiertos del docker**: 

```bash 
nmap -p- --open --min-rate 5000 -sS -Pn 172.17.0.2
```

![[Pasted image 20241023091043.png]]

2. **Ver la pagina web ubicada en el puerto 80**:

![[Pasted image 20241023091123.png]]

Como se puede apreciar es la página por defecto de apache. Vamos a intentar ver si hay alguna web adicional mediante Gobuster y nos encontramos lo siguiente:

![[Pasted image 20241023091616.png]]

![[Pasted image 20241023091635.png]]

Al parecer hay una imagen almacenada dentro de la web, vamos a analizarla a ver si contiene algo de información. La imagen es la siguiente:

![[Pasted image 20241023091714.png]]

3. **Análisis de la imagen**:

Tras analizar la imagen en detalle sin lograr nada (incluso probando fuerza bruta con steghide) volví a revisar las webs que teníamos y me topé con el siguiente código en el código fuente de la página principal de apache.

![[Pasted image 20241023093958.png]]

```brainfuck 
++++++++++[>++++++++++>++++++++++>++++++++++>++++++++++>++++++++++>++++++++++>++++++++++++>++++++++++>+++++++++++>++++++++++++>++++++++++>++++++++++++>++++++++++>+++++++++++>+++++++++++>+>+<<<<<<<<<<<<<<<<<-]>--.>+.>--.>+.>---.>+++.>---.>---.>+++.>---.>+..>-----..>---.>.>+.>+++.>.
```

Parece que está condificado con brainfuck y al descifrarlo sale la cadena "bebeaguaqueessano". Si probamos a utilizar steghide con bebeaguaqueessano no encontraremos nada y es que tenemos que utilizar el nombre de la imagen como usuario de ssh "agua" y la pass si que es la cadena que hemos decodificado.

![[Pasted image 20241023095328.png]]

## Escalada de privs:

3. **Escalada de privs**:

```bash 
sudo -l
```

Si observamos los permisos de sudo que tenemos para la ejecución de comandos nos encontramos que podemos ejecutar bettercap.

![[Pasted image 20241023095504.png]]

Si ejecutamos bettercup con

```bash 
sudo bettercup
```

podremos observar como tenemos privilegios de root

![[Pasted image 20241023100105.png]]

y si queremos ejecutar comandos desde fuera de bettercup podemos dar permisos de superusuario al /bin/bash.

```bash 
! chmod +s /bin/bash
```

y después de salir de bettercup ejecutar

```bash 
bin/bash -p
```


