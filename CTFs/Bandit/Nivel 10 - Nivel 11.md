Problema: La contraseña esta dentro de data.txt, pero está codificada en Base64. NO está encriptada (protegida con clave), solo esta "traducida" aun formato que usan las compuetadoras para mover datos. Se conoce porque son letras y números aleatorios  que suelen terminar en signos de igual (=)l

La Solución: Necesito usar la herramienta base64 para devolber ese texto  a su estado original (texto plano). Por defecto, el comando intenta crear codigo, así que debo usar una opción especifica para decirle que haga lo contrario 

El comando usado: base64 -d data.txt

Desglose: 

- base64(El Traductor): Es el comando que maneja este tipo de codificación.
- (-d)(Decodificador): Es la opcion clave (viene de *d*ecode)
  -Importante : El guión "-" antes de la letra es obligatorio.Sin él, Linux piensa que "d" es un archivo y falla.
  Le dice al comando: "No conviertas a código, devuélveme el texto original"

- data.txt(El Archivo): El archivo que contiene el texto codificado que queremos leer.

Al ejecutar el comando "base64 -d data.txt" nos devolvera lo que vendría siendo la contraseña de bandit11 que en mi caso seria: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr