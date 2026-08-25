## WhatWeb

**WhatWeb** identifica las **tecnologías** con las que está construida una web: el CMS (WordPress, Joomla...), el servidor, el lenguaje de programación (PHP, ASPX, JS), frameworks, librerías de JavaScript, etc.

Saber con qué está hecha una web cambia completamente lo que probarás después:

- Si es **WordPress** → usarás `wpscan`, plugins, themes...
- Si es **Joomla** → usarás `joomscan`.
- Si es **Drupal** → buscarás `settings.php` o versiones explotables.
- Si es **PHP casero** → probarás LFI, SQLi, subida de archivos...

### Uso

```bash
whatweb http://192.168.1.50
```

### Ejemplo con captura

![[Pasted image 20260705163017.png]]