# Orion — HackTheBox (Easy)

> Vía de ataque: web → panel admin con versión vulnerable → Metasploit → `.env` con credenciales de BD → crackear hash → SSH → CVE-2026-24061 en telnet → root.

## Resumen

| Etapa            | Hallazgo                                                |
| ---------------- | ------------------------------------------------------- |
| Escaneo          | Puertos 22 (SSH) y 80 (web)                             |
| Enumeración      | `/admin` con versión vulnerable                         |
| Explotación      | Exploit en Metasploit → shell                           |
| Credenciales     | `.env` con `db_user` / `db_password` en la BD           |
| Hash             | Se crackea la password de `adam`                        |
| SSH              | CVE-2026-24061 en **telnet** → shell de **root**        |

## 1. Escaneo inicial

Realizamos un escaneo para ver qué puertos tenemos activos:

![[Pasted image 20260707002352.png]]

## 2. Escaneo detallado

Sabiendo que tenemos el **22 y 80**, realizamos un escaneo más detallado:

![[Pasted image 20260707002450.png]]

## 3. Ver la web (puerto 80)

![[Pasted image 20260707002546.png]]

## 4. Buscar directorios ocultos

Miramos si existe algún subdirectorio oculto detrás de la web:

![[Pasted image 20260707002859.png]]

Apreciamos **`admin`** y **`assets`** con código **301** (redirección). Los comprobamos:

![[Pasted image 20260707002936.png]]

## 5. Login con versión vulnerable

Al entrar en `/admin` vemos un **login** y además nos muestra una **versión**:

![[Pasted image 20260707003309.png]]

## 6. Buscar exploit en Metasploit

Con esa versión comprobamos en `msfconsole` si existe algún exploit:

```bash
msfconsole
msf6> search <producto> <versión>
```

Encontramos uno y lo usamos:

![[Pasted image 20260707003415.png]]

![[Pasted image 20260707003441.png]]

Configuramos lo necesario (`RHOSTS`, `LHOST`, etc.) y lo lanzamos:

![[Pasted image 20260707003701.png]]

## 7. Obtener shell

Esperamos a que termine y nos diga que funcionó para lanzar una shell:

![[Pasted image 20260707003911.png]]

## 8. Buscar usuarios

Ahora buscamos usuarios / credenciales:

![[Pasted image 20260707003940.png]]

Vemos que existe el usuario **`adam`**. Seguimos mirando si hay algo más interesante:

![[Pasted image 20260707004124.png]]

## 9. Configuración en `.env`

Apreciamos un **`.env`** (archivo de entorno que suele contener secretos). Vemos qué contiene:

![[Pasted image 20260707004153.png]]

Encontramos `db_user` y `db_password`. Accedemos a la base de datos:

![[Pasted image 20260707004342.png]]

## 10. Acceso a la BD

Creamos una shell que podamos ver y accedemos a la base de datos:

![[Pasted image 20260707004515.png]]

Miramos qué **tablas** existen:

![[Pasted image 20260707004616.png]]

Vemos un montón, pero buscamos la de **users** y vemos su estructura:

![[Pasted image 20260707004933.png]]

Nos interesan `id`, `email` y `password`, así que vemos esos tres campos:

![[Pasted image 20260707005130.png]]

## 11. Crackear la password

Vemos una **password hasheada** (ya crackeada en la vista) — la copiamos y la desciframos con **John the Ripper / hashcat**:

![[Pasted image 20260707005234.png]]
![[Pasted image 20260707005635.png]]

## 12. Acceso por SSH

Con la password obtenida, nos conectamos por **SSH** al usuario `adam`:

```bash
ssh adam@IP
```

Vemos que funciona y sacamos la **flag** de ese usuario. Buscamos cómo escalar privilegios:

![[Pasted image 20260707005917.png]]

## 13. Escalada a root

Vemos que tiene **telnet** disponible. Probamos a explotar el **CVE-2026-24061** sobre telnet:

![[Pasted image 20260707005954.png]]

Nos da una **sesión como root** — sacamos la flag final.