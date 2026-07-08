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
- IP (`172.14.15.16`): ``