## Subfinder

**Subfinder** es una herramienta **pasiva** para descubrir subdominios consultando fuentes OSINT (certificados, DNS, bases de datos públicas). Similar a Sublist3r, pero suele ser más rápida y completa.

Viene con los repositorios de Kali/Linux:

```bash
sudo apt install subfinder
```

### Uso

```bash
subfinder -d dominios.com
```

- `-d` → dominio a enumerar.

![[Pasted image 20260705141034.png]]

### Ejecución

![[Pasted image 20260705140632.png]]

> Combínala con [Sublist3r](Sublist3r.md): cada herramienta usa fuentes ligeramente distintas, y al cruzarlas sueles encontrar subdominios que por separado pasarías por alto.