#后续有docker版本，敬请关注

# 环境要求

linux

mysql

C++

# 项目内容

buffer,log,threadpool,http等，相关解释请查看该目录下readme.md


# 虚拟机操作流程

clone项目到虚拟机

root@youyou-virtual-machine:~# git clone https://github.com/inesying/linux_webserver.git

查看目录

root@youyou-virtual-machine:~# ls

linux_webserver

进入目录

root@youyou-virtual-machine:~# cd linux_webserver/

创建bin目录

root@youyou-virtual-machine:~/linux_webserver# mkdir bin

开启mysql

root@youyou-virtual-machine:~/linux_webserver# service mysql start

root@youyou-virtual-machine:~/linux_webserver# mysql -uroot -p123456

进入了mysql

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

创建database

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

按自己需求改main.cpp

root@youyou-virtual-machine:~/linux_webserver# vim main.cpp 

WebServer server(

        2020, 3, 60000, false,     
        
        3306, "root", "123456", "mydb",
        
        12, 6, true, 1, 1024);   
        
2020为端口号，改mysql账号密码数据库"root","123456","mydb"

make

root@youyou-virtual-machine:~/linux_webserver# make

查看ip

root@youyou-virtual-machine:~/linux_webserver# ifconfig

开启

root@youyou-virtual-machine:~/linux_webserver# ./bin/server

![image](https://github.com/inesying/linux_webserver/blob/main/resources/images/index.jpg)

![image](https://github.com/inesying/linux_webserver/blob/main/resources/images/video.jpg)


