# MySQL Transactions

### **¿Qué es una Transacción en MySQL?**

Una **transacción en MySQL** es un conjunto de **operaciones SQL** que se ejecutan como una **unidad atómica**. Esto significa que todas las operaciones dentro de la transacción **deben completarse correctamente**; si alguna falla, la transacción **se revierte** para mantener la integridad de los datos.

---

### **📌 Propiedades ACID de una Transacción**

Las transacciones siguen las reglas **ACID**, que garantizan la fiabilidad de los datos:

1. **Atomicidad** (Atomicity): Se ejecutan todas las operaciones o ninguna.
2. **Consistencia** (Consistency): La base de datos siempre pasa de un estado válido a otro.
3. **Aislamiento** (Isolation): Una transacción no afecta a otra mientras se ejecuta.
4. **Durabilidad** (Durability): Una vez confirmada (`COMMIT`), la transacción es permanente.

---

### **🛠️ Uso de Transacciones en MySQL**

MySQL permite transacciones con motores como **InnoDB** (⚠️ _No funciona con MyISAM_).

#### **1️⃣ Estructura de una Transacción**

```sql
START TRANSACTION;  -- Inicia la transacción

UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;

COMMIT;  -- Confirma los cambios (guarda en la BD)
```

Si todas las operaciones son exitosas, `COMMIT` guarda los cambios.

---

#### **2️⃣ Deshacer una Transacción (`ROLLBACK`)**

Si algo sale mal, podemos revertir los cambios:

```sql
START TRANSACTION;

UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;

ROLLBACK;  -- Cancela la transacción y deshace el cambio
```

`ROLLBACK` revierte la tabla al estado antes de ejecutar `START TRANSACTION`.

---

#### **3️⃣ Modo Autocommit**

Por defecto, MySQL ejecuta cada consulta de inmediato (**modo autocommit activado**). Para desactivarlo y controlar manualmente las transacciones:

```sql
SET autocommit = 0;  -- Desactiva autocommit
START TRANSACTION;
```

Luego, se debe usar `COMMIT` o `ROLLBACK` explícitamente.

---

### **🚀 ¿Cuándo usar Transacciones?**

✅ **Transferencias bancarias** (Debitar y acreditar en diferentes cuentas).  
✅ **Sistemas de reservas** (Evitar dobles reservas en hoteles/vuelos).  
✅ **E-commerce** (Actualizar stock y generar facturas solo si el pago es exitoso).  
✅ **Manejo de registros complejos** (Insertar múltiples datos relacionados).

---

### **📌 Conclusión**

Las transacciones garantizan que los datos permanezcan **consistentes y seguros**, evitando errores por fallos en consultas intermedias. Son clave en operaciones críticas donde la integridad de los datos es esencial. 🚀