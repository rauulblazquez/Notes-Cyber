# Paperwork — HackTheBox (Easy)

> Vía de ataque: análisis del binario que ejecuta el puerto 1515 → comando con `shell=True` → **inyección de comandos (RCE)**.

> ⚠️ Este writeup está **en progreso** (acceso inicial analizado); queda completar la explotación de la inyección.

## Resumen

| Etapa            | Hallazgo                                                        |
| ---------------- | --------------------------------------------------------------- |
| Escaneo          | Puertos 22 (SSH), 80 (web), 1515 (protocolo propio)             |
| Análisis         | Binario Python en la web escucha en 1515 con `subprocess`       |
| Vulnerabilidad   | `shell=True` concatenando `job_name` → **RCE / command injection** |

## 1. Iniciar VPN y escanear

Lo primero es iniciar la VPN:

![[Pasted image 20260820230159.png]]

Escaneo básico de puertos abiertos:

![[Pasted image 20260820230608.png]]

Vemos los puertos **22, 80 y 1515**. Hacemos un escaneo más detallado de cada uno:

![[Pasted image 20260820230730.png]]

| Puerto | Servicio               |
| ------ | ---------------------- |
| 22     | ssh con versión 18.8p2 |
| 80     | http nginx 1.28.0      |
| 1515   | ifor protocol          |

## 2. Ver la web y descargar el archivo

Vemos qué contiene la web:

![[Pasted image 20260820230901.png]]

Descargamos el archivo que aparece en azul:

![[Pasted image 20260820230953.png]]

Lo descomprimimos para ver su contenido:

![[Pasted image 20260820231032.png]]

Vemos un archivo **Python**:

![[Pasted image 20260820231059.png]]

## 3. Análisis: qué hace el script

El script hace lo siguiente:

- 🖥️ Crea un **servidor TCP** en el puerto `1515`.
- 📥 Recibe datos de un cliente.
- 🔎 Busca una línea que empiece por `J` y obtiene el **nombre del trabajo**.
- 📝 Lo guarda en `/tmp/archive.log`.
- ⚠️ Usa `subprocess.Popen(..., shell=True)` con ese dato recibido.
- 💥 Si `job_name` es controlable por el cliente y no está sanitizado → **inyección de comandos / RCE**.

La parte clave es:

```python
subprocess.Popen(f"echo 'Archive: {job_name}' >> /tmp/archive.log", shell=True)
```

Al concatenar `job_name` dentro de un comando `echo` que se ejecuta con `shell=True`, un nombre de trabajo malicioso (p. ej. `foo'; id #`) se **interpreta como más comandos**.

## 4. Ejecución del script

Ejecutamos el archivo Python (en local o enviándolo a la víctima):

![[Pasted image 20260821002052.png]]

Ahora esperamos que la máquina de HTB se conecte al puerto 1515 y ver si podemos obtener algo interesante.

> **Siguiente paso sugerido:** conectar un netcat (`nc <IP> 1515`) y enviar una línea `J` con un `job_name` que inyecte un comando (p. ej. un `id` o una reverse shell) para confirmar la RCE.