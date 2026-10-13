Nmap (Nerwotk Mapper) es el estándar para el descubrimiento de hosts, enumeración de puertos y auditoría de servicios en auditorías de seguridad y pentesting.

## 1. `-sS`:  SYN Stealth Scan (Escaneo sigiloso)

- **Qué hace:** Es el modo por defecto cuando se ejecuta como `root` / `sudo`.  Envía un paquete con el flag `SYN`. SI el puerto responde con `SYN/ACK`, el puerto está **abierto**;  Nmap responde inmediatamente con un `RST` para derribar la conexión sin completar el handshake **TCP** de 3 vías. Si responder con `RST`, significa que está cerrado.

- **Por qué se usa:** Es rápido, eficiente y suele ser menos ruidoso a nivel de logs de capa de aplicación, ya que nunca establece una conexión **TCP** completa.

```
sudo nmap -sS <IP>
```


## 2. `-sT` : TCP Connect Scan

- **Qué hace**: Realiza el handshake completo de tres vías (`SYN` -> `SYN/ACK` -> `ACK`) utilizando las llamadas al sistema del SO subyacente. Es el escaneo por defecto cuando se ejecuta sin privilegios de **superusuario**.

- **Por qué se usa**: No requiere permisos de `root`  /  `sudo`, pero es más ruidoso y queda registrado en la mayoría de servicios de aplicación (logs web, firewalls de host).

```
nmap -sT <IP>
```


## 3. `-sU` : UDP Scan

- **Qué hace**: Envía paquetes **UDP** específicos a los puertos. Si no recibe respuesta o recibe un paquete **UDP**, el puerto se marca como  `open/filtered`. Si responde un `ICMP Port Unreachable` (Tipo 3, Código 3), el puerto está `closed`. 

- **Por qué se usa**: Esencial para detectar servicios críticos que corren sobre **UDP** (DNS en 53, SNMP en 161, DHCP en 67/68, TFTP en 69). Es significativamente más lento debido a las limitaciones de tasa (rate-limiting) de paquetes **ICMP** impuestas por los kernels del objetivo.

```
sudo nmap -sU <IP>
```



## 4. `-p` : Especificación de Puertos

- **Qué hace**: Define el rango exacto de puertos a escanear en lugar de los 1.000 por defecto.

- **Variantes comunes:**
	- `-p-`: Escanea todo el rango TCP/UDP (del 1 al 65.535).
	- `-p22,80,443`: Escanea únicamente puertos específicos separados por comas.
	- `-p1-1000`: Escanea un rango continuo.
	- `-p U:53,161,T:80,443`: Combina puertos UDP y TCP en un solo comando.

```
sudo nmap -p- <IP>
```


## 5. `-sV` : Detección de Versiones de Servicios

- **Qué hace** : Una vez identificado un puerto abierto, Nmap interroga el servicio enviando diferentes firmas y banners para determinar la aplicación exacta y la versión que se está ejecutando 

- **Por qué se usa** : Es fundamental en la fase de reconocimiento para correlacionar versiones exactas con vulnerabilidades conocidas y exploits en bases de datos (`cve` / `Exploit-db`).

```
nmap -sV <IP>
```


## 6. `-sC` : Escaneo con Scripts por Defecto (NSE)

- **Qué hace** : Ejecuta el motor de scripts de Nmap (Nmap Scripting Engine) utilizando el conjunto de scripts categorizados como `default`. Equivale a `--script=default`. 

- **Por qué se usa** : Automatiza la extracción de información básica y no intrusiva: títulos de páginas web, métodos **HTTP** permitidos, algoritmos **SSH** soportados, dominios en certificados **SSL/TLS**, etc.

```
nmap -sC <IP>
```


## 7. `-O` : Detección de Sistema Operativo (OS FingerPrinting)

- **Qué hace** :  Envía una serie de paquetes **TCP** y **UDP** especialmente manipulados y analiza diferencias mínimas en la respuesta del stack **TCP/IP** del objetivo (tamaño de ventana, **TTL**, orden de opciones, valores **IP** **ID**).

- **Por qué se usa** : Permite estimar la familia del sistema operativo, versión del kernel y tipo de dispositivo (**router**, **switch**, **servidor Linux/Windows**). 

```
sudo nmap -O <IP>
```


## 8. `-Pn` : Omitir Descubrimiento de Hosts (Tratar como Host Activo)

- **Qué hace** : Desactiva el sondeo inicial de ping (**ICMP** echo requets, **TCP SYN** a puerto **443**, **TCP ACK** a **80**, timestamp **ICMP**). Nmap asume directamente que la IP de destino está encendida y pasa directo a escanear los puertos. 

- **Por qué se usa** : Crucial cuando el objetivo tiene un firewall que bloquea paquetes ICMP o sondas de ping iniciales; sin `-Pn`, Nmap reportaría el host como "down" y terminaría la ejecución.

```
nmap -Pn <IP>
```


## 9. Plantillas de Temporización: `-T <0-5>` y Parámetros de Rendimiento

- **Qué hace** : Ajusta la velocidad y el paralelismo del escaneo. Va desde `paranoid` (`-T0`), `sneaky` (`-T1`)