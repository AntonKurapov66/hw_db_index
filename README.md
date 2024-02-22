# Домашнее задание к занятию "Индексы" - `Курапов Антон`

### Задание 1
*  Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

![alt text](https://github.com/AntonKurapov66/hw_db_index/blob/main/img/1.PNG)

 ```sql
	  SELECT table_schema, 
    ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS "Size all DB in (MB)",
    ROUND(SUM(index_length) / 1024 / 1024, 2) AS "Size index DB in (MB)",
    ROUND(SUM(index_length) / 1024 / 1024, 2)*100 / ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS "% index in (%)"
    FROM information_schema.TABLES
    WHERE table_schema = "sakila" 
    GROUP BY table_schema;
```
### Задание 2
*  Выполните explain analyze следующего запроса:
![alt text](https://github.com/AntonKurapov66/hw_db_index/blob/main/img/2.PNG)
  *  перечислите узкие места:
    *    Отсутствие индексов отсутствие условий объединения таблиц с помощью JOIN. 
    *    Не используется GROUP BY из-за этого могут появится недостоверные данные.


  *  оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.
![alt text](https://github.com/AntonKurapov66/hw_db_index/blob/main/img/3.PNG)

 ```sql
      SELECT DISTINCT CONCAT(c.last_name, ' ', c.first_name) AS FI, 
           SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title) AS total_amount
      FROM payment p
      JOIN rental r ON r.rental_id  = p.rental_id
      JOIN customer c ON r.customer_id = c.customer_id
      JOIN inventory i ON i.inventory_id = r.inventory_id
      JOIN film f ON f.film_id = i.film_id
      WHERE DATE(p.payment_date) = '2005-07-30'
      GROUP BY FI;
```
