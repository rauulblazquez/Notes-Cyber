# Hacking — Índice por metodología

> Mapa de contenidos que conecta la **teoría** con las **máquinas** donde se aplica. Sigue el flujo típico de un pentest: reconocimiento → enumeración → explotación → escalada.

## 🗺️ Flujo de trabajo

```
Reconocimiento → Enumeración → Explotación → Escalada de privilegios
```

## 1. Reconocimiento 🕵️

| Tema                  | Notas                                                          |
| --------------------- | -------------------------------------------------------------- |
| Escaneo de puertos    | [Nmap](Reconocimiento/Nmap.md)                                 |
| Enumeración local     | [Enumeración local](Reconocimiento/Enumeracion%20local.md)     |
| Enumeración web       | [Enumeración web](Reconocimiento/Enumeracion%20web.md)         |
| Peticiones HTTP       | [Curl](Reconocimiento/Curl.md)                                 |
| Detección tecnología  | [WhatWeb](Reconocimiento/WhatWeb.md)                           |

## 2. Web — por tipo de ataque 🌐

### Fuzzing & herramientas 🔎

| Tema               | Notas                                   |
| ------------------ | --------------------------------------- |
| Directorios (dir)  | [Gobuster](Web/Fuzzing/Gobuster.md)     |
| Vhosts/subdominios | [FFuF](Web/Fuzzing/FFuF.md)             |
| Interceptar tráfico| [BurpSuite](Web/Fuzzing/BurpSuite.md)   |
| Subdominios        | [Subdominios](Web/Subdominios/)         |

### Inyecciones 💉

| Técnica            | Notas                                |
| ------------------ | ------------------------------------ |
| SQL Injection      | [SQL](Web/Inyecciones/SQL.md)        |
| Cross-Site Scripting | [XSS](Web/Inyecciones/XSS.md)      |
| LFI / Path traversal | [LFI](Web/Inyecciones/LFI.md)     |

### CMS 💥

| CMS      | Notas                                  |
| -------- | -------------------------------------- |
| WordPress| [WordPress](Web/CMS/WordPress.md) · [WPScan](Web/CMS/WPSCAN.md) |
| Joomla   | [Joomla](Web/CMS/Joomla.md)            |
| Drupal   | [Drupal](Web/CMS/Drupal.md)            |

### Subdominios

- [wfuzz](Web/Subdominios/wfuzz.md) · [Sublist3r](Web/Subdominios/Sublist3r.md) · [Subfinder](Web/Subdominios/Subfinder.md) · [dnsrecon](Web/Subdominios/dnsrecon.md) · [DNSDumpster](Web/Subdominios/DNSDUMPSTER.md)

## 3. Fuerza bruta 🔓

| Herramienta         | Notas                                          |
| ------------------- | ---------------------------------------------- |
| Fuerza bruta general| [Hydra](Fuerza%20Bruta/Hydra.md)               |
| SMB/Samba           | [CrackMapExec](Fuerza%20Bruta/Crackmapexec.md) |

## 4. Escalada de privilegios ⬆️

### Linux

| Técnica                  | Notas                                   |
| ------------------------ | --------------------------------------- |
| sudo, SUID, capabilities | [Escalada Linux](Escalada/Linux/Basico.md) |
| NFS (`no_root_squash`)   | [NFS](Escalada/Linux/NFS.md)            |

### Binarios

| Técnica          | Notas                           |
| ---------------- | ------------------------------- |
| Abuso de binarios| [GTFObins](Escalada/GTFObins.md)|

### Windows *(pendiente)*

- Carpeta preparada en `Escalada/Windows` para apuntes de escalada en Windows (kernel, servicios, registry, etc.).

## 5. Utilidades 🧰

| Tema               | Notas                                  |
| ------------------ | -------------------------------------- |
| SQLMap (SQLi)      | [SQLDAMP](Utilidades/SQLDAMP.md)       |
| Reverse shells     | [Aplicaciones](Utilidades/Aplicaciones.md) |
| Conceptos/trucos   | [Conceptos](Utilidades/Conceptos.md)   |

---

## 🖥️ Máquinas resueltas

### HackTheBox

| Máquina | Dificultad | Notas                                         |
| ------- | ---------- | ---------------------------------------------- |
| Orion   | Easy       | [Maquinas/HackTheBox/Easy/Orion](../Maquinas/HackTheBox/Easy/Orion.md) |
| Paperwork | Easy     | [Maquinas/HackTheBox/Easy/Paperwork](../Maquinas/HackTheBox/Easy/Paperwork.md) (en progreso) |

### Dockerlabs

| Máquina | Notas                                        |
| ------- | -------------------------------------------- |
| Injection | [Maquinas/Dockerlabs/Injection](../Maquinas/Dockerlabs/Injection.md) |
| Move    | [Maquinas/Dockerlabs/Move](../Maquinas/Dockerlabs/Move.md) |
| allien  | [Maquinas/Dockerlabs/allien](../Maquinas/Dockerlabs/allien.md) |