# Ejercicios de Bases de Datos

## 1. Relaciona conceptos

Empareja cada término con su definición.

| # | Concepto | Definición |
|---|----------|------------|
| A | Clave primaria |  |
| B | Clave foránea |  |
| C | Índice |  |
| D | Transacción |  |
| E | Normalización |  |

---

## 2. Verdadero / Falso

1. ___ Las bases de datos NoSQL siempre garantizan ACID.
2. ___ `DELETE` borra una tabla completa.
3. ___ `JOIN` se usa para combinar registros de diferentes tablas.

---

## 3. Tipo de relación

Indica el tipo de relación entre las entidades.

| Entidad A | Entidad B | Relación |
|-----------|-----------|----------|
| Usuario | Pedido | |
| Médico | Paciente | |
| Estudiante | Curso | |

---

## 4. SQL o NoSQL

Para cada caso, elige **SQL** o **NoSQL** y justifica tu respuesta.

1. Plataforma de streaming de video con millones de reproducciones por minuto.
2. Sistema contable de una corporación.
3. Chat en tiempo real para gaming.

---

## 5. Analiza la consulta

```sql
SELECT nombre, precio
FROM productos
WHERE precio BETWEEN 10 AND 20
ORDER BY precio DESC;
```

1. ¿Qué hace la consulta?
2. ¿Se podría mejorar con un índice? ¿Por qué?

---

## 6. Reto práctico

Crea las tablas `usuarios`, `productos` y `pedidos` con las siguientes columnas:

- `usuarios`: `id`, `nombre`, `email`.
- `productos`: `id`, `nombre`, `precio`.
- `pedidos`: `id`, `usuario_id`, `producto_id`, `cantidad`.

Luego, escribe una consulta para obtener el nombre del usuario y el nombre del producto de cada pedido.
