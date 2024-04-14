1. Установить MySQL на VM.
   - установка `apt install mysql-server`;
   - пароль для рута mysql `mysqladmin password -u root -p`;
   - запускаем скрипт первичной настройки `sudo mysql_secure_installation`;
2. (**) Настроить Multi-Primary репликацию для MySQL на 2 VM согласно инструкции.
   - установил mysql на втором сервере;
   - создать UUID на обоих серверах `> SELECT UUID();`
      ```bash
      ee2d9ef8-eddf-11ee-84ad-08002761efec #server1
      21092191-ede0-11ee-9360-08002730f799 #server2
      ```
   - настройка конфигов на обоих серверах `nano /etc/mysql/my.cnf`
      ```bash
      #server1

     [mysqld]
   
     server_id=1
     bind-address=0.0.0.0
     gtid_mode=ON
     enforce_gtid_consistency=ON
     binlog_checksum=NONE
   
     plugin_load_add='group_replication.so'
     group_replication_single_primary_mode=OFF
     loose-group_replication_group_name="ee2d9ef8-eddf-11ee-84ad-08002761efec"
     loose-group_replication_start_on_boot=OFF
     loose-group_replication_local_address= "10.0.2.4:33061"
     loose-group_replication_group_seeds="10.0.2.4:33061, 10.0.2.8:33061"
     loose-group_replication_bootstrap_group=OFF
     report_host=10.0.2.4
      ```
      
3. Создать схему БД clinic и наполнить её данными используя скрипты из https://github.com/tms-dos21-onl/_sandbox/tree/main/lecture18/mysql/initdb.d/data.
4. Создать бэкап базы данных clinic.
5. Написать следующие SQL запросы:
- Вывести всех врачей, работающих в терапевтическом отделении.
- Вывести в каких отделениях побывал каждый пациент.
- Обновить дату приёма для пациента Ивана Иванова на 2022-02-09.
- Удалить врача Андрея Быкова и все его приёмы.
- Добавить нового врача Фила Ричардса и новую пациентку Василису Васильеву и записать её к Филу Ричардсу на приём на 2022-02-14.
6. Восстановить базу данных clinic из бэкапа и проверить, что данные соответствуют состоянию базы данных до внесенных в предыдущем задании изменений.
7. Установить MongoDB
8. Создать БД clinic и наполнить её данными используя скрипты из https://github.com/tms-dos21-onl/_sandbox/tree/main/lecture18/mongo/initdb.d.
9. Написать выборочно 3 запроса из задания 5 для MongoDB используя mongosh команды.