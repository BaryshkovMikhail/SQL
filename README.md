# Логический порядок

1. FROM
2. ON
3. JOIN
4. WHERE
5. GROUP BY
6. WITH CUBE или WITH ROLLUP
7. HAVING
8. SELECT
9. OVER
10. DISTINCT
11. ORDER BY
12. В начало

## Простые запросы. SELECT и FROM

- Нужно вывести всех пользователей из таблицы customer

```sql
SELECT * FROM customer;
```

- вывести определенные столбцы, то их нужно перечислить

```sql
SELECT customer_id, last_name, first_name FROM customer;
```

## Простые запросы. ALIAS

Алиасы (псевдонимы) нужны для того, чтобы задавать временные названия для столбцов и таблиц. Алиасы для столбцов позволяют дать понятные названия для вычисляемых значений, а краткие алиасы для таблиц позволяют упростить написание запросов. Для задания алиасов используется оператор AS, но его можно опускать. Чтобы не писать полные названия таблиц, зададим в запросе на следующем слайде краткие алиасы и для вычисляемого столбца также зададим временное имя с обозначением результата.

```sql
SELECT f.title, c.name, f.rental_rate/f.rental_duration AS cost_per_day
FROM film f
JOIN film_category fc ON fc.film_id = f.film_id
JOIN category c ON c.category_id = fc. category_id;
```

<img src = "img/img11.png" width = 40%>

## Простые запросы. ORDER BY

В независимости от того, в каком порядке данные хранятся в базе данных, SQL возвращает результат в непредсказуемом порядке. Чтобы явно задать порядок сортировки, используется оператор ORDER BY. Для того чтобы задать направление сортировки, нужно указывать `ASC` – от меньшего к большему (по умолчанию) или `DESC` – от большего к меньшему. Давайте возьмем запрос с получением стоимости аренды фильма за день и отсортируем по стоимости аренды за день от большего к меньшему, а потом по названию фильма.

```sql
SELECT title, rental_rate/rental_duration AS cost_per_day
FROM film
ORDER BY cost_per_day DESC, title;
```

## Простые запросы. LIMIT и OFFSET

Если нужно получить первые N записей из результата, используется оператор `LIMIT`. Если нужно исключить из результата первые N записей, используется оператор `OFFSET`. Возьмем предыдущий запрос и получим первые 10 записей начиная с 58.

```sql
SELECT title, rental_rate/rental_duration AS cost_per_day
FROM film
ORDER BY cost_per_day DESC, title
LIMIT 10
OFFSET 57;
```

<img src = "img/img10.png" width = 40%>

### Простые запросы. DISTINCT

Для получения уникальных значений в результате, используется оператор `DISTINCT`.

- К примеру, нужно получить уникальный список имен пользователей:

```sql
SELECT DISTINCT first_name
FROM customer;
```

<img src = "img/img9.png" width = 20%>

- Если нужно получить уникальные значения по нескольким столбцам, то данные столбцы перечисляются после оператора `DISTINCT`:

```sql
SELECT DISTINCT last_name, first_name
FROM customer;
```

<img src = "img/img8.png" width = 20%>

## Простые запросы. WHERE

Как правило, при получении данных нужно указать условия, по которым нужно их отфильтровать, для этого используется оператор `WHERE`. Если условий нужно использовать несколько, то используются логические операторы `AND` и `OR`. Для отрицания в условии используется оператор `NOT`.

Выведем в результат платежи:

- более 7 у.е. и которые совершил сотрудник с идентификатором равным 2,
- менее 5 у.е. и которые совершил сотрудник с идентификатором равным 1.

Обратите внимание, что оператор `AND` имеет приоритет перед `OR`

```sql
SELECT *
FROM payment
WHERE amount > 7 AND staff_id = 2 OR amount < 5 AND staff_id = 1;
```

<img src = "img/img7.png" width = 100%>

## Простые запросы. CAST

При работе с разными типами данных часто нужно преобразовывать один тип данных к другому, для этого используется оператор CAST со следующим синтаксисом:

```sql
CAST(value AS type)
```

<img src = "img/img6.png" width = 90%>

В таблице payment столбец payment_date имеет тип данных datetime, то есть дата и время, а нужно работать только с датой, для этого преобразуем datetime к date:

