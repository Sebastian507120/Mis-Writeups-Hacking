
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

En Python, la herramientas estándar para manejar datos estructurados es la librería `pandas`. EL cuaderno usa la función `pd.read_csv()` para descargar y leer el archivo de accidentes directamente desde una URL.