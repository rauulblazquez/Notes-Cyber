## Escalada de privilegios (básico)

Conseguir acceso a una máquina casi nunca nos da root al instante. Normalmente entramos como un usuario con una shell, y toca **subir privilegios** hasta root (u otro usuario con más permisos). Aquí se recogen las comprobaciones iniciales.

### Primeras comprobaciones

| Acción                      | Comando                                        | Por qué nos interesa                                        |
| --------------------------- | ---------------------------------------------- | ----------------------------------------------------------- |
| Qué puedo ejecutar como root| `sudo -l`                                      | Revela binarios que podemos correr con permisos elevados   |
| Binarios SUID               | `find / -perm -4000 -type f 2>/dev/null`       | Se ejecutan con permisos de su propietario (a menudo root) |
| Capacidades                 | `getcap -r / 2>/dev/null`                      | Binarios con capabilities inesperadas                      |
| Tareas programadas          | `cat /etc/crontab` y `ls /etc/cron.*`           | Scripts que se ejecutan con privilegios                    |
| Procesos                    | `ps aux`                                        | Servicios que pueden correr como root                      |

### sudo -l

`sudo -l` nos dice qué comandos podemos ejecutar como **root** o como **otros usuarios** sin conocer su contraseña:

```bash
sudo -l
```

![[Pasted image 20260825211519.png]]

> Si un binario aparece aquí (p. ej. `python3`, `find`, `vim`), busca en [GTFObins](../GTFObins.md): suelen tener una técnica listada para escalar a root.