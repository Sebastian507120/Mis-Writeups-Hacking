
# 1.  El problema: ¿Por qué no siempre nos sirve SQL?
Las bases de datos relacionales tradicionales **(SQL)** guardan la información en tablas estrictas con filas y columnas. Exigen que todo esté perfectamente ordenado y que cada dato cumpla reglas muy rígidas **(Propiedades ACID)**.

## ¿Cuando falla esto?
Cuando la aplicación crece demasiado y recibe millones de datos por segundo. Intentar conectar muchas tablas entre sí con consultas complejas (`JOIN`) en múltiples servidores al mismo tiempo vuelve la base de datos extremadamente lenta y genera bloqueos.


# 2. ¿Qué es NoSQL?
**NoSQL** (`Not Only SQL`) es una alternativa que rompe la estructura rígida de las tablas.

En lugar de obligarnos a definir la estructura exacta antes de guardar los datos (`schema-on-write`), nos permite guardar información sin una estructura fija previa (`schema-on-read`). Su objetivo principal no es reemplazar a **SQL** para todo, sino ofrecer **escalabilidad horizontal:** hacer crecer el sistema simplemente agregando más servidores en lugar de comprar un solo servidor gigante y costoso.


# 3. Las cuatro Familias de NoSQL
Así como en **SQL** todo son tablas, en NoSQL existen 4 formas distintas de organizar la información según el problema que queramos resolver:

## 3.1 Clave-valor (Key-Value)

- **¿Cómo funciona?** Funciona como un diccionario gigante o una tabla de buscar. Tenemos una **Clave** única `un ID` y un **Valor** asociado  (que puede ser cualquier tipo de dato).

- **Ventaja:** Es la forma más rápida de consultar datos. Le damos la clave y nos devuelve el valor de inmediato.

- **Casos de uso:** Guardar sesiones de usuarios activos, carritos de compras o memorias caché.

- **Ejemplos:** Redis, Amazon DynamoDB.


## 3.2 Documental (Document Store)

- **¿Cómo funciona?** Guarda la información en archivos estructurado llamados **documentos**, casi siempre en formato **JSON**. En lugar de repartir los datos de un usuario en 5 tablas distintas, guardamos toda la información de ese usuario dentro de un solo documento.

- **Ventaja:** Podemos buscar datos específicos dentro del documento sin tener que extraerlo completo. Además dos documentos no tienen que tener obligatoriamente los mismos campos

- **Casos de uso:** Perfiles de usuarios, catálogos de productos de tiendas virtuales, blogs.

- **Ejemplos:** MongoDB, CouchDB.


## 3.3 Columnar (Wide-Column Store)

- **¿Cómo funciona?** Guarda los datos agrupados por **Columnas** en lugar de por filas.

- **Ventaja:** Si tenemos una tabla con 100 atributos pero solo necesitamos calcular el promedio de una columna (ej. "edad"), la base de datos lee únicamente esa columna en disco sin tocar el resto de los datos.

- **Casos de uso:** Análisis de grandes volúmenes de datos (Big Data), registro de eventos (logs), sensores de IoT.

- **Ejemplos:** Apache Cassandra, HBase.


## 3.4 Grafos (Graph Databases)

- **¿Cómo funciona?** Guarda la información usando **Nodos** (`entidades o cosas`) y **Aristas** (`las relaciones entre esas cosas`).

- **Ventaja:** Es ideal para consultar cómo se conectan los datos entre sí. Averiguar "los amigos de los amigos de un usuario" en SQL requiere operaciones pesadísimas; en grafos es casi instantáneo. 

- **Casos de uso:** Redes sociales, sistemas de recomendación (ej. Netflix o Spotify), detección de fraudes bancarios.

- **Ejemplos:** Neo4j, Amazon Neptune.


## 4. El Teorema CAP
En un sistema de bases de datos distribuido (varios servidores trabajando juntos en red), el **Teorema CAP dice que solo podemos garantizar 2 de estas 3 propiedades al mismo tiempo:**

1. **Consistencia `(C)`:** Si actualizamos un dato, todos los servidores muestran la nueva información al mismo milisegundo. Nadie lee datos viejos.

2. **Disponibilidad (`A`):** El sistema siempre responde a las peticiones, nunca te da un error de **"servidor no disponible"**.

3. **Tolerancia a Particiones (`P`):** El sistema sigue funcionando aunque se rompa el cable de red que conecta los servidores entre sí.

**La realidad en sistemas distribuidos:** La Tolerancia a Particiones (`P`) es obligatoria (las redes siempre pueden fallar). Por lo tanto, en un fallo de red solo podemos elegir entre **C** o **A**:
- **Elegir C (`CP`):** Preferimos bloquear el sistema o dar error antes que entregar un dato desactualizado  `Ejemplo: MongoDB`.

- **Elegir A (`AP`):**  Preferimos responder rápido con lo que tengamos, aunque el dato esté desactualizado unos milisegundos mientras se sincronizan los servidores (Ejemplo: Cassandra).



## ¿Dónde encaja ETL?
**ETL** significa Extract (Extraer), Transform (Transformar) y Load (Cargar). Es el proceso o tubería de software que se encarga de mover datos de un lugar a otro:

1. **Extraer:** Sacar los datos desde sus orígenes (Bases de datos SQL viejas, archivos, APIs, etc.).

2. **Transformar:** Limpiar los datos, quitar lo que no sirve y cambiarles el formato (por ejemplo, convertir tablas de SQL a un JSON para NoSQL).

3. **Sacar:** Guardar esos datos ya transformados dentro de la base de datos NoSQL final.
