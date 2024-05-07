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

`Задание 21: Определить товары, которые покупали более 1 раза`
```sql
SELECT good_name
FROM Goods AS g
	JOIN Payments AS p ON p.good = g.good_id
GROUP BY good
HAVING COUNT(*) > 1;
```
`Задание 22: Найти имена всех матерей (mother)`
```sql
SELECT member_name
FROM FamilyMembers
WHERE STATUS = 'mother';
```

`Задание 23: Найдите самый дорогой деликатес (delicacies) и выведите его цену`
```sql
SELECT good_name, unit_price
FROM Goods AS g
	JOIN Payments AS p ON p.good = g.good_id
	JOIN GoodTypes AS gt ON gt.good_type_id = g.type
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC
LIMIT 1;
```

`Задание 24: Определить кто и сколько потратил в июне 2005`
```sql
SELECT member_name, SUM(amount * unit_price) AS costs
FROM FamilyMembers AS fm
    JOIN Payments AS p ON p.family_member = fm.member_id
WHERE YEAR(date) = '2005' AND MONTH(date) = 6
GROUP BY member_name;
```

`Задание 25: Определить, какие товары не покупались в 2005 году`
```sql
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
		SELECT good
		FROM Payments
		WHERE YEAR(date) = '2005');
```

`Задание 26: Определить группы товаров, которые не приобретались в 2005 году`
```sql
SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN(
		SELECT TYPE
		FROM Goods AS g
			JOIN Payments AS p ON p.good = g.good_id
		WHERE YEAR(date) = '2005');
```

`Задание 27: Узнать, сколько потрачено на каждую из групп товаров в 2005 году. Вывести название группы и сумму.`
```sql
SELECT good_type_name, SUM(amount * unit_price) AS costs
FROM GoodTypes AS gt
    JOIN Goods AS g ON g.type = gt.good_type_id
    JOIN Payments AS p ON p.good = g.good_id
WHERE YEAR(date) = '2005'
GROUP BY good_type_name;
```

`Задание 28: Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow)?`
```sql
SELECT COUNT(*) AS COUNT
FROM Trip
WHERE town_from = 'Rostov'AND town_to = 'Moscow'
```

`Задание 29: Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134`
```sql
SELECT DISTINCT name
FROM Trip AS t
	JOIN Pass_in_trip AS pit ON pit.trip = t.id
	JOIN Passenger AS p ON p.id = pit.passenger
WHERE town_to = 'Moscow' AND plane = 'TU-134';
```

`Задание 30: Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.`
```sql
SELECT trip, COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```

`Задание 31: Вывести всех членов семьи с фамилией Quincey.`
```sql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '% Quincey';
```

`Задание 32: Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.`
```sql
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, CURDATE()))) AS age
FROM FamilyMembers
```

`Задание 33: Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar).`
```sql
SELECT AVG(unit_price) AS cost
FROM Payments AS p
	JOIN Goods AS g ON g.good_id = p.good
WHERE good_name IN('red caviar', 'black caviar');
```

`Задание 34: Сколько всего 10-ых классов`
```sql
SELECT COUNT(name) AS count
FROM Class
WHERE name LIKE '10 %';
```

`Задание 35: Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?`
```sql
SELECT COUNT(classroom) AS count
FROM Schedule
WHERE YEAR(date) = '2019' AND MONTH(date) = '9' AND DAY(date) = '2';
```

`Задание 36: Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?`
```sql
SELECT *
FROM Student
WHERE address LIKE 'ul. Pushkina%';
```

`Задание 37: Сколько лет самому молодому обучающемуся?`
```sql
SELECT MIN(TIMESTAMPDIFF(YEAR, birthday, NOW())) AS year
FROM Student
```

`Задание 38: Сколько Анн (Anna) учится в школе?`
```sql
SELECT COUNT(*) AS count
FROM Student
WHERE first_name = 'Anna';
```

`Задание 39: Сколько обучающихся в 10 B классе?`
```sql
SELECT COUNT(*) AS count
FROM Class AS c
	JOIN Student_in_class AS sic ON sic.class = c.id
WHERE name = '10 B';
```

`Задание 40: Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.).`
```sql
SELECT name AS subjects
FROM Subject AS sj
	JOIN Schedule AS sc ON sc.subject = sj.id
	JOIN Teacher AS tc ON tc.id = sc.teacher
WHERE last_name = 'Romashkin'
	AND first_name LIKE 'P%'
	AND middle_name LIKE 'P%';
```

`Задание 41: Выясните, во сколько по расписанию начинается четвёртое занятие.`
```sql
SELECT start_pair
FROM Timepair
WHERE id = 4;
```

`Задание 42: Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?`
```sql
SELECT DISTINCT TIMEDIFF(
		(
			SELECT end_pair FROM Timepair
			WHERE id = 4
		),
		(
			SELECT start_pair FROM Timepair
			WHERE id = 2
		)) AS time
FROM Timepair;
```