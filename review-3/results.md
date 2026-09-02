# Resultados del Review 3

Para este review se utilizó una base de datos con las tablas `vendedores`, `productos` y `ventas`, relacionadas mediante llaves foráneas.

```sql
CREATE TABLE vendedores (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(60),
    apellido VARCHAR(60)
);

CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100),
    precio NUMERIC(10,2),
    stock INT
);

CREATE TABLE ventas (
    id SERIAL PRIMARY KEY,
    fecha DATE,
    monto NUMERIC(10,2),
    vendedor_id INT REFERENCES vendedores(id),
    producto_id INT REFERENCES productos(id)
);

-- Datos de vendedores
INSERT INTO vendedores (nombre, apellido) VALUES
('Carlos', 'Gómez'),
('María', 'López'),
('Pedro', 'Martínez'),
('Lucía', 'Fernández'),
('Jorge', 'Ramírez');

-- Datos de productos
INSERT INTO productos (nombre, precio, stock) VALUES
('Laptop Dell XPS 13', 1200.00, 15),
('Mouse Inalámbrico Logitech', 25.50, 50),
('Teclado Mecánico RGB', 85.00, 30),
('Monitor Gaming 27 IPS', 320.00, 20),
('Auriculares Bluetooth Sony', 150.00, 40),
('Silla Ergonómica de Oficina', 210.00, 12),
('Disco Duro Externo 2TB', 75.00, 25),
('Webcam Full HD 1080p', 45.00, 35),
('Micrófono Condensador USB', 95.00, 18),
('Hub USB-C 7 en 1', 35.00, 45);

-- Datos de ventas (fecha, monto, vendedor_id, producto_id)
INSERT INTO ventas (fecha, monto, vendedor_id, producto_id) VALUES
('2026-01-02', 1200.00, 1, 1),
('2026-01-03', 51.00, 2, 2),
('2026-01-04', 150.00, 3, 5),
('2026-01-05', 135.00, 4, 8),
('2026-01-07', 85.00, 5, 3),
('2026-01-08', 320.00, 1, 4),
('2026-01-10', 150.00, 2, 7),
('2026-01-11', 140.00, 3, 10),
('2026-01-12', 25.50, 4, 2),
('2026-01-14', 210.00, 5, 6),
('2026-01-15', 190.00, 1, 9),
('2026-01-17', 150.00, 2, 5),
('2026-01-18', 170.00, 3, 3),
('2026-01-20', 1200.00, 4, 1),
('2026-01-22', 45.00, 5, 8),
('2026-01-23', 127.50, 1, 2),
('2026-01-25', 70.00, 2, 10),
('2026-01-27', 75.00, 3, 7),
('2026-01-28', 320.00, 4, 4),
('2026-01-30', 450.00, 5, 5),
('2026-02-01', 51.00, 1, 2),
('2026-02-02', 210.00, 2, 6),
('2026-02-04', 95.00, 3, 9),
('2026-02-05', 85.00, 4, 3),
('2026-02-07', 90.00, 5, 8),
('2026-02-08', 105.00, 1, 10),
('2026-02-10', 1200.00, 2, 1),
('2026-02-11', 300.00, 3, 5),
('2026-02-13', 75.00, 4, 7),
('2026-02-14', 76.50, 5, 2),
('2026-02-15', 640.00, 1, 4),
('2026-02-17', 45.00, 2, 8),
('2026-02-18', 210.00, 3, 6),
('2026-02-20', 190.00, 4, 9),
('2026-02-21', 35.00, 5, 10),
('2026-02-23', 255.00, 1, 3),
('2026-02-24', 150.00, 2, 5),
('2026-02-26', 150.00, 3, 7),
('2026-02-27', 102.00, 4, 2),
('2026-02-28', 1200.00, 5, 1),
('2026-03-01', 90.00, 1, 8),
('2026-03-02', 70.00, 2, 10),
('2026-03-04', 320.00, 3, 4),
('2026-03-05', 76.50, 4, 2),
('2026-03-07', 150.00, 5, 5),
('2026-03-08', 95.00, 1, 9),
('2026-03-10', 210.00, 2, 6),
('2026-03-11', 170.00, 3, 3),
('2026-03-13', 225.00, 4, 7),
('2026-03-14', 1200.00, 5, 1),
('2026-03-16', 45.00, 1, 8),
('2026-03-17', 175.00, 2, 10),
('2026-03-19', 51.00, 3, 2),
('2026-03-20', 300.00, 4, 5),
('2026-03-22', 320.00, 5, 4),
('2026-03-23', 285.00, 1, 9),
('2026-03-25', 75.00, 2, 7),
('2026-03-26', 85.00, 3, 3),
('2026-03-28', 210.00, 4, 6),
('2026-03-30', 70.00, 5, 10);
```

