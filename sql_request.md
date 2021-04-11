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

Задание 4.4
Вопрос 1
SELECT min(f.scheduled_departure)
FROM dst_project.flights f
Вопрос 2
SELECT date_part('hour', f.scheduled_arrival - f.scheduled_departure) * 60 + 
       date_part('minute', f.scheduled_arrival - f.scheduled_departure)
FROM dst_project.flights f
ORDER BY 1 DESC
LIMIT 1
Вопрос 3
SELECT f.departure_airport,
       f.arrival_airport,
       date_part('hour', f.scheduled_arrival - f.scheduled_departure) * 60 + date_part('minute', f.scheduled_arrival - f.scheduled_departure)
FROM dst_project.flights f
ORDER BY 3 DESC
LIMIT 3
Вопрос 4
SELECT avg(date_part('hour', f.scheduled_arrival - f.scheduled_departure) * 60 + 
       date_part('minute', f.scheduled_arrival - f.scheduled_departure))
FROM dst_project.flights f

Задание 4.5
Вопрос 1
SELECT s.fare_conditions,
       count(s.seat_no)
FROM dst_project.seats s
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1
Вопрос 2
SELECT min(b.total_amount)
FROM dst_project.bookings b
Вопрос 3
SELECT bp.seat_no
FROM dst_project.tickets t
JOIN dst_project.boarding_passes bp ON t.ticket_no = bp.ticket_no
WHERE t.passenger_id = '4313 788533'

Задание 5.1
Вопрос 1
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE date_part('year', f.actual_arrival) = '2017'
  AND f.arrival_airport = 'AAQ'
Вопрос 2
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE date_part('year', f.scheduled_departure) = '2017'
  AND date_part('month', f.scheduled_departure) in ('1',
                                                    '2',
                                                    '12')
  AND f.departure_airport = 'AAQ'
Вопрос 3
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE f.departure_airport = 'AAQ'
  AND f.status = 'Cancelled'
Вопрос 4
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE f.departure_airport = 'AAQ'
  AND f.arrival_airport not in ('DME',
                                'SVO',
                                'VKO')
Вопрос 5
WITH uniq_aircraft AS
  (SELECT DISTINCT f.aircraft_code
   FROM dst_project.flights f
   WHERE f.departure_airport = 'AAQ')
SELECT distinct(a.model),
       count(s.seat_no)
FROM uniq_aircraft
JOIN dst_project.seats s ON uniq_aircraft.aircraft_code = s.aircraft_code
JOIN dst_project.aircrafts a ON uniq_aircraft.aircraft_code = a.aircraft_code
GROUP BY 1
ORDER BY 2 DESC

Итоговые запросы
Смотрим на аэропорта прибытия, самолёты, максимальное время в полёте и часовые пояса.
SELECT DISTINCT f.arrival_airport,
                a.city,
                a.timezone,
                ac.model,
                max(date_part('minute', f.actual_arrival - f.actual_departure) + date_part('hour', f.actual_arrival - f.actual_departure)*60)
FROM dst_project.flights f
JOIN dst_project.airports a ON f.arrival_airport = a.airport_code
JOIN dst_project.aircrafts ac ON f.aircraft_code = ac.aircraft_code
WHERE f.departure_airport = 'AAQ'
  AND (date_trunc('month', f.scheduled_departure) in ('2017-01-01',
                                                      '2017-02-01',
                                                      '2017-12-01'))
  AND f.status not in ('Cancelled')
GROUP BY 1,
         2,
         3,
         4
