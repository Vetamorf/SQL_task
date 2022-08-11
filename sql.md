# 11 задание

При решении задач использовалась СУДБ PostgreSQL.


### Задача 1. 
Напишите запрос, который выведет пилотов, которые в качестве второго пилота в августе этого года трижды ездили в аэропорт Шереметьево.

``` sql
SELECT pilot_id, name
FROM 
    pilot p INNER JOIN flight f 
    ON p.pilot_id = f.second_pilot_id
WHERE destination = 'Шереметьево' 
    AND EXTRACT(MONTH FROM flight_dt) = 8 
    AND EXTRACT(YEAR FROM flight_dt) = 2022
GROUP BY pilot_id, name
HAVING COUNT(*) = 3
ORDER BY pilot_id;
```

В задаче дополнительно выводится `pilot_id` на случай, если в таблице присутствуют пилоты с одинаковым именем.

### Задача 2.

Выведите пилотов старше 45 лет, совершали полеты на самолетах с количеством пассажиров больше 30.

``` sql
SELECT DISTINCT pilot_id, name
FROM
    pilot p INNER JOIN flight f
        ON p.pilot_id = f.first_pilot_id
        OR p.pilot_id = f.second_pilot_id
    INNER JOIN plane USING(plane_id)
WHERE age > 45
    AND cargo_flg = 0
    AND quantity > 30;
```

### Задача 3.

Выведите ТОП 10 пилотов-капитанов (first_pilot_id), которые совершили наибольшее число грузовых перелетов в этом году.

``` sql
SELECT pilot_id, name, COUNT(*) AS Число_перелетов
FROM
    pilot p INNER JOIN flight f
        ON p.pilot_id = f.first_pilot_id
    INNER JOIN plane USING(plane_id)
WHERE cargo_flg = 1
    AND EXTRACT(YEAR FROM flight_dt) = 2022
GROUP BY pilot_id, name
ORDER BY COUNT(*) DESC, name
LIMIT 10;
```

Запрос выше выводит 10 пилотов, совершивших наибольшее число грузовых перелетов в 2022 году. 
В случае, если ТОП 11-ый пилот имеет такое же количество перелетов, как ТОП 10-ый, выводится пилот, имя которого идет раньше в алфавитном порядке.



