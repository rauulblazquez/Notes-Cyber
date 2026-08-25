## Curl

`curl` es una herramienta para hacer peticiones HTTP desde la terminal. Es esencial para el reconocimiento web porque te permite ver el **código fuente**, las **cabeceras** y hacer **peticiones personalizadas** (POST, PUT, DELETE) sin usar el navegador.

### Uso básico

```bash
curl http://info.cern.ch/
```

Imprime el código fuente de la web en la terminal.

### Opciones principales

| Opción | Descripción                                                  | Comando                                                                                                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `-O`   | Descarga el archivo indicado en vez de mostrarlo             | `curl -O http://info.cern.ch/index.html`                                                                                                    |
| `-s`   | Silencio: no muestra barras de progreso ni errores           | `curl -s -O http://info.cern.ch/index.html`                                                                                                 |
| `-k`   | Omitir la comprobación del certificado SSL                   | `curl -k https://info.cern.ch/` (⚠️ usa minúscula `-k`, no `-K`)                                                                            |
| `-I`   | Ver solo las cabeceras HTTP (versión del servidor, etc.)     | `curl -I http://info.cern.ch/`                                                                                                              |
| `-u`   | Proporcionar credenciales                                    | `curl -u admin:admin http://<SERVER_IP>:<PORT>/`                                                                                            |
| `-X`   | Especificar un método HTTP (GET/POST/PUT/DELETE...)          | `curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'` |
| `-d`   | Enviar datos en el cuerpo de la petición (para POST)         | Se usa junto a `-X POST` con los datos en `'...'`                                                                                          |
| `-H`   | Añadir una cabecera personalizada (`Header`)                 | `curl -H 'Content-Type: application/json' http://<IP>/`                                                                                     |

### Ejemplo de petición POST con JSON

```bash
curl -X POST http://192.168.1.50/api/login \
  -d '{"username":"admin", "password":"admin"}' \
  -H 'Content-Type: application/json'
```