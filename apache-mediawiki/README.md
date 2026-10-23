# Настройка web-сервера

Организация работы web-сервера Apache, установка PHP, MySQL и развертывание MediaWiki.

## Что внутри

Настройка веб-сервера (LAMP) на первой виртуальной машине (server):
- **Apache** - веб-сервер;
- **PHP** - язык программирования и модули;
- **MySQL** - база данных;
- **MediaWiki** - тестовый сайт (вики).

## Шаги выполнения

1. Установка Apache: `sudo apt-get install apache2`. Открытие портов 80 и 443.
2. Настройка DNS: создание зоны `example.org` для привязки имени `lamp` к IP сервера.
3. Настройка DHCP: добавление `option domain-name "example.org"` для автоматического подставления суффикса.
4. Установка PHP и модулей: `sudo apt install php php-mysql php-{gd,zip,mbstring,curl,pear} php-intl`.
5. Установка MySQL: `sudo apt-get install mysql-server mysql-client`. Настройка `mysql_secure_installation`.
6. Скачивание и распаковка MediaWiki в `/var/www/html/mediawiki`.
7. Создание базы данных `wiki` в MySQL.
8. Прохождение мастера установки MediaWiki через браузер (с использованием проброса портов VirtualBox).
9. Перенос сгенерированного файла `LocalSettings.php` в папку с MediaWiki.
10. Перезапуск Apache.

## Проверка

1. HTTP-запрос к локальному серверу с клиентской ВМ: 
   `curl http://lamp` (должна отобразиться страница-заглушка или MediaWiki).
2. Проверка доступности установщика: 
   `curl http://lamp/mediawiki/` (должна отобразиться страница установки).
3. Авторизация в готовой MediaWiki через браузер (хост: `http://127.0.0.1:8080/mediawiki/`) и создание тестовой страницы.

## Конфигурации

- [000-default.conf](configs/000-default.conf) - настройка виртуального хоста Apache (разрешение `.htaccess` для MediaWiki);
- [example.org](configs/example.org) - файл зоны DNS (запись `lamp`).

## Стек

- **Ubuntu Server 25.04** (VirtualBox);
- **Веб-сервер:** `apache2`;
- **Язык/модули:** `php`, `php-mysql`, `php-intl`;
- **СУБД:** `mysql-server`;
- **CMS:** `mediawiki`.
