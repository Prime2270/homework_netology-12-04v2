# Домашнее задание к занятию "`SQL. Часть 2`" - `Яковлев Константин`

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей,
и выведите в результат следующую информацию:

- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
SELECT CONCAT(sta.last_name, ' ', sta.first_name) AS Фамилия_и_имя_сотрудника_магазина, ci.city AS город_нахождения_магазина, COUNT(cu.store_id) AS количество_пользователей_закреплённых_в_этом_магазине
FROM customer cu
JOIN store sto ON sto.store_id = cu.store_id
JOIN staff sta ON sta.store_id = sto.store_id
JOIN address a ON a.address_id = sto.address_id
JOIN city ci ON a.city_id = ci.city_id
GROUP by cu.store_id, sta.staff_id, a.address_id, ci.city_id
HAVING COUNT(cu.store_id) > 300;
```

![job1]()

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
SELECT count(film_id)
FROM film
WHERE LENGTH > (SELECT AVG(length) FROM film);
```

![job2]()

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, 
и добавьте информацию по количеству аренд за этот месяц.

```
SELECT date_format(payment_date, "%c.%y") AS месяц, COUNT(payment_id) AS кол_во_аренд, SUM(amount) AS max_sum FROM payment
WHERE date_format(payment_date, "%c.%y") = (SELECT date_format(payment_date, "%c.%y") FROM payment GROUP BY date_format(payment_date, "%c.%y") ORDER BY SUM(amount) desc limit 1)
GROUP BY date_format(payment_date, "%c.%y");
```

![job3]()
