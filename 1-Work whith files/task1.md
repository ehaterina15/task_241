
# Работа в консоли 

# 1. Создание папки с подпапками
```
mkdir -p main_folder/sub_folder
```
# 2. Переход в папку
```
cd main_folder || exit
```
# 3. Вывод списка файлов в директории
```
ls
```
# 4. Вывод списка всех файлов (включая скрытые)
```
ls -a
```

# 5. Создание файла и запись текста в него
```
echo "Пример текста" > file.txt
```
# 6. Перемещение файла в подпапку
```
mv file.txt sub_folder/
```
# 7. Копирование файла из одной директории в другую
```
cp sub_folder/file.txt .
```
# 8. Переименование файла
```
mv file.txt new_file.txt
```
# 9. Сравнение содержимого файлов
```
diff new_file.txt sub_folder/file.txt
```
# 10. Сортировка содержимого файла
# по возрастанию
```
sort new_file.txt
```
# по убыванию
```
sort -r new_file.txt
```
# 11. Переход назад и удаление всех папок и файлов
```
cd ..
rm -r main_folder
```
<img width="646" height="434" alt="image" src="https://github.com/user-attachments/assets/06637369-5139-4ea1-8b74-1450df2c4c31" />
