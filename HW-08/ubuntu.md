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
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git add README.md 
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git commit -m "Create file README.md"
    [main 06a6a51] Create file README.md
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 HW-8/README.md
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> 
```
6. Добавить фразу "Hello, DevOps" в README.md файл и сделать коммит.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git add -A
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git commit -m "Add text to README.nd"
    [main 4035c8b] Add text to README.nd
    1 file changed, 1 insertion(+), 1 deletion(-)
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git log -3
    commit 4035c8bfedb3fbadc873e76c81d0a92272aafd4b (HEAD -> main)
    Author: Sergey Novik 
    Date:   Thu Jul 4 19:32:44 2024 +0300

        Add text to README.nd

    commit f2f9638bba1f0b3af9d0a05a06145165137ab404 (origin/main, origin/HEAD)
    Author: Sergey Novik 
    Date:   Thu Jul 4 19:26:04 2024 +0300

        update

    commit 06a6a517b4ae29192e6f5daa44a1903ef95393c1
    Author: Sergey Novik 
    Date:   Thu Jun 27 18:32:05 2024 +0300

        Create file README.md
```
7. Сделать реверт последнего коммита. Вывести последние 3 коммитa с помощью git log.
```ps
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
    Author: Sergey Novik 
    Date:   Thu Jul 4 19:52:16 2024 +0300

        Reapply "Add text to README.nd"

        This reverts commit 731c7ae099ff5ddbd8ff3f24ef33fe64d3e03c96.

    commit 731c7ae099ff5ddbd8ff3f24ef33fe64d3e03c96
    Author: Sergey Novik 
    Date:   Thu Jul 4 19:50:10 2024 +0300

        Revert "Add text to README.nd"

        This reverts commit 4035c8bfedb3fbadc873e76c81d0a92272aafd4b.

    commit 4035c8bfedb3fbadc873e76c81d0a92272aafd4b (origin/main, origin/HEAD)
    Author: Sergey Novik 
    Date:   Thu Jul 4 19:32:44 2024 +0300

        Add text to README.nd
```
8. Удалить последние 3 коммита с помощью git reset.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git reset --hard HEAD~3
    HEAD is now at f2f9638 update
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git log -3
    commit f2f9638bba1f0b3af9d0a05a06145165137ab404 (HEAD -> main)
    Author: Sergey Novik 
    Date:   Thu Jul 4 19:26:04 2024 +0300

        update

    commit 06a6a517b4ae29192e6f5daa44a1903ef95393c1
    Author: Sergey Novik
    Date:   Thu Jun 27 18:32:05 2024 +0300

        Create file README.md

    commit 8be96b1f14b8a3b798602a9e561ff3428ab098b5
    Author: Sergey Novik 
    Date:   Thu Jun 27 17:04:08 2024 +0300    
```
9. Вернуть коммит, где добавляется пустой файл README.md. Для этого найти ID коммита в git reflog, а затем сделать cherry-pick.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git cherry-pick 06a6a51
    [main c727529] Create file README.md
    Date: Thu Jun 27 18:32:05 2024 +0300
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 HW-8/README.md    
```
10. Удалить последний коммит с помощью git reset.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\HW-8> git reset --hard HEAD~1
    HEAD is now at 8be96b1 Upload Ansible files for HW-17
```
11. Переключиться на ветку main или master. Если ветка называется master, то переименовать её в main.
```ps
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
```ps
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
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik\.github\workflows> git checkout -b develop
    Switched to a new branch 'develop'


```

> ⚠️ Для выполнения задания использовать Markdown, а именно заголовок и списки

14. Создать из ветки main ветку support и создать там файл LICENSE в корне репозитория с содержимым https://www.apache.org/licenses/LICENSE-2.0.txt. Сделать коммит. Вывести последние 3 коммитa.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git branch
    * develop
    homework-branch
    main
    
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git checkout main      
    M       HW-8/ubuntu.md
    Switched to branch 'main'
    Your branch is up to date with 'origin/main'.
    
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git checkout -b support
    Switched to a new branch 'support'

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> curl -L -o LICENSE https://www.apache.org/licenses/LICENSE-2.0.txt
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100 11358  100 11358    0     0  54711      0 --:--:-- --:--:-- --:--:-- 55950

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git add -A
    warning: in the working copy of 'LICENSE', LF will be replaced by CRLF the next time Git touches it

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git commit -m 'добавил файл LICENSE в корне'  
    [support 21a3ec1] ╨┤╨╛╨▒╨░╨▓╨╕╨╗ ╤Д╨░╨╣╨╗ LICENSE ╨▓ ╨║╨╛╤А╨╜╨╡
    2 files changed, 208 insertions(+)
    create mode 100644 LICENSE

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git log -3
    commit 21a3ec1f127bc0acf39a6e59139fc78db149e9ae (HEAD -> support)
    Author: Sergey Novik
    Date:   Mon Jul 8 18:25:56 2024 +0300

        добавил файл LICENSE в корне

    commit 66f7d95b1818a6480919e359ae27a206dec86175 (origin/main, origin/HEAD, main)
    Author: Sergey Novik
    Date:   Thu Jul 4 20:55:04 2024 +0300

        Update HW-8

    commit c7eac8ffa59eedfcc02d8c6a2e5d133af6ba861e
    Author: Sergey Novik
    Date:   Thu Jul 4 20:51:43 2024 +0300

