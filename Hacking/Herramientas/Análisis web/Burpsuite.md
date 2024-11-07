---
Herramienta: 🕷️ Burp Suite
Comando: burpsuite
Categoría: Análisis y prueba de aplicaciones web
fecha: 2024-10-21
tags:
  - herramienta
  - análisis-web
---
## Descripción 

**Burp Suite** es una herramienta integral utilizada para la prueba de seguridad en aplicaciones web. Su interfaz permite interceptar y modificar tráfico HTTP y HTTPS entre el navegador y el servidor web, facilitando la identificación de vulnerabilidades. Además, cuenta con módulos como **Intruder** (fuerza bruta), **Scanner** (para identificar vulnerabilidades conocidas) y **Repeater** (para probar solicitudes específicas). **Importante:** configurar el proxy en el navegador y ajustar los parámetros de seguridad según el alcance de la auditoría.

Como es una herramienta muy extensa voy a explicar sus funciones principales y las veces donde la he utilizado.

## Funcionalidades Principales

- **Proxy**: Intercepta y analiza el tráfico entre el navegador y la aplicación web.
- **Scanner**: Escanea la aplicación en busca de vulnerabilidades conocidas.
- **Intruder**: Realiza ataques personalizados (como fuerza bruta).
- **Repeater**: Permite repetir y modificar peticiones HTTP para observar respuestas.
- **Decoder**: Codifica y decodifica datos de diferentes formatos.
- **Comparer**: Compara respuestas para analizar cambios en el comportamiento de la aplicación.
- **Extender**: Permite añadir plugins personalizados para ampliar la funcionalidad.

## Utilizado en:

Maquinas -> Docker-labs -> Fácil -> Dockerlabs