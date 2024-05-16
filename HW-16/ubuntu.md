1. Установить MySQL на VM.
   - установка `apt install mysql-server`;
   - пароль для рута mysql `mysqladmin password -u root -p`;
   - запускаем скрипт первичной настройки `sudo mysql_secure_installation`;
2. (**) Настроить Multi-Primary репликацию для MySQL на 2 VM согласно инструкции.
      
3. Создать схему БД clinic и наполнить её данными используя скрипты из https://github.com/tms-dos21-onl/_sandbox/tree/main/lecture18/mysql/initdb.d/data.
   ```console
   mysql> source /home/novik/schema.sql;
   mysql> source /home/novik/data.sql;
   
   mysql> show databases;
   +--------------------+
   | Database           |
   +--------------------+
   | clinic             |
   | information_schema |
   | mysql              |
   | performance_schema |
   | sys                |
   +--------------------+
   5 rows in set (0.00 sec)
   ```

4. Создать бэкап базы данных clinic.
   ```console
   root@tms:/home/novik# mysqldump -u root -p clinic > clinic-dump.sql
   ```

5. Написать следующие SQL запросы:
- Вывести всех врачей, работающих в терапевтическом отделении.
   ```console
   SELECT FirstName, LastName, dep.Name
   FROM Job job 
   JOIN Department dep ON job.Department_id = dep.id AND dep.Name = 'Терапевтический' 
   JOIN Doctor doc ON job.Doctor_id = doc.id;
   ```
- Вывести в каких отделениях побывал каждый пациент.
   ```console
   SELECT pat.FirstName, pat.LastName, dep.Name AS Department, r.Number AS RoomNumber
   FROM Appointment a
   JOIN Patient pat ON a.Patient_id = pat.id
   JOIN Room r ON a.Room_id = r.id
   JOIN Department dep ON r.Department_id = dep.id;
   ```
- Обновить дату приёма для пациента Ивана Иванова на 2022-02-09.
   ```console
   UPDATE Appointment
   SET Date = '2022-02-09'
   WHERE Patient_id = (
   SELECT id
   FROM Patient
   WHERE FirstName = 'Иван'
   AND LastName = 'Иванов'
   );
   ```
- Удалить врача Андрея Быкова и все его приёмы.
   ```console
   DELETE FROM Job WHERE Doctor_id IN (SELECT id FROM Doctor WHERE FirstName = 'Андрей' AND LastName = 'Быков');
   DELETE FROM Appointment WHERE Doctor_id IN (SELECT id FROM Doctor WHERE FirstName = 'Андрей' AND LastName = 'Быков');
   DELETE FROM Doctor WHERE FirstName = 'Андрей' AND LastName = 'Быков';
   ```

- Добавить нового врача Фила Ричардса и новую пациентку Василису Васильеву и записать её к Филу Ричардсу на приём на 2022-02-14.
  ```console
   INSERT INTO Doctor (id, FirstName, LastName, BirthDate, Telephone, Email)
   VALUES (1, 'Фил', 'Ричардс', '1990-05-01', '+37529XXXXXXX', 'fil@clinic.by');
   
   
   INSERT INTO Patient (id, FirstName, LastName, BirthDate, Address, Telephone, Email)
   VALUES (6, 'Василиса', 'Васильева', '2001-01-01', 'Minsk', '+37533XXXXXXX', 'vasilisa@google.com');
   
   INSERT INTO Appointment (id, Patient_id, Doctor_id, Date, Room_id)
   VALUES (
   1,
   (SELECT id FROM Patient WHERE Patient.FirstName = 'Василиса' AND Patient.LastName = 'Васильева'),
   (SELECT id FROM Doctor WHERE Doctor.FirstName = 'Фил' AND Doctor.LastName = 'Ричардс'),
   '2022-02-14',
   '5'
   );
  ```
6. Восстановить базу данных clinic из бэкапа и проверить, что данные соответствуют состоянию базы данных до внесенных в предыдущем задании изменений.
7. Установить MongoDB
8. Создать БД clinic и наполнить её данными используя скрипты из https://github.com/tms-dos21-onl/_sandbox/tree/main/lecture18/mongo/initdb.d.
9. Написать выборочно 3 запроса из задания 5 для MongoDB используя mongosh команды.
