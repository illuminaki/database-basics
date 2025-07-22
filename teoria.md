# Teoría de Bases de Datos

## 1. ¿Qué es una base de datos?

Una base de datos (BD) es un **conjunto organizado de información** que se almacena y se accede electrónicamente desde un sistema informático. Su propósito principal es permitir **almacenamiento**, **consulta** y **manipulación eficiente** de los datos.

### 1.1 Ejemplos del mundo real

- Sistema bancario: registros de clientes, cuentas y transacciones.
- E-commerce: información de productos, usuarios y órdenes.
- Redes sociales: perfiles de usuario, publicaciones y relaciones de amistad.

### 1.2 Archivos vs. bases de datos

| Característica | Archivos planos (`.csv`, `.txt`) | Bases de datos |
|----------------|----------------|--------------|
| Estructura | Pobre o inexistente | Definida mediante un esquema |
| Crecimiento | Difícil de escalar | Escalable y optimizado |
| Concurrencia | Propenso a errores | Mecanismos de bloqueo y transacciones |

---

## 2. Historia y evolución

| Hito | Año | Descripción |
|------|-----|-------------|
| **Sistema R (IBM)** | 1974 | Primer prototipo de BD relacional y de SQL. |
| **Publicación de SQL** | 1986 | ANSI estandariza el lenguaje. |
| **Surge NoSQL** | ~2009 | Necesidad de escalar horizontalmente y manejar datos no estructurados.

---

## 3. Tipos de bases de datos

1. Relacionales (SQL).
2. No relacionales (NoSQL):
   - Documentos (MongoDB).
   - Clave-valor (Redis).
   - Grafos (Neo4j).
   - Columnas (Cassandra).

---

## 4. Bases de datos relacionales

```
+-------------+    1   +------------+
|   Usuarios  |<-------|  Pedidos   |
+-------------+        +------------+
   PK id                 PK id
                        FK usuario_id
```

- **Tabla**: colección de filas.
- **Fila (registro)**: conjunto de valores relacionados.
- **Clave primaria (PK)**: identifica de forma única cada fila de una tabla. Suele llamarse `id` y puede ser un entero autoincremental (`SERIAL`, `IDENTITY`) o un valor `UUID`.
- **Clave sustituta (Surrogate Key)**: variante de PK basada en un valor sin significado de negocio (por ejemplo, un número secuencial) que evita cambios si la lógica de negocio evoluciona.
- **Clave natural**: PK formada por un atributo con significado en el dominio del negocio (por ejemplo, `dni`, `correo_electronico`).
- **Clave compuesta**: PK compuesta por **dos o más columnas** (ej. `(id_pedido, id_producto)` en una tabla de detalle).
- **Clave candidata**: cualquier columna o conjunto de columnas que puede ser una PK (cumple unicidad y no nulidad).
- **Clave única (UNIQUE)**: garantiza que no existan duplicados en una columna, pero puede permitir valores `NULL` dependiendo del motor.
- **Clave foránea (FK)**: columna que referencia la PK (o una UNIQUE) de otra tabla, asegurando integridad referencial e implementando relaciones entre tablas.
- Relaciones:
  - Uno a uno.
  - Uno a muchos.
  - Muchos a muchos.

---

## 5. SQL: Lenguaje estructurado de consultas

```
-- Crear tabla
CREATE TABLE productos (
  id SERIAL PRIMARY KEY,
  nombre VARCHAR(100),
  precio NUMERIC(10,2)
);

-- Insertar
INSERT INTO productos (nombre, precio) VALUES ('Laptop', 1200.00);

-- Consultar
SELECT * FROM productos WHERE precio > 1000 ORDER BY precio DESC;

-- Actualizar
UPDATE productos SET precio = 1100 WHERE id = 1;

-- Borrar
DELETE FROM productos WHERE id = 1;
```

### 5.1 JOINs

