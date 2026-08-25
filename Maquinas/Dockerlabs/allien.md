# Allien — Dockerlabs

> Vía de ataque: SMB/IPC → token JWT en un share → fuerza bruta → compartidos privados → credenciales → reverse shell → escalada con `/service`.

## Resumen

| Etapa                | Hallazgo                                              |
| -------------------- | ----------------------------------------------------- |
| Escaneo              | Puertos 22, 80, 139 (SMB), 443                        |
| SMB anónimo          | Share `myshare` con un **token JWT**                  |
| JWT + fuerza bruta   | Usuario + password                                    |
| Recursos privados    | Credenciales → acceso a otras carpetas/recurso        |
| Web + reverse shell  | Subida de shell → sesión como `www-data`              |
| Escalada             | Binario `/service` con sudo → root (ver GTFObins)     |

## 1. Desplegar y escaneo

Habilitamos Docker con la máquina victima:

![[Pasted image 20260708005917.png]]

Escaneo sencillo de puertos activos:

![[Pasted image 20260708010008.png]]

Vemos 22, 80, 139 y 443. Sospechamos un **Windows/entorno** con SMB.

## 2. Ver la web (80)

Accedemos al puerto 80:

![[Pasted image 20260708010057.png]]

Hay un **login** y debajo un "registrate" en negrita, pero el botón no carga. Buscamos subdominios detrás de la web (no vemos nada interesante).

## 3. SMB anónimo

Probamos el recurso de **samba**: enumeramos recursos compartidos:

```bash
smbclient -L IP -N    # listar
enum4linux -a IP      # enumerar usuarios/grupos
```

![[Pasted image 20260708010905.png]]

Encontramos varios recursos interesantes:

| Recurso   | Comentario         |
| --------- | ------------------ |
| `myshare` | carpeta compartida |
| `backup24`| Privado            |
| `home`    | Produccion          |

El recurso **`myshare`** se puede acceder de forma anónima, vemos qué contiene:

![[Pasted image 20260708011102.png]]
![[Pasted image 20260708011114.png]]

## 4. Token JWT

Dentro vemos una cadena con forma de **token JWT**. Lo descodificamos:

![[Pasted image 20260708011156.png]]

Descodificado revela un **usuario**. Con él probamos **fuerza bruta** para sacar la password y acceder al recurso privado:

![[Pasted image 20260708011442.png]]

Esperamos a que nos dé una password válida:

![[Pasted image 20260708011457.png]]

## 5. Acceso a recursos privados

Tenemos un usuario y password. Accedemos con esas credenciales:

![[Pasted image 20260708011603.png]]
![[Pasted image 20260708011611.png]]

Seguimos explorando hasta encontrar algo de interés:

![[Pasted image 20260708011709.png]]

Vemos **notes** (algo raro) y unos **usuarios con credenciales**. Los probamos en **SSH** porque hay un usuario administrador:

![[Pasted image 20260708011854.png]]

## 6. Acceso inicial

Miraremos si podemos hacer algo con ese usuario:

![[Pasted image 20260708011945.png]]

Vemos que no, así que, sabiendo que hay una web, **subimos una reverse shell** para acceder como `www-data`:
- Modificamos la **IP** y el **puerto** al payload.
- Lo subimos a la máquina victima y lo ejecutamos.
- Ponemos la ruta de la reverse shell en la web y, en la terminal a la escucha, vemos la sesión entrar:

![[Pasted image 20260708012249.png|555]]

## 7. Escalada a root

Con `www-data`, vemos si podemos ejecutar algo como root:

![[Pasted image 20260708012502.png]]

Encontramos `/service` con permisos sudo. Mirando **GTFObins** ([Escalada/GTFObins](../Hacking/Escalada/GTFObins.md)) conseguimos una **shell como root**.