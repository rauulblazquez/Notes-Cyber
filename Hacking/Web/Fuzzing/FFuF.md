## FFuF (Fuzz Faster U Fool)

Herramienta de **fuzzing** muy rápida. Sirve para descubrir **vhosts**, **subdirectorios** y **subdominios**.

> ⚠️ Genera **muchísimo ruido** al ser un escaneo activo. Usa con cuidado en entornos donde importe no ser detectado.

### Reconocer VHosts (Host header)

```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -u http://172.17.0.2 \
  -H "Host: FUZZ.172.17.0.2"
```

- `-w` → wordlist.
- `-u` → URL objetivo.
- `-H "Host: FUZZ.172.17.0.2"` → inyecta cada palabra de la wordlist en el campo `Host`.

![[Pasted image 20260511203826.png]]

### Reconocer subdirectorios

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-lowercase-2.3-small.txt \
  -u http://172.17.0.2/FUZZ \
  -recursion -e .php,.html -v -o ffufscan
```

- `-recursion` → vuelve a fuzzerear dentro de los directorios encontrados.
- `-e .php,.html` → añade extensiones a cada prueba.
- `-v` → salida verbosa.
- `-o ffufscan` → guarda el resultado en un archivo.

![[Pasted image 20260708005130.png]]