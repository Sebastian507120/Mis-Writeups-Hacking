---
title: Direcciones IP (IPV4 e IPV6)
description: Explicación de que es una  ip y los 2 tipos que existen
tags:
  - Redes
---
# ❓ ¿Que es una dirección IP?

Una **Dirección IP** ( Internet Protocol )  es el número de identificación que tiene cada dispositivo en una red. Podemos compararlo a como si los dispositivos tuvieran un número de teléfono si un equipo quiere hablar con el mio, necesita marcar a mi número exacto de lo contrario no podrá comunicarse


## IPv4 : El estándar de siempre

Es el sistema que llevamos usando toda la vida, pero tiene un problema : se quedó pequeño.

- **Cómo se ve:**  Son 4 bloques de números separados por puntos  `(Ejemplo: 192.168.1.15)`

- **El gran límite:**  Solo permite crear unos 4.000 millones de direcciones. como hoy en día todo el mundo tiene celular, computadora, televisor y hasta bombillos inteligentes, las direcciones IPv4 ya se agotaron.

- **Calculo hecho en Linux:** 

![[Direcciones IP (IPV4 e IPv6) Cantidad direcciones IPs IPv4.png]]
- **Número de población según countrymeters.info :**

![[Direcciones IP (IPV4 e IPv6)  Poblacion_Mundial.png]]
- **Dato clave:**  Como se logra ver en la imagen de arriba a fecha de hoy 16 de mayo de 2026 hay 8.000 millones de personas en el mundo por eso literal ya se acabaron las direcciones **IPv4** pero se creo una solución y esa solución se llama **IPv6**

## IPv6: La solución al problema
Nació exclusivamente para solucionar la falta de espacio de **IPv4** y asegurarse de que nunca nos quedemos sin direcciones. 

- **Cómo se ve:** Es mucho más larga, mezcla números y letras, y se separa por dos puntos  ( Ejemplo: `2001:0db8:85a3:8a2e`) }

- **Calculo de direcciones IPv6:**

![](Direcciones%20IP%20(IPV4%20e%20IPV6)%20Cantidad%20posible%20de%20IPs%20%20IPv6.png)

- **Su ventaja:** Permite una cantidad de direcciones tan ridículamente alta que podríamos asignarle miles  de millones de **IPs** a cada persona y aun así  sobrarían.

- **Mejora extra:** Además de ser enorme, es más rápida organizando el tráfico de datos y viene con mejoras de seguridad de fábrica.


>***🗨️ Comando de esta clase***
> Para revisar nuestra  IP y nuestras interfaces de red basta con ejecutar el comando : `ifconfig` y la terminal nos arrojará algo como lo siguiente : 

```
enp6s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1360
        inet 192.168.1.116  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 2800:e2:2f80:207:b62e:99ff:fe35:f78d  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::b62e:99ff:fe35:f78d  prefixlen 64  scopeid 0x20<link>
        ether b4:2e:99:35:f7:8d  txqueuelen 1000  (Ethernet)
        RX packets 139406  bytes 178772232 (170.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 43671  bytes 5023671 (4.7 MiB)
        TX errors 0  dropped 2 overruns 0  carrier 0  collisions 0
        device memory 0xf7400000-f741ffff  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlx00c0cab234bd: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether de:21:7c:36:a9:05  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Prueba #2 
![697](../Capturas/Direcciones%20IP%20(IPV4%20e%20IPV6)-20260516181951.png)


![](../Capturas/Direcciones%20IP%20(IPV4%20e%20IPV6)-20260516183240.png)