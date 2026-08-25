# Injection — Dockerlabs

> Dificultad: fácil · Vía de ataque: SQLi → credenciales → escalada con binario vulnerable.

## Resumen

| Etapa            | Hallazgo                                        |
| ---------------- | ----------------------------------------------- |
| Escaneo          | Puertos 22 (SSH), 80 (web)                      |
| Enumeración web  | Login vulnerable a **SQL Injection**            |
| Explotación      | Bypass de login → obtenemos credenciales de SSH |
| Escalada         | Binario con `sudo` explotable                   |

## 1. Escaneo de puertos

Primero miramos qué puertos tiene abiertos:

![[Pasted image 20260518162439.png]]

## 2. Escaneo detallado de versiones

Sobre los puertos detectados, sacamos las versiones:

![[Pasted image 20260518162537.png]]

## 3. Ver la web

Comprobamos qué hay en el puerto 80:

![[Pasted image 20260518162707.png]]

Vemos un **login**.

## 4. Buscar directorios ocultos

Probamos con gobuster a ver si hay rutas ocultas:

![[Pasted image 20260518162854.png]]

No vemos gran cosa.

## 5. Probar vhosts

Como no sacamos rutas, probamos virtual hosts (con FFuF/wfuzz):

![[Pasted image 20260518163120.png]]

## 6. SQL Injection

Al ser un login, probamos una **inyección SQL** para saltarnos la autenticación:

```
User: admin
Password: ' or '1'='1
```

- `admin` → el usuario al que queremos acceder.
- `' or '1'='1` → hace que la condición `WHERE user='admin' AND password='...'` sea siempre verdadera.

![[Pasted image 20260518163355.png]]

## 7. Credenciales obtenidas

Logramos entrar como el usuario **dylan**, y la web nos muestra su **password**. Las probaremos en SSH:

![[Pasted image 20260518163438.png]]

## 8. Acceso por SSH

Nos conectamos con las credenciales obtenidas:

```bash
ssh dylan@IP
```

![[Pasted image 20260518163551.png]]

## 9. Escalada de privilegios

Con la shell del usuario `dylan`, buscamos cómo escalar:

![[Pasted image 20260518163751.png]]

Vemos un binario relacionado con `env` con permisos sudo — lo explotamos:

![[Pasted image 20260518163853.png]]

¡Y ya somos **root**!