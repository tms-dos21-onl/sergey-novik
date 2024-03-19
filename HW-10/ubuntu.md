1. Добавить новый диск к виртуальной машине и проверить, что система видит его.
    ```console
    root@ubuntu-tms-lvm:/home/novik# lsblk
    NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    loop0                       7:0    0  53.3M  1 loop /snap/snapd/19457
    loop1                       7:1    0  63.4M  1 loop /snap/core20/1974
    loop2                       7:2    0 111.9M  1 loop /snap/lxd/24322
    sda                         8:0    0    10G  0 disk
    ├─sda1                      8:1    0     1M  0 part
    ├─sda2                      8:2    0   1.8G  0 part /boot
    └─sda3                      8:3    0   8.2G  0 part
      └─ubuntu--vg-ubuntu--lv 253:0    0   8.2G  0 lvm  /
    sdb                         8:16   0     1G  0 disk
    sr0                        11:0    1  1024M  0 rom

    ```
2. Вывести в консоль информацию по текущему размеру файловой системы.
    ```console
    root@ubuntu-tms-lvm:/home/novik# df -h
    Filesystem                         Size  Used Avail Use% Mounted on
    tmpfs                               96M  1.1M   95M   2% /run
    /dev/mapper/ubuntu--vg-ubuntu--lv  8.1G  4.3G  3.4G  56% /
    tmpfs                              479M     0  479M   0% /dev/shm
    tmpfs                              5.0M     0  5.0M   0% /run/lock
    /dev/sda2                          1.7G  129M  1.5G   8% /boot
    tmpfs                               96M  4.0K   96M   1% /run/user/1000

    ```
3. Расширить корневую файловую систему за счёт добавленного диска.

4. Вывести информацию по новому размеру файловой системы.

5. Вывести в консоль текущую рабочую директорию.
6. Вывести в консоль все файлы из домашней директории.
7. Построить маршрут до google.com при помощи утилиты traceroute.
8. Установить Sonatype Nexus OSS по следующей инструкции, а именно:
- установку произвести в директорию /opt/nexus.
- запустить приложение от отдельного пользователя nexus.
- реализовать systemd оболочку для запуска приложения как сервис.
9. Создать в Nexus proxy репозиторий для пакетов ОС и разрешить анонимный доступ.
10. Поменять для текущей VM основной репозиторий пакетов на созданный ранее proxy в Nexus.
11. Выполнить установку пакета snap и убедиться, что на proxy репозитории в Nexus появились пакеты.
12. (**) На основании шагов из предыдущих пунктов создать DEB/RPM пакет для установки Nexus и загрузить его в Nexus.
