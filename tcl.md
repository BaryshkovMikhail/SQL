# TCL

Transaction Control Language (TCL) – группа операторов для управления транзакциями.

Эта группа состоит из следующих операторов:

- START TRANSACTION – служит для определения начала транзакции
- COMMIT – применяет транзакцию
- ROLLBACK – откатывает все изменения, сделанные в контексте текущей транзакции
- SAVEPOINT – устанавливает промежуточную точку сохранения внутри транзакции

----

## TCL Пример транзакции для MySQL

- начало транзакции

```sql
START TRANSACTION;
```

- обновляем данные

```sql
UPDATE accounts SET balance = balance - 1000.00 WHERE id = 10;
```

- ставим точку сохранения

```sql
SAVEPOINT my_savepoint;
```

- обновляем данные

```sql
UPDATE accounts SET balance = balance + 1000.00 WHERE id = 27;
```

- допустили ошибку, возвращаемся к my_savepoint

```sql
ROLLBACK TO my_savepoint;
```

- теперь правильно обновляем данные

```sql
UPDATE accounts SET balance = balance + 1000.00 WHERE id = 37;
```

- завершаем транзакцию

```sql
COMMIT;
```
