---
title: Direcciones MAC (OUI y NIC)
tags:
  - Redes
---

# Direcciones MAC (OUI y NIC)

## 🌐 Direcciones MAC y MAC Spoofing (macchanger)

### ¿Qué es una Dirección MAC?

La MAC (Media Access Control) es un identificador único asignado a la tarjeta de red (NIC) de un dispositivo desde su fabricación. Funciona en la capa 2 del modelo OSI ( Capa de enlace de datos )

* Formato: Está compuesta por 48 bits (6 bytes), representados generalmente en formato hexadecimal ( 00:1A:2B:3C:4D:5E ).

![542](<../../.gitbook/assets/Direcciones MAC (OUI y NIC)-20260607213046.png>)

## 🏢 Identificación del Fabricante (OUI)

La dirección MAC es rastreable porque su estructura está dividida en dos partes:

1. OUI ( Organizationally Unique Identifier ): Son los primeros 3 bytes (6 caracteres hexadecimales). IEEE asigna estos identificadores a los fabricantes. Esto permite saber de inmediato si la tarjeta de red es de Cisco, Intel, Apple, etc.
2. NIC Specific: Son los últimos 3 bytes, asignados por el propio fabricante de manera secuencial a cada dispositivo.

![649](<../../.gitbook/assets/Direcciones MAC (OUI y NIC)-20260607232429.png>)

## 🎭 MAC Spoofing

El MAC spoofing es la técnica de cambiar (falsificar) la dirección MAC de nuestra tarjeta de red por otra a nivel de sistema operativo.

### ¿Para que sirve?

* Evasión de listas de control de acceso (filtrado MAC en routers).
* Ocultar la identidad real del hardware del atacante en una red local.
* Bypass de portales cautivos (redes de hoteles, aeropuertos) clonanddo la MAC de un cliente ya autenticado

## Uso de Macchanger

macchanger es una herramienta en Linux ( muy común en Parrot OS y Kali ) para modificar la dirección MAC a la dirección que tu desees en un par de segundos.

#### ⚠️ Importante

Antes de cambiar la MAC, primero debemos asegurarnos de tenerlo instalado lo verificamos con un `sudo apt install macchanger`. Luego es necesario **bajar la interfaz de red** con el comando `sudo ip link set dev <interfaz> down` quitar la palabra **interfaz con sus signos** y poner su respectiva interfaz la interfaz de su antena. Una vez realizado el respectivo cambio se vuelve a levantar con

### Menú de macchanger :

![](<../../.gitbook/assets/Direcciones MAC (OUI y NIC)-20260607234521.png>)

### Comandos Principales

1. Ver la MAC actual y a original (permanente) :

```
macchanger -s <nombre_de_tu_interfaz>
```

2. Asignar una MAC aleatoria completa:

```
   sudo macchanger -r <nombre_de_tu_interfaz>
```

3. Asignar una MAC aleatoria, pero manteniendo el OUI del mismo fabricante: (Útil para no levantar sospechas en un entorno monitorizado)

```
   sudo macchanger -a <nombre_de_tu_interfaz>
```

4. Clonar / Asignar una MAC específica:

```
   sudo macchanger -m XX:XX:XX:XX:XX:XX <nombre_de_tu_interfaz>
```

5. Restaurar la MAC original (de fábrica):

```
sudo macchanger -p <nombre_de_tu_interfaz>
```
