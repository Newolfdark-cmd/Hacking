## Buffer Overflow

### Descripción

El **buffer overflow** es una vulnerabilidad que ocurre cuando un programa escribe más datos en un búfer de los que este puede almacenar, sobrescribiendo memoria adyacente. Esto puede llevar a la corrupción de datos, fallos del programa o incluso a la ejecución de código arbitrario.

En muchos casos, los atacantes pueden aprovechar esta vulnerabilidad para manipular la memoria y ejecutar instrucciones maliciosas, logrando escaladas de privilegios o toma de control del sistema.

### Ejemplo gráfico

Imagina un vaso con capacidad para 200 ml de agua. Si intentas verter 300 ml, el agua se desbordará y podría mojar documentos cercanos. En términos de memoria, si un programa asigna 200 bytes a un búfer pero recibe 300 bytes, los datos extra pueden sobrescribir partes críticas de la memoria.

📌 **Ejemplo en código C vulnerable a buffer overflow:**

```c
#include <stdio.h>
#include <string.h>

int main() {
    char buffer[10];
    printf("Introduce un texto: ");
    gets(buffer); // Función insegura que no limita el tamaño de entrada
    printf("Texto ingresado: %s\n", buffer);
    return 0;
}
```

💡 _El uso de `gets()` permite escribir más de 10 caracteres, sobrescribiendo memoria y causando un posible desbordamiento._

### Consecuencias del Buffer Overflow

- Corrupción de datos
- Ejecución de código arbitrario
- Caída del programa (segfault)
- Escalada de privilegios en el sistema

### Prevención

- Utilizar funciones seguras como `fgets()` en lugar de `gets()`.
- Implementar **canarios de pila** (stack canaries) para detectar sobreescrituras.
- Utilizar técnicas como **ASLR** (Address Space Layout Randomization) para evitar que un atacante prevea direcciones de memoria explotables.

---

## 🔍 Detección de Buffer Overflows

### 📌 Identificación de Operaciones Inseguras

Los **buffer overflows** suelen ocurrir cuando un programa maneja datos sin verificar correctamente los límites de memoria. Se pueden detectar revisando el código en busca de **operaciones inseguras** como:

```c
char buffer[10];  
strcpy(buffer, "Texto demasiado largo"); // ❌ Riesgo de overflow  
```

#### ⚠️ Funciones Inseguras a Supervisar

Las siguientes funciones en C/C++ pueden ser vulnerables a **buffer overflows** si no se manejan correctamente:

- `strcpy()` → No verifica el tamaño del destino.
- `sprintf()` / `vsprintf()` → Puede escribir más datos de los permitidos en el buffer.
- `gets()` → No controla la cantidad de caracteres ingresados.
- `scanf()` → Puede sobrescribir memoria si no se limita la longitud de entrada.

**Ejemplo inseguro:**

```c
char user_input[50];
gets(user_input);  // 🚨 Puede causar un buffer overflow si el input es mayor a 50 bytes
```

✅ **Solución recomendada:**

```c
fgets(user_input, sizeof(user_input), stdin); // Limita el número de caracteres leídos
```

---

### 📊 Supervisión de Entradas y Límites

Es fundamental **validar todos los inputs** y asegurarse de que los datos no excedan la memoria asignada. Algunas técnicas incluyen:

- **Uso de límites en funciones seguras** (`fgets()`, `snprintf()` en lugar de `gets()` y `sprintf()`).
- **Implementación de validaciones de tamaño** antes de copiar o procesar datos.
- **Uso de AddressSanitizer** para identificar accesos de memoria incorrectos.

---

## Fuzzing con Spike

### Descripción

Spike es una herramienta para realizar fuzzing de aplicaciones que aceptan datos por red. Funciona enviando entradas manipuladas al servicio objetivo para detectar vulnerabilidades como desbordamientos de búfer.

### Entorno de prueba

En clase se propuso la conexión entre una máquina Kali Linux y una máquina Windows con un servidor vulnerable en el puerto 9999.

- El servidor es accesible mediante:
    
    ```bash
    nc <IP_WINDOWS> 9999
    ```
    
- Al conectarnos, el servidor muestra un mensaje de bienvenida.
- Si enviamos `HELP`, devuelve una lista de comandos permitidos.

### Creación del archivo Spike (.spk)

Vamos a crear un archivo `template.spk` con el siguiente contenido:

```plaintext
s_readline();
s_string("TRUN ");
s_string_variable("COMMAND");
```

### Ejecución del fuzzing

El fuzzing se ejecuta con el siguiente comando:

```bash
generic_send_tcp <IP_WINDOWS> 9999 template.spk 0 0
```

Según el ejemplo visto en clase, esta prueba provoca que el servidor crashee.

### Explicación del fallo

1. `s_readline();` espera la respuesta del servidor.
2. `s_string("TRUN ");` envía el comando `TRUN`, que el servidor reconoce como válido.
3. `s_string_variable("COMMAND");` inyecta una cadena de longitud variable en el comando.
4. `generic_send_tcp` envía la entrada al puerto 9999 del servidor.

Si el servidor no maneja correctamente la longitud de la entrada en `COMMAND`, puede provocar un desbordamiento de búfer y generar una caída del servicio.

---

### 🔬 Detección en Tiempo de Ejecución

Algunas técnicas avanzadas para detectar buffer overflows en ejecución:

- **Canarios de Pila** 🛡️ → Detectan sobrescrituras en la memoria antes de que afecten la ejecución.
- **ASLR (Address Space Layout Randomization)** → Evita que los atacantes predigan direcciones de memoria explotables.
- **Compilación con Protecciones** 🏗️ →
    
    ```bash
    gcc -fstack-protector -D_FORTIFY_SOURCE=2 -o seguro programa.c
    ```
    

---