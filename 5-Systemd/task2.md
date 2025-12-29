![photo_2025-12-29_23-34-40](https://github.com/user-attachments/assets/03f9446f-76f6-4560-b868-2b111f509b10)
#!/bin/bash

# =========================================================
# Systemd: пользователь, сервис, таймер и скрипт
# =========================================================

# ---------------------------------------------------------
# 1. Создание пользователя для выполнения скрипта
# ---------------------------------------------------------
# По умолчанию systemd-юниты выполняются от root,
# поэтому создаётся отдельный пользователь.

sudo useradd -m info_user
sudo passwd info_user

# Проверка существования пользователя
id info_user

# ---------------------------------------------------------
# 2. Скрипт, выполняющий задание
# ---------------------------------------------------------
# Скрипт:
# - всегда работает в домашнем каталоге пользователя
# - создаёт папку systemd_info
# - создаёт файлы 1–4
# - записывает:
#   1 — текущую дату
#   2 — версию ядра
#   3 — имя компьютера
#   4 — список файлов домашнего каталога
# - содержит проверки на существование

sudo tee /usr/local/bin/info_script.sh > /dev/null <<'EOF'
#!/bin/bash

set -euo pipefail

# Переход в домашний каталог пользователя,
# от имени которого выполняется скрипт
cd "$HOME"

DIR="systemd_info"

# Создание директории, если её нет
if [ ! -d "$DIR" ]; then
    mkdir "$DIR"
fi

DATE_FILE="$DIR/1"
KERNEL_FILE="$DIR/2"
HOST_FILE="$DIR/3"
HOME_LIST_FILE="$DIR/4"

# Текущая дата
if [ ! -f "$DATE_FILE" ]; then
    date > "$DATE_FILE"
fi

# Версия ядра
if [ ! -f "$KERNEL_FILE" ]; then
    uname -r > "$KERNEL_FILE"
fi

# Имя компьютера
if [ ! -f "$HOST_FILE" ]; then
    hostname > "$HOST_FILE"
fi

# Список файлов домашнего каталога
if [ ! -f "$HOME_LIST_FILE" ]; then
    ls "$HOME" > "$HOME_LIST_FILE"
fi
EOF

# Права на выполнение
sudo chmod +x /usr/local/bin/info_script.sh

# Проверка ручного запуска от имени пользователя
sudo -u info_user /usr/local/bin/info_script.sh

# ---------------------------------------------------------
# 3. Systemd service
# ---------------------------------------------------------
# Юнит выполняет скрипт от имени info_user

sudo tee /etc/systemd/system/info_script.service > /dev/null <<'EOF'
[Unit]
Description=Create system info files in user home directory

[Service]
Type=oneshot
ExecStart=/usr/local/bin/info_script.sh
User=info_user
Group=info_user

[Install]
WantedBy=multi-user.target
EOF

# ---------------------------------------------------------
# 4. Systemd timer (каждые 5 минут)
# ---------------------------------------------------------
# Таймер вызывает одноимённый service-юнит

sudo tee /etc/systemd/system/info_script.timer > /dev/null <<'EOF'
[Unit]
Description=Run info_script every 5 minutes

[Timer]
OnBootSec=5min
OnUnitActiveSec=5min
Unit=info_script.service

[Install]
WantedBy=timers.target
EOF

# ---------------------------------------------------------
# 5. Активация systemd
# ---------------------------------------------------------
sudo systemctl daemon-reload

# Запуск сервиса вручную
sudo systemctl start info_script.service
sudo systemctl status info_script.service

# Включение и запуск таймера
sudo systemctl enable --now info_script.timer

# Проверка таймеров
systemctl list-timers

# ---------------------------------------------------------
# 6. Проверка результата
# ---------------------------------------------------------
sudo ls /home/info_user/systemd_info
sudo cat /home/info_user/systemd_info/1
sudo cat /home/info_user/systemd_info/2

# =========================================================
# Конец файла
# =========================================================
