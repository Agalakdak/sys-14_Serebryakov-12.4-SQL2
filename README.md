# Домашнее задание к занятию 12.4. «SQL. Часть 2» - Серебряков Руслан

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.


> Select CONCAT(s.last_name , ' ', s.first_name) as 'Фамилия и имя сотрудника', 
> c.city,
> COUNT(st.store_id)
> from staff s 
> inner join store st on st.store_id = s.store_id 
> inner join address addr on addr.address_id = st.address_id 
> inner join city c on c.city_id = addr.city_id 
> inner join customer cu on cu.store_id = st.store_id 
> GROUP BY CONCAT(s.last_name , ' ', s.first_name), c.city
> HAVING COUNT(cu.customer_id) > 300


### Задание 2


Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

SELECT COUNT(1)
AS 'Количество фильмов 
с продолжительностью 
выше среднего'
FROM film
WHERE film.length > ( SELECT AVG(film.length) from film)


### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

select DATE_FORMAT(payment_date, '%Y-%M') as 'Месяц',
SUM(amount) as 'Сумма', 
COUNT(payment.rental_id) as 'Количество аренд' 
from payment 
group by payment_date 
order by SUM(amount) DESC limit 1


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

На самом деле ответ я чуть-чуть подсмотрел в лекции =) 

select *
from film
LEFT JOIN inventory ON film.film_id = inventory.film_id
LEFT JOIN rental ON rental.inventory_id = inventory.inventory_id
WHERE rental.rental_id IS NULL




