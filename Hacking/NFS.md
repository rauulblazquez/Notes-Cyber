. ¿Qué es NFS?

- **Definición:** _Network File System_ (Sistema de Archivos de Red).


**Listar recursos compartidos (Showmount):**

```
showmount -e IP_OBJETIVO
```


**Montar en Linux:**

```
sudo mount -t nfs IP_OBJETIVO:/ruta/remota /punto/montaje/local -o nolock
```


