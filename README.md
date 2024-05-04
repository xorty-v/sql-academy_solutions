# sql_academy_solutions

Решение задач по SQL. Тренажер [sql-academy.org](https://sql-academy.org/ru/trainer)

`Задание 1: Вывести имена всех когда-либо обслуживаемых пассажиров авиакомпаний`
```sql
SELECT name from Passenger;
```

`Задание 2: Вывести названия всеx авиакомпаний`
```sql
SELECT name from Passenger;
```

`Задание 3: Вывести все рейсы, совершенные из Москвы`
```sql
SELECT * FROM Trip 
WHERE town_from = 'Moscow';
```

`Задание 4: Вывести имена людей, которые заканчиваются на "man"`
```sql
SELECT name FROM Passenger
WHERE name LIKE '%man';
```

`Задание 5: Вывести количество рейсов, совершенных на TU-134`
```sql
SELECT COUNT(plane) AS COUNT FROM Trip
WHERE plane = 'TU-134';
```

`Задание 6: Какие компании совершали перелеты на Boeing`
```sql
SELECT c.name
FROM Company AS c 
JOIN Trip AS t ON t.company = c.id
WHERE t.plane = 'Boeing'
GROUP BY c.id;
```

`Задание 7: Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)`
```sql
SELECT plane FROM Trip
WHERE town_to = 'Moscow'
GROUP BY plane;
```

`Задание 8: В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?`
```sql
SELECT town_to, TIMEDIFF(time_in, time_out) AS flight_time
FROM Trip
WHERE town_from = 'Paris';
```

`Задание 9: Какие компании организуют перелеты из Владивостока (Vladivostok)?`
```sql
SELECT c.name 
FROM Company AS c
JOIN Trip AS t ON t.company = c.id
WHERE town_from = 'Vladivostok';
```

`Задание 10: Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.`
```sql
SELECT * FROM Trip
WHERE time_out BETWEEN '1900-01-01T10:00' AND '1900-01-01T14:00';
```