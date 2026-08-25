## Gobuster

Herramienta para **fuzzing** de directorios y archivos en servidores web (también sirve para enumerar subdominios/vhosts con `dns` y `vhost`).

### Descubrir subdirectorios y archivos

```bash
gobuster dir -u http://facts.htb -w /usr/share/wordlists/dirb/common.txt
```

- `dir` → modo de enumeración de directorios.
- `-u` → URL objetivo.
- `-w` → wordlist de rutas/archivos.

![[Pasted image 20260705190823.png]]

### Parámetros útiles

| Parámetro    | Función                                        |
| ------------ | ---------------------------------------------- |
| `-x`         | Añadir extensiones (`-x php,txt,bak`)          |
| `-b`         | Excluir códigos de estado (`-b 404`)           |
| `--exclude-length` | Excluir respuestas por tamaño de cuerpo  |

> También puedes usarlo para comprobar si existe algún **plugin** en una web de WordPress (añadiendo el nombre del plugin a `/wp-content/plugins/`).