# DDL

Data Definition Language (DDL) — это группа операторов определения данных. С помощью этих операторов определяется структура базы данных и происходит работа с объектами базы данных.

В эту группу входят следующие операторы:

- CREATE — используется для создания объектов базы данных
- ALTER — используется для изменения объектов базы данных
- DROP — используется для удаления объектов базы данных

----

## DDL. CREATE

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

## DDL. ALTER

- Изменение имени базы данных:

```sql
ALTER DATABASE old_name MODIFY NAME = new_name;
```

 синтаксис T-SQL, не работает в MySQL

- Изменение имени таблицы:

```sql
ALTER TABLE customer RENAME TO persons;
```

- Изменение столбцов в таблице:

```sql
ALTER TABLE persons ADD password CHAR(20) NOT NULL;
```

- Изменение типа данных атрибута:

```sql
ALTER TABLE persons MODIFY fio VARCHAR(100);
```

## DDL. DROP

- Удалить базу данных:

```sql
DROP DATABASE `dvd-rental`;
```

- Удалить таблицу:

```sql
DROP TABLE persons;
```

- Удалить индекс:

```sql
DROP INDEX dob_idx ON persons;
```

- Удалить представление:

```sql
DROP VIEW underage;
```
