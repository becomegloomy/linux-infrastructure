# Настройка сервера баз данных

Организация работы сервера баз данных MySQL и предоставление удаленного доступа к базе данных.

## Что внутри

Настройка сервера БД на первой виртуальной машине (server) и подключение к нему с клиентской машины (server2).

- установка MySQL Server и настройка безопасности;
- создание базы данных, таблиц и пользователей;
- разграничение прав доступа (`grant`);
- открытие удаленного доступа к СУБД;
- подключение с клиентской машины.

## Шаги выполнения

1. Установка MySQL: `sudo apt install mysql-server mysql-client`.
2. Защита сервера: `sudo mysql_secure_installation` (удаление анонимных пользователей, тестовой БД, отключение удаленного root).
3. Создание базы данных `sakharova` и таблиц `sharetable` и `privtable`.
4. Создание пользователей `share_user` и `private_user` с паролями.
5. Выдача прав:
   - `share_user` - полный доступ к `sharetable`;
   - `private_user` - только чтение `privtable`.
6. Открытие внешнего доступа в `/etc/mysql/mysql.conf.d/mysqld.cnf` (смена `bind-address` на `0.0.0.0`).
7. Перезапуск MySQL: `sudo systemctl restart mysql`.
8. Установка клиента на второй ВМ: `sudo apt install mysql-client`.

## Проверка

1. Подключение с клиентской ВМ от имени `share_user`:
   `mysql -u share_user -h 192.168.56.2 -p`
2. Проверка прав `share_user`: 
   `show tables;` (видна только `sharetable`) и `insert into sharetable (scol) values ('testdata');` (успешно).
3. Подключение от имени `private_user`: 
   `mysql -u private_user -h 192.168.56.2 -p`
4. Проверка ограничений `private_user`: 
   `insert into privtable (pcol) values ('test2');` (должна вернуть ошибку `ERROR 1142 (42000): INSERT command denied`).

## Конфигурация

В файле `/etc/mysql/mysql.conf.d/mysqld.cnf` была изменена директива `bind-address`.

Полный фрагмент измененных строк находится в [configs/mysqld.cnf](configs/mysqld.cnf).

## Стек

- **Ubuntu Server 25.04** (VirtualBox);
- **СУБД:** `mysql-server`, `mysql-client`.
