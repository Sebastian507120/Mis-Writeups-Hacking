
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

- ¿Cómo funciona? guarda la información en archivos estructurado llamados **documentos**, casi siempre en formato **JSON**. En lugar de repartir los datos de un usuario en 5 tablas distintas, guardamos toda la información de ese usuario dentro de un solo documento.

- **Ventaja:** Podemos buscar datos específicos dentro del documento sin tener que extraerlo completo. Además dos documentos 