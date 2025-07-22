# Guía de Práctica SQL – Ejemplos de CRUD

Un script SQL completo y autocontenido que puedes copiar y pegar en tu cliente SQL favorito.
Crea un esquema de ejemplo (`example_db`) con dos tablas básicas (`customers` y `orders`) y
recorre las cuatro operaciones fundamentales de CRUD: **Crear, Leer, Actualizar y Eliminar**.

> 💡  Consejo: Ejecuta el script bloque por bloque para observar el efecto de cada consulta.

---

## 1. CREAR – esquema, tablas y datos

```sql
-- 1.1  Crear una base de datos de práctica dedicada
DROP DATABASE IF EXISTS example_db;
CREATE DATABASE example_db;
USE example_db;

-- 1.2  Crear tablas
CREATE TABLE customers (
    customer_id  INT AUTO_INCREMENT PRIMARY KEY,
    full_name    VARCHAR(100) NOT NULL,
    email        VARCHAR(100) UNIQUE,
    created_at   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
    order_id     INT AUTO_INCREMENT PRIMARY KEY,
    customer_id  INT       NOT NULL,
    order_date   DATE      NOT NULL,
    amount       DECIMAL(10,2) NOT NULL,
    CONSTRAINT fk_customer
        FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
        ON DELETE CASCADE ON UPDATE CASCADE
);

-- 1.3  Insertar datos de prueba
INSERT INTO customers (full_name, email) VALUES
  ('Ada Lovelace',       'ada@example.com'),
  ('Alan Turing',        'alan@computing.org'),
  ('Grace Hopper',       'grace@navy.mil'),
  ('Linus Torvalds',     'linus@kernel.org'),
  ('Sebastian Agudelo',  'sebastian@agudelo.dev');

INSERT INTO orders (customer_id, order_date, amount) VALUES
  (1, '2025-07-20', 120.50),
  (1, '2025-07-21',  75.00),
  (2, '2025-07-22', 230.00);
```

---

## 2. LEER – consultas de datos

```sql
-- 2.1  Ver todos los clientes
SELECT * FROM customers;

-- 2.2  Ver todas las órdenes con el nombre del cliente (ejemplo de JOIN)
SELECT o.order_id,
       c.full_name AS customer,
       o.order_date,
       o.amount
FROM   orders o
JOIN   customers c USING (customer_id)
ORDER  BY o.order_date DESC;

-- 2.3  Agregado: total gastado por cada cliente
SELECT c.full_name,
       SUM(o.amount) AS total_spent,
       COUNT(*)      AS orders_count
FROM   customers c
LEFT JOIN orders o USING (customer_id)
GROUP  BY c.customer_id
ORDER  BY total_spent DESC;
```

---

## 3. ACTUALIZAR – modificar filas existentes

```sql
-- 3.1  Actualizar la dirección de correo de un cliente
UPDATE customers
SET    email = 'ada.lovelace@analytical.engine'
WHERE  customer_id = 1;

-- 3.2  Dar un descuento a Alan: reducir la última orden en un 10 %
-- (MySQL no permite actualizar y seleccionar de la misma tabla en la misma sentencia)
UPDATE orders o
JOIN (
    SELECT order_id
    FROM   orders
    WHERE  customer_id = 2
    ORDER BY order_date DESC
    LIMIT 1
) AS ult ON o.order_id = ult.order_id
SET o.amount = o.amount * 0.9;

-- 3.3  Verificar los cambios
SELECT * FROM customers WHERE customer_id = 1;
SELECT * FROM orders   WHERE customer_id = 2;
```

---

## 4. ELIMINAR – borrar filas

```sql
-- 4.1  Eliminar una orden
DELETE FROM orders WHERE order_id = 2;

-- 4.2  Eliminar un cliente (cascada a orders por la FK)
DELETE FROM customers WHERE customer_id = 3;

-- 4.3  Revisar los datos restantes
SELECT * FROM customers;
SELECT * FROM orders;

-- 4.4  Eliminar una tabla completa (por ejemplo, `orders`) si necesitas recrearla
DROP TABLE IF EXISTS orders;

-- 4.5  Eliminar la base de datos completa (usa con precaución)
DROP DATABASE IF EXISTS example_db;
```

---

## 5. LIMPIEZA – (opcional)

```sql
DROP DATABASE example_db;
```

---

## 6. EJERCICIOS ADICIONALES – Tablas Relacionadas y Consultas Avanzadas

