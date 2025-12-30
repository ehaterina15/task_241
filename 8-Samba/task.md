Установка пакета Samba

Samba — это программный пакет, реализующий протокол SMB/CIFS и предназначенный для организации общего доступа к файлам и каталогам в локальной сети между различными пользователями и операционными системами.

Установка пакета Samba:
```
sudo apt update
sudo apt install samba
```
Что такое общая папка и зачем она может быть нужна

Общая папка — это каталог, доступ к которому могут получать несколько пользователей или компьютеров по сети.
Общие папки используются для совместной работы с файлами, обмена данными, централизованного хранения документов, а также для предоставления контролируемого доступа пользователям в локальной сети.

Создание общей папки без пароля с правами только на чтение

Создадим каталог:
```
sudo mkdir /srv/samba/public_read
sudo chmod 755 /srv/samba/public_read
```

Добавим конфигурацию в файл /etc/samba/smb.conf:
```
[public_read]
    path = /srv/samba/public_read
    browsable = yes
    guest ok = yes
    read only = yes
```

Перезапустим службу Samba:
```
sudo systemctl restart smbd
```
Создание общей папки с паролем и правами на чтение и запись

Создадим каталог:
```
sudo mkdir /srv/samba/private_rw
sudo chmod 770 /srv/samba/private_rw
```

Создадим пользователя и зададим пароль Samba:
```
sudo useradd sambauser
sudo smbpasswd -a sambauser

```
Добавим конфигурацию:
```
[private_rw]
    path = /srv/samba/private_rw
    browsable = yes
    valid users = sambauser
    read only = no
```

Перезапустим службу:
```
sudo systemctl restart smbd
```
Создание общей папки с доступом для группы с полными правами

Создадим группу и каталог:
```
sudo groupadd fullgroup
sudo mkdir /srv/samba/group_full
sudo chown :fullgroup /srv/samba/group_full
sudo chmod 770 /srv/samba/group_full
```

Добавим пользователя в группу:

sudo usermod -aG fullgroup sambauser


Добавим конфигурацию Samba:
```
[group_full]
    path = /srv/samba/group_full
    browsable = yes
    valid users = @fullgroup
    read only = no

```
Перезапустим Samba:
```
sudo systemctl restart smbd
```
Создание общей папки с разными уровнями доступа для групп

Создадим группы и каталог:
```
sudo groupadd rwgroup
sudo groupadd rogroup
sudo groupadd nogroup

sudo mkdir /srv/samba/mixed_access
sudo chown :rwgroup /srv/samba/mixed_access
sudo chmod 770 /srv/samba/mixed_access
```

Настроим ACL для группы с доступом только на чтение и запретим доступ остальным:
```
sudo setfacl -m g:rogroup:rx /srv/samba/mixed_access
sudo setfacl -m o::--- /srv/samba/mixed_access
```

Добавим конфигурацию Samba:
```
[mixed_access]
    path = /srv/samba/mixed_access
    browsable = yes
    valid users = @rwgroup, @rogroup
    write list = @rwgroup
    read list = @rogroup
```

Перезапустим службу Samba:
```
sudo systemctl restart smbd
```
![photo_2025-12-30_10-48-45](https://github.com/user-attachments/assets/51471785-cbed-4555-8c7b-88775919864f)
![photo_2025-12-30_10-48-51](https://github.com/user-attachments/assets/1406ce2f-a179-4354-a691-c5d80eabf4fd)
![photo_2025-12-30_10-48-56](https://github.com/user-attachments/assets/be953cc7-c55e-468c-8ca9-a10a86310565)
![photo_2025-12-30_10-49-00](https://github.com/user-attachments/assets/3f3c43c3-8a48-4d6b-852f-f1ab3eeef250)
![photo_2025-12-30_10-49-07](https://github.com/user-attachments/assets/059f84a4-d1b9-4d4d-9aff-37e331372b5e)