Un **JOIN** es una operación que **relaciona registros de distintas tablas** utilizando columnas en común (generalmente una clave primaria y una clave foránea).
Sirve para obtener en una sola consulta datos que se encuentran distribuidos, evitando duplicidad y manteniendo la normalización.
Los tipos de JOIN más usados son:

- `INNER JOIN`: devuelve solo las coincidencias presentes en ambas tablas.
- `LEFT JOIN` (`LEFT OUTER`): devuelve todas las filas de la tabla izquierda y las coincidencias de la derecha (o `NULL`).
- `RIGHT JOIN` y `FULL JOIN`: análogos al anterior pero desde la derecha o devolviendo todas las filas de ambas tablas.

```
SELECT u.nombre, p.total
FROM usuarios u
JOIN pedidos p ON u.id = p.usuario_id;
```

### 5.2 GROUP BY

`GROUP BY` agrupa filas que comparten valores en una o varias columnas para poder **aplicar funciones de agregación** (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) sobre cada grupo.
Es fundamental para generar reportes y estadísticas, ya que transforma conjuntos de registros detallados en resultados resumidos.

```
SELECT categoria, AVG(precio)
FROM productos
GROUP BY categoria;
```

---

## 6. ACID

| Propiedad | Descripción |
|-----------|-------------|
| **Atomicidad** | Toda transacción se ejecuta completa o nada. |
| **Consistencia** | Mantiene la integridad de los datos. |
| **Aislamiento** | Las transacciones concurrentes no se afectan mutuamente. |
| **Durabilidad** | Una vez confirmada, la transacción resiste fallos del sistema. |

---

## 7. NoSQL

### 7.1 ¿Cuándo usar NoSQL?

- Datos semi-estructurados o sin esquema fijo.
- Alta escalabilidad horizontal.
- Latencia mínima (tiempo real).

### 7.2 Comparación rápida

| | SQL | NoSQL |
|---|----|------|
| **Esquema** | Rígido | Flexible |
| **Escala** | Vertical | Horizontal |
| **Consistencia** | Fuerte (ACID) | Eventual (BASE) |

---

## 8. Casos de uso

- **Banca**: requiere ACID para la integridad de transacciones.
- **E-commerce**: catálogos de productos (NoSQL) y órdenes (SQL).
- **Redes sociales**: grafos para relaciones de amistad.
- **IoT**: series de tiempo y grandes volúmenes de datos.

---

## 9. Glosario básico

| Término | Definición |
|---------|------------|
| **Esquema** | Estructura que define tablas y columnas. |
| **Índice** | Estructura que acelera las consultas. |
| **Normalización** | Proceso de diseño para reducir redundancia. |
| **Transacción** | Unidad lógica de trabajo. |
| **Consulta** | Petición de datos a la BD. |

---

## 10. Diseño y Normalización

La **normalización** es un proceso sistemático que organiza los datos para reducir la redundancia y mejorar la integridad. Las formas normales más utilizadas son:

| Forma Normal | Reglas | Problema que resuelve |
|--------------|--------|-----------------------|
| **1NF** | Elimina grupos repetitivos, cada campo contiene solo un valor atómico. | Estructuras no tabulares y multi-valores. |
| **2NF** | Cumple 1NF y cada columna depende de la clave completa (no de parte de ella). | Dependencias parciales en claves compuestas. |
| **3NF** | Cumple 2NF y no existen dependencias transitivas (columna → columna → clave). | Dependencias transitivas. |
| **BCNF** | Variante estricta de 3NF donde toda dependencia funcional tiene super-clave como determinante. | Anomalías raras aún en 3NF. |

> **Ejemplo práctico**
>
> 1. Tabla `clientes_pedidos` (*no* 1NF) → separar en `clientes` y `pedidos`.
> 2. `pedidos` con PK `(id_pedido, id_cliente)` y columna `nombre_cliente` (*no* 2NF) → mover `nombre_cliente` a `clientes`.
> 3. `pedidos` con columnas `id_ciudad` y `nombre_ciudad` (*no* 3NF) → crear tabla `ciudades`.

