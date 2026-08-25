# Conceptos y trucos del navegador

Apuntes misceláneos de conceptos, ideas importantes y utilidades que no encajan en una herramienta concreta.

## Hacking Web

### Cookies

Las **cookies** guardan el estado de tu sesión (usuario, rol, carrito...). En un test conviene **manipularlas** para probar cambios de rol o de usuario:

- Clic derecho → **DevTools → Storage → Cookies** y modifica los valores.

> Si no surte efecto, hazlo en una **ventana de incógnito** (evita que el navegador o las cachés interfieran).

### Plugin "File Manager" en WordPress

Si accedes al **panel de administración** de WordPress, puedes añadir el plugin **file manager** para **gestionar archivos** del servidor (subir/editar) — útil para dejar una reverse shell. Ver [WordPress](../Web/CMS/WordPress.md).

## FTP

### Habilitar el servicio vsftpd (servidor)

1. Instalar el paquete:
   ![[Pasted image 20260705154724.png]]
2. Arrancar y habilitar el servicio:
   ![[Pasted image 20260705154752.png]]
3. Para cambiar el **puerto**, edita `/etc/vsftpd.conf`:
   ![[Pasted image 20260705155720.png]]
   Agrega la línea `listen_port=XXXX` si no existe:
   ![[Pasted image 20260705155810.png]]
4. Guarda, sal y **reinicia el servicio**.

> Cuidado: habilitar un FTP en tu Kali es para **laboratorio propio**. En un pentest usamos el FTP de la víctima (y su fuerza bruta con [Hydra](../Fuerza%20Bruta/Hydra.md)).

## SSH

1. Instala el servidor:
   ![[Pasted image 20260705160443.png]]
2. Inícialo y comprueba que está activo:
   ![[Pasted image 20260705160516.png]]

## Parámetros del navegador (búsquedas ocultas)

En algunos sitios, el parámetro `/?s="lo que sea"` sirve para hacer **búsquedas** (típico de WordPress). Podemos aprovecharlo para ver contenido "huérfano".

1. Copia el nombre de una lección/entrada:
   ![[Pasted image 20260706235822.png]]
2. Usa el parámetro `/s="nombre de la lección"` para ver todas las entradas relacionadas:
   ![[Pasted image 20260706235913.png]]
3. Si encuentras entradas **huérfanas**, a veces puedes ver su contenido.

> Para corregirlo en WordPress: en **Temas** revisa las entradas huérfanas y elimina la correcta.

## Impacket (compartir recurso en tu máquina)

**Impacket** incluye herramientas para jugar con SMB. Por ejemplo, levantar un **recurso compartido** temporal en tu Kali:

```bash
impacket-smbserver recurso $(pwd) -smb2support
```

- `recurso` → nombre del recurso compartido.
- `$(pwd)` → expone el **directorio actual**.
- `-smb2support` → aumenta la **compatibilidad** con clientes modernos.

![[Pasted image 20260705143705.png]]

> Esto es muy útil para transferir la reverse shell/exploit a la máquina víctima vía `\\TU_IP\recurso`.