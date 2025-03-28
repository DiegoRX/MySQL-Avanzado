### **JSON en MySQL**

Desde **MySQL 5.7**, se introdujo el tipo de dato **JSON**, que permite almacenar y manipular datos estructurados en formato JSON dentro de la base de datos.

---
## Operadores
En MySQL, cuando trabajas con columnas de tipo **JSON**, puedes usar los siguientes operadores para acceder a los datos dentro de ellas:

- **`->`**: Devuelve un objeto JSON.
- **`->>`**: Devuelve un valor de texto (_string_).

### **Ejemplos**

#### **Si deseas obtener un objeto JSON:**

sql

CopiarEditar

`SELECT Properties->'$.ClientID' FROM Activity;`

#### **Si deseas obtener un valor de texto:**

sql

CopiarEditar

`SELECT Properties->>'$.ClientID' FROM Activity;`

🚀 **Conclusión:**  
Si `ClientID` es un número o texto y quieres obtenerlo como un valor legible, usa **`->>`**.  
Si `ClientID` es un objeto JSON dentro de `Properties`, usa **`->`**.

4o
## **1️⃣ Creación de una tabla con JSON**

```sql
CREATE TABLE productos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    detalles JSON
);
```

Aquí, la columna `detalles` almacena datos en formato JSON.

---

## **2️⃣ Insertar datos en una columna JSON**

```sql
INSERT INTO productos (nombre, detalles) 
VALUES ('Laptop', '{"marca": "Dell", "precio": 1500, "es_gaming": true}');
```

---

## **3️⃣ Seleccionar datos de una columna JSON**

```sql
SELECT nombre, detalles->>'$.marca' AS marca, detalles->>'$.precio' AS precio 
FROM productos;
```

🔹 `->>` se usa para extraer valores como texto.

🔹 `'$.[clave]'` es la sintaxis para acceder a una clave dentro del JSON.

---

## **4️⃣ Actualizar un campo JSON**

```sql
UPDATE productos 
SET detalles = JSON_SET(detalles, '$.precio', 1600) 
WHERE nombre = 'Laptop';
```

🔹 `JSON_SET` modifica valores dentro del JSON sin sobrescribirlo.

---

## **5️⃣ Filtrar datos dentro de un JSON**

```sql
SELECT * FROM productos 
WHERE JSON_EXTRACT(detalles, '$.marca') = 'Dell';
```

🔹 `JSON_EXTRACT` extrae valores de un JSON para poder compararlos.

---

## **6️⃣ Agregar un nuevo campo dentro del JSON**

```sql
UPDATE productos 
SET detalles = JSON_INSERT(detalles, '$.descuento', 10) 
WHERE nombre = 'Laptop';
```

🔹 `JSON_INSERT` añade una nueva clave sin modificar los valores existentes.

---

## **7️⃣ Eliminar una clave dentro del JSON**

```sql
UPDATE productos 
SET detalles = JSON_REMOVE(detalles, '$.es_gaming') 
WHERE nombre = 'Laptop';
```

🔹 `JSON_REMOVE` elimina una clave específica dentro del JSON.

---

✅ **Ventajas del uso de JSON en MySQL**

- Permite **almacenar datos flexibles** sin esquemas fijos.
- Optimizado para **búsquedas y actualizaciones** de datos estructurados.
- Se puede combinar con **índices** para mejorar el rendimiento.

🚀 **Ideal para aplicaciones que manejan datos semiestructurados!**

### **Ejemplo de uso de `->` (Mantiene formato JSON)**

sql

CopiarEditar

`SELECT datos_json->'$.nombre' FROM clientes;`

📌 **Resultado:**

javascript

CopiarEditar

`"Juan"  (Tipo JSON con comillas)`

🔹 Devuelve el valor como **JSON**, no como una cadena de texto simple.

---

### **Ejemplo de uso de `->>` (Convierte a cadena de texto)**

sql

CopiarEditar

`SELECT datos_json->>'$.nombre' FROM clientes;`

📌 **Resultado:**

scss

CopiarEditar

`Juan  (Tipo STRING)`

🔹 Aquí el resultado es un **`VARCHAR`**, sin comillas JSON.