---

## 11. Índices y Optimización de Consultas

Los **índices** aceleran la recuperación de datos a costa de almacenamiento extra y tiempo de escritura.

- **B-tree**: defecto en la mayoría de motores relacionales.
- **Hash**: búsquedas exactas, no soporta rangos.
- **GiST / GIN**: datos geoespaciales y arrays.

```sql
EXPLAIN ANALYZE SELECT * FROM productos WHERE precio > 1000;
```

Recomendaciones:

1. Indexar columnas usadas en `JOIN`, `WHERE`, `ORDER BY` y `GROUP BY`.
2. Evitar funciones o cast en la columna indexada (`WHERE LOWER(email) = '...'`).
3. Usar **índices compuestos** en el mismo orden de filtrado.

---

## 12. Transacciones e Isolation Levels

Los motores SQL implementan varios **niveles de aislamiento** definidos por ANSI-SQL:

| Nivel | Problemas evitados | Permite |
|-------|-------------------|---------|
| **Read Uncommitted** | — | Lecturas sucias (dirty reads) |
| **Read Committed** | Dirty reads | Lecturas no repetibles, fantasma |
| **Repeatable Read** | Dirty & no-repeatable reads | Fantasmas (salvo MVCC) |
| **Serializable** | Todos los anteriores | — |

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN;
  UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
  UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
```

---

## 13. Procedimientos Almacenados, Vistas y Triggers

- **Procedimientos almacenados (SP)**: lógica en el servidor, reducen ida y vuelta.
- **Vistas**: consultas pre-definidas expuestas como tablas virtuales.
- **Triggers**: se disparan ante eventos `INSERT`, `UPDATE`, `DELETE`.

```sql
CREATE TRIGGER actualizar_stock
AFTER INSERT ON pedidos
FOR EACH ROW
  UPDATE productos SET stock = stock - NEW.cantidad
  WHERE id = NEW.producto_id;
```

---

## 14. Replicación y Alta Disponibilidad

| Estrategia | Descripción | Pros | Contras |
|------------|-------------|------|---------|
| **Master-Slave** | Lecturas en réplicas, escrituras en primario. | Lecturas escalan | Retraso de replicación |
| **Multi-Master** | Múltiples nodos aceptan escrituras. | Tolerancia a fallos | Conflictos de escritura |
| **Sharding** | Partición horizontal de datos. | Escala sin límite | Mayor complejidad |

---

## 15. Copias de Seguridad y Recuperación

- **Logical dump** (`pg_dump`, `mysqldump`): portables, lentos.
- **Physical backup** (copia de archivos de datos, WAL): rápidas, ligadas al motor.
- **Point-in-Time Recovery (PITR)**: restaurar hasta milisegundo específico.

Buenas prácticas:

1. Automatizar y verificar *restores* periódicos.
2. Guardar backups off-site y cifrados.

---

## 16. CAP, ACID y BASE

El **Teorema CAP** indica que un sistema distribuido solo puede garantizar dos de tres propiedades:

- **Consistencia**
- **Disponibilidad**
- **Tolerancia a particiones**

| Modelo | Filosofía |
|--------|-----------|
| **ACID** | Transacciones fuertes, integridad (SQL). |
| **BASE** | Consistencia eventual, flexibilidad (NoSQL). |

---

## 17. Seguridad en Bases de Datos

- **Autenticación y Autorización**: roles, privilegios `GRANT`/`REVOKE`.
- **Cifrado**: datos en reposo (TDE) y en tránsito (TLS).
- **Auditoría**: registro de eventos para cumplimiento normativo.

```sql
CREATE ROLE analista NOINHERIT;
GRANT SELECT ON productos TO analista;
```

---

## 18. Checklist para Elegir un Motor de Base de Datos

1. Requisitos de consistencia vs. disponibilidad.
2. Estructura y volumen de datos.
3. Escalabilidad esperada.
4. Ecosistema y soporte de la comunidad.
5. Costo y licenciamiento.

