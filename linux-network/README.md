# Настройка локальной сети

Создание двух виртуальных машин (Ubuntu) и локальной сети с выходом в интернет через NAT. Настройка DHCP и кэширующего DNS-сервера.

## Что внутри

ВМ 1 (server): NAT-адаптер (выход в интернет) + Internal Network (локальная сеть);
ВМ 2 (server2): Internal Network (клиент).

Сервисы на первой ВМ:
- `isc-dhcp-server` - раздаёт IP-адреса второй машине;
- `bind9` - кэширующий DNS-сервер;
- `iptables` + `sysctl` - маршрутизация и NAT (MASQUERADE).

## Шаги выполнения

1. Установка двух ВМ с Ubuntu Server 25.04.
2. Настройка сетевых адаптеров в VirtualBox:
   - `server`: Адаптер 1 - NAT, Адаптер 2 - Internal Network;
   - `server2`: Адаптер 1 - Internal Network.
3. Настройка работы с текстовым редактором (Vim).
4. Установка и настройка DHCP-сервера (`isc-dhcp-server`).
5. Установка и настройка DNS-сервера (`bind9`).
6. Включение маршрутизации и NAT.

## Проверка

1. На `server2`: `sudo dhclient -v enp0s3` - должен получить IP из диапазона `192.168.56.150-200`.
2. На `server2`: `ping www.ru` - проверка работы DNS (имя разрешается).
3. На `server2`: `ping www.wildberries.ru` - проверка выхода в интернет через NAT.

## Конфигурации

- [dhcpd.conf](configs/dhcpd.conf) - настройка DHCP (диапазон, шлюз, DNS);
- [named.conf.options](configs/named.conf.options) - форвардеры DNS;
- [sysctl.conf](configs/sysctl.conf) - включение пересылки пакетов (`net.ipv4.ip_forward=1`).

## Применение конфигураций

После копирования файлов на сервер необходимо применить настройки:

```bash
# Применить настройки ядра
sudo sysctl -p

# Перезапустить DHCP и DNS
sudo systemctl restart isc-dhcp-server
sudo systemctl restart bind9

# Сохранить правила iptables (NAT)
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

## Стек

- **Ubuntu Server 25.04** (VirtualBox);
- **Сеть:** `isc-dhcp-server`, `bind9`, `iptables`, `sysctl`.
