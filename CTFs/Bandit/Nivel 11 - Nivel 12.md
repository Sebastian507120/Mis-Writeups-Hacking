---
tags:
  - CTF_Bandit
---

**El Problema:** El texto del archivo **data.txt** está cifrado  con **ROT13**. Esto significa que cada letra del abecedario fue **rotada** 13 posiciones hacia adelante  (la `A` se volvió `N`, la `B` se volvió `O`, etc.) Necesito **decodificarlo** para leerlo.

**La Solución:** Usar el comando `tr` **( translate )** para cambiar las letras una por una. Como el alfabeto tiene 26 letras, rotar 13 es justo la mitad. Para arreglarlo, tengo  que decirle a la máquina que cambie la segunda mitad del abecedario por la primera.

El comando a usar: `cat data.txt |  tr 'a-zA-Z' 'n-za-mN-ZA-M'` 

**Desglose:**

- **tr** `(El Traductor):`  Es la herramienta que sustituye caracteres.

- **'a-zA-Z'**  `(El alfabeto original):` Este es el grupo de entrada. Le digo "Busca cualquier letra minúscula o mayúscula".

- **'n-za-mN-ZA-M'** `(El alfabeto rotado):` Este es el grupo de salida. Aquí le explico la rotación:
- **(n-z):** Empieza desde la mitad (letra 13) hasta el final.
 - **(a-m):** Luego da la vuelta y completa el principio.
 
 Y repito lo mismo `N-ZA-M` para que funcione también  con las Mayúsculas.

Luego de ejecutar el comando `(cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M')` nos estaría mostrando la contraseña del **bandit 12** que en mi caso es la siguiente: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`.