# Funciones avanzadas de MySQL y procedimiento almacenado
#### Funciones avanzadas de MySQL y procedimiento almacenado
##### Funciones y procedimiento almacenado en MySQL
Empacar código en una función o procedimiento para reutilizarlo múltiples veces
###### Ventajas
- Consistencia: código estandarizado
- Organización: código organizado
- Reusabilidad: código que se puede utilizar múltiples veces
### Procedure
Un **procedure** en MySQL es un procedimiento almacenado, es decir, un bloque de código SQL predefinido que se puede ejecutar en la base de datos. Sirve para encapsular una serie de instrucciones SQL que se pueden reutilizar, lo que mejora la eficiencia y la seguridad del sistema.

### **Características de un Procedure en MySQL**:

- Se almacena en la base de datos y se ejecuta con una llamada.
- Puede aceptar parámetros de entrada y salida.
- Reduce la repetición de código y mejora la organización del SQL.
- Mejora la seguridad al restringir el acceso a ciertas operaciones mediante la ejecución controlada del código SQL.
- Puede contener lógica condicional y bucles.

---

### **Sintaxis básica para crear un Procedure**

```sql
DELIMITER //

CREATE PROCEDURE nombre_procedimiento(IN parametro1 INT, OUT resultado INT)
BEGIN
    -- Código SQL aquí
    SELECT parametro1 * 2 INTO resultado;
END //

DELIMITER ;
---
```
### **Ejemplo práctico: Un Procedure para sumar dos números**
```sql
DELIMITER //

CREATE PROCEDURE SumarNumeros(IN num1 INT, IN num2 INT, OUT resultado INT)
BEGIN
    SET resultado = num1 + num2;
END //

DELIMITER ;
````
### **Ejecutar el Procedure**
```sql
CALL SumarNumeros(5, 10, @resultado);
SELECT @resultado; -- Esto mostrará 15
```
---

### **Eliminar un Procedure**

Si necesitas eliminar un procedimiento almacenado:

```sql
DROP PROCEDURE IF EXISTS nombre_procedimiento;
```

## Funciones en MySQL
En **MySQL**, una **función almacenada** es similar a un **procedure**, pero con algunas diferencias clave. Una función en MySQL devuelve un **valor único** y se puede utilizar dentro de consultas SQL, mientras que un procedimiento puede ejecutar múltiples instrucciones sin devolver necesariamente un valor.

---

## **Diferencias entre una Función y un Procedure**

|Característica|Function|Procedure|
|---|---|---|
|Retorna un valor único|✅ Sí|❌ No|
|Puede usarse en SELECT|✅ Sí|❌ No|
|Puede modificar datos|❌ No (En general)|✅ Sí|
|Llamada con CALL|❌ No|✅ Sí|

---

## **Sintaxis para crear una Función**

```sql
DELIMITER //  
CREATE FUNCTION nombre_funcion(param1 INT, param2 INT)  
RETURNS INT DETERMINISTIC 
BEGIN
	DECLARE resultado INT;
	SET resultado = param1 + param2;
	RETURN resultado; 
END //  
DELIMITER ;
```
📌 **Notas clave sobre la sintaxis:**

- `RETURNS INT` indica que la función devuelve un valor entero.
- `DETERMINISTIC` significa que la función siempre devuelve el mismo resultado para los mismos parámetros (esto es importante para la optimización de MySQL).

---

## **Ejemplo de una Función para sumar dos números**

```sql
DELIMITER //  

CREATE FUNCTION SumarNumeros(num1 INT, num2 INT)  
RETURNS INT DETERMINISTIC 
BEGIN     
	RETURN num1 + num2; 
END //  

DELIMITER ;
```
---

## **Cómo usar la Función en una consulta**

Puedes llamarla dentro de un `SELECT` como si fuera una función nativa de MySQL:

```sql
SELECT SumarNumeros(5, 10); -- Retorna 15
```
También se puede utilizar dentro de otras consultas:

```sql
SELECT id, nombre, SumarNumeros(precio, impuesto) AS precio_final FROM productos;
```
---

## **Eliminar una Función**

Si necesitas eliminar una función:

```sql
DROP FUNCTION IF EXISTS SumarNumeros;
```
---

## **Cuándo usar una Función en MySQL**

- Para cálculos rápidos y reutilizables dentro de consultas SQL.
- Cuando necesitas obtener un **único valor** en lugar de ejecutar múltiples instrucciones.
- Para mejorar la legibilidad y modularidad del código SQL.
## Variables y parámetros

```sql
SET @order_id = 3;
SELECT * FROM Orders WHERE OrderID = @order_id;
```
Crear una variable dentro de un procedimiento almacenado
```sql
DECLARE variable_name DATATYPE DEFAULT VALUE;
DECLARE minimun_order_cost DECIMAL(5, 2) 0;
```
#### **Ejemplo dentro de un PROCEDURE:**

```sql
DELIMITER // 

