## BurpSuite

Herramienta (de PortSwigger) para **interceptar, analizar y modificar** todas las peticiones entre tu navegador y el servidor. Es esencial para **explotar vulnerabilidades** web que requieren manipular el tráfico.

### Flujo básico

1. **Proxy > Intercept** activado.
2. Configura el navegador para que pase el tráfico por el proxy de Burp (normalmente `127.0.0.1:8080`).
3. Envía una petición desde la web: Burp la **intercepta** y te deja editarla antes de que llegue al servidor.
4. **Forward** para dejar pasar, o edita la petición (cookies, headers, body) para alterar el comportamiento.
5. En **HTTP History** quedan registradas todas las peticiones/respuestas para su análisis.

### Usos habituales

- **Interceptar login** y modificar parámetros (token, rol, ID de usuario).
- Editar cookies y cabeceras (p. ej. `X-Forwarded-For`, `Content-Type`).
- Reenviar peticiones para probar SQLi, IDOR, subida de archivos maliciosos, etc.
- Extraer el formato de una petición POST para **Hydra** (mira el apunte de [Hydra](../Fuerza%20Bruta/Hydra.md)).

### Encodear contenido con Burp

Si una reverse shell o un payload no funciona, a veces necesitas **encodearlo** (URL, Base64, etc.). Burp tiene un **Decoder** que te permite codificar/decodificar entre formatos de forma rápida.

> Relacionado: reverse shells en [Aplicaciones](../Utilidades/Aplicaciones.md).