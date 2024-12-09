---
Herramienta: 🧩 FingerprintJS2
Comando: No aplica (librería JS)
Categoría: Huella digital
SO: Multiplataforma
tags:
  - fingerprinting
  - seguridad_web
  - analisis
  - herramienta
---

## Descripción

**FingerprintJS2** es una librería de JavaScript que permite generar una huella digital única de los navegadores de los usuarios, recopilando información del entorno del cliente sin necesidad de cookies. Es útil para la detección de bots, el análisis de comportamiento y la protección contra fraudes.

## Características clave

- **Huella digital sin cookies**: Genera identificadores únicos basados en parámetros del navegador y del dispositivo.
- **Multiplataforma**: Funciona en cualquier navegador moderno.
- **Privacidad respetada**: No recopila información personal directamente identificable.
- **Extensibilidad**: Permite personalizar qué componentes se utilizan para generar la huella.

## Ejemplo de uso

### Instalación

Puedes instalar FingerprintJS2 usando npm o incluirlo directamente en tu proyecto:

```bash
npm install fingerprintjs2
```

**Ejemplo 1**

Para este ejemplo vamos a necesitar tener un servidor apache en nuestro PC

```bash
sudo apt intall apache2
sudo systemctl start apache2
#Así ya tendriamos el servidor apache funcionando

cd /var/www/html
#Nos vemos a la carpeta de apache

git clone https://github.com/LukasDrgon/fingerprintjs2.git
#Clonamos el repositorio de fingerprinths2
```

Ahora desde el navegador web podemos acceder al servidor apache (simplemente con localhost) y revisar la información que vamos dejando por el navegador con la libreria fingerprintjs2.