CREATE PROCEDURE CheckOrderCost() 
BEGIN
DECLARE minimum_order_cost DECIMAL(5,2) DEFAULT 50.00;
SELECT minimum_order_cost AS "Costo Mínimo del Pedido";
END // 
DELIMITER ;
```
### **Ejecutar el Procedure:**

```sql
CALL CheckOrderCost();
```
🔹 Esto devolverá **50.00** como resultado.

---

## **Tipos de operadores de asignación en MySQL**
En **MySQL**, el **operador de asignación** (`=`) se usa para asignar valores a **variables**, **columnas** y **campos** en una consulta. También existen operadores específicos como `:=` y `SET` para asignaciones en ciertos contextos.

| Operador | Descripción                                          | Ejemplo              |
| -------- | ---------------------------------------------------- | -------------------- |
| `=`      | Asigna un valor a una variable, columna o campo.     | `SET variable = 10;` |
| `:=`     | Asigna un valor dentro de una consulta o bloque SQL. | `SET @x := 20;`      |

---

## **Ejemplos de uso en diferentes contextos**

### **1️⃣ Asignación a una variable con `SET`**

```sql
SET @mi_variable = 100;
SELECT @mi_variable; -- Retorna 100
```

### **2️⃣ Uso de `:=` dentro de una consulta**

```sql
SELECT @total := COUNT(*) FROM usuarios;
SELECT @total; -- Retorna el número de usuarios
```

### **3️⃣ Asignación en un procedimiento almacenado**

```sql
DELIMITER //

CREATE PROCEDURE AsignarValor()
BEGIN
    DECLARE mi_variable INT;
    SET mi_variable = 50;
    SELECT mi_variable AS "Valor de la variable";
END //

DELIMITER ;
```

### **4️⃣ Asignación en una actualización de tabla**

```sql
UPDATE productos 
SET precio = precio * 1.10 
WHERE categoria = 'Electrónica';
```

🔹 **Aquí `=` se usa para asignar el nuevo valor de `precio` multiplicado por 1.10**.

---

## **📌 Diferencias clave entre `=` y `:=`**

1. **`=`** se usa en `SET`, `UPDATE`, y dentro de procedimientos.
2. **`:=`** es más flexible en `SELECT` y expresiones dentro de consultas.

🚀 **¿Necesitas un ejemplo específico? Dime tu caso de uso.**
## Parametros
Los parámetros en un `PROCEDURE` pueden ser de tres tipos:

|Tipo de Parámetro|Descripción|
|---|---|
|**IN**|Entrada: Recibe un valor cuando se llama al procedimiento (solo lectura).|
|**OUT**|Salida: Devuelve un valor después de ejecutar el procedimiento.|
|**INOUT**|Entrada/Salida: Recibe y devuelve un valor modificado.|

---

## **Ejemplos de uso de parámetros en MySQL**

### **1️⃣ Parámetro `IN` (Entrada)**

📌 **Ejemplo:** Procedimiento que recibe un `id_cliente` y devuelve su nombre.

```sql
DELIMITER // 
CREATE PROCEDURE ObtenerNombreCliente(IN id_cliente INT) 
BEGIN     
	SELECT nombre FROM clientes WHERE id = id_cliente; 
END //  
DELIMITER ;
```
✔ **Uso:**

```sql
CALL ObtenerNombreCliente(1);
```
---

### **2️⃣ Parámetro `OUT` (Salida)**

📌 **Ejemplo:** Procedimiento que devuelve la cantidad total de clientes.

```sql
`DELIMITER //  
CREATE PROCEDURE ContarClientes(OUT total_clientes INT) 
BEGIN     
	SELECT COUNT(*) INTO total_clientes FROM clientes; 
END //  
DELIMITER ;`
```
✔ **Uso:**

```sql
CALL ContarClientes(@total); 
SELECT @total; -- Devuelve el número de clientes`
```
---

### **3️⃣ Parámetro `INOUT` (Entrada/Salida)**

📌 **Ejemplo:** Procedimiento que multiplica un número por 2 y devuelve el resultado.

```sql
DELIMITER //  
CREATE PROCEDURE DuplicarValor(INOUT valor INT) 
BEGIN     
	SET valor = valor * 2; 
END //  
DELIMITER ;`
```
✔ **Uso:**

```sql
SET @mi_numero = 5; 
CALL DuplicarValor(@mi_numero); 
SELECT @mi_numero; -- Retorna 10`
```
---

## **📌 Resumen**

