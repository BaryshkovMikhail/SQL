# Репликация

Создадим контейнер с именем replication-master и зададим пароль root пользователя 123456

```bash
docker run -d --name replication-master -e MYSQL_ROOT_PASSWORD=123456 -p 3307:3306 mysql:8.3
```

Создадим контейнер с именем replication-slave и зададим пароль root пользователя 123456

```bash
docker run -d --name replication-slave -e MYSQL_ROOT_PASSWORD=123456 -p 3308:3306 mysql:8.3
```

Для реализации взаимодействия создадим мост и сеть:

```bash
docker network create replication
docker network connect replication replication-master
docker network connect replication replication-slave
```

Создадим учетную запись master для сервера репликации:

```bash
docker exec -it replication-master mysql -u root -p
```

В контейнере выполним:

```bash
mysql> CREATE USER 'replication'@'%';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'replication'@'%';
```

Проверим права пользователя:

```bash
SHOW GRANTS FOR replication@'%';
```

Настройка компонентов

С помощью редактора файлов вносим изменения папка etc в файл my.cnf-> секция [mysqld] добавляем следующие параметры:

```text
server_id = 1
log_bin = mysql-bin
```

При изменении конфигурации сервера требуется перезагрузка:

```bash
docker restart replication-master
```

После требуется зайти в контейнер и проверить состояние:

```bash
docker exec -it replication-master mysql -u root -p
mysql> SHOW MASTER STATUS;
```

Значениия File и Position меняются каждый раз.

<img src = "img/img50.png" width = 60%>

**!!! Важно запомнить Значения ячеек File и Position**


Master готов,переходим к slave:

```bash
docker run --name replication-slave -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8.3
```

Открываем конфигурационный файл на Slave my.cnf.
my.cnf-> секция [mysqld] добавляем следующие параметры:

```text
log_bin = mysql-bin
server_id = 2
relay-log = /var/lib/mysql/mysql-relay-bin
relay-log-index = /var/lib/mysql/mysql-relay-bin.index
read_only = 1
```

Перезагружаем slave:

```bash
docker restart replication-slave
```

Следующим шагом требуется прописать в базе данных на сервер **slave**,кто является master и данные **File** и **Position**,
полученные а результате выполнения команды **SHOW MASTER STATUS;** на мастере:

```shell
docker exec -it replication-slave mysql -u root -p
mysql> CHANGE MASTER TO
MASTER_HOST='replication-master',
MASTER_USER='replication',
MASTER_LOG_FILE='mysql-bin.000001',
MASTER_LOG_POS=156;
```

Далее запускаем журнал ретрансляции и проверим статус операций:

```sql
mysql> START REPLICA IO_THREAD;
mysql> SHOW SLAVE STATUS\G
```