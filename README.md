# SQL

1. Посчитайте количество курьеров мужского и женского пола в таблице couriers. Новую колонку с числом курьеров назовите couriers_count. Результат отсортируйте по этой колонке по возрастанию.

Решение:

SELECT sex,
       count(distinct courier_id) as couriers_count
FROM   couriers
GROUP BY sex


2. Разбейте пользователей из таблицы users на группы по возрасту и посчитайте количество пользователей каждого возраста. Колонку с возрастом назовите age, а колонку с числом пользователей — users_count. Преобразуйте значения в колонке с возрастом в формат INTEGER, чтобы возраст был выражен целым числом.
Результат отсортируйте по колонке с возрастом по возрастанию.

Решение:

SELECT date_part('year', age(birth_date)) :: integer as age,
       count(user_id) as users_count
FROM   users
GROUP BY age
ORDER BY age

3. По данным из таблицы orders рассчитайте средний размер заказа по выходным и будням. Группу с выходными днями (суббота и воскресенье) назовите «weekend», а группу с будними днями (с понедельника по пятницу) — «weekdays» (без кавычек). В результат включите две колонки: колонку с группами назовите week_part, а колонку со средним размером заказа — avg_order_size. Средний размер заказа округлите до двух знаков после запятой. Результат отсортируйте по колонке со средним размером заказа — по возрастанию.

Решение: 

SELECT case when to_char(creation_time, 'Dy') in ('Sat', 'Sun') then 'weekend'
            else 'weekdays' end as week_part,
       round(avg(array_length(product_ids, 1)), 2) as avg_order_size
FROM   orders
GROUP BY week_part
ORDER BY avg_order_size