- `IN`: Recibe un valor de entrada.
- `OUT`: Devuelve un valor de salida.
- `INOUT`: Recibe y devuelve un valor modificado.
## Desarrollo de funciones definidas por el usuario
Funciones que define el usuario para ser invocadas cuando se necesiten.
Se crean porque no vienen built-in.

En **MySQL**, `CREATE FUNCTION` permite definir **funciones almacenadas**, que son bloques de código reutilizables que **reciben parámetros**, **realizan cálculos** y **devuelven un valor**.

✅ Se usan en consultas `SELECT`.  
✅ Devuelven un único valor (`RETURN`).  
✅ Pueden usarse dentro de procedimientos almacenados, `SELECT`, `WHERE`, etc.

---

## **🔹 Sintaxis básica**

```sql
DELIMITER //
CREATE FUNCTION nombre_funcion(parametros) 
RETURNS tipo_de_dato
DETERMINISTIC 
BEGIN    
	-- Lógica de la función    
	RETURN valor; 
END //  
DELIMITER ;`
```
📌 **Explicación:**

- `RETURNS tipo_de_dato`: Especifica el tipo de dato que la función devolverá.
- `DETERMINISTIC`: Indica que **siempre** devuelve el mismo resultado con los mismos valores de entrada. (Se puede usar `NOT DETERMINISTIC` si depende de variables externas).
- `RETURN valor;`: Devuelve el resultado de la función.

---

## **🔹 Ejemplo 1: Función para calcular el IVA**

```sql
DELIMITER // 
CREATE FUNCTION CalcularIVA(precio DECIMAL(10,2)) 
RETURNS DECIMAL(10,2) 
DETERMINISTIC 
BEGIN     
	RETURN precio * 1.19;  -- Suponiendo un 19% de IVA 
END //  
DELIMITER ;`
```
✔ **Uso:**

```sql
SELECT CalcularIVA(100); -- Retorna 119.00`
```
---

## **🔹 Ejemplo 2: Función para obtener el nombre de un cliente por su ID**

```sql
DELIMITER $$  
CREATE FUNCTION ObtenerNombreCliente(cliente_id INT) 
RETURNS VARCHAR(100) 
DETERMINISTIC 
BEGIN     
	DECLARE nombre_cliente VARCHAR(100);          
	SELECT nombre INTO nombre_cliente FROM clientes WHERE id = cliente_id;
	RETURN nombre_cliente; 
END $$  
DELIMITER ;`
```
✔ **Uso:**

```sql
SELECT ObtenerNombreCliente(1);
```
---

## **🔹 Diferencias entre `CREATE FUNCTION` y `CREATE PROCEDURE`**

| Característica                                           | `FUNCTION`          | `PROCEDURE`                                         |
| -------------------------------------------------------- | ------------------- | --------------------------------------------------- |
| **Devuelve un valor**                                    | ✅ Sí (con `RETURN`) | ❌ No directamente (usa `OUT` para devolver valores) |
| **Usable en `SELECT`**                                   | ✅ Sí                | ❌ No                                                |
| **Puede modificar datos (`INSERT`, `UPDATE`, `DELETE`)** | ❌ No                | ✅ Sí                                                |
| **Útil para cálculos y transformaciones**                | ✅ Sí                | ❌ No                                                |
## Crear procedimientos almacenados complejos
Los **Stored Procedures** permiten ejecutar múltiples instrucciones en una sola llamada, lo que mejora el rendimiento y facilita la automatización de procesos en MySQL.

✅ **Usan lógica condicional (`IF`, `CASE`)**  
✅ **Pueden realizar bucles (`WHILE`, `LOOP`, `REPEAT`)**  
✅ **Manejan transacciones (`START TRANSACTION`, `COMMIT`, `ROLLBACK`)**  
✅ **Pueden devolver valores mediante parámetros `OUT`**

---

## **🔹 Sintaxis general de `CREATE PROCEDURE`**

```sql
DELIMITER // 
CREATE PROCEDURE nombre_procedimiento(IN param1 TIPO, OUT param2 TIPO, INOUT param3 TIPO ) 
BEGIN     
	-- Declaraciones de variables     
	DECLARE variable1 TIPO;          
	-- Lógica del procedimiento     
	-- (INSERT, UPDATE, DELETE, SELECT, IF, CASE, LOOPS, etc.)      
END //  
DELIMITER ;
```
📌 **Tipos de parámetros:**

- `IN`: Recibe un valor de entrada.
- `OUT`: Devuelve un valor de salida.
- `INOUT`: Recibe un valor y lo modifica dentro del procedimiento.

---

## **🔹 Ejemplo 1: Procedimiento con `IF` y `CASE`**

