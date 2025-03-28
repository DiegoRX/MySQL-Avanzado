# Tablas Temporales
### 📌 **¿Dónde se guardan las tablas temporales en MySQL?**

Las **tablas temporales** en MySQL se almacenan en **memoria RAM** o en el **disco**, dependiendo del tamaño de los datos y la configuración del servidor.

🔹 **Si la tabla temporal es pequeña**, MySQL la guarda en memoria usando el motor de almacenamiento **[[MEMORY MySQL]]**.  
🔹 **Si la tabla temporal es grande**, MySQL la traslada al disco dentro del directorio de almacenamiento temporal.

---

### 📌 **Ubicación de almacenamiento de tablas temporales**

#### 🔹 **1. Tablas temporales en memoria (RAM)**

Si una tabla temporal cabe en la memoria, se almacena en el **heap memory** del motor `MEMORY`.

- Se usa cuando las columnas **no contienen tipos de datos TEXT o BLOB**.
- Es más rápido porque accede directamente a la memoria sin escribir en el disco.

#### 🔹 **2. Tablas temporales en disco**

Cuando la tabla temporal excede el límite de memoria configurado en `tmp_table_size` o `max_heap_table_size`, MySQL la guarda en el **directorio temporal del sistema**, que varía según la configuración:

- **Linux/macOS**:  
    Por defecto en `/tmp/`
- **Windows**:  
    En una carpeta temporal como `C:\Windows\Temp\`

Puedes ver o cambiar la ubicación con:

```sql
SHOW VARIABLES LIKE 'tmpdir';
```

Si quieres cambiar la ubicación de las tablas temporales en disco, edita el archivo de configuración `my.cnf` o `my.ini` y agrega:

```ini
[mysqld]
tmpdir = /ruta/a/tu/directorio
```

---

### 📌 **¿Cuándo se eliminan las tablas temporales?**

1. Cuando la sesión de MySQL que las creó se cierra.
2. Cuando el servidor MySQL se reinicia.
3. Si ejecutas `DROP TEMPORARY TABLE nombre_tabla;`

---

### 📌 **Ejemplo de uso de tablas temporales**

```sql
CREATE TEMPORARY TABLE temp_clientes (
    id INT PRIMARY KEY,
    nombre VARCHAR(100),
    correo VARCHAR(100)
);
```

Las puedes usar como una tabla normal, pero solo dentro de la misma sesión.

🚀 **Conclusión:** MySQL almacena tablas temporales en memoria (RAM) cuando son pequeñas y en disco cuando superan el límite de memoria.