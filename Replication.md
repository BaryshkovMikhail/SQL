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