📌 **Consulta el saldo de un cliente y devuelve si tiene fondos suficientes**

```sql
DELIMITER //  
CREATE PROCEDURE VerificarSaldo(IN cliente_id INT, OUT estado VARCHAR(50)) 
BEGIN     
	DECLARE saldo_actual DECIMAL(10,2);
	
	-- Obtener el saldo del cliente 
	SELECT saldo INTO saldo_actual FROM clientes WHERE id = cliente_id; 
	 
	-- Evaluar el saldo   
	IF saldo_actual IS NULL THEN  
		SET estado = 'Cliente no encontrado'; 
	ELSEIF saldo_actual < 0 THEN    
		SET estado = 'Saldo negativo'; 
	ELSEIF saldo_actual = 0 THEN     
		SET estado = 'Sin fondos';   
	ELSE        
		SET estado = 'Saldo disponible';  
	END IF;    
END //  

DELIMITER ;`
```
✔ **Uso:**

```sql
CALL VerificarSaldo(1, @estado); 
SELECT @estado; -- Muestra el estado del saldo del cliente`
```
---

## **🔹 Ejemplo 2: Uso de `LOOP` para generar una secuencia de números**

📌 **Genera una tabla de multiplicar hasta cierto número**

```sql
DELIMITER //  

CREATE PROCEDURE TablaMultiplicar(IN numero INT, IN limite INT) 
BEGIN     
	DECLARE i INT DEFAULT 1;       
	   
	-- Bucle para imprimir la tabla de multiplicar    
	tabla_loop: LOOP      
		IF i > limite THEN        
			LEAVE tabla_loop;  -- Sale del bucle cuando se alcanza el límite     
		END IF;           
		    
		SELECT CONCAT(numero, ' x ', i, ' = ', numero * i) AS resultado;  
		SET i = i + 1;     
	END LOOP;   
END //  
DELIMITER ;`
```
✔ **Uso:**
\
```sql
CALL TablaMultiplicar(5, 10);
```
---

## **🔹 Ejemplo 3: Uso de `TRANSACTIONS` para garantizar integridad en pagos**

📌 **Registra un pago y actualiza el saldo de un cliente de manera segura**

```sql
DELIMITER // 
CREATE PROCEDURE RealizarPago(IN cliente_id INT, IN monto DECIMAL(10,2)) 
BEGIN    
	DECLARE saldo_actual DECIMAL(10,2);      
	
	-- Iniciar la transacción    
	START TRANSACTION;        
	
	-- Obtener el saldo actual del cliente 
	SELECT saldo INTO saldo_actual FROM clientes WHERE id = cliente_id FOR UPDATE;
	
	-- Validar si el cliente tiene saldo suficiente  
	IF saldo_actual IS NULL THEN      
		ROLLBACK; -- Deshacer la transacción si el cliente no existe 
	ELSEIF saldo_actual < monto THEN   
		ROLLBACK; -- Deshacer la transacción si no hay saldo suficiente   
	ELSE        
		-- Descontar el monto y registrar el pago       
		UPDATE clientes SET saldo = saldo - monto WHERE id = cliente_id;   
		INSERT INTO pagos(cliente_id, monto, fecha) VALUES (cliente_id, monto, NOW());
		
		-- Confirmar la transacción       
		COMMIT;     
	END IF;     
	END // 
DELIMITER ;
```
✔ **Uso:**

```sql
CALL RealizarPago(1, 50);
```
---

## **📌 Conclusión**

- **`IF`, `CASE`** → Permiten **evaluaciones condicionales**.
- **`LOOP`, `WHILE`, `REPEAT`** → Se usan para **iteraciones y procesos repetitivos**.
- **`TRANSACTIONS`** → Garantizan **consistencia en operaciones críticas**.
## 📌 Diferencias entre Funciones y Procedimientos en MySQL

| **#** | **Funciones**                                             | **Procedimientos**                                         |
| ----- | --------------------------------------------------------- | ---------------------------------------------------------- |
| **1** | Creado mediante el comando `CREATE FUNCTION`              | Creado mediante el comando `CREATE PROCEDURE`              |
| **2** | Invocado mediante la instrucción `SELECT`                 | Invocado mediante la instrucción `CALL`                    |
| **3** | Debe devolver un solo valor                               | Devuelve valores a través del parámetro `OUT`              |
| **4** | Solo toma parámetros `IN`                                 | Toma parámetros `IN`, `OUT` e `INOUT`                      |
| **5** | Encapsula fórmulas comunes o reglas comerciales genéricas | Se usa para procesar, manipular y modificar datos en la BD |
| **6** | Debe especificar el tipo de datos del valor devuelto      | Se debe especificar el tipo del parámetro `OUT`            |
