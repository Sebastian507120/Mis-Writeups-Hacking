**El problema:** El archivo **data.txt** esta lleno de líneas repetidas y desordenadas. La contraseña es la única línea que aparece una sola vez, pero encontrarla a ojo es imposible entre tanto ruido.

**La solución:** Usar el comando `uniq` para filtrar repetidos. Pero cuidado: `unique` es **miope** y solo detecta duplicados si están uno al lado del otro. Por eso, es obligarorio ordenar el archivo antes de filtrar.

**El comando a utilizar :** `sort data.txt | uniq -u`

**Desglose:** 

- `sort data.txt`**(El ordenador):** Toma todo el desorden del archivo y lo organiza alfabéticamente.

- `|` **(La Tubería / Pipe):** Es el conector mágico. Toma el resultado del comando anterior (el texto ya ordenado) y se lo pasa directamente al siguiente comando

- `uniq` **(EL inspector):** Es la herramienta que revisa las lineas repetidas.

- `(-u)` **(Solo Únicos):** Es la opción clave
  - Sin esto, `uniq` solo borraría los duplicados (dejando una copia de cada uno)

  - Con `-u`, le decimos: Elimina todo lo que se repita  y muéstrame solo lo que aparezca una unica vez.

  Entoces luego de ejecutar el comando "sort data.txt | uniq -u" encontrariamos la contraseña para el bandit9 que en mi caso es : 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM