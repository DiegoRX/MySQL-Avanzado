# Common Table Expressions (CTE) en MySQL
Las **Common Table Expressions (CTE)** en MySQL son consultas temporales que permiten definir un conjunto de resultados reutilizable dentro de una consulta más grande. Se definen con la cláusula **`WITH`** y mejoran la legibilidad y reutilización del código SQL.

---

## **📌 Características Clave de las CTE**

✅ **Reutilización**: Permiten definir una subconsulta que se puede usar varias veces en la misma consulta.  
✅ **Legibilidad**: Hacen que las consultas complejas sean más fáciles de entender.  
✅ **Simplicidad**: Evitan el uso de subconsultas anidadas, lo que mejora la organización del código.  
✅ **Compatibilidad**: Introducidas en **MySQL 8.0** (⚠️ _No están disponibles en versiones anteriores_).

---

## **📌 Sintaxis de una CTE**

```sql
WITH nombre_cte AS (
    SELECT columna1, columna2 
    FROM tabla 
    WHERE condicion
)
SELECT * FROM nombre_cte;
```

🔹 `WITH nombre_cte AS (...)` → Define la CTE con una subconsulta.  
🔹 `SELECT * FROM nombre_cte;` → Usa la CTE en una consulta principal.

---

## **📌 Ejemplo 1: Usando una CTE para Filtrar Datos**

```sql
WITH clientes_activos AS (
    SELECT id, nombre, saldo
    FROM clientes
    WHERE saldo > 1000
)
SELECT * FROM clientes_activos;
```

📌 Aquí, `clientes_activos` almacena temporalmente los clientes con saldo superior a 1000.

---

## **📌 Ejemplo 2: Usando una CTE con JOIN**

```sql
WITH ventas_2024 AS (
    SELECT cliente_id, SUM(total) AS total_compras
    FROM ventas
    WHERE YEAR(fecha) = 2024
    GROUP BY cliente_id
)
SELECT c.nombre, v.total_compras
FROM clientes c
JOIN ventas_2024 v ON c.id = v.cliente_id;
```

📌 Aquí, `ventas_2024` calcula el total de compras por cliente en 2024 y luego se une con la tabla `clientes`.
### **Datos de Ejemplo**

#### **Tabla `clientes`**

|id|nombre|
|---|---|
|1|Juan|
|2|María|
|3|Carlos|

#### **Tabla `ventas`**

| id  | cliente_id | total | fecha      |
| --- | ---------- | ----- | ---------- |
| 1   | 1          | 100   | 2024-01-15 |
| 2   | 1          | 200   | 2024-03-22 |
| 3   | 2          | 50    | 2024-02-10 |
| 4   | 2          | 150   | 2024-06-05 |
| 5   | 3          | 300   | 2023-11-20 |

### **Resultado de la Consulta**

|nombre|total_compras|
|---|---|
|Juan|300|
|María|200|

---

## **📌 Ejemplo 3: CTE Recursiva (Jerarquías)**

Las **CTE recursivas** permiten consultas jerárquicas, como estructuras de empleados y gerentes.

```sql
WITH RECURSIVE empleados_jefes AS (
    SELECT id, nombre, jefe_id
    FROM empleados
    WHERE id = 1  -- Empezamos con un empleado específico

    UNION ALL

    SELECT e.id, e.nombre, e.jefe_id
    FROM empleados e
    JOIN empleados_jefes ej ON e.jefe_id = ej.id
)
SELECT * FROM empleados_jefes;
```

### **📌 Resultado de la consulta con `id = 1` como jefe inicial**

| id  | nombre | jefe_id |
| --- | ------ | ------- |
| 1   | Carlos | NULL    |
| 2   | Ana    | 1       |
| 3   | Pedro  | 1       |
| 4   | Laura  | 2       |
| 5   | Juan   | 2       |
| 6   | Marta  | 3       |
| 7   | Luis   | 3       |

📌 La consulta **recorre la jerarquía** desde el jefe `id = 1` y encuentra todos los empleados que dependen de él, ya sea **directa o indirectamente**.

✅ Primero selecciona a **Carlos (id=1)**.  
✅ Luego encuentra a **Ana (id=2) y Pedro (id=3)** porque su `jefe_id` es `1`.  
✅ Luego encuentra a **Laura (id=4) y Juan (id=5)** porque su `jefe_id` es `2`.  
✅ Finalmente, encuentra a **Marta (id=6) y Luis (id=7)** porque su `jefe_id` es `3`.

---

### **📌 Explicación del CTE recursivo**

1. **Caso base (`SELECT id, nombre, jefe_id FROM empleados WHERE id = 1`)**
    - Encuentra al **primer jefe** (`Carlos` en este caso).
2. **Caso recursivo (`UNION ALL`)**
    - Toma los empleados que tienen un `jefe_id` dentro de los empleados ya seleccionados.
    - **Se repite** hasta que no haya más empleados que coincidan.

---

## **📌 Visualización de la Jerarquía en Forma de Árbol**

scss

CopiarEditar

`Carlos (1)  

├── Ana (2)  │   
├── Laura (4)  │   ├── Juan (5)  ├── Pedro (3)      ├── Marta (6)      ├── Luis (7)`

📌 Como se puede ver, la consulta construye la jerarquía de manera automática. 🚀

---

## **🚀 Ventajas de Usar CTEs**

✅ **Mejor organización del código**.  
✅ **Reemplaza subconsultas complejas**, haciéndolas más legibles.  
✅ **Facilita consultas jerárquicas** con CTE recursivas.  
✅ **Mejor rendimiento** en algunos casos al reutilizar datos en la misma consulta.

---

### **📌 Conclusión**

Las **CTEs en MySQL 8.0** son una herramienta poderosa para organizar y optimizar consultas SQL, especialmente cuando se manejan estructuras de datos complejas o jerárquicas. 🚀