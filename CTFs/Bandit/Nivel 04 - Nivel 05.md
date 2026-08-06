Buenos muchachos al entrar a **bandit4** nos encontraremos con una carpeta con el nombre de inhere dentro de ella estará la contraseña para entrar en **bandit5**  asi que vamos a por ello!!

# PASO 1: 
Entramos en la carpeta de nombre inhere con el comando : `cd inhere`

# PASO 2: 
Hacemos un poderosisimo ls para verlos archivos que tenemos dentro y nos percatamos que tienen cierto nombre algo peculiar

# PASO 3:
Utilizamos el comando `file` para ver que tipo de archivos son los que tenemos el comando completo seria : `file./*`

`file` -> No abre el archivo solo lo escanea para decirnos que tipo de archivo es

`./` -> Le dice a la terminal busca en la carpeta actual hacerlo de esta forma tambien evita que la consola confunda los archivos que empiezan con `-` y piense que son opciones de algún comando

`*` -> Significa **TODO**  en lugar de escribir los 10 archivos este asterisco le dice a linux aplica el comando para todo lo que encuentres aquí


Al hacer este comando veremos que de los 10 archivos solo el numero 07 es el que dice ASCII text que ACSII text se puede traducir como texto plano o texto legible en pocas palabras es un archivo que contiene solo letras/palabras/numeros que podemos leer 

como identificamos que el archivo 07 es el que podemos leer le hacemos un cat para ver el contenido de este archivo con el siguiente comando  : `cat ./-file007` al hacer el cat de esta manera podemos leer el archivo asi inicie con un guión "-" en el nombre


Al hacer el `cat` **felicitaciones!** habremos encontrago la contraseña del **bandit5** que en mi caso es :
`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

>## 📌 Recordar que estas contraseñas cambian constantemente