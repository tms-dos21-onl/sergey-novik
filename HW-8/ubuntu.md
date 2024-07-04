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
    ```console
    novik@w-adm:/mnt/c/Users/s.novik/sergey-novik$ git log -3
    commit 444df26757ddb007debed402feed7821d0a174dd (HEAD -> main, origin/main, origin/HEAD)
    Author: madrunner-ops <77771829+madrunner-ops@users.noreply.github.com>
    Date:   Thu Apr 18 16:29:19 2024 +0300
    
        Update ubuntu.md
    
    commit ba2623eec16cedc976fe7abc2060e4def087ced6
    Author: madrunner-ops <77771829+madrunner-ops@users.noreply.github.com>
    Date:   Thu Apr 18 16:03:57 2024 +0300
    
        Update ubuntu.md
    
    commit 8a9df4e5c17f69afd01441eaaa35717e6110b7e5
    Author: madrunner-ops <77771829+madrunner-ops@users.noreply.github.com>
    Date:   Wed Apr 17 17:10:35 2024 +0300
    
        Update ubuntu.md
    ```
#до этой строчки делал задания давно, поэтому дальше будет немного другой вывод комманд;

5. Создать пустой файл README.md и сделать коммит.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git add README.md 
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git commit -m "Create file README.md"
    [main 06a6a51] Create file README.md
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 HW-8/README.md
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> 
```
6. Добавить фразу "Hello, DevOps" в README.md файл и сделать коммит.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git add -A
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git commit -m "Add text to README.nd"
    [main 4035c8b] Add text to README.nd
    1 file changed, 1 insertion(+), 1 deletion(-)
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git log -3
    commit 4035c8bfedb3fbadc873e76c81d0a92272aafd4b (HEAD -> main)
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jul 4 19:32:44 2024 +0300

        Add text to README.nd

    commit f2f9638bba1f0b3af9d0a05a06145165137ab404 (origin/main, origin/HEAD)
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jul 4 19:26:04 2024 +0300

        update

    commit 06a6a517b4ae29192e6f5daa44a1903ef95393c1
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jun 27 18:32:05 2024 +0300

        Create file README.md
```
7. Сделать реверт последнего коммита. Вывести последние 3 коммитa с помощью git log.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git revert HEAD --no-edit
    [main c7b44fc] Reapply "Add text to README.nd"
    Date: Thu Jul 4 19:52:16 2024 +0300
    1 file changed, 1 insertion(+), 1 deletion(-)
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git status     
    On branch main
    Your branch is ahead of 'origin/main' by 2 commits.
    (use "git push" to publish your local commits)

    nothing to commit, working tree clean
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git log -3
    commit c7b44fcf974ebd1251e0a62ce41fa5fdecb601e7 (HEAD -> main)
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jul 4 19:52:16 2024 +0300

        Reapply "Add text to README.nd"

        This reverts commit 731c7ae099ff5ddbd8ff3f24ef33fe64d3e03c96.

    commit 731c7ae099ff5ddbd8ff3f24ef33fe64d3e03c96
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jul 4 19:50:10 2024 +0300

        Revert "Add text to README.nd"

        This reverts commit 4035c8bfedb3fbadc873e76c81d0a92272aafd4b.

    commit 4035c8bfedb3fbadc873e76c81d0a92272aafd4b (origin/main, origin/HEAD)
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jul 4 19:32:44 2024 +0300

        Add text to README.nd