```sql
SELECT payment_id, CAST(payment_date AS DATE)
FROM payment;
```

Таблица приведения типов, на примере MSSQL

<img src = "img/img5.png" width = 100%>

## Округление

Для округления в MySQL используются следующие функции:

- ROUND – округляет число до заданного числа десятичных знаков,
- TRUNCATE – усекает число до указанного числа десятичных знаков,
- FLOOR – возвращает наибольшее целочисленное значение, которое меньше или равно числу,
- CEIL – возвращает наименьшее целочисленное значение, которое больше или равно числу,
- ABS – возвращает абсолютное (положительное) значение числа.

Округлим значения используя разные функции:

```sql
SELECT ROUND(100.576); -- 101
SELECT ROUND(100.576, 2); -- 100.58
SELECT TRUNCATE(100.576, 2); -- 100.57
SELECT FLOOR(100.576); -- 100
SELECT CEIL(100.576); -- 101
SELECT ABS(-100.576); -- 100.576
```

<img src = "img/img4.png" width = 50%>

Получим «красивый» результат стоимости аренды за день:

```sql
SELECT title, ROUND(rental_rate/rental_duration, 2) AS cost_per_day
FROM film
ORDER BY cost_per_day DESC, title
```

## Арифметические операторы

SQL поддерживает все основные арифметические операторы:

- `+ – * /` – стандартные операторы,
- POWER – возведение в степень,
- SQRT – возвращает квадратный корень числа,
- COS, SIN, TAN, COT, etc – геометрические операторы,
- DIV – целочисленное деление,
- % – остаток от деления,
- GREATEST/LEAST – возвращает наибольшее/наименьшее значение из списка,
- RAND – возвращает случайное число в диапазоне от 0 (включительно) до 1 (исключительно).

Посмотрим на работу некоторых функций:

```sql
SELECT POWER(2, 3); -- 8
SELECT SQRT(64); -- 8
SELECT 64 DIV 6; -- 10
SELECT 64%6; -- 4
SELECT GREATEST(17, 5, 18, 21, 16); -- 21
SELECT LEAST(17, 5, 18, 21, 16); -- 5
SELECT RAND(); -- 0.005757967015502944
```

Посмотрим на работу некоторых функций на данных:

```sql
SELECT  rental_rate, rental_duration,
        rental_rate + rental_duration a,
        rental_rate – rental_duration b,
        rental_rate * rental_duration c,
        rental_rate / rental_duration d,
        rental_rate % rental_duration e,
        rental_rate DIV rental_duration f,
        POWER(rental_rate, rental_duration) g,
        COS(rental_rate) h, SIN(rental_duration) j
FROM film;
```

<img src = "img/img3.png" width = 100%>

## Работа со строками

Разберем основные функции для работы с подстроками и строками:

- CONCAT, CONCAT_WS – соединяет строки в одну,_WS – по сепаратору,
- LENGTH – возвращает длину строки в байтах,
- CHAR_LENGTH – возвращает длину строки в символах,
- POSITION – возвращает позицию первого вхождения подстроки в строку,
- SUBSTR – извлекает подстроку из строки.
- LEFT / RIGHT – извлекает ряд символов из строки начиная слева / справа,
- LOWER / UPPER – преобразует строку в нижний / верхний регистр,
- INSERT – вставляет подстроку в строку в указанной позиции и для определенного количества символов,
- TRIM – удаляет начальные и конечные пробелы из строки,
- REPLACE – заменяет все вхождения подстроки в строке на новую подстроку,
- SUBSTRING_INDEX – возвращает подстроку строки до того, как появится указанное число разделителей.

Давайте разберем, как эти функции работают на практике:

```sql
SELECT CONCAT(last_name, ' ', first_name, ' ', email) FROM customer;
SELECT CONCAT_WS(' ', last_name, first_name, email) FROM customer;
```

<img src = "img/img1.png" width = 70%>

```sql
SELECT  LENGTH(last_name), CHAR_LENGTH(last_name),
        LENGTH('Привет'), CHAR_LENGTH('Привет')
FROM customer;
```

<img src = "img/img2.png" width = 80%>

