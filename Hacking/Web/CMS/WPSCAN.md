## WPScan

**WPScan** es una herramienta dedicada a **enumerar y atacar** sitios WordPress. Detecta versiones, temas, plugins, usuarios, vulnerabilidades conocidas y permite fuerza bruta.

### Escaneo básico

```bash
wpscan --url http://IP
```

### Enumerar usuarios/plugins

```bash
wpscan --url http://IP -e u,p
```

- `-e` → habilita la enumeración:
  - `u` → **usuarios**
  - `p` → **plugins**
  - `t` → **temas (themes)**

![[Pasted image 20260514231932.png]]

> En el apunte de [WordPress](WordPress.md) tienes las capturas del proceso completo.

### Uso de API token

WPScan necesita un token de la web oficial para mostrar **vulnerabilidades conocidas** (la base de datos local y el research). Crea una cuenta en wpscan.com y obtén tu **API token**:

![[Pasted image 20260705190551.png]]

Añádelo al escaneo:

```bash
wpscan --url http://IP -e u,p --api-token TU_TOKEN
```

> ⚠️ Si añades `--plugins-detection aggressive` la herramienta genera **muchísimo ruido**. Úsalo solo si con el análisis pasivo no obtienes suficiente información:

![[Pasted image 20260705190618.png]]

### Fuerza bruta de usuarios

```bash
wpscan --url http://IP --usernames admin --passwords /usr/share/wordlists/rockyou.txt
```

### Wordlist con nombres de plugins

Puedes descargar una lista ampliada de nombres de plugins:

```
https://github.com/Perfectdotexe/WordPress-Plugins-List
```

![[Pasted image 20260705190759.png]]