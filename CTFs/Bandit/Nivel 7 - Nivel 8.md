El Problema: El archivo data.txt es gigante. Si uso cat, me inunda la pantalla con miles de líneas y es imposible leer nada. Necesito algo que busque por mí.

La Solución: Usar el comando grep, que funciona como un filtro inteligente. Solo me muestra las líneas que contienen exactamente lo que yo quiero ver.

El Comando: grep "millionth" data.txt

Desglose:

- grep (El Filtro): Significa "Global Regular Expression Print". Es la herramienta que busca patrones de texto. Básicamente le dice a Linux: "Ignora todo lo que no te sirva".

- "millionth" (La Aguja): Es la palabra exacta que estamos buscando. La ponemos entre comillas para asegurarnos de que busque ese texto literal. La elegimos porque las instrucciones decían que la contraseña está "al lado de la palabra millionth".

- data.txt (El Pajar): Es el archivo donde vamos a buscar.

Luego de haber escrito el comando grep "millionth" data.txt habremos encontrado la contraseña de bandit8 que en mi caso es : dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc