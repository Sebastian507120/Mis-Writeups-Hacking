---
title: MEOW
description: Resolución de la máquina Meow de HackTheBox
---

- **Nombre:** MEOW
    
- **SO:** Linux 
    
- **Dificultad:** Very Easy
    
- **IP:** `10.129.73.192`

											Start

![412](imagenes/Acronym%20VM.png)

Una `Virtual Machine` o `Maquina Virtual` en español es una emulación de un sistema operativo. En ciberseguridad, las usamos para aislar nuestro entorno de ataque evitando riesgos en  nuestro sistema anfitrión. 

![](imagenes/Terminal-1.png)

La `terminal` `(o consola)` es el intérprete de comandos. A diferencia de una interfaz gráfica `(GUI)`, la terminal nos permite ejecutar herramientas de hacking que no tienen "botones", dándonos un control total y más rápido sobre el sistema.

![](imagenes/Openvpn.png)

El servicio que usamos para conectarnos a la vpn de Hack The Box es openvpn  el ejecutarlo es sencillo, estando en la carpeta donde descargamos la vpn de Hack The Box ejecutamos el comando :  `sudo + openvpn + nombre de la vpn` en mi caso quedaría de la siguiente manera : `sudo openvpn hackerbolt.ovpn` 

> ### ✅ Tip de Conexión
> Siempre verifica que el comando `sudo openvpn` no arroje errores de "Auth Failed".


```bash
sudo openvpn hackerbolt.ovpn
```
