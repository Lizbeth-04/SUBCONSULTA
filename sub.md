1.Numero de facturas realizadas por cliente .


SELECT c.full_name AS nombre_cliente, c.address AS direccion, COUNT(i.id) AS nro_facturas
FROM client c
LEFT JOIN invoice i ON c.id = i.client_id
GROUP BY c.full_name, c.address;

2.lista del cliente y sus compras mas caras.

SELECT c.full_name AS nombres, c.email AS correo, (
    SELECT MAX(total)
    FROM invoice i
    WHERE i.client_id = c.id
) AS total_mas_alto
FROM client c;


SELECT c.nombre_cliente, c.direccion, COUNT(i.id) AS nro_facturas
FROM client c
LEFT JOIN invoice i ON c.id = i.client_id
GROUP BY c.nombre_cliente, c.direccion;

3.EL total es mayor al promedio delas facturas.
SELECT create_at AS fecha_factura, total
FROM invoice
WHERE total > (
    SELECT AVG(total)
    FROM invoice
);

4.subconsulta con where 

SELECT id, description, start_date, end_date, total_attendees, city
FROM event e1
WHERE EXISTS (
    SELECT 1
    FROM event e2
    WHERE e2.city = e1.city
      AND e2.total_attendees = e1.total_attendees
      AND e2.id <> e1.id
);

5.subconsulata con select
SELECT id, description, start_date, end_date, total_attendees, city,
       (SELECT COUNT(*)
        FROM event e2
        WHERE e2.total_attendees > 100
       ) AS eventos_con_mas_de_100_asistentes
FROM event e1;
