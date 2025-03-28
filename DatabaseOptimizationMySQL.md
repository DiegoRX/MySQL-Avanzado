# **📌 Database Optimization en MySQL**

**Database Optimization** en MySQL se refiere al conjunto de técnicas y estrategias utilizadas para mejorar el **rendimiento**, la **eficiencia** y la **velocidad** de las consultas y operaciones en la base de datos. Esto es clave para evitar problemas de lentitud, alto consumo de recursos y bloqueos en las transacciones.

---

## **🔹 Principales Técnicas de Optimización en MySQL**

### **1️⃣ Optimización de Índices**

[[Index MySQL]]

**📌 ¿Qué son?**  
Los índices en MySQL permiten acceder más rápido a los datos sin recorrer toda la tabla.

✅ **Ejemplo de Índice**

```sql
CREATE INDEX idx_nombre ON clientes(nombre);
```

Esto mejora búsquedas con:

```sql
SELECT * FROM clientes WHERE nombre = 'Juan';
```

**🛠 Buenas prácticas:**  
✔ Usa índices en columnas frecuentemente consultadas.  
✔ No indexes **todo**, ya que afectan las inserciones y actualizaciones.

---

### **2️⃣ Optimización de Consultas SQL**

### Optimización de `SELECT`
- Target Required Columns
- Avoid Functions
- Evitar el uso de [[leading wildcards]] o comodines
- Usa [[INNER JOIN]] cuando sea posible
- use clauses sparingly


✅ **Ejemplo de consulta optimizada**

```sql
-- Evita SELECT * (carga innecesaria)
SELECT nombre, saldo FROM clientes WHERE saldo > 1000;

-- NO HACER:
SELECT DISTINCT id, nombre, edad, ciudad
FROM usuarios
LEFT JOIN pedidos ON usuarios.id = pedidos.usuario_id
RIGHT JOIN pagos ON usuarios.id = pagos.usuario_id
FULL JOIN direcciones ON usuarios.id = direcciones.usuario_id
WHERE (edad > 18 OR edad <= 18) 
  AND (ciudad IS NOT NULL OR ciudad IS NULL) 
  AND (nombre LIKE '%a%' OR nombre NOT LIKE '%a%') 
  AND (id IN (SELECT id FROM usuarios)) 
  AND 1=1 
  AND EXISTS (SELECT 1 FROM usuarios WHERE usuarios.id = usuarios.id)
GROUP BY id, nombre, edad, ciudad
HAVING COUNT(pedidos.id) >= 0
ORDER BY nombre ASC, nombre DESC
LIMIT 1000000000;

```

✔ **Usa `EXPLAIN`** para analizar el rendimiento de una consulta:

```sql
EXPLAIN SELECT nombre FROM clientes WHERE saldo > 1000;
```

Esto muestra cómo MySQL ejecuta la consulta y si usa índices.

---

### **3️⃣ Normalización vs. Desnormalización**

✔ **Normalización:** Evita redundancia y mantiene integridad de datos.  
✔ **Desnormalización:** Mejora rendimiento combinando tablas en una sola consulta.

✅ **Ejemplo de Normalización (Tablas separadas)**

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nombre VARCHAR(100)
);

CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT,
    total DECIMAL(10,2),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

Esto reduce la duplicación de datos.

---

### **4️⃣ Particionamiento de Tablas**

Divide grandes tablas en secciones más pequeñas para mejorar la velocidad de consulta.

✅ **Ejemplo de partición por rango**

```sql
ALTER TABLE pedidos
PARTITION BY RANGE (YEAR(fecha_pedido)) (
    PARTITION p0 VALUES LESS THAN (2020),
    PARTITION p1 VALUES LESS THAN (2023),
    PARTITION p2 VALUES LESS THAN (2025)
);
```

🔹 **Ideal para grandes volúmenes de datos**.

---

### **5️⃣ Uso de Caché y Query Cache**

**📌 ¿Qué es?**  
Permite almacenar resultados de consultas frecuentes para acelerar respuestas.

✅ **Activar caché de consultas en MySQL**

```sql
SET GLOBAL query_cache_size = 16777216; -- 16MB
SET GLOBAL query_cache_type = ON;
```

🔹 **Nota:** Desde MySQL 8+, usa Redis o Memcached en su lugar.

---

### **6️⃣ Optimización de Tablas (`OPTIMIZE TABLE`)**

Limpia registros eliminados y mejora el almacenamiento.

✅ **Ejemplo**

```sql
OPTIMIZE TABLE clientes;
```

✔ Útil después de muchas eliminaciones (`DELETE`).
🚀
![[Pasted image 20250322002850.png]]
![[Pasted image 20250322002902.png]]![[Pasted image 20250322003138.png]]![[Pasted image 20250322003146.png]]![[Pasted image 20250322003153.png]]![[Pasted image 20250322003201.png]]![[Pasted image 20250322003208.png]]