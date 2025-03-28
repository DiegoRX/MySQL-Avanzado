# INFORMATION_SCHEMA en MySQL
### 📌 **¿Qué es `INFORMATION_SCHEMA` en MySQL?**

`INFORMATION_SCHEMA` es un esquema especial en MySQL que actúa como un **diccionario de datos**. Contiene metadatos sobre las bases de datos, tablas, columnas, índices, vistas, procedimientos almacenados y otros objetos del sistema.

🔹 **No almacena datos de usuario**, sino información sobre la estructura y configuración de MySQL.  
🔹 Se puede consultar como cualquier otra base de datos, pero **no se pueden modificar sus tablas directamente**.

---

### 📌 **¿Para qué sirve `INFORMATION_SCHEMA`?**

✅ Obtener información sobre bases de datos y tablas.  
✅ Listar columnas, índices y restricciones de una tabla.  
✅ Ver detalles sobre procedimientos almacenados y funciones.  
✅ Consultar el tamaño de bases de datos y tablas.  
✅ Revisar permisos de usuarios.

---

### 📌 **Tablas principales de `INFORMATION_SCHEMA`**

Aquí algunas de las tablas más utilizadas en `INFORMATION_SCHEMA`:

| Tabla              | Descripción                                                        |
| ------------------ | ------------------------------------------------------------------ |
| `SCHEMATA`         | Contiene información sobre las bases de datos.                     |
| `TABLES`           | Lista todas las tablas y vistas en MySQL.                          |
| `COLUMNS`          | Muestra información sobre las columnas de cada tabla.              |
| `ROUTINES`         | Contiene información sobre funciones y procedimientos almacenados. |
| `VIEWS`            | Muestra todas las vistas de las bases de datos.                    |
| `KEY_COLUMN_USAGE` | Muestra claves primarias y foráneas.                               |
| `USER_PRIVILEGES`  | Lista los privilegios de los usuarios.                             |

---
## Todas las tablas en el `INFORMATION_SCHEMA`
Todas las tablas que contiene el **`INFORMATION_SCHEMA`** en MySQL, junto con una descripción de cada una:

| **Tabla**                   | **Descripción**                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------- |
| **SCHEMATA**                | Contiene información sobre todas las bases de datos en el servidor.                           |
| **TABLES**                  | Lista todas las tablas y vistas en cada base de datos.                                        |
| **COLUMNS**                 | Contiene detalles sobre las columnas de todas las tablas.                                     |
| **VIEWS**                   | Información sobre todas las vistas en el servidor.                                            |
| **ROUTINES**                | Almacena detalles sobre funciones y procedimientos almacenados.                               |
| **TRIGGERS**                | Contiene información sobre todos los triggers creados en MySQL.                               |
| **EVENTS**                  | Lista todos los eventos programados en MySQL.                                                 |
| **STATISTICS**              | Información sobre los índices de cada tabla.                                                  |
| **KEY_COLUMN_USAGE**        | Contiene información sobre claves primarias y foráneas en las tablas.                         |
| **REFERENTIAL_CONSTRAINTS** | Muestra restricciones de claves foráneas en la base de datos.                                 |
| **TABLE_CONSTRAINTS**       | Contiene información sobre restricciones en tablas (PRIMARY KEY, FOREIGN KEY, UNIQUE, CHECK). |
| **COLLATIONS**              | Lista las collation (ordenamientos de caracteres) disponibles en el servidor.                 |
| **CHARACTER_SETS**          | Información sobre los conjuntos de caracteres disponibles en MySQL.                           |
| **ENGINES**                 | Lista los motores de almacenamiento soportados en MySQL.                                      |
| **PROCESSLIST**             | Muestra los procesos activos en MySQL (similar a `SHOW PROCESSLIST`).                         |
| **USER_PRIVILEGES**         | Lista los privilegios globales de los usuarios en el servidor.                                |
| **SCHEMA_PRIVILEGES**       | Contiene los privilegios de cada usuario sobre bases de datos específicas.                    |
| **TABLE_PRIVILEGES**        | Lista los permisos de usuarios sobre tablas específicas.                                      |
| **COLUMN_PRIVILEGES**       | Muestra los privilegios de usuarios sobre columnas específicas.                               |
| **PARAMETERS**              | Información sobre los parámetros de procedimientos almacenados y funciones.                   |
| **INNODB_TRX**              | Muestra información sobre transacciones activas en InnoDB.                                    |
| **INNODB_LOCKS**            | Lista los bloqueos actuales en tablas InnoDB.                                                 |
| **INNODB_LOCK_WAITS**       | Muestra qué transacciones están esperando por un bloqueo.                                     |
| **PARTITIONS**              | Contiene información sobre las particiones de tablas.                                         |
| **OPTIMIZER_TRACE**         | Información sobre el optimizador de consultas en MySQL.                                       |

### 📌 **Ejemplos de uso**

#### 🔹 **Ver todas las bases de datos en MySQL**

```sql
SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;
```

#### 🔹 **Listar todas las tablas de una base de datos específica**

```sql
SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_SCHEMA = 'nombre_de_tu_base_de_datos';
```

#### 🔹 **Obtener información sobre las columnas de una tabla**

```sql
SELECT COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE TABLE_SCHEMA = 'nombre_de_tu_base_de_datos' 
AND TABLE_NAME = 'nombre_de_tu_tabla';


SELECT ROUTINE_NAME, ROUTINE_TYPE, DATA_TYPE, CREATED, LAST_ALTERED  FROM INFORMATION_SCHEMA.ROUTINES  WHERE ROUTINE_SCHEMA = 'tienda';
```

#### 🔹 **Ver todas las funciones y procedimientos almacenados**

```sql
SELECT ROUTINE_NAME, ROUTINE_TYPE 
FROM INFORMATION_SCHEMA.ROUTINES 
WHERE ROUTINE_SCHEMA = 'nombre_de_tu_base_de_datos';
```

#### 🔹 **Obtener el tamaño de una base de datos en MB**

```sql
SELECT table_schema AS base_de_datos, 
       ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS tamaño_MB 
FROM INFORMATION_SCHEMA.TABLES 
GROUP BY table_schema;
```

---

### 📌 **¿Dónde se encuentra `INFORMATION_SCHEMA`?**

Es un esquema **virtual** (no físico), por lo que no lo verás al ejecutar `SHOW DATABASES;`. Sin embargo, puedes acceder a él como cualquier otra base de datos usando:

```sql
USE INFORMATION_SCHEMA;
SHOW TABLES;
```

🚀 **Conclusión:** `INFORMATION_SCHEMA` es una herramienta clave para administrar bases de datos en MySQL, permitiéndote acceder a metadatos sin necesidad de acceder directamente a los archivos del sistema.