```sql
SELECT  POSITION('D' IN last_name), SUBSTR(last_name, 2, 3),
        LEFT(last_name, 3), RIGHT(last_name, 3)
FROM customer;
```

<img src = "img/img12.png" width = 80%>

```sql
SELECT LOWER(last_name), INSERT(last_name, 'MAX', 1, 5),
REPLACE(last_name, 'A', 'X')
FROM customer;
```

<img src = "img/img13.png" width = 80%>

Выражение `LIKE` возвращает true, если строка соответствует заданному шаблону. Выражение `NOT LIKE` возвращает `false`, когда`LIKE` возвращает true и наоборот. Если шаблон не содержит знаков процента и подчеркиваний, тогда шаблон представляет в точности строку и `LIKE` работает как оператор сравнения. Подчеркивание (_) в шаблоне подменяет (вместо него подходит) любой символ. Знак процента (%) подменяет любую (в том числе и пустую) последовательность символов.

```sql
SELECT CONCAT(last_name, ' ', first_name)
FROM customer
WHERE first_name LIKE '%jam%';
```

<img src = "img/img14.png" width = 40%>

## Работа с датами и временем

Разберем основные функции для работы с датами и временем:

- NOW / CURDATE – возвращает текущие дату и время / дату,
- DATE_ADD – добавляет интервал времени/даты к дате, а затем возвращает дату, работает как с датой, так и со временем,
- DATE_SUB – вычитает интервал времени/даты из даты, а затем возвращает дату, работает как с датой, так и со временем,
- YEAR / MONTH / DAY — возвращает год / месяц / день месяца для заданной даты,
- EXTRACT – извлекает часть из заданной даты,
- DATEDIFF – возвращает количество дней между двумя значениями даты,
- QUARTER – возвращает квартал года для заданного значения даты,
- DATE_FORMAT – форматирует указанную дату,
- TIME_FORMAT – форматирует время по заданному формату,
- DATE – извлекает дату из выражения datetime.

```sql
SELECT DATE_ADD(NOW(), INTERVAL 3 DAY);
```

<img src = "img/img16.png" width = 40%>

```sql
SELECT DATE_SUB(CURDATE(), INTERVAL 3 DAY);
```

<img src = "img/img15.png" width = 40%>

```sql
SELECT YEAR(NOW()), MONTH(NOW()), WEEK(NOW()), DAY(NOW());
```

<img src = "img/img17.png" width = 40%>

```sql
SELECT EXTRACT(HOUR FROM NOW()), EXTRACT(DAY_MINUTE FROM NOW()),
EXTRACT(DAY FROM NOW());
```

<img src = "img/img18.png" width = 70%>

```sql
SELECT DATEDIFF(return_date, rental_date), QUARTER(return_date) FROM rental;
```

<img src = "img/img19.png" width = 50%>

```sql
SELECT DATE_FORMAT(payment_date, '%D – %A – %Y'),
TIME_FORMAT(TIME(payment_date), '%R') FROM payment;
```

<img src = "img/img20.png" width = 70%>

При работе с датой и временем нужно помнить: если из `DATE` сделать `DATETIME`, то это будет дата с нулевым временем. К примеру, есть дата ‘2020-01-01’, если с ней работать, как с
`DATETIME`, то SQL будет эту дату воспринимать, как ‘2020-01-01 00:00:00’. Соответственно, если значения в БД хранятся в DATETIME, но в запросе нужно работать именно с датами, то нужно явно приводить значения к нужному типу данных. Причем это касается не только дат, но и остальных типов данных.

## BETWEEN

Для того чтобы найти значения в заданном диапазоне, используется оператор BETWEEN. Данный оператор можно использовать с числами, строками и датами. Крайние значения включаются в результат. К примеру, нужно найти все платежи, стоимость которых между 5 и 7 включительно:

```sql
SELECT * FROM payment WHERE amount BETWEEN 5 AND 7;
```

<img src = "img/img21.png" width = 80%>

## JOIN

В SQL JOIN используются для соединения нескольких таблиц и получения из них данных. Существуют следующие типы JOIN:

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN
- CROSS JOIN

