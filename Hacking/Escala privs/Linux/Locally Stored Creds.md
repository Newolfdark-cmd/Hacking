
Directorio interesante para buscar:
- /var/www/html

```bash
#Ejmeplos para detectar posibles usuarios/passwd
grep -nr "db_user"
cat /local/config/database.inc.php
```

Si encontramos cualquier passwd en un archivo es muy interesante probar esa contraseña para el usuario root y ver si así podemos escalar privs.