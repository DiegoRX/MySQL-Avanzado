# MySQL Triggers
## MySQL Triggers
### 🔥 **¿Qué es un Trigger en MySQL?**

Un **Trigger** en MySQL es un objeto de base de datos que **se ejecuta automáticamente** cuando ocurre un evento específico en una tabla, como una inserción, actualización o eliminación de datos.

📌 **En otras palabras:** Es una especie de "disparador" que ejecuta código SQL antes o después de que se realice una operación en una tabla.

---

## 📌 **Tipos de Triggers en MySQL**

Los triggers en MySQL pueden activarse en dos momentos diferentes:

| **Momento de Ejecución** | **Descripción**                                         |
| ------------------------ | ------------------------------------------------------- |
| `BEFORE INSERT`          | Se ejecuta antes de insertar un nuevo registro.         |
| `AFTER INSERT`           | Se ejecuta después de insertar un nuevo registro.       |
| `BEFORE UPDATE`          | Se ejecuta antes de actualizar un registro existente.   |
| `AFTER UPDATE`           | Se ejecuta después de actualizar un registro existente. |
| `BEFORE DELETE`          | Se ejecuta antes de eliminar un registro.               |
| `AFTER DELETE`           | Se ejecuta después de eliminar un registro.             |

🚀 **Nota:** MySQL **no permite triggers para eventos SELECT o TRUNCATE**.

---

## 📌 **Ejemplo de Trigger en MySQL**

### ✅ **Ejemplo 1: Crear un trigger que registre inserciones en una tabla de logs**

Supongamos que tenemos una tabla `clientes` y queremos registrar cada inserción en una tabla `clientes_log`.

```sql
-- Crear la tabla de logs
CREATE TABLE clientes_log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    accion VARCHAR(50),
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Crear el trigger AFTER INSERT
DELIMITER //
CREATE TRIGGER after_cliente_insert
AFTER INSERT 
ON clientes FOR EACH ROW
BEGIN
    INSERT INTO clientes_log (cliente_id, accion)
    VALUES (NEW.id, 'Cliente Insertado');
END;
//
DELIMITER ;
```

🔹 **Explicación:**

- Este trigger se ejecuta **después (`AFTER`) de cada inserción** en la tabla `clientes`.
- Registra en `clientes_log` el `id` del nuevo cliente y la acción realizada.

---

### ✅ **Ejemplo 2: Evitar que un usuario modifique un campo específico**

Supongamos que no queremos permitir que se actualice el campo `email` en la tabla `usuarios`.

```sql
DELIMITER //
CREATE TRIGGER before_email_update
BEFORE UPDATE ON usuarios
FOR EACH ROW
BEGIN
    IF NEW.email <> OLD.email THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'No puedes modificar el correo electrónico';
    END IF;
END;
//
DELIMITER ;
```

🔹 **Explicación:**

- Antes de actualizar un registro, verifica si el campo `email` cambia.
- Si cambia, MySQL genera un error y **evita la actualización**.

---

## 📌 **Ver Triggers Existentes en MySQL**

Para ver los triggers creados en la base de datos actual, usa:

```sql
SHOW TRIGGERS;
```

O consulta la tabla `INFORMATION_SCHEMA.TRIGGERS`:

```sql
SELECT * FROM information_schema.TRIGGERS 
WHERE TRIGGER_SCHEMA = 'nombre_de_tu_base_de_datos';
```

---

## 📌 **Eliminar un Trigger en MySQL**

Si necesitas borrar un trigger, usa:

```sql
DROP TRIGGER nombre_del_trigger;
```

Por ejemplo:

```sql
DROP TRIGGER after_cliente_insert;
```

---

## **🔹 1. Row-Level Triggers (Disparadores por Fila)**

### 📌 ¿Qué son?

Son triggers que **se ejecutan una vez por cada fila afectada** por la operación `INSERT`, `UPDATE` o `DELETE`.

📌 **Ejemplo:** Si un `UPDATE` afecta 10 filas, **el trigger se ejecuta 10 veces** (una por cada fila modificada).

---

### **✅ Ejemplo de Row-Level Trigger**

👉 Queremos registrar en una tabla `usuarios_log` cada vez que se **inserta** un usuario en la tabla `usuarios`.

sql

CopiarEditar

`DELIMITER // CREATE TRIGGER after_insert_usuario AFTER INSERT ON usuarios FOR EACH ROW BEGIN     INSERT INTO usuarios_log (usuario_id, accion, fecha)     VALUES (NEW.id, 'Usuario Creado', NOW()); END; // DELIMITER ;`

📌 **Explicación:**

- `FOR EACH ROW` indica que el trigger se ejecutará **para cada fila insertada**.
- Si se insertan 5 usuarios en una sola consulta, **el trigger se ejecutará 5 veces**, insertando 5 registros en `usuarios_log`.

---

## **🔹 2. Statement-Level Triggers (Disparadores por Sentencia)**

### 📌 ¿Qué son?

Son triggers que **se ejecutan una sola vez por cada sentencia SQL**, sin importar cuántas filas sean afectadas.

❌ **MySQL NO soporta Statement-Level Triggers.**  
🔥 **Solo bases de datos como Oracle, PostgreSQL o SQL Server los permiten.**

📌 **Ejemplo (en PostgreSQL, no en MySQL):**

sql

CopiarEditar

`CREATE TRIGGER statement_level_trigger AFTER UPDATE ON empleados FOR EACH STATEMENT EXECUTE FUNCTION registrar_cambio();`

📌 **Explicación:**

- Este trigger se ejecuta **solo una vez**, sin importar si el `UPDATE` afecta 1 o 1000 filas.
- En MySQL, solo podríamos lograr un efecto similar usando procedimientos almacenados.

---

## **🔍 Diferencias Clave entre Row-Level y Statement-Level Triggers**

| Característica               | Row-Level Triggers                                                                                                                | Statement-Level Triggers                       |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **Soportado en MySQL**       | ✅ Sí                                                                                                                              | ❌ No                                           |
| **Cuántas veces se ejecuta** | Una vez por cada fila afectada                                                                                                    | Una vez por cada sentencia SQL                 |
| **Ejemplo de activación**    | `UPDATE empleados SET salario = salario * 1.1 WHERE departamento = 'Ventas'` con 10 empleados en Ventas **se ejecutará 10 veces** | El mismo `UPDATE` **se ejecutaría solo 1 vez** |
| **Usado para**               | Registro detallado de cambios en cada fila                                                                                        | Acciones globales tras una operación masiva    |