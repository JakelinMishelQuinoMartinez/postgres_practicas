
# Resultados del Review 1
## Estructura de la Tabla

Antes de ejecutar las consultas, se creo la base de datos `campers`, la tabla `estudiantes` y se le insertaron 50 filas de datos.
```sql
CREATE DATABASE campus;
-- \c campus
CREATE TABLE estudiantes (
   id SERIAL,
   nombre VARCHAR(60),
   edad INT,
   genero CHAR(1),
   promedio FLOAT,
   altura NUMERIC(3,2),
   fecha_ingreso DATE,
   hora_ingreso  TIME,
   fecha_hora_registro TIMESTAMP,
   duración_tests INTERVAL,
   analisis_perfil TEXT,
   activo BOOLEAN
);
```
![Creación de la base de datos y tabla](evidences/create_bd_table.png)
![Insert 50 rows](evidences/insert.png)

## SELECT

1. Obtener el nombre, edad y promedio de todos los estudiantes que se encuentren activos.
```sql
SELECT nombre, edad, promedio 
FROM estudiantes 
WHERE activo = TRUE;
```
![Select 1](evidences/select1.png)

---

2. Listar todos los estudiantes del género femenino que tengan un promedio mayor o igual a 4.5.
```sql
SELECT * 
FROM estudiantes 
WHERE genero = 'F' AND promedio >= 4.5;
```
![Select 2](evidences/select2.png)

---

3. Consultar los estudiantes ingresados en el año 2024, ordenados de forma descendente por su fecha de ingreso.
```sql
SELECT * FROM estudiantes 
WHERE fecha_ingreso BETWEEN '2024-01-01' AND '2024-12-31' 
ORDER BY fecha_ingreso DESC;
```
![Select 3](evidences/select3.png)

---

4. Obtener el promedio de edad y el promedio general de calificaciones de todos los estudiantes registrados.
```sql
SELECT 
    AVG(edad) AS promedio_edad, 
    AVG(promedio) AS promedio_calificaciones 
FROM estudiantes;
```
![Select 4](evidences/select4.png)

---

5. Contar cuántos estudiantes hay registrados por cada género.
```sql
SELECT genero, COUNT(*) AS total 
FROM estudiantes 
GROUP BY genero;
```
![Select 5](evidences/select5.png)

---

6. Listar los 5 estudiantes con los promedios más altos de toda la tabla.
```sql
SELECT nombre, promedio 
FROM estudiantes 
ORDER BY promedio DESC 
LIMIT 5;
```
![Select 6](evidences/select6.png)

---

7. Seleccionar los estudiantes cuya duración de tests haya sido mayor a 2 horas y media.
```sql
SELECT nombre, duración_tests 
FROM estudiantes 
WHERE duración_tests >= '02:30:00';
```
![Select 7](evidences/select7.png)

---

8. Buscar a los estudiantes cuyo análisis de perfil contenga la palabra "bases de datos" o "algoritmos".
```sql
SELECT nombre, analisis_perfil 
FROM estudiantes 
WHERE analisis_perfil LIKE '%bases de datos%' OR analisis_perfil LIKE '%algoritmos%';
```
![Select 8](evidences/select8.png)

---

9. Calcular la altura máxima y mínima registrada entre los estudiantes hombres.
```sql
SELECT
    MIN(altura) AS altura_minima, 
    MAX(altura) AS altura_maxima 
FROM estudiantes 
WHERE genero = 'M';
```
![Select 9](evidences/select9.png)

---

10. Mostrar el nombre, fecha y hora exacta de registro de los estudiantes que ingresaron antes de las 09:00:00 AM.
```sql
SELECT nombre, fecha_hora_registro 
FROM estudiantes 
WHERE hora_ingreso < '09:00:00';
```
![Select 10](evidences/select10.png)

---