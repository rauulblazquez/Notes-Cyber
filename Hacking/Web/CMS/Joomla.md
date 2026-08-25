## Joomla como objetivo

**Joomla** es otro CMS muy extendido (escrito en PHP). Como con WordPress, el primer paso es identificar la **versión** y, si es posible, ejecutar un escáner dedicado.

### JoomScan

**joomscan** es la herramienta específica para **encontrar fallos en Joomla** (versión, componentes vulnerables, directorios expuestos, etc.).

Repositorio oficial:
```
https://github.com/OWASP/joomscan
```

> Kali ya incluye los repositorios de `joomscan` en sus paquetes, así que normalmente solo tienes que instalarlo:

```bash
sudo apt install joomscan
```

### Uso básico

```bash
joomscan -u http://IP
```

![[Pasted image 20260707001444.png]]

### Rutas típicas de Joomla

| Ruta                     | Descripción                                    |
| ------------------------ | ---------------------------------------------- |
| `/administrator/`        | Panel de administración                         |
| `configuration.php`      | Archivo de configuración (si está expuesto, fuga credenciales de BD) |

> Como en cualquier CMS, primero **identifica la tecnología** con `whatweb` y luego pasa al escáner específico. Mira [Enumeración web](../Reconocimiento/Enumeracion%20web.md).