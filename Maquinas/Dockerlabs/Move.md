# Move — Dockerlabs

> Vía de ataque: FTP anónimo → archivo keepass → LFI → credenciales → escalada con binario editable.

## Resumen

| Etapa            | Hallazgo                                                    |
| ---------------- | ----------------------------------------------------------- |
| Escaneo          | FTP, web :80, app :3000 (PPP) y SSH                         |
| FTP anónimo      | Base de datos de **Keepass**                                |
| Fuzzing          | Rutas con **LFI** y solapamiento de archivos                |
| Credenciales     | Usuario `freddy` + password                                 |
| Escalada         | Script Python editable ejecutable como root                 |

## 1. Desplegar y escanear

Desplegamos la máquina:

![[Pasted image 20260514235428.png]]

Vemos la IP y escaneamos puertos abiertos:

![[Pasted image 20260514235515.png]]

## 2. Escaneo completo

Hacemos un escaneo más completo:

![[Pasted image 20260514235639.png]]

Vemos:
- Un servicio **FTP** (con **anonymous** habilitado).
- Dos web: una en el **:80** y otra en el **:3000** (PPP).
- Un **SSH**.

## 3. FTP anónimo

Accedemos al FTP de forma anónima:

```bash
ftp 172.17.0.2
Name: anonymous
```

![[Pasted image 20260514235752.png]]

Dentro vemos el directorio de **mantenimiento** y lo abrimos:

![[Pasted image 20260514235835.png]]

Contiene una **base de datos de passwords de Keepass**:

![[Pasted image 20260515000101.png]]

La abrimos:

![[Pasted image 20260515000358.png]]

Pide una contraseña — lo dejamos aparcado.

## 4. Explorar la web :80

Vemos la web del puerto 80:

![[Pasted image 20260515000506.png]]

Es la página por defecto de **Apache**, sin mucho interés.

## 5. Explorar la web :3000

Probamos la app del puerto 3000:

![[Pasted image 20260515000533.png]]

Nos redirige a un **login** y probamos credenciales comunes:

![[Pasted image 20260515222308.png]]

Accedemos con **admin:admin**:

![[Pasted image 20260515222347.png]]

La configuración no revela nada útil.

## 6. Directorios ocultos en :3000

Buscamos rutas ocultas en ese puerto:

![[Pasted image 20260515223937.png]]

Encontramos varias; nos interesa una en concreto:

![[Pasted image 20260518160637.png]]

## 7. LFI (Local File Inclusion)

La web está en mantenimiento, pero obtenemos la **ruta de acceso**:

![[Pasted image 20260518161148.png]]

Probamos a **leer archivos locales** (LFI) y descubrimos el usuario **freddy**:

![[Pasted image 20260518161226.png]]

Probamos acceder con `freddy` y la password que hemos encontrado:

![[Pasted image 20260518161312.png]]

## 8. Acceso como freddy

Entramos como `freddy` y buscamos cómo escalar:

![[Pasted image 20260518161431.png]]

## 9. Escalada de privilegios

Podemos ejecutar el **script Python de mantenimiento** como root. Contiene un `print` y podemos **modificarlo**, así que inyectamos una shell:

![[Pasted image 20260518161604.png]]

Lo lanzamos, nos pide la password del usuario y obtenemos **root**:

![[Pasted image 20260518161649.png]]