## Wfuzz

**Wfuzz** es otra herramienta de **fuzzing** web. Sirve para descubrir subdominios y subdirectorios, a veces como alternativa a **Gobuster** y **FFuF**; su punto fuerte es la **flexibilidad** para fuzzear cualquier parte de la URL o de los headers.

### Enumerar subdominios

```bash
wfuzz -c --hc=404 -w /usr/share/wordlists/subdomains.txt -H "Host: FUZZ.web.com" -u http://IP
```

- `-c` → salida con colores.
- `--hc=404` → **no muestres** las respuestas con estado 404 (ruido).
- `-w` → wordlist.
- `-H` → módulo de fuzzing en el header `Host`.
- `-u` → URL objetivo.
- `FUZZ` → palabra reservada que se sustituye por cada entrada de la wordlist.

![[Pasted image 20260705135840.png]]

### Filtrar resultados

Saldrán muchos datos. Para quedarnos solo con los que devuelven algo, filtramos por el **número de líneas de la respuesta** (`--hl`):

```bash
wfuzz -c --hc=404 --hl=1 -w wordlist.txt -H "Host: FUZZ.web.com" -u http://IP
```

> La lógica es: identificar cuántas líneas tienen las respuestas "falsas" (p. ej. un error 404 tiene 1 línea) y ocultarlas con `--hl=1`. Si es por caracteres se usa `--hh=NNN`.

![[Pasted image 20260705135937.png]]