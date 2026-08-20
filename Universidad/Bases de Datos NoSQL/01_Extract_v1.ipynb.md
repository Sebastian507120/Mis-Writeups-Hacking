
# 1. ¿Qué es un Pipeline ETL y por qué separar sus fases?

Un pipeline ETL es un flujo de procesamiento de datos dividido en tres etapas:

- **Extract (`Extraer`):** Traer los datos tal cual están desde sus fuentes originales (archivos Excel/CSV, APIs, bases de datos) y convertirlos en DataFrames crudos y sin modificar.

- **Transform (`Transformar`):** Limpiar, normalizar y unir las distintas fuentes par que hablen el mismo idioma y formen un DataFrame consistente.

- **Load (`Cargar`):** Guardar los datos ya procesados en su destino final, listos para ser consumidos.

## ¿Por qué no hacer todo al mismo tiempo?
Si intentamos **leer, limpiar y unir** datos a la vez, el código se vuelve frágil y difícil de mantener. Al separarlo, si cambia el **formato de la API**, solo modificamos **la fase de extracción**; si cambian las **reglas de limpieza**, solo tocamos **la transformación**, Cada fase es independiente, reemplazable y tiene una responsabilidad única.


# 2. El proyecto Práctico del Cuaderno

EL objetivo a lo largo de los cuadernos de la clase es responder a la pregunta: **¿Se relacionan las condiciones meteorológicas con la frecuencia y gravedad de los accidentes viales?**. Para resolverlo, el profesor plantea extraer e integrar dos fuentes de datos sobre la ciudad de **Leeds** `(Reino Unido)` en el año 2014:

- Un archivo **CSV** con los registros de accidentes de tránsito.
- Una **API** que devuelve el clima diario de esa zona.


# 3. Extracción desde CSV (Accidentes Viales)

Para trabajar con datos estructurados y hacer peticiones a internet en Python, el profesor inicia importando las dos librerías fundamentales para  el proceso ETL:

```
impoert pandas as pd
import requests
```


## 3.1 Lectura básica y Exploración Inicial 
Se carga el dataset de accidentes viales de **Leeds (2014)** directamente desde una **URL** usando la función `read_csv` de pandas.

```
# Defininr la URL del archivo
url_csv = "https://raw.githubusercontent.com/damianoc90/Road-accidents-analysis/master/dataset.csv"


# Leer el CSV y guardarlo en un DataFrame llamado `df_cdv`
df_csv = pd.read_csv(url_csv)

# Mostrar la cantidad de filas y columnas
Print(f"Filas: {df_csv.shape[0]}, Columnas: {df_csv.shape[1]}")

# Mostrar las primeras 5 filas para ver cómo lucen los datos 
df_csv.head()
``` 

Una vez cargado, el profesor utiliza comandos de exploración para hacerle una "radiografía" a los datos: 

```
# 1. Muestra la estructura general: cantidad de datos no nulos y el tipo de dato por columna 
df_csv.info()

# 2. Genera estadísticas matemáticas (promedio, min, max, desviación) de las columnas numéricas
df_csv.describre()

# 3. Imprime únicamente los tipos de datos de cada columna (int64 para números, object para texto)
print(df_csv.dtypes)

# 4. Cuenta cuántos valores nulos (vacíos) hay exactamente en cada columna
print(df_csv.isnull().sum())
```

## 3.2 CSV con variaciones reales (Manejo de Errores)
En la vida real, los **CSV** suelen fallar al leerse. El profesor explicó cómo solucionar los tres problemas más comunes mediantes código: 

## Caso 1: Problemas de codificación (Caracteres extraños, tildes o ñ)
Si el archivo falla por el idioma, se usa un bloque `try-except` para intentar leerlo con una codificación estándar **(utf-8)** y, si falla, usar otra **(latin-1):** 

```
try: 
	df_enc = pd.read_csv(url_csv, encoding='utf-8')
	print("Lectura con utf-8 exitosa")
except UnicodeDecodeError:
	df_enc = pd.read_csv(url_csv, encoding='latin-1')
	print("Lectura con latin-1 exitosa")
```

## Caso 2: Separador diferente (Ej:  punto y coma en lugar de coma)
Si el archivo no está separado por comas, todo se cargará en una sola columna. Para arreglarlo, sele especifica a pandas el separador exacto usando el parámetro `sep`: 

```
# Se lee especificando que el separador es un punto y coma
df_sep = pd.read_csv(StringIO(csv_con_punto_coma), sep=';')
```

