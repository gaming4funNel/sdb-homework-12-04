# Домашнее задание к занятию «SQL. Часть 2» - Иванов Игорь

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

SELECT ss.store_id, CONCAT(staff.first_name, ' ', staff.last_name) AS stuff_name, city.city AS store_city, ss.customer_ss 
FROM ( 
    SELECT store_id, COUNT(store_id) AS customer_ss 
    FROM customer 
    GROUP BY store_id 
    HAVING COUNT(store_id) > 300
) AS ss 
LEFT JOIN staff on staff.store_id = ss.store_id 
LEFT JOIN store ON store.store_id = ss.store_id 
LEFT JOIN address ON address.address_id = store.address_id 
LEFT JOIN city ON city.city_id = address.city_id;

![sql](https://github.com/gaming4funNel/sdb-homework-12-04/blob/main/img/sql1.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

SELECT COUNT(film_id) FROM film
WHERE `length` > (SELECT AVG(`length`) FROM film)

![sql](https://github.com/gaming4funNel/sdb-homework-12-04/blob/main/img/sql2.png)

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

SELECT 
    DATE_FORMAT(payment_date, '%M %Y') AS payment_month, 
    SUM(amount) AS total_amount, 
    COUNT(rental_id) AS rental_max 
FROM payment
GROUP BY payment_month
ORDER BY total_amount DESC
LIMIT 1

![sql](https://github.com/gaming4funNel/sdb-homework-12-04/blob/main/img/sql3.png)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

SELECT
    staff_id,
    COUNT(rental_id) AS sales_count,
    CASE
        WHEN COUNT(rental_id) > 8000 THEN "Да"
        ELSE "Нет"
    END AS premium
FROM
    rental
GROUP BY
    staff_id;

![sql](https://github.com/gaming4funNel/sdb-homework-12-04/blob/main/img/sql4.png)

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

SELECT
    f.film_id,
    f.title
FROM
    film f
WHERE
    f.film_id NOT IN (
        SELECT
            i.film_id
        FROM
            inventory i
            INNER JOIN rental r ON i.inventory_id = r.inventory_id
    );

![sql](https://github.com/gaming4funNel/sdb-homework-12-04/blob/main/img/sql5.png)