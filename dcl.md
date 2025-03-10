# DСL

Data Control Language (DCL) – группа операторов определения доступа к данным. Эти операторы нужны для управления разрешениями доступа к данным и выполнения операций над объектами базы данных. Права назначаются на пользователя или на роли.

В эту группу входят следующие операторы:

- GRANT – предоставляет пользователю или группе разрешения на определенные операции с объектами
- REVOKE – отзывает выданные разрешения
- DENY – задает запрет, имеющий приоритет над разрешением, отсутствует в MySQL

----

## DCL. GRANT

- Дать все права пользователю к базе данных и, если его не существует, то создает его, до версии 8.0:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

- Создать пользователя и дать все права к базе данных, после версии 8.0:

```sql
CREATE USER 'dbuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'localhost';
```

Дать определённые права пользователю:

```sql
GRANT SELECT, UPDATE ON `dvd-rental`.* TO 'dbuser'@'localhost'
```

----

## DCL. REVOKE

- Убрать все права пользователя к базе данных:

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'dbuser'@'localhost';
```

- Удалить пользователя после того, как убрали права:

```sql
DROP USER 'dbuser'@'localhost';
```

Убрать определённые права у пользователя:

```sql
REVOKE INSERT, DELETE ON `dvd-rental`.* FROM 'dbuser'@'localhost'
```

----

DCL. DENY

- В MySQL отсутствует оператор DENY, все права назначаются через GRANT и удаляются через REVOKE.
На примере других СУБД синтаксис будет выглядеть следующим образом:

```sql
DENY privilege_name ON object_name TO {user_name | public | role_name};
```
