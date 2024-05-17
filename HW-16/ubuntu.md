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
    ```console
      root@tms:/home/novik# mysql -u root -p clinic < clinic-dump.sql
   
      #проверяем список докторов
      mysql> USE clinic;
      mysql> SELECT * FROM Doctor
          -> ;
      +----+----------------+--------------------+---------------+-----------------------------+------------+
      | id | FirstName      | LastName           | Telephone     | Email                       | BirthDate  |
      +----+----------------+--------------------+---------------+-----------------------------+------------+
      |  1 | Андрей         | Быков              | +37529XXXXXXX | andrey.bykov@clinic.com     | 1966-06-22 |
      |  2 | Иван           | Купитман           | +37529XXXXXXX | ivan.kupitman@clinic.com    | 1963-03-13 |
      |  3 | Борис          | Левин              | +37529XXXXXXX | dmitry.levin@clinic.com     | 1986-01-15 |
      |  4 | Варвара        | Черноус            | +37529XXXXXXX | varvara.chernous@clinic.com | 1988-04-14 |
      |  5 | Глеб           | Романенко          | +37529XXXXXXX | gleb.romanenko@clinic.com   | 1984-09-19 |
      |  6 | Семён          | Лобанов            | +37529XXXXXXX | semen.lobanoff@clinic.com   | 1983-11-22 |
      +----+----------------+--------------------+---------------+-----------------------------+------------+
      6 rows in set (0.00 sec)
    ```
7. Установить MongoDB
   ```console
   apt install mongodb-server
   
   root@tms:/home/novik# systemctl status mongodb
   ● mongodb.service - An object/document-oriented database
        Loaded: loaded (/lib/systemd/system/mongodb.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2024-05-16 13:43:57 UTC; 2min 26s ago
          Docs: man:mongod(1)
      Main PID: 12606 (mongod)
         Tasks: 23 (limit: 2256)
        Memory: 42.4M
        CGroup: /system.slice/mongodb.service
                └─12606 /usr/bin/mongod --unixSocketPrefix=/run/mongodb --config /etc/mongodb.conf
   
   May 16 13:43:57 tms systemd[1]: Started An object/document-oriented database.
   root@tms:/home/novik#

   ```
8. Создать БД clinic и наполнить её данными используя скрипты из https://github.com/tms-dos21-onl/_sandbox/tree/main/lecture18/mongo/initdb.d.
   ```console
   use clinic

   > use clinic
   switched to db clinic
   > load("/home/novik/schema.js")
   true
   > load("/home/novik/data.js")
   true
   > show collections
   appointment
   department
   doctor
   job
   patient
   room

   ```
9. Написать выборочно 3 запроса из задания 5 для MongoDB используя mongosh команды.
   - Обновить дату приёма для пациента Ивана Иванова на 2022-02-09.
    ```
      clinic> db.appointment.updateOne(
      ...   { "Patient_id": 1 },
      ...   { $set: { "Date": "2022-02-09 00:00:00.000000" } }
      ... )
      {
        acknowledged: true,
        insertedId: null,
        matchedCount: 1,
        modifiedCount: 1,
        upsertedCount: 0
      } 
    ```
   - Удалить врача Андрея Быкова и все его приёмы.
    ```
    clinic> db.doctor.find()
   [
     {
       _id: ObjectId('661a4832456c9d825fef636d'),
       id: 1,
       Email: 'andrey.bykov@clinic.com',
       LastName: 'Быков',
       BirthDate: '1966-06-22',
       FirstName: 'Андрей',
       Telephone: '+37529XXXXXXX'
     }
    ]
   clinic> db.appointment.find()
   [
     {
       _id: ObjectId('661a4832456c9d825fef6380'),
       id: 1,
       Date: '2022-01-08 00:00:00.000000',
       Room_id: 5,
       Doctor_id: 1,
       Patient_id: 5
     }
   ]
    ```
   - Вывести всех врачей, работающих в терапевтическом отделении.
    ```
    clinic> db.job.aggregate([
      ...   { $match: { "Department_id": 1 } },
      ...   { $project: { "Doctor_id": 1, "_id": 0 } }
      ... ])
      [
        { Doctor_id: 1 },
        { Doctor_id: 4 },
        { Doctor_id: 5 },
        { Doctor_id: 6 }
      ]
    ```
