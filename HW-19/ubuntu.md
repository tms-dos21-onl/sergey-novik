1. Установить Docker на хостовую ОС. В случае с Windows использовать WSL backend.
    ```
    - WSL у меня уже включен, скачал и установил Docker Desktop Installer.exe, перезагрузил pc.
    ```
2. Убедиться, что Docker работает исправно путем запуска контейнера hello-world.
    ```
    - поиском нашел образ hello-world и запустил
    ```
    ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/52a5018a-a65d-4d42-81c1-4458a1b94cfc)

3. Установить Nginx используя Docker образ
    ```
    - в PS: docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx
    - через вкладку Exec убедился в работе nginx
    ```

4. Изучить следующие команды и их флаги:
- docker run (-i, -t, -d, --rm)
  ```
    -d - запускает контейнер в фоновом режиме;
    -t - прикрепляет к контейнеру псевдо-TTY-консоль;
    -i - выводит в терминал STDIN поток контейнера;
    --rm - удаляет контейнер после завершения его работы;
  ```   
- docker ps (-a, -l, -q)
    ```
    Для просмотра списка контейнеров используется команда docker ps. Она позволяет смотреть как запущенные контейнеры Docker, так и все контейнеры, которые есть в системе.
    ```
- docker images
    ```
    Команда 'docker images' выводит список образов верхнего уровня (top-level images). Фактически, ничего особенного не отличает образ от слоя для чтения. Только те образы, которые имеют присоединенные контейнеры или те, что были получены с помощью pull, считаются      образами верхнего уровня. Это различие нужно для удобства, так как за каждым образом верхнего уровня может быть множество слоев.
    ```
- docker start/docker stop/docker restart
    ```
    - управоение контейнерами
    docker start [OPTIONS] CONTAINER
    ```
- docker exec
    ```
     - взаимодействие с системой внутри контейнерв, часто используют с параметрами -it;
    ``` 
5. Установить Nexus используя Docker образ
    ```console
    docker run -d -p 8081:8081 --name nexus sonatype/nexus3
    curl http://localhost:8081/ #для проверки в консоли
    ```
        
6. Установить Jenkins используя Docker образ
    ```console
    docker pull jenkins/jenkins
    docker run -p 8080:8080 jenkins/jenkins

    # проверил в браузере http://localhost:8080
    ```
