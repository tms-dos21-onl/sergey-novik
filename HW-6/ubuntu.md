1. Определить все IP адреса, маски подсетей и соответствующие MAC адреса Linux VM. Определите класс и адреса подсетей, в которых находится VM.
   - ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/e6f9c561-5b64-4e42-b49c-4da28fa48e2c)
   - 10.0.2.4 - Class A, адрес сети 10.0.2.0

2. Определить публичный IP адрес хоста и Linux VM? Чем они отличаются?
   - публичный ip в консоли смотрел так: `curl ipinfo.io`, ip не отличаются. Хост и VM находятся за одним и тем же NATом по отношению к провайдеру, соответственно у них одинаковые белый ip.
     
3. Вывести ARP таблицу на хосте и найти там запись, соответствующую MAC адресу с предыдущего задания. Если её нет, то объяснить почему.
   - в windows вывел ARP-таблицу в cmd  командой `arp -a`. MAC моей VM в таблице нету, так как VM находится за NATом по отношению к Хосту, а это уже другая подсеть.
    
4. Выполнить разбиение сети 172.20.0.0/16 на подсети с маской /24 и ответить на следующие вопросы:
- Сколько всего подсетей будет в сети?  
  :heavy_check_mark: 256 подсетей. 172.20.0.0-172.20.255.0
  
- Сколько узлов будет в каждой подсети?
  
  :heavy_check_mark: 254 узла. В каждой подсети 256 адресов, из них два зарезервированы.
    
- Каким будет сетевой адрес первой и второй подсети?

  :heavy_check_mark: 172.20.0.0/24 и 172.20.1.0/24
  
- Каким будут адреса первого и последнего хостов в первой и второй подсетях?
  
  :heavy_check_mark: - в первой сети 172.20.0.1 и 172.20.0.254
                     - во второй сети 172.20.1.1 и 172.20.1.254
  
- Каким будет широковещательный адрес в последней подсети?

  :heavy_check_mark: 172.20.255.255
    
5. Найти IP адрес соответствующий доменному имени ya.ru. Выполнить HTTP запрос на указанный IP адрес, чтобы скачать страницу с помощью утилиты curl. В результате должна вывестись HTML страничка в консоль. Подсказка: https://stackoverflow.com/questions/46563730/can-i-access-to-website-using-ip-address
   - сделал ping ya.ru и по логике ip=5.255.255.242
     
     ```console
      root@ubuntu-tms:/home/novik# curl http://ya.ru
      root@ubuntu-tms:/home/novik# curl ya.ru
      root@ubuntu-tms:/home/novik# curl 5.255.255.242
      root@ubuntu-tms:/home/novik# curl -v 5.255.255.242
      *   Trying 5.255.255.242:80...
      * Connected to 5.255.255.242 (5.255.255.242) port 80 (#0)
      > GET / HTTP/1.1
      > Host: 5.255.255.242
      > User-Agent: curl/7.81.0
      > Accept: */*
      >
      * Mark bundle as not supporting multiuse
      < HTTP/1.1 406 Not acceptable
      < Accept-CH: Sec-CH-UA-Platform-Version, Sec-CH-UA-Mobile, Sec-CH-UA-Model, Sec-CH-UA, Sec-CH-UA-Full-Version-List, Sec-CH-UA-WoW64, Sec-CH-UA-Arch, Sec-CH-UA-Bitness, Sec-CH-UA-Platform, Sec-CH-UA-Full-Version, Viewport-Width, DPR, Device-Memory, RTT, Downlink, ECT
      < Connection: Close
      < Content-Length: 0
      < NEL: {"report_to": "network-errors", "max_age": 100, "success_fraction": 0.001, "failure_fraction": 0.1}
      < Report-To: { "group": "network-errors", "max_age": 100, "endpoints": [{"url": "https://dr.yandex.net/nel", "priority": 1}, {"url": "https://dr2.yandex.net/nel", "priority": 2}]}
      < X-Content-Type-Options: nosniff
      < X-Yandex-Req-Id: 1708955477251626-15031158530283160183-balancer-l7leveler-kubr-yp-vla-203-BAL
      <
      * Closing connection 0


     ```
:heavy_check_mark: по итогу я не совсем понял это задание). 

     
