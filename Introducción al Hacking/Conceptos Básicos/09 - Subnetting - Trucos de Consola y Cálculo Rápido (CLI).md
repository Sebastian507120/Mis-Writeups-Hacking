---
title: Subnetting - Trucos de Consola y Cálculo Rápido (CLI)
tags:
  - Redes
---
Cuando realizamos subnetting en auditorías, la agilidad es calve. Aunque sabemos hacer los cálculos mentalmente o a papel, apoyarnos en herramientas de la terminal de Linux nos permite evitar errores humanos en rangos complejos (como un `/17` o `/13`).

El comando estrella para esta tarea es  `bc` (Basic Calculator), el cual nos permite cambiar la "base" numérica de entrada y salida rápidamente.

# 🛠️ La Herramienta: Comando `bc` 
Podemos enviarle operaciones matemáticas a `bc` a través de un `echo` usando un pipe (`|`). Para las conversiones de direcciones IP, jugamos con dos variables internas de la calculadora:
- `obase` **(Output Base):** Define la base en la que queremos el **resultado.**
- `ibase` **(Input Base):** Define la base en la que le estamos **entregando** el dato.

## 1. Decimal a Binario (`obase=2`)
Si tenemos un octeto en decimal (ej. `16`) y queremos saber cómo se ve en bits:

```
echo "obase=2; 16" | bc
10000
```

>**⚠️ Importante:** `bc`  recorta los ceros a la izquierda. Como sabemos que un octeto siempre tiene 8 bits, nosotros debemos de rellenarlo mentalmente. Ese `10000` en realidad representa el octeto completo: `00010000`.

## 2. Binario a Decimal (`ibase=2`)
Si armamos un octeto en binario y queremos pasarlo a decimal rápido para escribir nuestra IP:

```
> echo "ibase=2; 01111111" | bc
127
```

# 🎯 Ejemplo Práctico Paso  a Paso
Supongamos que nuestro objetivo es la IP `172.168.15.16/17`. Queremos hallar el Network ID y el Broadcast rápidamente utilizando el análisis binario apoyado por nuestra terminal.

## Paso 1: Convertir todo a Binario
Nos apoyamos en `bc` para convertir cada octeto de la IP si tenemos dudas.
- IP (`172.14.15.16`): ``10101100 . 00001110 . 00001111 . 00010000``
- Máscara (`/17`): (17 bits en "1", el resto en "0") `1111111 . 11111111 . 10000000 . 00000000` (Decimal: `255.255.128.0`)


## Paso 2: Calcular el Network ID (Operación AND)
Alineamos los bits. La regla de la compuerta lógica AND es simple: donde la máscara tiene un `1`, el bit de la IP baja intacto; donde la máscara tiene un `0`, se fuerza a `0`. 

```
IP:   10101100 . 00001110 . 00001111 . 00010000
Mask: 11111111 . 11111111 . 10000000 . 00000000
------------------------------------------------  [AND]
Net:  10101100 . 00001110 . 00000000 . 00000000
```

Convertimos de vuelta a decimal : `172.14.0.0` (Este es nuestro Network ID).


# Paso 3: Calcular el Broadcast
Para encontrar la última dirección (Broadcast), tomamos nuestro Network ID binario y convertimos todos los **bits de host** en `1`. Sabiendo que la máscara usaba 17 bits para red, nos quedan 15 bits para hosts (los últimos 15 bits).

```
Net:   10101100 . 00001110 . 00000000 . 00000000
Bcast: 10101100 . 00001110 . 01111111 . 11111111

```

Ahora convertimos esto a decimal. El primer y segundo octeto no cambiaron (`172.14`). El cuarto octeto está en 1 (`255`). ara el tercer octeto, usamos nuestro truco de consola: 

```
> echo "ibase=2; 01111111" | bc
127

```

Juntamos todo: `172.14.127.255` (Esta es nuestra Dirección de Broadcast).

** 💡Resumen Visual del Rango**
- **Red:** `172.14.0.0`
- **Primer Host:** `172.14.0.1`
- **Último Host:** `172.14.127.254`
- **Broadcast:** `172.14.127.255`