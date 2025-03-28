# MySQL Events
Aquí tienes el texto con el código SQL correctamente formateado:

---

### **📌 MySQL Events: Programando Tareas Automáticas**

Los **EVENTOS** en MySQL permiten programar la ejecución automática de consultas SQL en intervalos de tiempo específicos, similar a un **cron job** en Linux o una **tarea programada** en Windows.

---

## **🔹 1. ¿Qué es un MySQL Event?**

Un **Evento en MySQL** es un proceso programado que ejecuta una o varias sentencias SQL en un momento determinado o de forma repetitiva.

📌 **Ejemplos de usos comunes:**  
✅ Limpieza automática de datos antiguos  
✅ Generación de reportes periódicos  
✅ Envío de notificaciones o actualizaciones  
✅ Mantenimiento de bases de datos

---

## **🔹 2. Activar el Event Scheduler**

Antes de crear eventos, asegúrate de que el programador de eventos está activado:

```sql
SHOW VARIABLES LIKE 'event_scheduler';
```

Si está **desactivado**, actívalo temporalmente con:

```sql
SET GLOBAL event_scheduler = ON;
```

Para activarlo permanentemente, edita el archivo `my.cnf` o `my.ini` y añade:

```
[mysqld]
event_scheduler=ON
```

Luego reinicia MySQL.

---

## **🔹 3. Crear un Evento en MySQL**

👉 **Ejemplo:** Borremos registros de la tabla `logs` que tengan más de 30 días, todos los días a la medianoche.

```sql
DELIMITER //
CREATE EVENT limpiar_logs
ON SCHEDULE EVERY 1 DAY 
STARTS TIMESTAMP(CURRENT_DATE, '00:00:00')
DO
BEGIN
    DELETE FROM logs WHERE fecha < NOW() - INTERVAL 30 DAY;
END;
//
DELIMITER ;
```


## Ejemplo en clase. 
Pasar facturas a una nueva tabla y eliminar la anterior

```sql
DELIMITER //
CREATE EVENT limpiar_logs
ON SCHEDULE EVERY 30 DAY 
STARTS TIMESTAMP(CURRENT_DATE, '00:00:00')
DO
BEGIN
	INSERT INTO facturas_vencidas (id, nombre_producto, precio_venta, hora, fecha)
    SELECT id, nombre_producto, precio_venta, hora, fecha FROM facturas;

	DELETE FROM facturas;
END;
//
DELIMITER ;
```

📌 **Explicación:**

- `ON SCHEDULE EVERY 1 DAY` → El evento se ejecuta **cada día**.
- `STARTS TIMESTAMP(CURRENT_DATE, '00:00:00')` → Inicia a la **medianoche**.
- `DELETE FROM logs WHERE fecha < NOW() - INTERVAL 30 DAY;` → Elimina registros de más de **30 días**.

---

## **🔹 4. Ver los Eventos Creados**

Para listar todos los eventos programados en la base de datos actual:

```sql
SHOW EVENTS;
```

Para ver eventos de todas las bases de datos:

```sql
SELECT * FROM information_schema.EVENTS;
```

---

## **🔹 5. Eliminar un Evento**

Si ya no necesitas un evento, elimínalo con:

```sql
DROP EVENT limpiar_logs;
```

---

## **🔹 6. Deshabilitar y Habilitar un Evento**

Si quieres **pausar** temporalmente un evento:

```sql
ALTER EVENT limpiar_logs DISABLE;
```

Para **reactivarlo**:

```sql
ALTER EVENT limpiar_logs ENABLE;
```



✅ Los **Eventos en MySQL** permiten programar tareas automatizadas.  
✅ Son útiles para mantenimiento, reportes y limpieza de datos.  
✅ **Requieren que el `event_scheduler` esté activado** en MySQL.  
✅ Puedes **crear, listar, deshabilitar o eliminar eventos** fácilmente con comandos SQL.
![[Pasted image 20250320174623.png]]![[Pasted image 20250320174639.png]]