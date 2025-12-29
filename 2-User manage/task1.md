#!/bin/bash

# =========================
# Управление пользователями (Linux)
# =========================

# 1. Добавление пользователей
# 1.1 user1 с оболочкой bash
sudo useradd -m -s /bin/bash user1

# 1.2 user2 с оболочкой sh
sudo useradd -m -s /bin/sh user2

# 1.3 Установка паролей
sudo passwd user1
sudo passwd user2

# 2. Назначение групп
# Добавить user1 в группу администраторов (sudo)
sudo usermod -aG sudo user1

# Добавить user2 в группу user1
sudo usermod -aG user1 user2

# 3. Права доступа
# Права доступа — это разрешения на чтение (r), запись (w) и выполнение (x)
# Вывод прав доступа в домашней директории пользователя
ls -l /home/user1

# 4. Изменение прав доступа
# Создание файла с полными правами для всех пользователей
touch /home/user1/all_rights.txt
chmod 777 /home/user1/all_rights.txt

# 5. Учётная запись встроенного администратора
# В Linux встроенный администратор — root

# 6. Выполнение команды от имени администратора
sudo ls /root

# 7. Ограничения суперпользователя
# Ограничений по правам нет, но можно повредить систему

# 8. Удаление пользователя user2 с помощью user1
# (user1 должен быть в группе sudo)
sudo userdel -r user2

# 9. Изменение владельца папки
# Изменение владельца файла из пункта 4
sudo chown user1:user1 /home/user1/all_rights.txt
![photo_2025-12-29_23-40-27](https://github.com/user-attachments/assets/f2015f71-9ded-469b-a3ec-09294069a824)

