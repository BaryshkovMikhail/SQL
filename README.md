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

## DDL

Data Definition Language (DDL) — это группа операторов определения данных. С помощью этих операторов определяется структура базы данных и происходит работа с объектами базы данных.

В эту группу входят следующие операторы:

- CREATE — используется для создания объектов базы данных
- ALTER — используется для изменения объектов базы данных
- DROP — используется для удаления объектов базы данных

### DDL. CREATE

- Создание базы данных:

```sql
CREATE DATABASE `dvd-rental`;
```

- Создание таблицы:

```sql
CREATE TABLE customer(id SERIAL PRIMARY KEY, fio CHAR(50) UNIQUE, dob DATE);
```

- Создание представления:

```sql
CREATE VIEW underage AS SELECT fio FROM customer WHERE TIMESTAMPDIFF(YEAR, dob, 
CURDATE()) >= 18;
```

- Создание индекса:

```sql
CREATE UNIQUE INDEX dob_idx ON customer(dob);
```