```
15. Переключиться обратно на ветку main и создать там файл LICENSE в корне репозитория с содержимым https://github.com/git/git-scm.com/blob/main/MIT-LICENSE.txt. Сделать коммит. Вывести последние 3 коммитa.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git checkout main
    Switched to branch 'main'
    Your branch is up to date with 'origin/main'.

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> curl -L -o LICENSE https://github.com/git/git-scm.com/blob/main/MIT-LICENSE.txt
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100  303k    0  303k    0     0   474k      0 --:--:-- --:--:-- --:--:--  476k

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git add -A
    warning: in the working copy of 'LICENSE', LF will be replaced by CRLF the next time Git touches it

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git commit -m 'add file LICENSE in branch main'
    [main e985cdd] add file LICENSE in branch main
    1 file changed, 2493 insertions(+)
    create mode 100644 LICENSE

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git log -3
    commit e985cdd59da71cb6b8a30acfefcc924fb6adf63a (HEAD -> main)
    Author: Sergey Novik 
    Date:   Mon Jul 8 18:31:26 2024 +0300

        add file LICENSE in branch main

    commit 66f7d95b1818a6480919e359ae27a206dec86175 (origin/main, origin/HEAD)
    Author: Sergey Novik 
    Date:   Thu Jul 4 20:55:04 2024 +0300

        Update HW-8

    commit c7eac8ffa59eedfcc02d8c6a2e5d133af6ba861e
    Author: Sergey Novik 
    Date:   Thu Jul 4 20:51:43 2024 +0300
```
16. Сделать merge ветки support в ветку main и решить конфликты путем выбора содержимого любой одной лицензии.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git checkout main
    Already on 'main'
    Your branch is ahead of 'origin/main' by 1 commit.
    (use "git push" to publish your local commits)

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git branch       
    develop
    homework-branch
    * main
    support

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git merge support
    Auto-merging LICENSE
    CONFLICT (add/add): Merge conflict in LICENSE
    Automatic merge failed; fix conflicts and then commit the result.

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git checkout --ours LICENSE
    Updated 1 path from the index

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git merge support
    error: Merging is not possible because you have unmerged files.
    hint: Fix them up in the work tree, and then use 'git add/rm <file>'
    hint: as appropriate to mark resolution and make a commit.
    fatal: Exiting because of an unresolved conflict.

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git add -A       
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git merge support
    fatal: You have not concluded your merge (MERGE_HEAD exists).
    Please, commit your changes before you merge.

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git commit -m 'update file LICENSE in branch main'
    [main 32f0947] update file LICENSE in branch main

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git merge support
    Already up to date.
```
17. Переключиться на ветку develop и сделать rebase относительно ветки main.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git checkout develop
    Switched to branch 'develop'

    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git rebase main
    Successfully rebased and updated refs/heads/develop.

```
18. Вывести историю последних 10 коммитов в виде графа с помощью команды git log -10 --oneline --graph.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git log -10 --oneline --graph
    * 1a12ed7 (HEAD -> develop) добавил файл README.md в корне
    *   32f0947 (main) update file LICENSE in branch main
    |\  
    | * 9c9a028 (support) update file HW-8\ubuntu.md
    | * 21a3ec1 добавил файл LICENSE в корне
    * | e985cdd add file LICENSE in branch main
    |/  
    * 66f7d95 (origin/main, origin/HEAD) Update HW-8
    * c7eac8f Update HW-8
    * 812a9c6 Update ubuntu.md
    * 19a3991 Update ubuntu.md
    * a32478e Update HW-8
```
19. Запушить ветку develop. В истории коммитов должен быть мерж support -> main.
```ps
    PS C:\Users\s.novik\Documents\GitHub\sergey-novik> git push origin develop
    Enumerating objects: 64, done.
    Counting objects: 100% (64/64), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (56/56), done.
    Writing objects: 100% (61/61), 61.42 KiB | 2.12 MiB/s, done.
    Total 61 (delta 24), reused 0 (delta 0), pack-reused 0 (from 0)
    remote: Resolving deltas: 100% (24/24), completed with 1 local object.
    remote: 
    remote: Create a pull request for 'develop' on GitHub by visiting:
    remote:      https://github.com/tms-dos21-onl/sergey-novik/pull/new/develop
    remote:
    To https://github.com/tms-dos21-onl/sergey-novik.git
    * [new branch]      develop -> develop
```

20. Зайти в свой репозиторий на GitHub и создать Pull Request из ветки develop в ветку main.
```
    Сделал.
```