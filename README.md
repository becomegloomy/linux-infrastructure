# linux-infrastructure

Работы по администрированию Linux: настройка локальной сети, DHCP, DNS, SSH, Samba, MySQL, Apache и почтового сервера (Postfix + Dovecot). Все работы выполнялись на виртуальных машинах Ubuntu Server (VirtualBox), конфигурации вынесены в отдельные файлы.

## Что внутри

- **`linux-network/`** - две виртуальные машины, NAT + Internal Network, DHCP-сервер (`isc-dhcp-server`), кэширующий DNS (`bind9`), маршрутизация и NAT через `iptables`;
- **`ssh/`** - SSH-сервер: смена порта, отключение входа root, вход по ключу, отключение пароля;
- **`samba/`** - файловый сервер Samba: общие папки (read-only, read-write), разграничение доступа по пользователям (`user1`, `user2`), привязка к DNS-имени;
- **`mysql/`** - сервер БД MySQL: создание базы, таблиц, пользователей, выдача прав (`grant`), удалённый доступ (`bind-address = 0.0.0.0`), подключение с клиентской машины;
- **`apache-mediawiki/`** - веб-сервер Apache + PHP + MySQL (LAMP), установка MediaWiki, настройка DNS-имени `lamp` и DHCP-суффикса `example.org`;
- **`postfix-dovecot/`** - почтовый сервер Postfix (SMTP) + Dovecot (IMAP/POP3), создание пользователей, настройка клиента `mutt`, отправка и получение писем.

## Стек

- **Ubuntu Server 25.04** (VirtualBox);
- **Сеть:** `isc-dhcp-server`, `bind9`, `iptables`, `sysctl`;
- **Удалённый доступ:** `openssh-server`;
- **Файловый сервер:** `samba`;
- **Базы данных:** `mysql-server`, `mysql-client`;
- **Веб:** `apache2`, `php`, `php-mysql`, `mediawiki`;
- **Почта:** `postfix`, `dovecot-core`, `dovecot-imapd`, `dovecot-pop3d`, `mutt`.

## Требования

- **VirtualBox** (или другая система виртуализации);
- **ISO-образ Ubuntu Server** (25.04 или новее);
- **Две виртуальные машины** (одна - сервер, вторая - клиент), соединённые через Internal Network;
- **Права root / sudo** на обеих машинах.
## Лицензия

Код и конфигурации свободны для использования и модификации.
