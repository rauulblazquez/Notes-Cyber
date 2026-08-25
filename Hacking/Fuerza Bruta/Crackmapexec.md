## CrackMapExec (CME)

Herramienta para hacer **fuerza bruta** y post-explotación contra **Samba / SMB** (y también SMB de Windows, SSH, WinRM, etc.).

### Fuerza bruta sobre un recurso SMB

```bash
crackmapexec smb 172.17.0.2 -u usuarios.txt -p passwords.txt
```

- `-u` → usuario o lista de usuarios.
- `-p` → contraseña o lista de contraseñas.
- Muestra en la salida qué combinación `usuario:password` es **válida** (marcada en `[+]`).

### Verificar usuarios y recursos compartidos

Una vez tienes credenciales válidas, puedes analizar el recurso compartido y volcar la info con `--shares`:

```bash
crackmapexec smb 172.17.0.2 -u 'usuario' -p 'password' --shares
```

### Nota

CME es el sucesor moderno de herramientas como `enum4linux` para validar credenciales y enumerar la topología SMB de forma rápida. Combínalo con los apuntes de [Samba](../Reconocimiento/Enumeracion%20local.md).