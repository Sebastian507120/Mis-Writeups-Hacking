---
tags:
  - CTF_Bandit
---

**El Problema:** El archivo **data.txt** es un hexdump (representación en texto de datos binarios) de un archivo que ha sido comprimido muchas veces, una capa sobre otra. Además, no tengo permisos de escritura en mi carpeta actual para descomprimir cosas.

**La Solución:** Crear una carpeta de trabajo en `/tmp` donde sí tenga permisos, revertir el `hexdump` a binario usando `xxd`, y ejecutar un ciclo iterativo de identificar, renombrar y descomprimir hasta llegar al archivo de texto puro con la contraseña.

# Metodología de Resolución 
Existen dos formas de resolver este nivel: de forma **manual** (ideal para entender la lógica paso a paso) o de forma **automatizada** mediante un script en Bash.

## 1️⃣ Opción 1: Resolución Manual
### Paso 1:  Crear un entorno de trabajo temporal
Dado que no tenemos permisos de escritura en el directorio actual, nos trasladamos a `/tmp` y creamos una carpeta de trabajo:

```
cd $(mktemp -d)
cp ~/data.txt .
```

### Paso 2:  Revertir el Hexdump
Convertimos el archivo de texto hexadecimal a su formato binario real usando la herramienta `xdd`.

```
xxd -r data.txt data_original
rm data.txt
```

### Paso 3:  Identificar y Descomprimir (El bucle lógico)
Para cada archivo que vayamos obteniendo, debemos repetir una serie de pasos:
1. Ver el tipo de archivo con `file`: `file <nombre_archivo>`

2. Renombrarlo con su extensión correspondiente (`.gz`, `.bz2` o `.tar`).

3. Descomprimirlo con el comando adecuado.

- Si es **Gzip**:

 ```
 mv archivo archivo.gz
gzip -d archivo.gz
 ```

- Si es **Bzip2**: 

```
mv archivo archivo.bz2
bzip2 -d archivo.bz2
```

- Si es **Tar**:

```
mv archivo archivo.tar
tar -xf archivo.tar
```

### Paso 4: Obtener la Flag
Repetimos el proceso iterativamente hasta que el comando `file` nos indique que estamos ante un **"ASCII text"**. En ese momento, solo debemos leer su contenido:

```
cat <nombre_archivo_final>
```

# 2️⃣ Opción 2: Resolución Automatizada (Script en Bash)
Si prefieres agilizar el proceso y ver la magia de la automatización, puedes crear un script que detecte automáticamente el tipo de compresión y extraiga las capas una por una hasta revelar la contraseña.

### Paso 1: Prepara tu espacio temporal:

```
cd $(mktemp -d)
cp ~/data.txt .
xxd -r data.txt data_original
rm data.txt
```

### Paso 2: Crea el script de descompresión:
Crea un archivo llamado `descompresor.sh` usando tu editor favorito (por ejemplo, `nano`):

```
nano descompresor.sh
```

Pega el siguiente código optimizado dentro del archivo:


<details>

<summary>Ver banner de bienvenida completo (Telnet)</summary>

#!/bin/bash

# --- PALETA DE COLORES ---
RED="\e[1;31m"
GREEN="\e[1;32m"
YELLOW="\e[1;33m"
CYAN="\e[1;36m"
PURPLE="\e[1;35m"
RESET="\e[0m"

fichero="data_original"

echo -e "${CYAN}==============================================${RESET}"
echo -e "${YELLOW}  [*] INICIANDO SECUENCIA DE DESCOMPRESIÓN...${RESET}"
echo -e "${CYAN}==============================================${RESET}"
sleep 1

while true; do
    if [ -f "$fichero" ]; then
        tipo_archivo=$(file "$fichero")
    else
        echo -e "${RED}[!] Error: El archivo '$fichero' no existe.${RESET}"
        break
    fi

    if [[ "$tipo_archivo" == *"gzip compressed data"* ]]; then
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

    # Actualizamos la variable para apuntar al archivo resultante más reciente
    fichero=$(ls -t | grep -v "descompresor.sh" | head -n 1)
    sleep 0.2
done

</details>

### Paso 3: Le damos permisos de ejecución y lo ejecutamos:

```
chmod +x descompresor.sh
./descompresor.sh
```

Luego de completar el proceso obtendríamos la contraseña del bandit13 (que para este nivel 12 a 13 suele ser `wbWdlBxWKcf4Du3TcyLaVl83YZ8JyEms`). 

> # Recuerda que las contraseñas pueden variar o cambiar con el tiempo.