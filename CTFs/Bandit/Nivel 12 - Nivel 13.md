Para resolver este nivel hay dos formas de completarlo  forma manual y de forma automatizada con un script

-------------------------------------------DE FORMA MANUAL------------------------------------------------
El Problema: El archivo data.txt es un hexdump (representación en texto de datos binarios) de un archivo que ha sido comprimido muchas veces, una capa sobre otra. Además, no tengo permisos de escritura en mi carpeta actual para descomprimir cosas.

La Solución:

1.Crear una carpeta de trabajo en /tmp donde sí tenga permisos.

2.Revertir el hexdump a binario usando xxd.

3.Entrar en un ciclo de Identificar -> Renombrar -> Descomprimir hasta llegar al archivo de texto puro.

🛠️ PASO 1: Crear un Espacio de Trabajo
¿Por qué? No tenemos permiso para crear archivos en la carpeta donde aparecemos. Necesitamos ir a /tmp, que es la única zona donde Linux nos deja "hacer desorden".

1.Crea una carpeta con un nombre que recuerdes (ej: micarpeta123) dentro de tmp:
Comando: "mkdir /tmp/micarpeta123"

2.Copia el archivo del reto a tu nueva carpeta:
Comando: "cp data.txt /tmp/micarpeta123"

3.Entra en tu carpeta para empezar a trabajar:
Comando: "cd /tmp/micarpeta123"

🔄 PASO 2: Revertir el Hexdump
¿Por qué? El archivo data.txt es solo texto que describe los bits (números hexadecimales). Necesitamos convertir ese texto en un archivo binario real para poder trabajar con él.

1.Convierte el hexdump a binario y guárdalo con un nombre nuevo (ej: data_original):
Comando: "xxd -r data.txt data_original"

🔁 PASO 3: El Ciclo de Descompresión (El Bucle)
La Lógica: El archivo está comprimido muchas veces con diferentes formatos (gzip, bzip2, tar). No sabemos el orden, así que debemos repetir estos 3 pasos constantemente hasta encontrar la contraseña.

Instrucciones Generales:

1. Usa ls para ver cómo se llama tu archivo actual.

2. Usa file [nombre_archivo] para ver qué tipo de compresión tiene.

3. Renómbralo con mv para ponerle la extensión correcta (.gz, .bz2, .tar).

Ejecuta el comando de descompresión correspondiente.

📋 TABLA DE REFERENCIA RÁPIDA (Copia esto tal cual)

CASO A: Si el comando file dice "gzip compressed data"
1. Renombrar:
Comando: "mv nombre_archivo nombre_archivo.gz"

2. Descomprimir:
Comando: "gzip -d nombre_archivo.gz"

CASO B: Si el comando file dice "bzip2 compressed data"
1. Renombrar:
Comando: "mv nombre_archivo nombre_archivo.bz2"

2. Descomprimir: 
Comando: "mv nombre_archivo nombre_archivo.bz2"

CASO C: Si el comando file dice "POSIX tar archive"
1. Renombrar:
Comando: "mv nombre_archivo nombre_archivo.tar"

2. Descomprimir
Comando: "tar -xf nombre_archivo.tar"

(Repite este paso unas 6 o 7 veces con el archivo nuevo que vaya saliendo).


🏁 PASO 4: El Resultado Final
Sigue descomprimiendo hasta que el comando file te diga: "ASCII text".

1. Cuando sea texto, léelo para ver la contraseña:
Comando: cat "nombre_del_archivo_final"

y ya obtendriamos la contraseña del bandit13 que en mi caso es: 


--------------------------------------DE FORMA AUTOMATIZADA----------------------------------------------
Paso 1: Preparar el entorno de trabajo
Como no tengo permisos de escritura en el "home", tuve que crear un espacio temporal.

1. Creé una carpeta temporal y entré en ella:
Comando: "cd $(mktemp -d)"

2. Copié el archivo del nivel a mi carpeta:
Comando: "cp ~/data.txt ."

