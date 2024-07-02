# a.	Реализуйте собственную службу для создания бэкапов домашней директории (home), по пути /back
# b.	Сервис должен выполняться автоматически каждый день в 12 часов, если это время пропущено то сразу после перезагрузки
# c.	Так же учтите что старые бэкапы старше 30 дней должны удаляться 
_______________

## Шаг 1: Создание скрипта для резервного копирования
Создадим скрипт `/usr/local/bin/backup_home.sh`, который будет выполнять резервное копирование домашней директории и удаление старых бэкапов.
```bash
vim /usr/local/bin/backup_home.sh
```
**Скрипт:**
```bash
#!/bin/bash

# Путь к директории для бэкапов
BACKUP_DIR="/back"

# Создание директории для бэкапов, если она не существует
mkdir -p $BACKUP_DIR

# Название архива с текущей датой
BACKUP_FILE="$BACKUP_DIR/home_backup_$(date +'%Y%m%d%H%M%S').tar.gz"

# Архивирование домашней директории
tar -czvf $BACKUP_FILE /home

# Удаление старых бэкапов старше 30 дней
find $BACKUP_DIR -type f -name "*.tar.gz" -mtime +30 -exec rm {} \;
```
---

**Сделаем скрипт исполняемым:**
```bash
chmod +x /usr/local/bin/backup_home.sh
```

## Шаг 2: Создание системной службы
**Создадим системную службу /etc/systemd/system/backup_home.service, которая будет запускать скрипт резервного копирования.**
```bash
vim /etc/systemd/system/backup_home.service
```
```bash
[Unit]
Description=Backup Home Directory
Wants=backup_home.timer
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup_home.sh
User=root

[Install]
WantedBy=multi-user.target
```
___

## Шаг 3: Создание таймера для системной службы
**Создадим таймер /etc/systemd/system/backup_home.timer, который будет запускать службу каждый день в 12 часов.**

**Создадим файл таймера: **
```bash
vim /etc/systemd/system/backup_home.timer
```
```bash
[Unit]
Description=Daily Backup Home Directory

[Timer]
OnCalendar=*-*-* 12:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

## Шаг 4: Запуск и активация таймера
**Активируем и запустим таймер для автоматического выполнения.**
```
systemctl daemon-reload
systemctl enable backup_home.timer
systemctl start backup_home.timer
```

## Шаг 5: Проверка состояния службы и таймера
**Проверим состояние службы и таймера, чтобы убедиться, что они работают правильно.**

***Проверка состояния таймера:***
```
systemctl status backup_home.timer
```

**Проверка состояния службы:**
```
systemctl status backup_home.service
```
