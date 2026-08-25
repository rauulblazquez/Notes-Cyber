## Sublist3r

**Sublist3r** es una herramienta de **OSINT** que enumera subdominios de un dominio consultando **múltiples fuentes pasivas** (buscadores, DNS públicos, bases de datos de certificados...). Al ser pasiva, **genera poco ruido** (ideal para no ser detectado).

Está disponible en los repositorios de Kali/Linux:

```bash
sudo apt install sublist3r
```

### Uso

```bash
sublist3r -d dominios.com
```

- `-d` → dominio a enumerar.

![[Pasted image 20260705141101.png]]

### Ejecución

![[Pasted image 20260705141134.png]]

> Como es pasiva, funciona bien contra un objetivo donde queramos evitar el escaneo activo. Para un marco más completo combínala con [Subfinder](../Subdominios/Subfinder.md) y [dnsrecon](../Subdominios/dnsrecon.md).