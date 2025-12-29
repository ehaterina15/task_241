#!/bin/bash

# Перенаправление ввода и вывода (Linux)

# 1. Как работают > и >>
# >  — перезаписывает файл
# >> — дописывает в конец файла
echo "Первая строка" > redirect.txt
echo "Вторая строка" >> redirect.txt

# 2. Перенаправление ввода и потоков
# stdin  — стандартный ввод (0)
# stdout — стандартный вывод (1)
# stderr — поток ошибок (2)

# 3. Вывести содержимое файла без текстовых редакторов
cat redirect.txt

# 4. Создать файл с содержимым без текстовых редакторов
echo "Текст внутри файла" > created.txt

# 5. Перенаправление stdout и stderr
# stdout -> stderr
ping -c 1 google.com 1>&2

# stderr -> stdout
ping -c 1 invalid_host 2>&1

# Пример с kinit (если команда есть в системе)
kinit invalid_user 2> kinit_error.txt
kinit invalid_user > kinit_out.txt 2>&1

# 6. Отличие stdout и stderr
# stdout — обычный вывод команды
# stderr — сообщения об ошибках

# 7. stdin
# stdin — данные, которые команда получает на вход
cat < created.txt

# 8. Отправить весь вывод команды в пустоту
ping -c 1 google.com > /dev/null 2>&1
![1](https://github.com/user-attachments/assets/4224c741-8960-4981-86f1-47646c79aac5)

