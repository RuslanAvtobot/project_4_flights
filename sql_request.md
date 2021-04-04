# project_4_flights

Вопрос 4
SELECT count(f.flight_id)
FROM dst_project.flights f
WHERE f.status = 'Arrived'
  AND f.actual_arrival::date BETWEEN '2017-4-1'::date AND '2017-9-1'::date
