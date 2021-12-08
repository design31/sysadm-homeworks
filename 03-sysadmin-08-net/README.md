# Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
```
telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32
```

Мои результаты:
```
route-views>show ip route 185.171.100.142
Routing entry for 185.171.100.0/23
  Known via "bgp 6447", distance 20, metric 0
  Tag 6939, type external
  Last update from 64.71.137.241 7w0d ago
  Routing Descriptor Blocks:
  * 64.71.137.241, from 64.71.137.241, 7w0d ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 6939
      MPLS label: none
route-views>show bgp 185.171.100.142
BGP routing table entry for 185.171.100.0/23, version 1027283279
Paths: (24 available, best #23, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 20485 61111, (aggregated by 61111 185.171.100.0)
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
      Origin IGP, metric 0, localpref 100, valid, external, atomic-aggregate
      path 7FE0D88B34D0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 9002 61111, (aggregated by 61111 185.171.100.0)
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external, atomic-aggregate
      Community: 3257:8052 3257:50001 3257:54900 3257:54901 20912:65004 65535:65284
      path 7FE12719A6A8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 3
  3303 9002 61111, (aggregated by 61111 185.171.100.0)
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external, atomic-aggregate
      Community: 3303:1004 3303:1007 3303:1030 3303:3067 9002:64667
      path 7FE0F137A3D0 RPKI State not found
      rx pathid: 0, tx pathid: 0
```

---

2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

```
root@ubuntu:/home/us# modprobe -v dummy numdummies=2
insmod /lib/modules/5.11.0-41-generic/kernel/drivers/net/dummy.ko numdummies=0 numdummies=2
root@ubuntu:/home/us# lsmod | grep dummy
dummy                  16384  0
root@ubuntu:/home/us# ip a
3: dummy0: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 76:88:ad:79:3f:10 brd ff:ff:ff:ff:ff:ff
4: dummy1: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 62:44:89:83:bb:c5 brd ff:ff:ff:ff:ff:ff

root@ubuntu:/home/us# ip addr add 192.168.7.253/24 dev dummy0
root@ubuntu:/home/us# ip addr add 192.168.7.254/24 dev dummy1
root@ubuntu:/home/us# ip -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128
enp0s3           UP             192.168.5.162/24 fe80::eb66:e6e5:f2c9:fa73/64
dummy0           UNKNOWN        192.168.7.253/32 fe80::7488:adff:fe79:3f10/64
dummy1           UNKNOWN        192.168.7.254/32 fe80::6044:89ff:fe83:bbc5/64

```
Добавлю маршруты:
```
root@ubuntu:/home/us# ip -br r
default via 192.168.5.2 dev enp0s3 proto dhcp metric 100
10.0.10.0/30 via 192.168.5.100 dev enp0s3
10.0.10.0/24 via 192.168.5.100 dev enp0s3
169.254.0.0/16 dev enp0s3 scope link metric 1000
172.16.10.0/24 via 192.168.5.1 dev enp0s3
192.168.5.0/24 dev enp0s3 proto kernel scope link src 192.168.5.162 metric 100
192.168.7.0/30 via 192.168.7.254 dev dummy1
```

---

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали. 

 ---
## Задание для самостоятельной отработки (необязательно к выполнению)

6*. Установите Nginx, настройте в режиме балансировщика TCP или UDP.

7*. Установите bird2, настройте динамический протокол маршрутизации RIP.

8*. Установите Netbox, создайте несколько IP префиксов, используя curl проверьте работу API.

 ---

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Также вы можете выполнить задание в [Google Docs](https://docs.google.com/document/u/0/?tgif=d) и отправить в личном кабинете на проверку ссылку на ваш документ.
Название файла Google Docs должно содержать номер лекции и фамилию студента. Пример названия: "1.1. Введение в DevOps — Сусанна Алиева".

Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Перед тем как выслать ссылку, убедитесь, что ее содержимое не является приватным (открыто на комментирование всем, у кого есть ссылка), иначе преподаватель не сможет проверить работу. Чтобы это проверить, откройте ссылку в браузере в режиме инкогнито.

[Как предоставить доступ к файлам и папкам на Google Диске](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop)

[Как запустить chrome в режиме инкогнито ](https://support.google.com/chrome/answer/95464?co=GENIE.Platform%3DDesktop&hl=ru)

[Как запустить  Safari в режиме инкогнито ](https://support.apple.com/ru-ru/guide/safari/ibrw1069/mac)

Любые вопросы по решению задач задавайте в чате Slack.

---

