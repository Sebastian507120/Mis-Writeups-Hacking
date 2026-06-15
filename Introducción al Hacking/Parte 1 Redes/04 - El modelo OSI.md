---
title: El Modelo OSI
description: ¿En qué consiste y cómo se estructura la actividad de red en capas?
tags:
  - Redes
---
# 📚 El modelo OSI (Open Systems Interconnection)

## ¿Qué es y por qué es importante?
El modelo OSI es una estructura teórica de **7 capas** que describe cómo se comunican los dispositivos en una red.

>##📌 Relevancia en Ciberseguridad
>Comprender esta estructura permite tener una visión completa del flujo de datos, aislar problemas rápidamente e identificar **vulnerabilidades específicas** en cada nivel para aplicar las medidas de seguridad adecuadas durante un pentesting.

> [ !📌 Relevancia en Ciberseguridad ] 
> Comprender esta estructura permite tener una visión completa del flujo de datos, aislar problemas rápidamente e identificar **vulnerabilidades específicas** en cada nivel para aplicar las medidas de seguridad adecuadas durante un pentesting.

## 🗃️ Estructura de las 7 capas (Orden Ascendente: 1 a 7)

| #   | Capa            | Unidad de Datos (PDU)  | Función Principal                                              | Ejemplos / Conceptos                                    |
| --- | --------------- | ---------------------- | -------------------------------------------------------------- | ------------------------------------------------------- |
| 1   | Física          | Bits                   | Transmisión de datos crudos sobre el medio físico.             | Cables de cobre, fibra optica, Wifi, hubs               |
| 2   | Enlace de datos | Tramas (Frames)        | Transferencia confiable de datos dentro de la misma red local  | Direcciones MAC, switches, detección de errores.        |
| 3   | Red             | Paquetes (Packets)     | Enrutamiento de datos a través de múltiples redes distintas.   | Direcciones IP (IPv4 /IPv6, routers, ICMP)              |
| 4   | Transporte      | Segmentos / Datagramas | Entrega de datos de extremo a extremo y control de flujo.      | Protocolos TCP y UDP, puertos                           |
| 5   | Sesión          | Datos                  | Controla las conexiones y diálogos entre aplicaciones.         | Establecimiento, mantenimiento y cierre de sesiones.    |
| 6   | Presentación    | Datos                  | Traduce, formatea, comprime y cifra los datos.                 | Cifrado SSL/TLS, formatos (JPEG, MP3, Caracteres)       |
| 7   | Aplicación      | Datos                  | Interfaz directa con los servicios y aplicaciones del usuario. | Navegadores, clientes de correo (HTTP, HTTPS, SSH, FTP) |

## 📨 Flujo de Datos (Encapsulación)  
Para recordar cómo se construyen los paquetes cuando usamos por el modelo del 1 al 7:

- La señal eléctrica llega al dispositivo *(1. Física).
- Se comprueba que la dirección MAC de destino sea la correcta *(2. Enlace).
- Se revisa la dirección IP para saber hacia dónde va el paquete *(3. Red). 
- Se identifica el puerto de destino para saber qué servicio lo procesará *(4. Transporte).
- Se verifica que la sesión de comunicación esté activa *(5. Sesión).
- Se descifran o descomprimen los datos recibidos *(6. Presentación).
- El software o navegador web muestra la información al usuario final *(7. Aplicación).

[04 - El modelo OSI](04%20-%20El%20modelo%20OSI.md) | OSI ]]

 