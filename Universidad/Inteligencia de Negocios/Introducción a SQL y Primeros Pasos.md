# 1. Acceso Inicial a Oracle Database (SQL* Plus)
Trans completar la instalación de Oracle 10g, la interacción inicial y la administración de la base de datos se realizan a través de la interfaz de línea de comando **SQL* Plus***

## Paso 1.1: Abrir la Terminal de Windows
En el buscador de **Windows** buscamos **cmd** y lo ejecutamos como administrador 

## Paso 1.2: Conectarse como Superadministrador (SYSDBA)
En el **CMD** Ejecutamos el siguiente comando compuesto por nuestro usuario (por defecto es `system`) y nuestra contraseña que pusimos en la instalación  (en mi caso es `2222`)

```
sqlplus system/2222
```

Al conectarse exitosamente, el indicador de la terminal cambiará de la ruta de **Windows** `(C:\Users\...)` al prompt interactivo de Oracle:

```
SQL>
```


# 2. Ajustes de Visualización de la Consola
Por defecto, la salida de las tablas en **SQL* Plus*** se corta y desordena si las columnas superan el ancho de la ventana. Para corregirlo en la sesión actual:

```
-- Ajustar el ancho de línea a 200 caracteres para evitar saltos desordenados
SET LINESIZE 200;

-- Definir que se muestren 50 filas antes de repetir los encabezados
SET PAGESIZE 50;
```


# 3. Creación y Configuración del Usuario de Trabajo 
No es recomendable crear tablas ni almacenar datos dentro de los esquemas administrativos (`SYS` o `SYSTEM`). Es necesario crear un nuevo usuario independiente con permisos suficientes.

## Paso 3.1: Crear el Usuario

```
CREATE USER sebastian IDENTIFIED BY sebas;
```

- `sebastian`: Nombre del usuario a registrar.
- `sebas`: Contraseña asignada.


## 3.2: Asignar Privilegios y Rol DBA
Un usuario nuevo en Oracle no tiene permiso ni para iniciar sesión. Se le deben de otorgar  los roles necesarios:

```
-- Otorgar permisos de conexión, creación de objetos y privilegios totales de administración
GRANT CONNECT, RESOURSE, DBA TO sebastian;
```


# 4. Iniciar Sesión con el Nuevo Usuario
Una vez creado el usuario y otorgados los privilegios, cambiamos de sesión para trabajar bajo su propio esquema:

```
-- Cambiar de usuario en la sesión activa
CONNECT sebastian/sebas
```

## Conectarse a un usuario en especifico 
Cuando recién iniciemos el pc y queramos conectarnos a un usuario en especifico luego de haber abierto en cmd escribiremos lo siguiente (`cada quien con sus respectivas credenciales`)

```
sqlplus sebastian/sebas
```

### Verificar en que usuario estamos conectados:

```
SHOW USER;
```

### Salida esperada:

```
User is "SEBASTIAN"
```


# 5. Primeras Operaciones con Tablas (DDL y DML)

## Paso 5.1: Crear una Tabla (DDL)
Definimos una tabla para gestionar registros básicos:

```
CREATE TABLE estudiantes (
	id_estudiante NUMBER(5) PRIMARY KEY,
	nombre        VARCHAR2(50) NOT NULL,
	apellido      VARCHAR2(50) NOT NULL,
	carrera       VARCHAR2(50), 
	semestre      NUMBER(2),
	fecha_ingreso DATE DEFAULT SYSDATE
);
```

![[Introducción a SQL y Primeros Pasos-20260822213308.png]]

### Verificar la estructura  creada

```
DESCRIBE estudiantes;
```

![[Introducción a SQL y Primeros Pasos-20260822213321.png]]

### Verificar todas las tablas creadas por el usuario actual 

```
SELECT table_name FROM user_tables;
```

![[Introducción a SQL y Primeros Pasos-20260822232334.png|481]]
## Paso 5.2: Insertar Datos (DML)

```
INSERT INTO estudiantes (id_estudiante, nombre, apellido, carrera, semestre)
VALUES (1001, 'Sebastian', 'Diaz', 'Ingenieria de Sistemas', 9);

INSERT INTO estudiantes (id_estudiante, nombre, apellido, carrera, semestre)
VALUES (1002, 'Daniela', 'Cardona', 'Ingenieria de Sistemas', 7);

INSERT INTO estudiantes (id_estudiante, nombre, apellido, carrera, semestre)
VALUES (1003, 'Carlos', 'Fernandez', 'Psicología', 10);
```


![[Pasted image 20260817224715.png]]

## Paso 5.3: Consultar Datos (DQL)

```
-- Listar toda la información de la tabla
SELECT * FROM estudiantes;
```

![[Introducción a SQL y Primeros Pasos-20260822235734.png]]

```
-- Filtrar registros específicos
SELECT nombre, apellido, carrera
FROM estudiantes
WHERE semestre >= 8;
```

![[Introducción a SQL y Primeros Pasos-20260822234705.png|481]]
## Paso 5.4: Modificar y Eliminar Registros (DML)

### Modificar

```
-- Actualizar un dato específico
UPDATE estudiantes
SET semestre = 10
WHERE id_estudiante = 1001;
```

![[Introducción a SQL y Primeros Pasos-20260822235623.png|384]]

### Eliminar 

```
-- Eliminar un registro
DELETE FROM estudiantes
WHERE id_estudiante = 1002;

-- Confirmar los cambios
COMMIT;
```

![[Introducción a SQL y Primeros Pasos-20260823000056.png|383]]
