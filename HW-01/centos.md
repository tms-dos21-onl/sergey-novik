1. Произвести минимальную настройку (время, локаль, custom motd)  
   - Текущие настройки времи и даты команда timedatectl  
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/bdcf6095-2c74-46df-bc04-631ac70080ed)

   - вывести список часовых поясов timedatectl list-timezones
   - установить timedatectl set-timezone <часовой_пояс>
   - изменить дату и время (предварительно отключить NTP синхронизацию) timedatectl -- set-time "<дата> <время>"  
     пример: timedatectl -- set-time "2002-08-28 00:00:00"
   - отдельно дату timedatectl -- set-time "2002-08-28"
   - отдельно время timedatectl -- set-time "15:30:00"
   - custom motd это приветствие при входе, создаем файл nano /etc/motd, переподключаем ssh
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/5fa47f7b-45dc-4079-9b97-d918ac4a8d84)

2. Определить точную версию ядра.
   -  hostnamectl, uname -a  
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/334c09d7-de32-4186-8524-ab55f19c517c)

3. Вывести список модулей ядра и записать в файл
   - все модули записаны в папке /lib/modules/. Учитывая, что модули рассчитаны только для определенной версии ядра, то в этой папке создается отдельная подпапка, для каждой установленной в системе версии ядра. В этой папке находятся сами модули и дополнительные конфигурационные файлы, модули отсортированы по категориям, в зависимости от назначения например: ls /lib/modules/5.4.0.45-generic/kernel/  
   - lsmod - посмотреть загруженные модули
   - все модули записаны в конфигурационном файле /lib/modules/modules.aliases, поэтому мы можем просто посмотреть его содержимое: modprobe -c
   - записать все модули в файл modprobe -c >> modules-kernel.txt  

4. Просмотреть информацию о процессоре и модулях оперативной памяти  
   - информация о процессоре cat /proc/cpuinfo или lscpu  
   - информация о обьеме ОЗУ  free -h (-h ключ указывает показать в Гб) или cat /proc/meminfo  
   - информация о производителе ОЗУ dmidecode
     
5. Получить информацию о жестком диске  
   - информацию о всех дисках fdisk -l
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/6459813a-a6e9-44b9-8612-2f585d4a7a97)  

   - найти все диски ls -l /dev | grep sd  
   - полезные команды: lsblk, df -H  
   - работа с разделами в удобном интерфейсе cfdisk  


6. Добавить в виртуальную машину второй сетевой интерфейс (вывести информацию о нем в виртуалках)
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/d182dfd6-6249-4e51-90d8-d66069b5efde)
    
7. (**) Узнать полную информацию об использованной и неиспользованной памяти  
   - команда free -h, top
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/c129ec81-4df0-4107-b091-ce730c11d9c9)  

   - вывести список процессов и отсортировать их ps aux --sort -rss  
   - можно установить аналоги диспетчера задач windows: htop, nmon
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/f314faa3-46c1-406f-8b23-f32bcfbe373d)
      
9. (**) Создать пользователя new_admin_user, Настроить ssh доступ пользователю по ключу на VM, запретить ему авторизацию по паролю  
    - создать пользователя (в этот момент не создается его домашний каталог): useradd new_admin_user  
    - создать пользователя и домашний каталог useradd -m new_admin_user  
    - создать домашний каталог для существующего пользователя mkhomedir_helper new_admin_user  
    - создаем пользователю каталог .ssh и меняем владельца: chown new_admin_user:new_admin_user .ssh/  
    - создаем приватный и публичный ключ ssh-keygen  
    - копируем pub ключ пользователю cp id_rsa.pub /home/new_admin_user/.ssh/authorized_keys  
    - копируем приватный ключ себе и подключаемся с помощью ключа по ssh  
    - желательно отключить пользователю вход по паролю. В файл /etc/ssh/sshd_config добавляем строчки:
      >Match User new_admin_user  
      >PasswordAuthentication no
    - перезагружаем ssh: systemctl restart sshd  
      
10. (**) Вывести список файловых систем, которые поддерживаются ядром
    -  cat /proc/filesystems покажет вам, какие файловые системы может поддерживать ваше работающее ядро прямо сейчас.
![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/00a5846e-1720-4edf-8390-c8dea33cff18)  

    -  ls /lib/modules/$(uname -r)/kernel/fs предоставит подсказки относительно того, какие дополнительные файловые системы оно могло бы поддерживать, если вы загрузили соответствующий модуль
      ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/4bace541-1cd9-4456-89c2-90c2be87ab04)
          
** и *** не обязательны к выполнению. Задачи на интерес  