В **LEFT OUTER JOIN**, **RIGHT OUTER JOIN** и **FULL OUTER JOIN** ключевое слово **OUTER** можно опустить, оно не обязательно для использования.
Также при использовании **INNER JOIN** можно опустить ключевое слово **INNER** При работе с **JOIN** желательно использовать алиасы, для удобства чтения или написания запросов и указания,из каких таблиц какие столбцы нужно получать.

### INNER JOIN

**INNER JOIN** возвращает данные по строкам, содержащимодинаковые значения. Если смотреть на таблицы как на множества строк, то результат их выполнения можно представить на следующей диаграмме Венна:

Нужно вывести названия фильмов и имена актеров, которые снимались в этих фильмах.

```sql
SELECT f.title, CONCAT(a.last_name, ' ', a.first_name) AS actor_name
FROM film f
INNER JOIN film_actor fa ON fa.film_id = f.film_id
INNER JOIN actor a ON a.actor_id = fa.actor_id;
```

<img src = "img/img22.png" width = 40%>

В этом случае можно использовать **INNER JOIN** без потери данных, так как в таблицах есть необходимые ограничения

### LEFT JOIN

**LEFT JOIN** возвращает все данные из левой таблицы. Если по ним есть совпадения в правой, они обогащаются соответствующими данными, иначе туда записывается специальное значение **NULL**

Нужно получить данные по всем пользователям и добавить информацию по городам, в которых они живут

```sql
SELECT CONCAT(c.last_name, ' ', c.first_name), c2.city
FROM customer c
LEFT JOIN address a ON a.address_id = c.address_id
LEFT JOIN city c2 ON c2.city_id = a.city_id;
```

<img src = "img/img23.png" width = 60%>

Чтобы получить только те строки, которые не содержат данных в правой таблице, можно использовать оператор **WHERE**

Нужно получить все фильмы, которые не брали в аренду.

```sql
SELECT f.title
FROM film f
LEFT JOIN inventory i ON i.film_id = f.film_id
LEFT JOIN rental r ON r.inventory_id = i.inventory_id
WHERE r.rental_id IS NULL;
```

Здесь получаем 100% данных по всем фильмам, обогащаем через LEFT JOIN данными по аренде. Фильмы, которые не брали в аренду дополняются значениями NULL, по которым и фильтруем в конце запроса

<img src = "img/img24.png" width = 30%>

### RIGHT JOIN

**RIGHT JOIN** — это обратная версия **LEFT JOIN**. Возвращает все данные из правой таблицы.Если по ним есть совпадения в левой, они обогащаются соответствующими данными, иначе туда записывается специальное значение **NULL**

Нужно получить список всех городов и добавить информацию по пользователям, которые живут в этих городах

```sql
SELECT CONCAT(c.last_name, ' ', c.first_name), c2.city
FROM customer c
RIGHT JOIN address a ON a.address_id = c.address_id
RIGHT JOIN city c2 ON c2.city_id = a.city_id;
```

<img src = "img/img25.png" width = 60%>

### FULL JOIN

**FULL JOIN** не поддерживается MySQL. Рассмотрим его синтаксис в других СУБД и как реализовать в MySQL.
**FULL JOIN** позволяет получить сопоставление по всем строкам в обеих таблицах. То есть получаем все данные из левой и правой таблиц, а там, где сопоставлений нет — добавляются значения **NULL**

Нужно получить данные по всем арендам и платежам по этим арендам. Пример выполнен в **PostgreSQL**

```sql
SELECT r.rental_id, r.rental_date, p.payment_id, p.payment_date, p.amount
FROM rental r
FULL JOIN payment p ON p.rental_id = r.rental_id;
```

<img src = "img/img26.png" width = 60%>

Чтобы получить список уникальных строк из обеих таблиц, можно также воспользоваться оператором **WHERE**

Нужно найти записи по арендам и платежам, по которым нет пересечения. Пример выполнен в PostgreSQL

```sql
SELECT r.rental_id, r.rental_date, p.payment_id, p.payment_date, p.amount
FROM rental r
FULL JOIN payment p ON p.rental_id = r.rental_id
WHERE r.rental_id IS NULL OR p.payment_id IS NULL;
```

<img src = "img/img27.png" width = 60%>

Реализация FULL JOIN в MySQL с помощью оператора UNION. Нужно получить данные по всем арендам и платежам по этим арендам

