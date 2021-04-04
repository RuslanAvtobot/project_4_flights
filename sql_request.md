# project_4_flights

Задание 4.1
SELECT a.city
FROM dst_project.airports a
GROUP BY 1
HAVING count(airport_name) > 1

Задание 4.2.
Вопрос 1
SELECT count(DISTINCT status)
FROM dst_project.flights f
Вопрос 2
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE f.status = 'Departed'
Вопрос 3
SELECT count(s.seat_no)
FROM dst_project.seats s
WHERE s.aircraft_code = '773'
Вопрос 4
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE f.status = 'Arrived'
  AND f.actual_arrival::date BETWEEN '2017-4-1'::date AND '2017-9-1'::date
  
Задание 4.3

