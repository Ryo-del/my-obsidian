`systemd` — стандартная система управления службами в Linux (например, веб-сервер, база данных и т.д.).

### 🔹 Основные команды

|Команда|Что делает|Пример|
|---|---|---|
|`systemctl start`|Запускает сервис|`sudo systemctl start nginx`|
|`systemctl stop`|Останавливает сервис|`sudo systemctl stop apache2`|
|`systemctl restart`|Перезапускает сервис|`sudo systemctl restart mysql`|
|`systemctl status`|Показывает статус|`systemctl status sshd`|
|`systemctl enable`|Включает автозапуск|`sudo systemctl enable docker`|
|`systemctl disable`|Выключает автозапуск|`sudo systemctl disable postgresql`|

### 🔹 Пример: управление Nginx

bash

# Проверим статус
sudo systemctl status nginx

# Если сервис не работает — запустим
sudo systemctl start nginx

# Добавим в автозагрузку (чтобы запускался при старте системы)
sudo systemctl enable nginx

### 🔹 Как посмотреть все сервисы?

bash

systemctl list-units --type=service  # Все активные сервисы
systemctl list-unit-files           # Все доступные сервисы

---

# **2. Планирование задач через `cron`**

`cron` — это «расписание» для Linux. Он запускает команды или скрипты **в нужное время**.

### 🔹 Основы синтаксиса

Каждая задача в `cron` выглядит так:

* * * * * команда
│ │ │ │ │
│ │ │ │ └── День недели (0-7, 0=воскресенье)
│ │ │ └──── Месяц (1-12)
│ │ └────── День месяца (1-31)
│ └──────── Час (0-23)
└────────── Минута (0-59)

### 🔹 Примеры расписаний

| Расписание    | Когда выполнится          | Пример команды                           |
| ------------- | ------------------------- | ---------------------------------------- |
| `* * * * *`   | Каждую минуту             | `echo "Проверка" >> /tmp/log.txt`        |
| `*/5 * * * *` | Каждые 5 минут            | `python3 /scripts/backup.py`             |
| `0 3 * * *`   | Каждый день в 3:00        | `/bin/bash /home/user/nightly_job.sh`    |
| `0 0 1 * *`   | 1-го числа каждого месяца | `tar -czf /backups/monthly.tar.gz /data` |

### 🔹 Как редактировать расписание?

bash

crontab -e  # Открыть редактор (добавляем свои задачи)
crontab -l  # Показать текущие задачи

### 🔹 Пример: бэкап каждую ночь

1. Создаём скрипт `/home/user/backup.sh`:
    
    bash
    

- #!/bin/bash
    tar -czf /backups/daily_$(date +%d-%m-%Y).tar.gz /home/user
    
- Даём права на выполнение:
    
    bash
    
- chmod +x /home/user/backup.sh
    
- Добавляем в `cron`:
    
    bash
    

crontab -e

Добавляем строку:

1. 0 2 * * * /home/user/backup.sh  # Каждый день в 2:00 ночи
    

---

# **3. Полезные фишки**

### 🔸 Проверка логов `cron`

Если задача не сработала, смотрим логи:

bash

grep CRON /var/log/syslog  # Ubuntu/Debian
journalctl -u cron         # Systemd-журнал

### 🔸 Перезагрузка `cron` после изменений

bash

sudo systemctl restart cron  # Обновить конфигурацию

### 🔸 Запуск скрипта при перезагрузке сервера

Через `@reboot` в `crontab`:

@reboot /path/to/script.sh

---

# **4. Практика**

### 🎯 Задание 1

- Останови сервис `nginx`, проверь его статус, затем снова запусти.
    
- Добавь его в автозагрузку.
    

**Решение**:

bash

sudo systemctl stop nginx
systemctl status nginx       # Должен быть "inactive"
sudo systemctl start nginx
sudo systemctl enable nginx

### 🎯 Задание 2

- Напиши `cron`-задачу, которая каждые 10 минут записывает текущее время в файл `/tmp/time.log`.
    

**Решение**:

bash

crontab -e

Добавляем:

*/10 * * * * date >> /tmp/time.log

---

# **Итог**

- **`systemd`** — для управления сервисами (старт/стоп, автозагрузка).
    
- **`cron`** — для запуска задач по расписанию.
    
- **Логи** — чтобы находить проблемы.