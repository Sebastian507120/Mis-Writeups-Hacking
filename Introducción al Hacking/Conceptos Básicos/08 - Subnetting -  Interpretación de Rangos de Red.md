---
title: Subnetting - Interpretación de Rangos de Red
tags:
  - Redes
---
# 🎯 El objetivo Práctico en Pentesting
En auditorías reales, el cliente casi nunca nos da una red "limpia" (como un `/24`). Por lo general , nos proporcionan rangos intermedios o direcciones IP específicas con su respectivo prefijo CIDR (por ejemplo, `10.10.1.15/23`).

Nuestro trabajo como auditores es saber interpretar matemáticamente ese dato para descubrir el **Network ID** y la **Dirección de Broadcast**. Al encontrar estos dos extremos, revelamos el rango exacto de máquinas que tenemos permitido atacar dentro del alcance.

## 🧮 Ejercicios de Interpretación (Cheat Sheet)

### Repaso Rápido de Cálculos
Antes de ver la tabla, recordemos de dónde sale cada dato tomando como ejemplo el primer registro (`192.168.1.0/26`):
- **Máscara de Red:** Se obtiene revisando los 32 bits totales. El `/26` indica que hay 26 bits encendidos ("1"). Los primeros tres octetos se llenan (`255.255.255`), y en el cuarto octetos quedan 2 bits encendidos. Sumando sus valores de la tabla binaria (128 + 64), obtenemos `192`. Así se forma la máscara `255.255.255.192`.
- **Hosts Asignables:** Se calcula viendo los bits apagados ("0") en ese mismo cuarto octeto. Como sobran 6 bits, elevamos `2⁶` y nos da 64 hosts totales. Siempre le restamos 2 (la red y el broadcast), dándonos **62 hosts asignables.**
- **Identificador (Network ID):** Es siempre la **primera dirección** de nuestro bloque de red calculado.
- **Dirección de Broadcast:** Es siempre la **última dirección del bloque.**

> # ⚠️**Ojo con los casos especiales:**
> Aunque el Network ID y el Broadcast parecen sencillos de sacar en redes pequeñas, hay casos específicos (como el CIDR `/23` o menores) que requieren atención extra. En estos casos, el salto de direcciones es tan grande que afecta al tercer octetos en lugar del cuarto, agrupando direcciones IP aparentemente distintas bajo una misma subred lógica.

A continuación, el registro de los cálculos realizados. (Nota: En los últimos ejemplos se ha completado el rango para demostrar cómo IPs diferentes pueden pertenecer al mismo bloque de subred).

| IP / Rango Objetivo  | Máscara de Red  | Hosts Asignables |  Network ID   | Dirección de Broadcast |
| :------------------: | :-------------: | :--------------: | :-----------: | :--------------------: |
|   `192.168.1.0/26`   | 255.255.255.192 |        62        |  192.168.1.0  |      192.168.1.63      |
|    `10.10.0.0/24`    |  255.255.255.0  |       254        |   10.10.0.0   |      10.10.0.255       |
|   `10.10.1.15/23`    |  255.255.254.0  |       510        |   10.10.0.0   |      10.10.1.255       |
| `192.168.112.165/25` | 255.255.255.128 |       126        | 192.168.112.0 |                        |
|   `192.168.1.0/23`   |  255.255.254.0  |       510        |  192.168.0.0  |     192.168.1.255      |
|   `192.168.2.0/23`   |  255.255.254.0  |       510        |  192.168.2.0  |     192.168.3.255      |
|   `192.168.3.0/23`   |  255.255.254.0  |       510        |  192.168.2.0  |     192.168.3.255      |
 
## 💡Observación Clave: 
Observemos cómo en las dos últimas filas (`192.168.2.0/23` y `192.168.3.0/23`), el Network ID y el Broadcast son **exactamente los mismos.** Esto ocurre porque un bloque `/23` es tan grande que abarca dos subredes `/24` completas. Ambas direcciones IP, aunque parezcan de redes distinta, operan dentro de la misma subred lógica.

# 🛠️ Herramientas de Apoyo
Aunque dominar el cálculo de forma manual es vitar para las bases teóricas y certificaciones, en el día a día profesional podemos agilizar este proceso utilizando conversores online de CIDR a IPv4:
- 🔗 **Calculadora CIDR (recomendada por S4vitar):** [IP Address Guide - CIDR](https://www.ipaddressguide.com/cidr "null")

