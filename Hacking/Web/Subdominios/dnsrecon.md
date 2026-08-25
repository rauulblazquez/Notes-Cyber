## dnsrecon

**dnsrecon** es una herramienta de **reconocimiento DNS**. No se enfoca solo en subdominios: también obtiene **información DNS relevante** (registros MX, TXT, NS, transferencia de zona...) que puede revelar la infraestructura del objetivo.

Disponible en los repositorios de Kali:

```bash
sudo apt install dnsrecon
```

### Enumeración básica

```bash
dnsrecon -d dominios.com
```

- `-d` → dominio objetivo.

### Descubrimiento de subdominios por fuerza bruta

```bash
dnsrecon -d dominios.com -t brt -D /usr/share/wordlists/subdomains.txt
```

- `-t` → **tipo** de enumeración. `brt` = **brute force**.
- `-D` → wordlist de nombres de subdominios.

![[Pasted image 20260705142833.png]]

> A diferencia de Subfinder/Sublist3r (pasivas y silenciosas), `dnsrecon -t brt` realiza consultas masivas: **genera ruido** porque pregunta por cada nombre de la wordlist a los servidores DNS públicos.