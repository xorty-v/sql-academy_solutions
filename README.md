# sql_academy_solutions

Решение задач по SQL. Тренажер [sql-academy.org](https://sql-academy.org/ru/trainer)

`Задание 1: Вывести имена всех когда-либо обслуживаемых пассажиров авиакомпаний`
```sql
SELECT name from Passenger;
```

`Задание 2: Вывести названия всеx авиакомпаний`
```sql
SELECT name FROM Company;
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

`Задание 11: Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.`
```sql
SELECT name
FROM Passenger
WHERE LENGTH(name) = (
		SELECT MAX(LENGTH(name))
		FROM Passenger);
```

`Задание 12: Вывести id и количество пассажиров для всех прошедших полётов`
```sql
SELECT trip, COUNT(passenger) AS count
FROM Pass_in_trip
GROUP  BY trip;
```

`Задание 13: Вывести имена людей, у которых есть полный тёзка среди пассажиров`
```sql
SELECT name FROM Passenger
GROUP BY name
HAVING COUNT(name) > 1;
```

`Задание 14: В какие города летал Bruce Willis`
```sql
SELECT t.town_to 
FROM Trip AS t
JOIN Pass_in_trip AS pit ON pit.trip = t.id
JOIN Passenger AS p ON p.id = pit.passenger
WHERE name = 'Bruce Willis';
```

`Задание 15: Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London)`
```sql
SELECT t.time_in
FROM Trip AS t
JOIN Pass_in_trip AS pit ON pit.trip = t.id
JOIN Passenger AS p ON p.id = pit.passenger
WHERE name = 'Steve Martin' AND town_to = 'London'; 
```

`Задание 16: Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.`
```sql
SELECT p.name, COUNT(pit.passenger) AS count
FROM Passenger AS p
JOIN Pass_in_trip AS pit ON pit.passenger = p.id
GROUP BY passenger
HAVING COUNT(trip) > 0
ORDER BY COUNT(trip) DESC, name;
```

`Задание 17: Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.`
```sql
SELECT member_name, status, SUM(amount * unit_price) AS costs
FROM FamilyMembers AS fm
JOIN Payments AS p ON p.family_member = fm.member_id
WHERE YEAR(date) = 2005
GROUP BY member_name, status;
```

`Задание 18: Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.`
```sql
SELECT member_name
FROM FamilyMembers
WHERE birthday = (
		SELECT MIN(birthday)
		FROM FamilyMembers);
```

`Задание 19: Определить, кто из членов семьи покупал картошку (potato)`
```sql
SELECT DISTINCT STATUS
FROM FamilyMembers AS fm
	JOIN Payments AS p ON p.family_member = fm.member_id
	JOIN Goods AS g ON g.good_id = p.good
WHERE good_name = 'potato';
```

`Задание 20: Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму`
```sql
SELECT status,
	member_name,
	SUM(amount * unit_price) AS costs
FROM FamilyMembers AS fm
	JOIN Payments AS p ON p.family_member = fm.member_id
	JOIN Goods AS g ON g.good_id = p.good
	JOIN GoodTypes AS gt ON gt.good_type_id = g.type
WHERE good_type_name = 'entertainment'
GROUP BY fm.status, fm.member_name;
```