---
Herramienta: 🕵️‍♂️ OWASP ZAP
Comando: zap.sh (en Linux) o zap.bat (en Windows)
Categoría: Análisis de seguridad web
SO: Multiplataforma
tags:
  - owasp
  - seguridad_web
  - sql_injection
  - testing
---

## OWASP ZAP

### Descripción

**OWASP ZAP (Zed Attack Proxy)** es una herramienta gratuita y de código abierto para realizar pruebas de seguridad en aplicaciones web. Es una de las herramientas más utilizadas por analistas de seguridad y pentesters para identificar vulnerabilidades como **inyección SQL**, **XSS**, y **CSRF**, entre otras.

Su interfaz gráfica y capacidades de automatización la hacen ideal tanto para principiantes como para expertos.

---

### SQL Injection con OWASP ZAP

La inyección SQL es una de las vulnerabilidades más críticas, y OWASP ZAP permite detectarla de forma eficiente mediante su funcionalidad de escaneo pasivo y activo.

---

### Pasos para utilizar OWASP ZAP para identificar SQL Injection

1. **Configurar el entorno**:
   - Inicia ZAP desde tu terminal:
```bash
zap.sh
```
   - Configura tu navegador para que utilice ZAP como proxy:
     - Proxy por defecto: `http://127.0.0.1:8080`.

2. **Captura del tráfico**:
   - Navega por el sitio web objetivo mientras ZAP intercepta y registra las solicitudes.

3. **Exploración activa**:
   - En ZAP, selecciona el sitio desde el árbol en la pestaña **Sites**.
   - Haz clic derecho y selecciona **Attack → Active Scan**.
   - Configura los parámetros del escaneo para enfocarte en **SQL Injection**:
     - Incluye métodos como `GET`, `POST` o ambos.
     - Habilita solo los test relacionados con inyección SQL desde la configuración de la política.

4. **Análisis manual** (opcional):
   - Inspecciona las solicitudes interceptadas.
   - Busca parámetros que podrían ser vulnerables (por ejemplo, IDs, nombres de usuario, etc.).
   - Modifica manualmente los parámetros para incluir payloads básicos de SQL Injection:    
   
```sql
' OR '1'='1' --      
```

5. **Interpretación de resultados**:
   - ZAP marcará las posibles vulnerabilidades con detalles sobre los parámetros afectados.
   - Los reportes indicarán los payloads probados y los resultados obtenidos.

---

### Ejemplo práctico

#### Escaneo activo para SQL Injection
1. Captura la solicitud:
   - Accede a `http://ejemplo.com/login?user=admin`.
   
2. Configura ZAP para probar inyecciones:
   - Desde el menú de escaneo activo, selecciona **SQL Injection**.

3. Resultado:
   - ZAP detecta una posible vulnerabilidad en el parámetro `user` con el payload:

```sql
admin' OR '1'='1
```

---

### Buenas prácticas al usar ZAP

- **Entorno controlado**: Nunca utilices ZAP en sistemas donde no tengas permiso.
- **Reducción de ruido**: Realiza pruebas sobre páginas específicas para evitar falsos positivos.
- **Validación manual**: Siempre confirma las vulnerabilidades detectadas mediante análisis adicionales.

---

### Notas adicionales

- **Integración CI/CD**: ZAP se puede integrar con pipelines de desarrollo para pruebas automatizadas.
- **Complementos avanzados**: Incluye scripts para generar payloads personalizados o analizar respuestas más complejas.

