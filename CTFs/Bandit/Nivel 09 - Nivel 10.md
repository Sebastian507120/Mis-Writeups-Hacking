El Problema: El archivo data.txt es un archivo binario (codigo de maquina). Si intento leer con cat, me salen simbolos raros y se puede bloquear la terminal (como paso en mi caso).La contraseña esta escondida ahí dentro como "texto humano", pero rodeada de basura.

La Solución:Necesito una herramienta que funcione como un "colador" para separar el texto legible del código binario. Esa herramienta es "strings". Luego, uso "grep" para encontrar la línea especifica que tiene varias "="

El comando usado: `strings data.txt | grep "=="`

Desglose: 

- `strings data.txt` **(El Colador):** Este comando lee el archivo binario y extrae solo los caracteres imprimibles (letras, números y signos). Tira toda la "basura" binaria  que no podemos leer.

- `|` **(La tubería):** Toma el texto limpio que sacó "strings" y se lo envía al siguiente comando 

- `grep "=="`**(El buscador):** Busca entre el texto limpio las líneas que contengan varios signos de igual `(==)`, ya que la pista del nivel decía que la contraseña estaba marcada así.

Luego de ejecutar el comando `strings data.txt | grep "=="` se nos mostrara por consola en mi caso 4 salidas pero es identificable a ojo para cualquiera cual es la contraseña las otras 3 son palabras sueltas en mi caso la contraseña es: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey` 