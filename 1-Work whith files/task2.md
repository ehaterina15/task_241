1. Что такое потоки (streams)
   
stdin	Standard Input	Ввод данных в программу
stdout	Standard Output	Обычный вывод программы
stderr	Standard Error	Вывод сообщений об ошибках

stdin — клавиатура

stdout — экран

stderr — экран (но отдельный поток)

2. Операторы > и >>
> — перезапись файла

Перенаправляет вывод в файл, удаляя старое содержимое.
```
echo Hello > file.txt
```
Если файл не существует — он будет создан.

>> — добавление в файл

Добавляет вывод в конец файла.
```
echo World >> file.txt
```
3. Перенаправление ввода (stdin)

Перенаправление ввода позволяет передать файлу роль клавиатуры.
```
program < input.txt

```
```
sort < numbers.txt
```

Программа sort читает данные из файла, а не с клавиатуры.

4. stdout и stderr — в чём разница
stdout	stderr
Обычный результат работы	Ошибки и диагностика
Поток 1	Поток 2
Можно перенаправлять отдельно	Не смешивается со stdout
```
command > out.txt
```

Ошибки не попадут в файл.

5. Перенаправление stdout и stderr
Только stdout
```
command > out.txt
```
Только stderr
```
command 2> error.txt
```
stdout + stderr в один файл
```
command > all.txt 2>&1
```
6. Перенаправление stdout в stderr и наоборот
   
stdout → stderr
```
command 1>&2
```
stderr → stdout
```
command 2>&1
```
7. Примеры с kinit, ping, tracert
ping — stdout в файл, stderr в другой
```
ping google.com > ping_out.txt 2> ping_err.txt
```
ping — весь вывод в один файл
```
ping google.com > ping_all.txt 2>&1
```
tracert — stderr в stdout
```
tracert google.com 2>&1
```
kinit — stdout → stderr
```
kinit user@REALM 1>&2
```
8. Вывести содержимое файла без текстовых редакторов

```
cat file.txt
```
9. Создать файл с содержимым без текстового редактора
Через echo
```
echo Hello world > file.txt
```
Несколько строк
```
echo Line 1 > file.txt
echo Line 2 >> file.txt
```
here-document
```
cat << EOF > file.txt
```
First line
Second line
EOF

10. Что такое stdin

stdin — это источник данных для программы.

Источники stdin:

клавиатура

файл (<)

вывод другой команды (|)
```
echo test | sort
```
11. Как отправить весь вывод команды «в пустоту»
```
command > /dev/null 2>&1
```



Используется для:

подавления логов

фоновых скриптов

автоматизации
