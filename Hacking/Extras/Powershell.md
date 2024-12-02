---
Extra: 🪜 Powershell
fecha: 2024-12-02
tags:
  - extra
---

## Powershell:


Ruta del ejecutable -> This PC/Local Disk (C:)/Windows/WindowsPowerShell/vX/Powershell.exe

Esta instrucción nos permite saber si estamos es un entorno de 64 bit ->

```Powershell
[Enviroment]::Is64BitProcess
#Devuelve true si es correcto
```

Podemos crear un script en powershell que no muestre el propio powershell si añadimos -WindowStyle Hidden al script antes de ejecutarlo. ej

```powershell
powershel.exe -WindowStyle Hidden calc.exe
```

La ejecución del script en cmd puedo darnos problemas, para ello podemos añadir -ep Bypass. ej:

```Powershell
poweshell.exe -ep Bypass .\script.ps1
```

Con el siguiente comando podemos listar los procesos del sistema ->

```powershell
powershell.exe -Command Get-Process

#También podemos conseguir los logs de un proceso pero necesitaremos privilegios elevados...
powershell.exe -ep Unrestricted -Command "& { Get-EventLog -LogName security }"

#También podemos poner comandos codificados
powershell.exe -encondedCommand $calc.exe

#Para buscar los diferentes comandos con parte de una string ->
powershell.exe Get-Command -Name *Firewall*
```

Los cmdlets vienen pregenerados en los ordenadores con powershell, son pequeños fragmentos de código destinados a una función, pero también podemos crear los nuestros.

Generalmente siguen la estructura verbo-comando (get-help) también podemos incluir el símbolo | para redirigir la salida del comando e incluso darle formato.

```powershell
#Conseguir todos los procesos únicos y listar solo sus nombres
Get-Process | Sort-Object -Unique | Select-Object ProcessName
```

Con el símbolo de > también podemos redirigir el output para dejarlo en un archivo. También podemos extender la información añadiendo el formato lista "Format-List".

```powershell
#Conseguir el path donde se ubica un proceso
Get-Process firefox | Sort-Object -Unique | Format-List Path
```

```powershell
#Conseguir la información del sistema operativo
Get-WmiObject -class win32_operatingsystem | select -Property *

#Si queremos información más detallada podemos añadir lo siguiente
Get-WmiObject -class win32_operatingsystem | select -Property * | Select-Object version
```

