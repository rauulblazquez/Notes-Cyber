## Enumeración web: La regla de oro

### Mirar antes de tocar

La enumeración web es el arte de **observar pacientemente antes de actuar**.

- Observar la página principal sin prisas.
- Hacer clic en todos los enlaces visibles.
- Probar acciones simples como formularios, botones y búsquedas.
- Prestar atención a los mensajes de error.
- Tomar nota de cada comportamiento sospechoso.

Toda página web está hecha de **HTML, CSS y JavaScript**. Ese código viaja hasta tu navegador y puedes verlo con **clic derecho > "Ver código fuente de la página"** o con **Ctrl+U**.

Los programadores a veces dejan notas para ellos mismos en forma de comentarios HTML. A veces son inofensivas, pero otras revelan cosas como "**backup** temporal en **/backup.zip**" o "el panel de **admin** está en **/admin-dev**".

También puedes encontrar enlaces a archivos internos, rutas antiguas que siguen funcionando o nombres de archivos que dan pistas sobre la estructura del servidor.

### Las cabeceras invisibles: Headers HTTP

Cuando tu navegador pide una página, el servidor responde con la página y con información oculta llamada **cabeceras HTTP**.

Puedes ver estas cabeceras con:

```bash
curl -I http://192.168.1.50
```

Busca: tecnologías, cookies, versiones y **redirecciones raras**.

### Con qué está construida la web: Identificar tecnologías

Saber si una web usa **WordPress, Drupal, PHP casero** o una aplicación **Node.js** cambia completamente lo que vas a probar después.

```bash
whatweb http://192.168.1.50
```

También puedes fijarte en:

- Extensiones como **`.php`**, **`.aspx`** o **`.js`**.
- El **favicon**.
- Mensajes de error.
- Rutas típicas (`/wp-admin`, `/administrator`, `/sites/default`).

### Siguiente paso según la tecnología detectada

| Si detectas... | Ve a...                                              |
| -------------- | ---------------------------------------------------- |
| WordPress      | [WordPress](../Web/CMS/WordPress.md) y `wpscan`      |
| Joomla         | [Joomla](../Web/CMS/Joomla.md) y `joomscan             |
| Drupal         | [Drupal](../Web/CMS/Drupal.md)                          |
| Backups/rutas  | Fuzzing con [Gobuster](../Web/Fuzzing/Gobuster.md) o FFuF |
| Formularios    | Probar [SQLi](../Web/Inyecciones/SQL.md) y fuerza bruta |