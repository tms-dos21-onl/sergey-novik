1. Зарегистрироваться на облачном провайдере Google Cloud Platform (GCP)
- В качестве локации указать Грузию.
- Привязать банковскую карту РБ/РФ.
- Получить кредиты от GCP (300$) путем верификации карты.

    ![alt text](image.png)
2. Настроить предупреждения для бюджета (например, 50$ на месяц).

    ![alt text](image-1.png)
3. Создать свою первую VM в облаке, зайти на неё по SSH и установить Nginx/Apache.

    ![alt text](image-2.png)

    ```console
    root@instance-hw-gcp:/home/sergeinowik# hostnamectl 
    Static hostname: instance-hw-gcp
        Icon name: computer-vm
            Chassis: vm 🖴
        Machine ID: 6db0933a3635429d8a9689483aa23b60
            Boot ID: 4f2dc47c0c5d4d1f9bd70e73afb7d773
    Virtualization: google
    Operating System: Debian GNU/Linux 12 (bookworm)  
            Kernel: Linux 6.1.0-22-cloud-amd64
        Architecture: x86-64
    Hardware Vendor: Google
    Hardware Model: Google Compute Engine
    Firmware Version: Google
    root@instance-hw-gcp:/home/sergeinowik# systemctl status nginx
    ● nginx.service - A high performance web server and a reverse proxy server
        Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enabled)
        Active: active (running) since Mon 2024-07-22 14:57:09 UTC; 3min 34s ago
        Docs: man:nginx(8)
        Process: 1041 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0>
        Process: 1042 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Main PID: 1067 (nginx)
        Tasks: 3 (limit: 2344)
        Memory: 2.2M
            CPU: 29ms
        CGroup: /system.slice/nginx.service
                ├─1067 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
                ├─1069 "nginx: worker process"
                └─1070 "nginx: worker process"
    ```
4. Создать Firewall правило для подключения к этой VM со своей локальной машины по порту 80. Проверить, что доступ работает.
    

    ```console
    sergeinowik@instance-hw-gcp:~$ systemctl status sshd
    ● ssh.service - OpenBSD Secure Shell server
        Loaded: loaded (/lib/systemd/system/ssh.service; enabled; preset: enabled)
        Active: active (running) since Mon 2024-07-22 16:22:11 UTC; 2min 0s ago
        Docs: man:sshd(8)
                man:sshd_config(5)
        Process: 2952 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
    Main PID: 2953 (sshd)
        Tasks: 1 (limit: 2344)
        Memory: 4.8M
            CPU: 121ms
        CGroup: /system.slice/ssh.service
                └─2953 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

    Jul 22 16:22:11 instance-hw-gcp systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
    Jul 22 16:22:11 instance-hw-gcp sshd[2953]: Server listening on 0.0.0.0 port 80.
    Jul 22 16:22:11 instance-hw-gcp sshd[2953]: Server listening on :: port 80.
    Jul 22 16:22:11 instance-hw-gcp systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
    ```