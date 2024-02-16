1. Познакомиться с утилитой Midnight Commander, установить её на VM и узнать с помощью неё все папки верхнего уровня файловой системы.

   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/f391950a-e513-4c1e-bbfa-0457a045ceff)

2. Установить PowerShell на VM и проверить, что он работаёт путем выполнения каких-нибудь простейших команд.  

   ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/028425f8-193d-4325-b390-b3ab4b4c8d67)  
  
3. Создать простейший bash-скрипт sysinfo.sh, который собирает данные о:
 - количестве свободной оперативной памяти
 - текущей загрузке процессора
 - текущем IP адресе(ах)
 и выводит их в формате ключ: значение, причем все ключи заменить на русские названия. Например, чтобы вместо «Mem: 1024Mb» выводилось «Память: 1024Мб». Для написания скрипта рекомендуется использовать утилиты awk, grep и 
 sed.

```bash
#!/bin/bash

mem="$(free -m | awk '/Mem/ {print $4}')"
echo 'Свободная ОЗУ:' $mem'Мб'

cpu="$(mpstat | awk '/all/{print $NF}')"
echo 'Процессор свободен на: '$cpu'%'

lan="$(hostname -I)"
echo 'IP адрес: '$lan
```
результат:  

![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/3e0309a6-e926-4c2d-b1b2-61049f1a2321)
  
4. (**) Cоздать файл immortalfile, запретить его удаление даже пользователем root и попытаться его удалить из под root, результатом должно быть “Operation not permitted”. Подсказка: CHATTR(1).

```console
root@ubuntu-tms:/home/novik# touch immortalfile
root@ubuntu-tms:/home/novik# ls -l
total 0
-rw-r--r-- 1 root root 0 Feb 16 14:11 immortalfile
root@ubuntu-tms:/home/novik# lsattr
--------------e------- ./immortalfile
root@ubuntu-tms:/home/novik# chattr +i immortalfile
root@ubuntu-tms:/home/novik# lsattr
----i---------e------- ./immortalfile
root@ubuntu-tms:/home/novik# rm immortalfile
rm: cannot remove 'immortalfile': Operation not permitted
root@ubuntu-tms:/home/novik#

```
   
6. (***) Выполнить команду и разобраться, что она делает и что сохраняется в file.log

env -i bash -x -l -c 'echo hello_there!' > file.log 2>&1

** и *** не обязательны к выполнению. Задачи на интерес
