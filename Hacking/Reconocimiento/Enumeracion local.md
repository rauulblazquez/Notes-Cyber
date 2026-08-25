## Enumeración local: El ritual que debes repetir siempre

Ya tienes **shell**. Ahora toca **enumeración local**: mirar desde dentro del sistema igual que mirabas desde fuera con **Nmap**.

Antes de explotar nada, responde preguntas: qué **usuario** soy, a qué **grupos** pertenezco, qué comandos puedo ejecutar como **root**, qué **procesos** corren y dónde puede haber **credenciales**.

La **enumeración local** no es memorizar comandos. Es una actitud de detective: entender el sistema antes de tocarlo.

### Comandos esenciales

| Comando                                                       | Descripción                                        | Uso                    |
| ------------------------------------------------------------- | -------------------------------------------------- | ---------------------- |
| `ping -c1 <IP>`                                               | Ver si un sistema está levantado o no              | `ping -c1 192.168.1.1` |
| `whoami` / `id` / `groups`                                    | Identidad: usuario, UID/GID y grupos               | `whoami`               |
| `cat /etc/passwd` / `ls /home`                                | Listar usuarios del sistema                        |                        |
| `ps aux`                                                      | Ver procesos en ejecución                          |                        |
| `ss -tulpn` / `netstat -tulpn`                                | Ver conexiones y puertos en escucha                |                        |
| `getcap -r / 2>/dev/null`                                     | Buscar binarios con capacidades (capabilities)     |                        |
| `find / -perm -4000 -type f 2>/dev/null \| xargs ls -la`      | Buscar binarios con **SUID** (escalada)            |                        |
| `cat <fichero>`                                               | Ver el contenido de un fichero                     | `cat /etc/passwd`      |

### Buscar binarios SUID

```bash
find / -perm -4000 -type f 2>/dev/null
```

Los binarios SUID se ejecutan con los permisos de su propietario (normalmente root), lo que es un vector clásico de **escalada de privilegios**. Consulta [GTFObins](../Escalada/GTFObins.md) para saber cómo abusar de ellos.

### Listar capacidades

```bash
getcap -r / 2>/dev/null
```

Imprimir las capabilities puede revelar binarios con permisos inesperados (p. ej. `python3` con `cap_setuid`).

> ⚠️ Fíjate: existe una webshell en el propio Kali y también el binario de **netcat**. Cuando consigas una shell, compárala con lo que ya tienes en local para anticipar qué herramientas habrá disponibles.
>
> ![[Pasted image 20260705143330.png|350]]
> ![[Pasted image 20260705143428.png|404]]

### Directorios

| Ruta  | Descripción                                                        |
| ----- | ------------------------------------------------------------------ |
| `/opt` | Destinado a la instalación de **paquetes de software adicionales** |

### Puertos

| Nombre | Puerto | Descripción                                                                                |
| ------ | ------ | ------------------------------------------------------------------------------------------ |
| _ppp_  | 3000   | Usado en desarrollo de software para ejecutar servidores web y aplicaciones locales         |

### Conceptos

#### CMS
Gestor de contenidos para la creación y administración de contenidos digitales. Ejemplo: **WordPress**.

#### Shell (Meterpreter)
Si estamos en una shell de **meterpreter**, con el siguiente comando podremos ver una shell interactiva:

![[Pasted image 20260707004438.png]]

#### Samba

Para acceder de forma **anónima** a los recursos compartidos:

```bash
smbclient -L //172.17.0.2 -N
```

![[Pasted image 20260708010834.png]]

Para **enumerar usuarios** de samba:

```bash
enum4linux -a 172.17.0.2
```