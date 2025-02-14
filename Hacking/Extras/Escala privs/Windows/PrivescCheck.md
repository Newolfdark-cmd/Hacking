Script automatizado para detectar vectores de escalada de privilegios en máquians windows.

En el ejemplo se ha ejecutado así (el script estaba en C:\Users\student\Desktop\Powersploit\Privesc):

Comando para ejecutar el script:
```powershell
powershell -ap bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"
```

En este ejemplo se puede observar lo siguiente:

![[Pasted image 20250214092415.png]]

El mismo caso que en PowerUp así que el procedimiento sería el mismo que el visto anteriormente (preferible PowerUp).