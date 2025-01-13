---
Vulnerabilidad: 📜 XSS DOM
Categoría: Vulnerabilidad de seguridad web
Impacto: Alto
tags:
  - xss
  - dom
  - seguridad_web
---

## XSS DOM

### Descripción

El **XSS DOM (Cross-Site Scripting Document Object Model)** es una variante del XSS que ocurre directamente en el lado del cliente, es decir, en el navegador del usuario. A diferencia de otras formas de XSS, el código malicioso se inyecta y ejecuta manipulando el DOM del navegador, sin que pase por el servidor.

Esta vulnerabilidad ocurre cuando un script en el cliente toma datos de fuentes no confiables (como la URL, cookies, o inputs) y los utiliza directamente en el DOM sin la debida sanitización o validación.

---

### Cómo funciona

1. **Origen del dato no confiable**: El atacante manipula una entrada como la URL, hash fragment, o parámetros GET para inyectar código malicioso.
   
2. **Ejecución en el cliente**: El navegador ejecuta este código debido a una mala gestión en la manipulación del DOM, permitiendo al atacante realizar acciones como robo de cookies, redirección a sitios maliciosos, o manipulación del contenido de la página.

---

### Ejemplo práctico

#### Código vulnerable
```javascript
// Toma el parámetro 'user' de la URL y lo muestra directamente en el DOM
const user = new URLSearchParams(window.location.search).get('user');
document.getElementById('greeting').innerHTML = `Hola, ${user}!`;
```

