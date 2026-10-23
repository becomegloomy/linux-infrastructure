# Установка почтового сервера Postfix

Организация работы почтового сервера Postfix и настройка почтового клиента.

## Что внутри

Настройка связки почтовых серверов на первой виртуальной машине (server):
- **Postfix** - SMTP-сервер (отправка и приём почты);
- **Dovecot** - сервер IMAP/POP3 (хранение и доступ к почте);
- **Mutt** - консольный почтовый клиент.

## Шаги выполнения

1. Установка Postfix: `sudo apt-get install postfix -y`. Выбор типа конфигурации: **Internet Site**.
2. Указание системного почтового имени: `mail.example.org`.
3. Установка Dovecot (IMAP, POP3, LMTP): `sudo apt install dovecot-core dovecot-imapd dovecot-lmtpd dovecot-pop3d`.
4. Включение протоколов в `/etc/dovecot/dovecot.conf`: `protocols = imap pop3`.
5. Создание пользователей системы (`sudo adduser user3`) и инициализация их почтовых ящиков (`sudo doveadm user '*'`).
6. Перезапуск Dovecot: `sudo systemctl restart dovecot`.
7. Установка клиента `mutt`.
8. Настройка клиента в `~/.muttrc` (IMAP и SMTP для `user3`).

## Проверка

1. Вход в почтовый клиент под `user3`: `mutt` (должен открыться пустой ящик).
2. Отправка письма `user4` через меню `mutt` (клавиша `m`, заполнение темы и текста, `y` для отправки).
3. На сервере: `sudo -u user4 mutt` — проверка получения письма.
4. Ответ на письмо от `user4` и проверка его получения на клиенте `user3`.
5. Пометка письма на удаление (клавиша `d`) и очистка ящика при выходе (`q`).

## Конфигурации

- [main.cf](configs/main.cf) - основные настройки Postfix (доменное имя, локальные сети, путь к ящикам);
- [dovecot.conf](configs/dovecot.conf) - включение протоколов `imap` и `pop3`.

## Стек

- **Ubuntu Server 25.04** (VirtualBox);
- **SMTP:** `postfix`;
- **IMAP/POP3:** `dovecot-core`, `dovecot-imapd`, `dovecot-pop3d`;
- **Почтовый клиент:** `mutt`.
