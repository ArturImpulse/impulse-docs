# Работа с базой данных в Docker (Impulse API)

## PhpMyAdmin (веб-интерфейс)

Доступ:

    http://localhost:8081/

------------------------------------------------------------------------

## 📌 Создание новой базы данных

Выполните команду:

``` bash
docker exec -it impulse-api-database-1 mysql -uroot -ppassword -e "CREATE DATABASE IF NOT EXISTS impulsnewdb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
```

------------------------------------------------------------------------

## 📌 Чтобы база появилась в PhpMyAdmin

Добавляем права пользователю `impulse`:

``` bash
docker exec -it impulse-api-database-1 mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON impulsnewdb.* TO 'impulse'@'%'; FLUSH PRIVILEGES;"
```

------------------------------------------------------------------------

## 📌 Просмотр списка всех баз данных

``` bash
docker exec -it impulse-api-database-1 mysql -uroot -ppassword -e "SHOW DATABASES;"
```