Para practicar escenarios más cercanos a un entorno de producción, amplía el esquema con
productos, categorías y el detalle de las órdenes. A continuación encontrarás scripts de
creación, datos de prueba y consultas desafiantes.

### 6.1  Extender el esquema

```sql
-- Tabla de categorías
CREATE TABLE categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(50) NOT NULL
);

-- Tabla de productos
CREATE TABLE products (
    product_id   INT AUTO_INCREMENT PRIMARY KEY,
    category_id  INT,
    name         VARCHAR(100) NOT NULL,
    price        DECIMAL(10,2) NOT NULL,
    stock_qty    INT DEFAULT 0,
    CONSTRAINT fk_category
        FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

-- Tabla intermedia: detalle de cada orden
CREATE TABLE order_items (
    item_id     INT AUTO_INCREMENT PRIMARY KEY,
    order_id    INT NOT NULL,
    product_id  INT NOT NULL,
    quantity    INT NOT NULL,
    unit_price  DECIMAL(10,2) NOT NULL,
    CONSTRAINT fk_order   FOREIGN KEY (order_id)   REFERENCES orders(order_id)   ON DELETE CASCADE,
    CONSTRAINT fk_product FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

### 6.2  Poblar con datos de ejemplo

```sql
-- Categorías
INSERT INTO categories (name) VALUES ('Periféricos'), ('Componentes'), ('Accesorios');

-- Productos (10 ejemplos)
INSERT INTO products (category_id, name, price, stock_qty) VALUES
 (1, 'Mouse Inalámbrico',   18.99, 120),
 (1, 'Teclado Mecánico',    79.90,  85),
 (2, 'SSD 1TB',            105.50,  40),
 (2, 'Memoria RAM 16GB',    65.20,  60),
 (2, 'CPU Ryzen 7',        299.00,  25),
 (3, 'Base para Laptop',    24.75, 150),
 (3, 'Cable HDMI 2m',        8.50, 300),
 (3, 'Audífonos',           49.99,  70),
 (1, 'Webcam HD',           38.00, 110),
 (3, 'Micrófono USB',       59.90,  90);

-- Ordenes más pobladas
INSERT INTO orders (customer_id, order_date, amount) VALUES
  (1, '2025-07-22',  49.99),
  (2, '2025-07-22', 299.00),
  (2, '2025-07-23', 125.70),
  (3, '2025-07-24', 210.60);

-- Ítems de orden (relacionados con las nuevas órdenes)
INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES
  (4, 8, 1, 49.99),
  (5, 5, 1, 299.00),
  (6, 3, 1, 105.50), (6, 4, 1, 65.20),
  (7, 2, 2, 79.90);
```

### 6.3  Consultas de práctica

```sql
-- 6.3.1  Top 3 clientes con mayor gasto total
SELECT c.full_name,
       SUM(o.amount) AS total_gastado
FROM   customers c
JOIN   orders o USING (customer_id)
GROUP  BY c.customer_id
ORDER  BY total_gastado DESC
LIMIT 3;

-- 6.3.2  Productos más vendidos (por cantidad)
SELECT p.name,
       SUM(oi.quantity) AS total_vendido
FROM   products p
JOIN   order_items oi USING (product_id)
GROUP  BY p.product_id
ORDER  BY total_vendido DESC;

-- 6.3.3  Detalle de órdenes con subtotal, IVA (19 %) y total
SELECT o.order_id,
       SUM(oi.quantity * oi.unit_price)            AS subtotal,
       SUM(oi.quantity * oi.unit_price) * 0.19     AS iva,
       SUM(oi.quantity * oi.unit_price) * 1.19     AS total
FROM   orders o
JOIN   order_items oi USING (order_id)
GROUP  BY o.order_id;

-- 6.3.4  Órdenes cuyo valor supera el promedio de todas las órdenes
SELECT *
FROM   orders
WHERE  amount > (SELECT AVG(amount) FROM orders);

-- 6.3.5  Clientes que aún no han realizado ninguna orden (LEFT JOIN)
SELECT c.*
FROM   customers c
LEFT JOIN orders o USING (customer_id)
WHERE  o.order_id IS NULL;
```

---

### Cómo usar este archivo
1. Abre tu cliente SQL (MySQL, MariaDB u otro motor compatible con la sintaxis).
2. Copia cada sección y ejecútala de forma incremental, o ejecuta todo el script de una sola vez.
3. Siéntete libre de modificar valores, agregar nuevas columnas o ampliar las consultas: ¡este
   es tu laboratorio para dominar operaciones CRUD y consultas avanzadas!

¡Felices consultas!