```sql
SELECT r.rental_id, r.rental_date, p.payment_id, p.payment_date, p.amount
FROM rental r
LEFT JOIN payment p ON p.rental_id = r.rental_id
UNION
SELECT r.rental_id, r.rental_date, p.payment_id, p.payment_date, p.amount
FROM rental r
RIGHT JOIN payment p ON p.rental_id = r.rental_id;
```

<img src = "img/img28.png" width = 60%>

### CROSS JOIN

CROSS JOIN — это Декартово произведение, когда каждая строка
левой таблицы сопоставляется с каждой строкой правой таблицы.
В результате получается таблица со всеми возможными
сочетаниями строк обеих таблиц.
Нужно получить все возможные пары городов и убрать
зеркальные варианты А-Б, Б-А

```sql
SELECT c.city, c2.city
FROM city c
CROSS JOIN city c2
WHERE c.city > c2.city;

=

SELECT c.city, c2.city
FROM city c, city c2
WHERE c.city > c2.city;
```

### UNION/EXCEPT

Если при работе с JOIN соединение данных происходит «слева» или «справа», то при работе с операторами UNION или EXCEPT работа происходит «сверху» и «снизу». Создадим две таблицы и внесем в них данные:

```sql
CREATE TABLE table_1 (color_1 VARCHAR(10) NOT NULL);

CREATE TABLE table_2 (color_2 VARCHAR(10) NOT NULL);

INSERT
 INTO
 table_1
VALUES('white'),('black'),('red'),('green');

INSERT
 INTO
 table_2
VALUES('black'),('yellow'),('blue'),('red');
```

<img src = "img/img29.png" width = 30%>

При объединении данных через оператор UNION в результате будет список уникальных значений для двух таблиц:

```sql
SELECT color_1 FROM table_1
UNION
SELECT color_2 FROM table_2;
```

Обязательное условие при работе с операторами **UNION** или **EXCEPT** — количество столбцов и их типы
данных в таблицах сверху и снизу должно быть одинаковым

<img src = "img/img30.png" width = 20%>

При объединении данных через оператор UNION ALL в результате будет список всех значений для двух таблиц:

```sql
SELECT color_1 FROM table_1
UNION ALL
SELECT color_2 FROM table_2;
```

### EXCEPT

При использовании оператора **EXCEPT** из значений, полученных в верхней части запроса, будут вычтены значения, которые совпадут со значениями, полученными в нижней части запроса.Запрос выполнен в **PostgreSQL**:

```sql
SELECT color_1 FROM table_1
EXCEPT
SELECT color_2 FROM table_2;
```

<img src = "img/img31.png" width = 10%>

Оператор **EXCEPT** не поддерживается **MySQL**, но можно такой жерезультат получить следующим запросом:

```sql
SELECT
 color_1
FROM
 table_1
WHERE
 color_1 NOT IN (
 SELECT
  color_2
 FROM
  table_2);
```

## Агрегатные функции

Агрегация — когда данные группируются по ключу, в качестве которого выступает один или несколько атрибутов, и внутри каждой группы вычисляются некоторые статистики.

- SUM — возвращает общую сумму числового столбца
- COUNT — возвращает количество строк, соответствующих заданному критерию
- AVG — возвращает среднее значение числового столбца
- MIN — возвращает наименьшее значение выбранного столбца
- MAX — возвращает наибольшее значение выбранного столбца

Посчитаем, сколько фильмов в базе начинается на букву А:

```sql
SELECT
 COUNT(1)
FROM
 film
WHERE
 LOWER(LEFT(title, 1)) = 'a';
```

<img src = "img/img32.png" width = 10%>

Так как функция COUNT возвращает количество строк, полученных в результате запроса, то аргументом можно передать любое значение, главное, чтобы оно соответствовало смыслу задачи

В одном запросе получим информацию по количеству платежей, общей сумме платежей, среднему платежу, максимальному и минимальному платежу по каждому пользователю:

```sql
SELECT customer_id, COUNT(payment_id), SUM(amount),
AVG(amount), MIN(amount), MAX(amount)
FROM payment
GROUP BY customer_id;
```
<img src = "img/img33.png" width = 70%>