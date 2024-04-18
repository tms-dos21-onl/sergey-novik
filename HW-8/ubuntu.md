1. Склонировать текущий репозиторий <FIRSTNAME>-<LASTNAME> (например, ivan-ivanov) на локальную машину.
    ```
    git clone  https://github.com/tms-dos21-onl/sergey-novik.git  
    ```
2. Вывести список всех удаленных репозиториев для локального.
    ```console
    novik@w-adm:~/sergey-novik$ git remote -v
    origin  https://github.com/tms-dos21-onl/sergey-novik.git (fetch)
    origin  https://github.com/tms-dos21-onl/sergey-novik.git (push)
    ```
3. Вывести список всех веток.
    ```console
    novik@w-adm:~/sergey-novik$ git branch -a
    * main
    remotes/origin/HEAD -> origin/main
    remotes/origin/main
    ```
4. Вывести последние 3 коммитa с помощью git log.
5. Создать пустой файл README.md и сделать коммит.
6. Добавить фразу "Hello, DevOps" в README.md файл и сделать коммит.
7. Сделать реверт последнего коммита. Вывести последние 3 коммитa с помощью git log.
8. Удалить последние 3 коммита с помощью git reset.
9. Вернуть коммит, где добавляется пустой файл README.md. Для этого найти ID коммита в git reflog, а затем сделать cherry-pick.
10. Удалить последний коммит с помощью git reset.
11. Переключиться на ветку main или master. Если ветка называется master, то переименовать её в main.
12. Скопировать файл https://github.com/tms-dos21-onl/_sandbox/blob/main/.github/workflows/validate-shell.yaml, положить его по такому же относительному пути в репозиторий. Создать коммит и запушить его в удаленный репозиторий.
13. Создать из ветки main ветку develop. Переключиться на неё и создать README.md в корне репозитория. Написать в этом файле какие инструменты DevOps вам знакомы и с какими вы бы хотели познакомиться больше всего (2-3 пункта). Сделать коммит.

> ⚠️ Для выполнения задания использовать Markdown, а именно заголовок и списки

14. Создать из ветки main ветку support и создать там файл LICENSE в корне репозитория с содержимым https://www.apache.org/licenses/LICENSE-2.0.txt. Сделать коммит. Вывести последние 3 коммитa.
15. Переключиться обратно на ветку main и создать там файл LICENSE в корне репозитория с содержимым https://github.com/git/git-scm.com/blob/main/MIT-LICENSE.txt. Сделать коммит. Вывести последние 3 коммитa.
16. Сделать merge ветки support в ветку main и решить конфликты путем выбора содержимого любой одной лицензии.
17. Переключиться на ветку develop и сделать rebase относительно ветки main.
18. Вывести историю последних 10 коммитов в виде графа с помощью команды git log -10 --oneline --graph.
19. Запушить ветку develop. В истории коммитов должен быть мерж support -> main.
20. Зайти в свой репозиторий на GitHub и создать Pull Request из ветки develop в ветку main.
