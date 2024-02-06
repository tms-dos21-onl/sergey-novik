Дедлайн: 14/02/2024

1. Создать новый жёсткий диск, присоеденить его к VM, создать раздел (partition) и инициализировать на нём файловую системую.
   - создаем раздел утилитой fdisk:
     > fdisk /dev/sdb
   - вводим n, далее указываем сектора и тд. Для выхода из fdisk и применения изменений вводим w.  
   - форматируем раздел в ext4:
     > mkfs.ext4 /dev/sdb1
     
2. Смонтировать директорию /mnt/home на только что созданный раздел.  
   - создаем каталог home в /mnt  
   - монтируем в /mnt/home. В файл /etc/fstab добавляем строчку:
     > /dev/sdb1 /mnt/home ext4 defaults 0 0
   - применяем настройки командой:  
     > mount -a
   
   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/aeb88d91-f891-4bc1-894e-76ec877e9d94)
       
     
3. Создать нового пользователя penguin с home-директорией /mnt/home/penguin.  
   - команда `useradd` ключ `-m` создает сразу home-директорию, `-d` - путь.  
     > useradd penguin -m -d /mnt/home/penguin
     
4. Создать новую группу пользователей birds, перенести в нее пользователя penguin.  
   - создаем группу:  
     > groupadd birds
   - добавляем penguin в birds:
     > usermod -a -G birds penguin
   
   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/3b565d1a-7ae4-4da8-ad82-33892a767096)  
     
5. Cоздать директорию /var/wintering и выдать права на нее только группе birds.  
   - меняем группу доступа для каталога `chgrp birds wintering/`  
   - устанвливаем права `chmod 070 wintering/`
   
   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/c2fe224f-1b49-4c66-8c56-90c142117460)  


   
6. Установить ntpd (или chrony) и разрешить пользователю penguin выполнять команду systemctl restart chronyd (нужны права sudo).
   - устанавливаем ntpd:
     > apt install ntp

   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/69b2cbdc-555c-47ff-b7b3-a897a80d7adc)
   
   - задание можно выполить по разному, я просто дам права sudo пользователю penguin:
   
   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/2b631bdf-fcab-4dc6-a43d-67cb33cd850e)

     
   
7. (**) Вывод команды iostat -x в последней колонке показывает загрузку дисков в процентах. Откуда утилита это понимает? Достаточно ли вывода команды iostat -x для того, чтобы оценить реальную нагрузку на диски, или нужны дополнительные условия или ключи?  
   - utilization = ( (read requests + write requests) * service time in ms / 1000 ms ) * 100% или %util = ( r + w ) * svctim /10  
   - iostat -x показывает нагрузку с момента включения, для анализа производительности в данный момент нужно указывать интервал опроса, например:  
     > iostat -x 2 (собирает данные каждые 2 секунды)  


9. (***) Подумать, что сделает команда chmod -x $(which chmod). Выполнить её на виртуальной машине и вернуть всё как было не прибегая к скачиванию\копированию chmod с другого хоста.  
   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/3249a1ea-faf3-4c2a-94bd-7640e30a10e7)

   - в переменную `$(which chmod)` записан путь к файлу(команде) chmod. После выполнения chmod -x $(which chmod) у файла chmod не будет прав на запуск, соответственно команду chmod нельзя будет запустить.
   - решил эту задачу путем подмены содержимого файла:  
   
   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/05d95e2c-cfc9-417c-aab3-3c7fd13ee623)
   
     
** и *** не обязательны к выполнению. Задачи на интерес
