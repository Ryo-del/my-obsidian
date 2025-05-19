```markdown
# Настройка DNS-сервера

## Установка BIND9 (Linux)
```bash
sudo apt update && sudo apt install bind9 -y
```

## Базовая конфигурация
Файл `/etc/bind/named.conf.options`:
```
options {
    directory "/var/cache/bind";
    forwarders { 8.8.8.8; 1.1.1.1; };
    dnssec-validation auto;
    listen-on { any; };
    allow-query { any; };
};
```

## Перезапуск службы
```bash
sudo systemctl restart bind9
sudo systemctl enable bind9
```

## Проверка работы
```bash
nslookup google.com 127.0.0.1
dig @localhost ya.ru
```

## Создание своей зоны
1. Добавить в `/etc/bind/named.conf.local`:
```
zone "mydomain.local" {
    type master;
    file "/etc/bind/db.mydomain.local";
};
```

2. Создать файл зоны `/etc/bind/db.mydomain.local`:
```
$TTL 86400
@ IN SOA ns1.mydomain.local. admin.mydomain.local. (
    2024052001 ; Serial
    3600       ; Refresh
    1800       ; Retry
    604800     ; Expire
    86400 )    ; Minimum TTL

@       IN NS  ns1.mydomain.local.
ns1     IN A   192.168.1.10
server  IN A   192.168.1.20
```

## Проверка зоны
```bash
sudo named-checkzone mydomain.local /etc/bind/db.mydomain.local
sudo systemctl restart bind9
```

## Готово!
Теперь можно использовать свой DNS:
```bash
nslookup server.mydomain.local 127.0.0.1
```