## Caso 3: Filas de encabezado mal ubicadas (Metadata)
Si el archivo tiene texto descriptivo en las primeras filas antes de los nombres reales de las columnas, se ignoran esas filas basura usando el parámetro `skiprows`

```
# Se salta las primeras 2 filas del archivo al leerlo 

df_header = pd.read_csv(StringIO(csv_con_header_raro), skiprows=2)
```

# 4. Extracción desde API (Datos del Clima)

Para obtener los datos del clima, se consulta la API de Open-Meteo mediante una petición GET. En lugar de descargar un archivo, se le envían parámetros (latitud, longitud, fechas) para recibir la información exacta.

## 4.1 Construyendo la petición y analizando el JSON

```
# URL base de la API
url_api = "https://archive-api.open-meteo.com/v1/archive"

# Diccionario con los parámetros obligatorios de la consulta
params = {
	"latitude": 53.8008,
	"longitude": -1.5491,
	"start_date": "2014-01-01"
	"end_date": "2014-12-31"
	"daily": "temperature_2m_max,temperature_2m_min,precipitation_sum,weathercode",
	"timezone": "Europe/London"
}

# Ejecutar la petición
response = requests.get(url_api, params=params)

# Extraer la respuesta en formato JSON (diccionario de Python)
data_json = response.json()

# Inspeccionar las claves principales del JSON para saber dónde están los datos útiles
print(list(data_json.keys()))
# Salida esperada: ['latitude', 'longitude', ..., 'daily']

# Mirar qué hay dentro de la clave 'daily'
print(list(data_json['daily'].keys)))
# Salida esperada: ['time', 'temperature_2m_max', ...]
```


## 4.2 De JSON  a DataFrame
Al ver que los datos importantes están agrupados dentro de la clave `daily`, se convierte esa sección específica directamente en una tabla estructurada:

```
# Convertimos el bloque 'daily' a un DataFrame de pandas
df_api = pd.DataFrame(data_json['daily'])

print(f"Filas: {df_api.shape[0]}, Columnas: {df_api.shape[1]}")
df_api.head()
```

*(Luego de esto, el profesor también le aplica a este DataFrame los mismos comandos exploratorios `.info()`, `.describe()` y `.isnull().sum()` para verificar su estado).*

# 5. El Diagnóstico (Preparando el terreno para Transformar)

### 1. Buscar si comparten alguna columna por nombre exacto:

```
# Se normalizan los nomres de las columnas (minúsculas y sin espacios) y se busca la intersección
cols_csv = set(df_csv.columns.str.lower().str.strip())
cols_api = set(df_api.columns.str.lower().str.strip())

comunes = cols_csv.intersection(cols_api)
print(f"Columnas en común (normalizadas): {comunes}")
# Resultado: set() -> No hay ninguna columna que se llame igual en ambos lados.
 
```

### 2. Identificar manualmente las columnas de fecha (ya que serán nuestra clave de unión)

```
# Buscar en el CVS cualquier columna que contenga las palabras 'fecha', 'date', o 'tiempo'
for col in df_csv.columns:
	if 'fecha' in col.lower() or 'date' in col.lower() or 'tiempo' in col.lower():
		print(f"CVS - posible columna de fecha: '{col}' | tipo: {df_csv[col].dtype}")
		
# Hace lo mismo para la API
for col in df_api.columns:
	if 'fecha' in col.lower() or 'date' in col.lower() or 'tiempo' in col.lower():
		print(f"API - posible columna de fecha: '{col}' | tipo: {df_api[col].dtype}")
```

- *Hallazgo: En el CVS la fecha se llama `Accident Date` y en la API se llama `time`. Ambas son de tipo texto (`object`). 

# Resumen de problemas a resolver en la Transformación:

El profesor documenta todo en un diccionario final detallando los problemas a resolver en el siguiente cuaderno:

- **En df_csv:**
	- La fecha `Accident Date` es texto (`object`), hay que convertirla a fecha real (`datatime`).
	- La hora `Time (24hr)` es numérica, hay que darle formato `HH:MM`
	- Variables como el clima vienen en códigos numéricos, requieren etiquetas.
	- Hay un registro por víctima, generando accidentes duplicados.
- **En df_api:**
	- La fecha `time` es texto (`object`), hay que pasarla a `datatime`.
	- `weathercode` es numérico, necesita etiquetas descriptivas.
- **Para el Cruce (Merge):**
	- No hay columnas con el mismo nombre; habrá que normalizar la fecha en ambos para unirlos.
	- La granularidad es distinta (el CVS tiene varios filas por día, la API solo una); habrá que agrupar el CSV por día antes del cruce.

