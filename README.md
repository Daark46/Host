# Домашнее задание к занятию «Защита хоста»

Инструкция по выполнению домашнего задания

Сделайте fork репозитория c шаблоном решения к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).

Выполните клонирование этого репозитория к себе на ПК с помощью команды git clone.

Выполните домашнее задание и заполните у себя локально этот файл README.md:

впишите вверху название занятия и ваши фамилию и имя;

в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;

для корректного добавления скриншотов воспользуйтесь инструкцией «Как вставить скриншот в шаблон с решением»;

при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в инструкции по MarkDown.

После завершения работы над домашним заданием сделайте коммит (git commit -m "comment") и отправьте его на Github (git push origin).

Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.

Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

### Задание 1

Установите eCryptfs.

Добавьте пользователя cryptouser.

Зашифруйте домашний каталог пользователя с помощью eCryptfs.

В качестве ответа пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.

### Ответ 1

1. Установите eCryptfs
```
sudo apt install ecryptfs-utils  lsof

sudo modprobe ecryptfs
```
2. Добавьте пользователя cryptouser

```
sudo adduser cryptouser

sudo usermod -aG sudo cryptouser
```

3. Зашифруйте домашний каталог пользователя с помощью eCryptfs

```
sudo ecryptfs-migrate-home -u cryptouser
sudo ls /home/
su cryptouser
cd ~
ecryptfs-unwrap-passphrase
sudo ecryptfs-setup-swap
sudo rm -Rf /home/cryptouser.7LC6DOKx/
sudo ls /home/cryptouser/

sudo cat /home/cryptouser/Access-Your-Private-Data.desktop
[Desktop Entry]
_Name=Access Your Private Data
_GenericName=Access Your Private Data
Exec=/usr/bin/ecryptfs-mount-private
Terminal=true
Type=Application
Categories=System;Security;
X-Ubuntu-Gettext-Domain=ecryptfs-utils

sudo cat /home/cryptouser/README.txt
THIS DIRECTORY HAS BEEN UNMOUNTED TO PROTECT YOUR DATA.

From the graphical desktop, click on:
 "Access Your Private Data"

or

From the command line, run:
 ecryptfs-mount-private
```
![alt text](https://github.com/Daark46/Host/blob/main/1.png)

### Задание 2

Установите поддержку LUKS.

Создайте небольшой раздел, например, 100 Мб.

Зашифруйте созданный раздел с помощью LUKS.

В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.

### Ответ 2

1. Установите поддержку LUKS
```
sudo apt install cryptsetup 

cryptsetup --version
```

2. Создайте небольшой раздел, например, 100 Мб.
```
lsblk
sudo fdisk /dev/sdb
n
p
i
w
lsblk
```

3. Зашифруйте созданный раздел с помощью LUKS.

```
sudo cryptsetup -y -v --type luks2 luksFormat /dev/sdb1
```
![alt text](https://github.com/Daark46/Host/blob/main/2.png)

```
sudo cryptsetup luksOpen /dev/sdb1 disk
ls /dev/mapper/disk
```
![alt text](https://github.com/Daark46/Host/blob/main/3.png)

```
sudo dd if=/dev/zero of=/dev/mapper/disk
sudo mkfs.ext4 /dev/mapper/disk
```
![alt text](https://github.com/Daark46/Host/blob/main/4.png)

```
mkdir .secret
sudo mount /dev/mapper/disk .secret/
```
![alt text](https://github.com/Daark46/Host/blob/main/5.png)

```
sudo umount .secret
sudo cryptsetup luksClose disk
```
![alt text](https://github.com/Daark46/Host/blob/main/6.png)
![alt text](https://github.com/Daark46/Host/blob/main/7.png)
