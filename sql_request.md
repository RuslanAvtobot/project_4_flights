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
Вопрос 1
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE f.status = 'Cancelled'
Вопрос 2
Boeing
SELECT count(a.model)
FROM dst_project.aircrafts a
WHERE a.model LIKE 'Boeing%'
Sukhoi Superjet
SELECT count(a.model)
FROM dst_project.aircrafts a
WHERE a.model LIKE 'Sukhoi Superjet%'
Airbus
SELECT count(a.model)
FROM dst_project.aircrafts a
WHERE a.model LIKE 'Airbus%'
Вопрос 3
SELECT count(DISTINCT a.city)
FROM dst_project.airports a
WHERE a.timezone like 'Australia%'
Вопрос 4
SELECT f.flight_id,
       (f.actual_arrival - f.scheduled_arrival) delay_time
FROM dst_project.flights f
GROUP BY 1
HAVING (f.actual_arrival - f.scheduled_arrival) IS NOT NULL
ORDER BY 2 DESC
LIMIT 1
