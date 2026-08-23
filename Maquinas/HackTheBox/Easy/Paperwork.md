Lo primero es iniciar la vpn
![[Pasted image 20260820230159.png]]
Luego realizamos un escaneo básico para ver que puertos están abiertos
![[Pasted image 20260820230608.png]]
Vemos los puertos 22, 80 y 1515, realizamos un escaneo mas detallado a ver que tiene realmente cada uno
![[Pasted image 20260820230730.png]]

| Puerto | Servicio               |
| ------ | ---------------------- |
| 22     | ssh con version 18.8p2 |
| 80     | http nginx 1.28.0      |
| 1515   | ifor protocol          |
Vemos que contiene la web
![[Pasted image 20260820230901.png]]
Descargamos el archivo que sale en azul
![[Pasted image 20260820230953.png]]lo descomprimimos para ver que es
![[Pasted image 20260820231032.png]]
vemos un archivo de python que contiene lo siguiente
![[Pasted image 20260820231059.png]]
esto lo que hace es
- 🖥️ Crea un **servidor TCP** en el puerto `1515`.
- 📥 Recibe datos de un cliente.
- 🔎 Busca una línea que empiece por `J` y obtiene el nombre del trabajo.
- 📝 Lo guarda en `/tmp/archive.log`.
- ⚠️ Usa `subprocess.Popen(..., shell=True)` con ese dato recibido.
- 💥 Si `job_name` es controlable por el cliente y no está sanitizado, **puede existir una inyección de comandos/RCE**.

La parte clave es:
subprocess.Popen(f"echo 'Archive: {job_name}' >> /tmp/archive.log", shell=True)

Lo que hacemos es ejecutar el archivo python
![[Pasted image 20260821002052.png]]
ahora esperamos recibir una conexión de la maquina de htb y ver si podemos obtener algo interesante