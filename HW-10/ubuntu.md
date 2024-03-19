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
    ```console
    pvdisplay         #посмотрел что на уровне PV
    pvcreate /dev/sdb #создал новый PV
    pvdisplay         #чекнул еще раз
    vgdisplay         #посмотрел что на уровне VG
    vgextend ubuntu-vg /dev/sdb # расширил существующий VG 'ubuntu-vg' за счет PV '/dev/sdb'
    lvdisplay         #посмотрел название существующего LV
    lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv #расширил LV
    lsblk             # посмотрел
    df -h             # посмотрел
    resize2fs /dev/ubuntu-vg/ubuntu-lv # расширил файловую систему
    df -h             # посмотрел, готово
    ```
4. Вывести информацию по новому размеру файловой системы.
    ```console
    root@ubuntu-tms-lvm:/home/novik# df -h
    Filesystem                         Size  Used Avail Use% Mounted on
    tmpfs                               96M  1.1M   95M   2% /run
    /dev/mapper/ubuntu--vg-ubuntu--lv  9.0G  4.3G  4.3G  50% /
    tmpfs                              479M     0  479M   0% /dev/shm
    tmpfs                              5.0M     0  5.0M   0% /run/lock
    /dev/sda2                          1.7G  129M  1.5G   8% /boot
    tmpfs                               96M  4.0K   96M   1% /run/user/1000

    ```
5. Вывести в консоль текущую рабочую директорию.
    ```console
    novik@ubuntu-tms-lvm:~$ pwd
    /home/novik

    ```
6. Вывести в консоль все файлы из домашней директории.
    ```console
    novik@ubuntu-tms-lvm:~$ ls -la ~
    total 36
    drwxr-x--- 4 novik novik 4096 Mar 19 09:48 .
    drwxr-xr-x 3 root  root  4096 Mar 19 09:34 ..
    -rw------- 1 novik novik   13 Mar 19 09:45 .bash_history
    -rw-r--r-- 1 novik novik  220 Jan  6  2022 .bash_logout
    -rw-r--r-- 1 novik novik 3771 Jan  6  2022 .bashrc
    drwx------ 2 novik novik 4096 Mar 19 09:36 .cache
    -rw-r--r-- 1 novik novik  807 Jan  6  2022 .profile
    drwx------ 2 novik novik 4096 Mar 19 09:34 .ssh
    -rw-r--r-- 1 novik novik    0 Mar 19 09:38 .sudo_as_admin_successful
    -rw------- 1 novik novik   60 Mar 19 09:48 .Xauthority
    novik@ubuntu-tms-lvm:~$

    ```
7. Построить маршрут до google.com при помощи утилиты traceroute.
    ```console
    root@ubuntu-tms-lvm:/home/novik# apt install traceroute
    root@ubuntu-tms-lvm:/home/novik# traceroute google.com
    traceroute to google.com (142.250.203.142), 30 hops max, 60 byte packets
     1  unifi.devxed (192.168.1.1)  0.329 ms  0.315 ms  0.314 ms
     2  mm-128-154-57-86.static.canopy.mgts.by (86.57.154.128)  2.336 ms  3.777 ms  3.430 ms
     3  mm-49-80-84-93.dynamic.pppoe.mgts.by (93.84.80.49)  4.115 ms mm-53-80-84-93.dynamic.pppoe.mgts.by (93.84.80.53)  4.358 ms  1.787 ms
     4  core2.net.belpak.by (93.85.80.57)  2.302 ms core1.net.belpak.by (93.85.80.45)  4.703 ms  3.196 ms
     5  ie1.net.belpak.by (93.85.80.50)  8.780 ms  8.438 ms ie1.net.belpak.by (93.85.80.38)  3.847 ms
     6  asbr9.net.belpak.by (93.85.80.238)  2.698 ms  2.249 ms  1.772 ms
     7  74.125.146.96 (74.125.146.96)  25.357 ms  10.168 ms  9.833 ms
     8  216.239.42.179 (216.239.42.179)  11.717 ms  11.799 ms 192.178.99.185 (192.178.99.185)  9.589 ms
     9  209.85.253.225 (209.85.253.225)  10.692 ms  10.136 ms  9.704 ms
    10  waw07s06-in-f14.1e100.net (142.250.203.142)  10.141 ms  9.797 ms  9.453 ms
    root@ubuntu-tms-lvm:/home/novik#


    ```
8. Установить Sonatype Nexus OSS по следующей инструкции, а именно:
- установку произвести в директорию /opt/nexus.

- запустить приложение от отдельного пользователя nexus.

- реализовать systemd оболочку для запуска приложения как сервис.

9. Создать в Nexus proxy репозиторий для пакетов ОС и разрешить анонимный доступ.

10. Поменять для текущей VM основной репозиторий пакетов на созданный ранее proxy в Nexus.

11. Выполнить установку пакета snap и убедиться, что на proxy репозитории в Nexus появились пакеты.

12. (**) На основании шагов из предыдущих пунктов создать DEB/RPM пакет для установки Nexus и загрузить его в Nexus.