3. Revertí el hexdump a binario (paso obligatorio antes de empezar):
Comando: "xxd -r data.txt data_original"

4. Borré el archivo viejo para no confundir al script:
Comando: "rm data.txt"


Paso 2: Crear el script de descompresión
Creé un archivo llamado "descompresor.sh" usando "nano":
Comando: "nano descompresor.sh"

Y le pegué este código que detecta si es gzip, bzip2 o tar y actúa en consecuencia:
Script:
#!/bin/bash

# --- PALETA DE COLORES (Estilo Hacker Pro) ---
# \e[1;3Xm activa el modo "Bold/Brillante" para que no se vea plano
RED="\e[1;31m"      # Rojo neón (Errores)
GREEN="\e[1;32m"    # Verde neón (Éxito final)
YELLOW="\e[1;33m"   # Amarillo (Alertas)
CYAN="\e[1;36m"     # Cian (Procesos e info)
PURPLE="\e[1;35m"   # Morado (Archivos)
RESET="\e[0m"       # Resetear color

# Archivo inicial
fichero="data_original"

echo -e "${CYAN}==============================================${RESET}"
echo -e "${YELLOW}  [*] INICIANDO SECUENCIA DE DESCOMPRESIÓN...${RESET}"
echo -e "${CYAN}==============================================${RESET}"
sleep 1

while true; do
    # Identificamos el archivo
    if [ -f "$fichero" ]; then
        tipo_archivo=$(file "$fichero")
    else
        echo -e "${RED}[!] Error: El archivo '$fichero' no existe.${RESET}"
        break
    fi

    # --- LÓGICA DEL BUCLE ---
    
    if [[ "$tipo_archivo" == *"gzip compressed data"* ]]; then
        # Usamos printf para un formato más limpio
        echo -e "${CYAN}[*] Detectado GZIP  -> ${RESET}Renombrando y extrayendo..."
        mv "$fichero" "$fichero.gz"
        gzip -d "$fichero.gz"

    elif [[ "$tipo_archivo" == *"bzip2 compressed data"* ]]; then
        echo -e "${CYAN}[*] Detectado BZIP2 -> ${RESET}Renombrando y extrayendo..."
        mv "$fichero" "$fichero.bz2"
        bzip2 -d "$fichero.bz2"

    elif [[ "$tipo_archivo" == *"POSIX tar archive"* ]]; then
        echo -e "${CYAN}[*] Detectado TAR   -> ${RESET}Desempaquetando..."
        mv "$fichero" "$fichero.tar"
        tar -xf "$fichero.tar"
        rm "$fichero.tar" 
        
        # Actualizamos la variable buscando el nuevo archivo (excluyendo el script)
        fichero=$(ls | grep -v "descompresor.sh")
        echo -e "${PURPLE}    [+] Nuevo archivo extraído: $fichero${RESET}"

    elif [[ "$tipo_archivo" == *"ASCII text"* ]]; then
        echo -e "\n${GREEN}==============================================${RESET}"
        echo -e "${GREEN} [✔] ACCESO CONCEDIDO - CONTRASEÑA ENCONTRADA ${RESET}"
        echo -e "${GREEN}==============================================${RESET}"
        echo -e "${YELLOW}Key: ${RESET}$(cat "$fichero")"
        echo -e "${GREEN}==============================================${RESET}\n"
        break

    else
        echo -e "${RED}[!] Formato desconocido o fin del proceso.${RESET}"
        echo -e "Info: $tipo_archivo"
        break
    fi
    
    # Pausa dramática de 0.2 segundos para ver el flujo
    sleep 0.2
done
  

Paso 3: Ejecutar la solución

1. Le di permisos de ejecución al script:
Comando: "chmod +x descompresor.sh"

2. Lo ejecuté y obtuve la contraseña:
Comando: "./descompresor.sh"

Luego de esto veremos por pantalla como se descomprime cada uno y hasta abajo el mensaje con la contraseña del bandit13 que en mi caso es: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn


~Recordar que las contraseñas cambian con el tiempo