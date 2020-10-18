##虚拟机操作流程
root@youyou-virtual-machine:~# git clone https://github.com/inesying/linux_webserver.git

Cloning into 'linux_webserver'...
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 101 (delta 1), reused 0 (delta 0), pack-reused 94
Receiving objects: 100% (101/101), 4.59 MiB | 60.00 KiB/s, done.
Resolving deltas: 100% (13/13), done.

root@youyou-virtual-machine:~# ls
linux_webserver

root@youyou-virtual-machine:~# cd linux_webserver/

root@youyou-virtual-machine:~/linux_webserver# mkdir bin

root@youyou-virtual-machine:~/linux_webserver# service mysql start

root@youyou-virtual-machine:~/linux_webserver# mysql -uroot -p123456

mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 182
Server version: 8.0.21-0ubuntu0.20.04.4 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database mydb;
Query OK, 1 row affected (0.01 sec)

mysql> use mydb;
Database changed

mysql> create table usr(
    -> username char(50) null,
    -> password char(50) null)engine=InnoDB;
Query OK, 0 rows affected (0.01 sec)

mysql> insert into usr(username,password) values('name','password');
Query OK, 1 row affected (0.00 sec)

mysql> quit;
Bye

root@youyou-virtual-machine:~/linux_webserver# make
g++ -std=c++14 -O2 -Wall -g  ./log/*.cpp ./pool/*.cpp ./timer/*.cpp ./http/*.cpp ./server/*.cpp ./buffer/*.cpp ./main.cpp -o ./bin/server  -pthread -lmysqlclient
./http/httprequest.cpp: In static member function ‘static bool HttpRequest::UserVerify(const string&, const string&, bool)’:
./http/httprequest.cpp:185:18: warning: variable ‘j’ set but not used [-Wunused-but-set-variable]
  185 |     unsigned int j = 0;
      |                  ^
./http/httprequest.cpp:187:18: warning: variable ‘fields’ set but not used [-Wunused-but-set-variable]
  187 |     MYSQL_FIELD *fields = nullptr;
      |                  ^~~~~~
      
root@youyou-virtual-machine:~/linux_webserver# ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:1c:e2:90:03  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.209.128  netmask 255.255.255.0  broadcast 192.168.209.255
        inet6 fe80::7a10:30f8:6bc0:c678  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:3d:ca:50  txqueuelen 1000  (Ethernet)
        RX packets 25654  bytes 26399946 (26.3 MB)
        RX errors 7  dropped 7  overruns 0  frame 0
        TX packets 22925  bytes 13483918 (13.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 19  base 0x2000  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 3639  bytes 17977214 (17.9 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3639  bytes 17977214 (17.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@youyou-virtual-machine:~/linux_webserver# ./bin/server


