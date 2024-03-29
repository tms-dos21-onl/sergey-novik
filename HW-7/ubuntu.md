1. Познакомиться с веб-приложением
- backend
- frontend
2. Познакомиться с вариантами хостинга этого веб-приложения:
App-Hosting-Options
3. Установить веб-приложение (backend + frontend) на Linux VM и настроить запуск через SystemD  
  - Скачал оба проекта в домашний каталог:
    ```console
    git clone https://github.com/bezkoder/django-rest-api.git
    git clone https://github.com/bezkoder/react-crud-web-api.git
    ```

  - создал каталог для приложений:
    ```console
    mkdir /app
    chmod 777 /app
    ```
  
  - скопировал файлы DjangoRestApi `cp -R django-rest-api/DjangoRestApi/* /app`
  
  - установил:
    ```console
    apt install python3-pip
    pip install django==3.2.10
    pip install django-cors-headers djangorestframework
    ```
  
  - запустил и проверил в браузере на этой же тачке:
    ```console
    python3 manage.py runserver localhost:8080
    ```
  
  - добавил в конфиг DjangoRestApi/settings.py:
    ```bash
    DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
    ALLOWED_HOSTS = ['10.0.2.4','localhost']
    ```
  
  - сделал миграцию в базу `python3 manage.py migrate`;
  
  - проверил, увидел json.  
  
  - скопировал приложение React:
    ```console
    cp -R react-crud-web-api/* /app
    cd /app
    ```
  
  - установил `sudo apt install npm`;
  
  - запустил установку/сборку приложения `npm install`;
  - запустил `nmp start`, проверил в браузере с этого же хоста localhost:3000;
  - изменил порт:
    ```console
    export PORT=8081
    ```
  - изменил порт в  `package.json`:
    ```bash
    "start": "export PORT=8081 && react-scripts start",
    ```

    ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/14410c45-4546-49e2-9961-76cc0226401a)

4. Установить веб-приложение (backend + frontend) на Linux VM и настроить запуск через SystemD
   
  - создал пользователей backend и frontend
  - изменил права:
    ```console
    chown backend:backend db.sqlite3
    chown frontend:frontend ./node_modules
    ```
  - файл SystemD для backend `nano /etc/systemd/system/backend.service`:
    ```bash
    [Unit]
    Description=Unit for starting a backend app
    
    [Service]
    Restart=on-failure
    WorkingDirectory=/app
    PreExecStart=python3 manage.py migrate
    ExecStart=python3 manage.py runserver localhost:8080
    User=backend
    
    [Install]
    WantedBy=multi-user.target
    ```
 - из под пользователя backend установил:
   ```console
   pip install django==3.2.10
   pip install django-cors-headers djangorestframework
   ```
 - запустил и проверил работу службы `backend.service`
   ```console
   root@ubuntu-tms:/app# systemctl status backend.service
   ● backend.service - Unit for starting a backend app
         Loaded: loaded (/etc/systemd/system/backend.service; enabled; vendor preset: enabled)
         Active: active (running) since Thu 2024-03-28 13:57:08 UTC; 1s ago
       Main PID: 8365 (python3)
          Tasks: 2 (limit: 2212)
         Memory: 65.1M
            CPU: 717ms
         CGroup: /system.slice/backend.service
                 ├─8365 python3 manage.py runserver localhost:8080
                 └─8372 /usr/bin/python3 manage.py runserver localhost:8080
    
    Mar 28 13:57:08 ubuntu-tms systemd[1]: Started Unit for starting a backend app.

   ```
- файл SystemD для frontend `nano /etc/systemd/system/frontend.service`:
  ```bash
  [Unit]
  Description=Unit for starting a frontend app via Webpack DevServer
  
  [Service]
  Restart=on-failure
  WorkingDirectory=/app
  ExecStart=npm start
  User=frontend
  
  [Install]
  WantedBy=multi-user.target
  ```
- проверил, работает!