## 1. Vista vw_ventas_destacadas

Crear una vista llamada vw_ventas_destacadas que contenga únicamente los registros de ventas cuyo monto sea igual o superior a $300.00, incluyendo la fecha, el vendedor y el monto.
```sql
CREATE VIEW vw_ventas_destacadas AS
SELECT 
    v.fecha,
    CONCAT(ve.nombre, ' ', ve.apellido) AS vendedor,
    v.monto
FROM ventas v
INNER JOIN vendedores ve ON v.vendedor_id = ve.id
WHERE v.monto >= 300.00;

SELECT * FROM vw_ventas_destacadas LIMIT 10;
```
![Vista 1](evidences/vista1.png)

---

## 2. Vista vw_resumen_vendedores

Crear una vista llamada vw_resumen_vendedores que muestre el nombre de cada vendedor, el número total de transacciones realizadas y el precio promedio de sus ventas redondeado a dos decimales.
```sql
CREATE VIEW vw_resumen_vendedores AS
SELECT 
    CONCAT(ve.nombre, ' ', ve.apellido) AS nombre_vendedor,
    COUNT(v.id) AS total_transacciones,
    ROUND(AVG(v.monto), 2) AS precio_promedio
FROM vendedores ve
LEFT JOIN ventas v ON ve.id = v.vendedor_id
GROUP BY ve.id, ve.nombre, ve.apellido;

SELECT * FROM vw_resumen_vendedores LIMIT 10;
```
![Vista 2](evidences/vista2.png)

---

## 3. Procedimiento sp_ajustar_precios_bajo_stock

Crear un procedimiento llamado sp_ajustar_precios_bajo_stock que aplique un incremento porcentual al precio de todos los productos cuyo stock sea menor a cierto límite recibido por parámetro (por ejemplo, aumentar un 10% el precio a productos con menos de 15 unidades en existencia).
```sql
CREATE OR REPLACE PROCEDURE sp_ajustar_precios_bajo_stock(
    p_limite_stock INT,
    p_porcentaje_incremento NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE productos
    SET precio = precio * (1 + (p_porcentaje_incremento / 100.0))
    WHERE stock < p_limite_stock;
END;
$$;

-- Ejemplo de uso: aumentar un 10% el precio a productos con menos de 15 unidades
CALL sp_ajustar_precios_bajo_stock(15, 10);
```
![Procedimiento](evidences/procedimiento.png)

---

## 4. Función fn_aplicar_descuento_producto

Crear una función llamada fn_aplicar_descuento_producto que reciba el id del producto y un porcentaje de descuento (por ejemplo, 15.00 para 15%). La función debe calcular el precio final restando el descuento al precio original.
```sql
CREATE OR REPLACE FUNCTION fn_aplicar_descuento_producto(
    p_producto_id INT,
    p_porcentaje_descuento NUMERIC
)
RETURNS NUMERIC
LANGUAGE plpgsql
AS $$
DECLARE
    v_precio_original NUMERIC;
    v_precio_final NUMERIC;
BEGIN
    SELECT precio INTO v_precio_original
    FROM productos
    WHERE id = p_producto_id;

    v_precio_final := v_precio_original - (v_precio_original * (p_porcentaje_descuento / 100.0));

    RETURN ROUND(v_precio_final, 2);
END;
$$;

-- Ejemplo de uso: aplicar un 15% de descuento al producto con id 1
SELECT fn_aplicar_descuento_producto(1, 15.00) AS precio_final;
```
![Función](evidences/funcion.png)