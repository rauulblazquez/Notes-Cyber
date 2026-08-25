## ¿Qué es Nmap?

**Nmap** es la **herramienta de reconocimiento** por excelencia. Convierte algo abstracto como "la máquina **192.168.1.50**" en una **lista concreta** de cosas que puedes investigar: puertos abiertos, servicios, versiones y sistemas operativos.

### Los dos tipos de "llamada": TCP (educado) vs. UDP (directo)

- **TCP** (Protocolo de Control de Transmisión): es el método educado. Nmap dice "Hola, ¿estás ahí?" y espera un apretón de manos. Si la máquina responde, sabes que hay alguien. Es rápido y fiable para la mayoría de servicios como páginas web (**HTTP**) o terminales remotas (**SSH**).

- **UDP** (Protocolo de Datagramas de Usuario): es más brusco. Nmap lanza un paquete y se queda esperando... y esperando. Muchos servicios **UDP** no responden a menos que les digas la frase exacta. Por eso escanear **UDP** es más lento y frustrante. Pero algunos servicios cruciales como **DNS** (el que traduce nombres a **IPs**) viven aquí. Ignorar **UDP** es como no mirar el sótano.

Cuando Nmap termina, verás algo como "**22/tcp open ssh**". Respira hondo. Esto no significa que puedas entrar. Significa que hay una puerta con un cartel que dice "**SSH**". Ahora toca la parte de detective.

### Escaneo rápido básico

```bash
nmap -p- --open --min-rate 5000 -n -Pn -vvv <IP> -oN escaneo
```

| Parámetro      | Función                                                         |
| -------------- | --------------------------------------------------------------- |
| `-sS`          | Escaneo sutil y silencioso (SYN scan, por defecto con root)      |
| `-sC`          | Sacar mucha información automatizada a través de scripts de nmap |
| `--min-rate 5000` | Ir más rápido en el escaneo                                    |
| `-n`           | No intentar resolución DNS                                       |
| `-vvv`         | Ir mostrando lo que encuentra mientras enumera                   |
| `-Pn`          | No hacer ping para comprobar si está activo                      |
| `-oN`          | Guardar la información en un archivo                             |
| `-sV`          | Detectar versiones de los servicios                              |
| `-sC`          | Ejecutar scripts básicos seguros (enumeración)                   |
| `-p-`          | Escanear todos los puertos TCP                                   |
| `-sU --top-ports 20` | Escanear los puertos UDP más comunes                       |

### Escaneo detallado tras conocer los puertos abiertos

Una vez sabes qué puertos están abiertos (p. ej. el 22, 80 y 443):

```bash
nmap -sCV -p22,80,443 -Pn <IP>
```

> `-sCV` equivale a combinar `-sC` (scripts) + `-sV` (versión). Es el escaneo que te da más información útil para seguir avanzando.

## Timing y Performance

| Plantilla      | Opción | Descripción                       | Velocidad |
| -------------- | ------ | --------------------------------- | --------- |
| **Paranoid**   | `-T0`  | Muy lento, evasión máxima         | 🐌 5min+  |
| **Sneaky**     | `-T1`  | Lento, buena evasión              | 🐢 15s    |
| **Polite**     | `-T2`  | Normal, bajo ancho de banda       | 🚶 5s     |
| **Normal**     | `-T3`  | Default, equilibrado              | 🏃 1s     |
| **Aggressive** | `-T4`  | Rápido, red confiable             | 🚗 300ms  |
| **Insane**     | `-T5`  | Muy rápido, puede perder paquetes ✈️ | 100ms   |

## Interpretación de Resultados

| Estado             | Descripción                                        | Significado                              |
| ------------------ | -------------------------------------------------- | ---------------------------------------- |
| **open**           | Puerto abierto y aceptando conexiones              | Servicio activo y accesible              |
| **closed**         | Puerto accesible pero no hay servicio              | Host vivo pero puerto no usado           |
| **filtered**       | Firewall/IPS bloqueando el puerto                  | No se pudo determinar el estado          |
| **unfiltered**     | Puerto accesible pero estado indeterminado         | Generalmente en escaneos ACK             |
| **open\|filtered** | No se determina si está abierto o filtrado         | Común en escaneos UDP o IP               |
| **closed\|filtered**| No se determina si está cerrado o filtrado        | Muy raro                                 |