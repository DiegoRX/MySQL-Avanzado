# MySQL Prepared Statements (Consultas Preparadas)
Un **Prepared Statement** en MySQL es una consulta precompilada que permite ejecutar la misma instrucción SQL varias veces con diferentes valores. Se usa principalmente para **mejorar el rendimiento** y **prevenir inyecciones SQL**.

---

## **✅ Ventajas de los Prepared Statements**

✔ **Evitan inyecciones SQL**: Los valores se envían por separado, evitando ataques de inyección.  
✔ **Mejoran el rendimiento**: La consulta se compila una sola vez y se reutiliza.  
✔ **Separan la lógica SQL y los datos**: Se pueden ejecutar con distintos parámetros.

---

## **🚀 Uso de Prepared Statements en MySQL**

### **1️⃣ Sintaxis en MySQL**

Los **Prepared Statements** funcionan en **MySQL a nivel de SQL** con la siguiente estructura:


```sql
PREPARE stmt FROM 'SQL_QUERY';   
SET @variable = 'valor';   
EXECUTE stmt USING @variable;   
DEALLOCATE PREPARE stmt;
````
📌 **Ejemplo con una consulta SELECT**:

```sql
PREPARE mi_consulta FROM 'SELECT * FROM clientes WHERE ciudad = ?';   
SET @ciudad = 'Bogotá';  
EXECUTE mi_consulta USING @ciudad; 
DEALLOCATE PREPARE mi_consulta;
````
🔹 `?` es un **placeholder** para los valores que se pasan con `USING`.