```
8. Удалить последние 3 коммита с помощью git reset.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git reset --hard HEAD~3
    HEAD is now at f2f9638 update
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git log -3
    commit f2f9638bba1f0b3af9d0a05a06145165137ab404 (HEAD -> main)
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jul 4 19:26:04 2024 +0300

        update

    commit 06a6a517b4ae29192e6f5daa44a1903ef95393c1
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jun 27 18:32:05 2024 +0300

        Create file README.md

    commit 8be96b1f14b8a3b798602a9e561ff3428ab098b5
    Author: Sergey Novik <sergeinowik@gmail.com>
    Date:   Thu Jun 27 17:04:08 2024 +0300    
```
9. Вернуть коммит, где добавляется пустой файл README.md. Для этого найти ID коммита в git reflog, а затем сделать cherry-pick.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git cherry-pick 06a6a51
    [main c727529] Create file README.md
    Date: Thu Jun 27 18:32:05 2024 +0300
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 HW-8/README.md    
```
10. Удалить последний коммит с помощью git reset.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git reset --hard HEAD~1
    HEAD is now at 8be96b1 Upload Ansible files for HW-17
```
11. Переключиться на ветку main или master. Если ветка называется master, то переименовать её в main.
```console
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git branch
    homework-branch
    * main
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git checkout main      
    Already on 'main'
    Your branch is behind 'origin/main' by 3 commits, and can be fast-forwarded.
    (use "git pull" to update your local branch)
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git branch       
    homework-branch
    * main
```
12. Скопировать файл https://github.com/tms-dos21-onl/_sandbox/blob/main/.github/workflows/validate-shell.yaml, положить его по такому же относительному пути в репозиторий. Создать коммит и запушить его в удаленный репозиторий.
```console
    # создал папки при помощи mkdir in PowerShell
    # зашел в браузере по ссылке, копировал ссылку RAW
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> Invoke-WebRequest https://raw.githubusercontent.com/tms-dos21-onl/_sandbox/main/.github/workflows/validate-shell.yaml?token=GHSAT0AAAAAACOGREX2HM64LWAUABDWL4ZUZUG3TSA -OutFile .\validate-shell.yaml

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> git add -A

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> git commit -m "Download validate-shell.yaml"
    [main 9700d04] Download validate-shell.yaml
    1 file changed, 24 insertions(+)
    create mode 100644 .github/workflows/validate-shell.yaml

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> git push
    To https://github.com/tms-dos21-onl/sergey-novik.git
    ! [rejected]        main -> main (non-fast-forward)
    error: failed to push some refs to 'https://github.com/tms-dos21-onl/sergey-novik.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. If you want to integrate the remote changes,
    hint: use 'git pull' before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> git pull
    Merge made by the 'ort' strategy.
    HW-8/README.md |  1 +
    HW-8/ubuntu.md | 13 +++++++++++++
    2 files changed, 14 insertions(+)
    create mode 100644 HW-8/README.md
    
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> git push
    Enumerating objects: 9, done.
    Counting objects: 100% (9/9), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (7/7), 898 bytes | 449.00 KiB/s, done.
    Total 7 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
    remote: Resolving deltas: 100% (2/2), completed with 1 local object.
    To https://github.com/tms-dos21-onl/sergey-novik.git
    4035c8b..4144198  main -> main

```
13. Создать из ветки main ветку develop. Переключиться на неё и создать README.md в корне репозитория. Написать в этом файле какие инструменты DevOps вам знакомы и с какими вы бы хотели познакомиться больше всего (2-3 пункта). Сделать коммит.

> ⚠️ Для выполнения задания использовать Markdown, а именно заголовок и списки

14. Создать из ветки main ветку support и создать там файл LICENSE в корне репозитория с содержимым https://www.apache.org/licenses/LICENSE-2.0.txt. Сделать коммит. Вывести последние 3 коммитa.
15. Переключиться обратно на ветку main и создать там файл LICENSE в корне репозитория с содержимым https://github.com/git/git-scm.com/blob/main/MIT-LICENSE.txt. Сделать коммит. Вывести последние 3 коммитa.
16. Сделать merge ветки support в ветку main и решить конфликты путем выбора содержимого любой одной лицензии.
17. Переключиться на ветку develop и сделать rebase относительно ветки main.
18. Вывести историю последних 10 коммитов в виде графа с помощью команды git log -10 --oneline --graph.
19. Запушить ветку develop. В истории коммитов должен быть мерж support -> main.
20. Зайти в свой репозиторий на GitHub и создать Pull Request из ветки develop в ветку main.
