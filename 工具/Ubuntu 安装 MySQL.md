## 起因

自己有个服务器，最近准备搭建一个web服务，需要用到数据库，以前都是直接用阿里云的mysql服务，这次自己在linux上装一下mysql，遇到挺多坑，这里记录一下，遇到同样问题的小伙伴也可以参考一下解决办法。



## 安装

1. 安装 MySQL, 直接用 apt-get 方式，记得前面加上 sudo 获取 root 权限：

   ```
   sudo apt-get install mysql-server mysql-client
   ```

2. 安装过程中，会提示设置初始密码，设置一下就好

3. 安装完后，测试是否安装成功

   ```
   sudo netstat -tap | grep mysql
   ```

## 让MySQL服务器可以被远程访问

安装完成后，默认只能本地登录，远程使用 Navicat 等工具是没办法连接的，所以需要配置下

1. 修改 Mysql 配置文件，去掉绑定的 ip

   ```
   sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
   
   # bind-address = 127.0.0.1
   (注释掉：bind-address = 127.0.0.1 （在前面加上 #）)
   
   // 修改后进行重启
   sudo /etc/init.d/mysql restart
   ```

2. 进入 mysql 界面，进行登录授权

   ```
   // 登录
   mysql -uroot -p
   // 登陆后进行授权，注意这里的 password 是数据库密码
   grant all privileges on *.* to 'root'@'%' identified by 'password';
   flush privileges;
   ```

## 修改 MySQL 字符集

1. Mysql 默认的字符集是 Latin ，需要修改成 utf-8：

```
// 编辑配置文件
sudo vim /etc/mysql/my.cnf

分别在 client，mysql，mysqld 下加上配置，如果这三个标签，需要手动加
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
character-set-server=utf8
```

2. 重启

   ``sudo /etc/init.d/mysql restart``

3. 这个时候验证下，配置成功~

```
mysql> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
```

做完上面的操作，大部分情况下就已经完成了，但是，还可能出现远程连接不上等其他问题。

## 其他可能遇到的问题

远程mysql连接不上

 1. 根据 `/etc/mysql/mysql.conf.d/mysqld.cnf` 头部，可能需要把这个文件拷贝到 两个不同的地方, 如下所示：

    ```
    # The MySQL database server configuration file.
    #
    # You can copy this to one of:
    # - "/etc/mysql/my.cnf" to set global options,
    # - "~/.my.cnf" to set user-specific options.
    ```

 2. 使用 `netstat -an | grep 3306` 查看 3306 端口有没有开放，如果是下面的情况则未开放：

    ```
    tcp    0   0 127.0.0.1:3306      0.0.0.0:*         LISTEN
    ```

 3.  user 表中的 root 用户 的 host 字段没有设置为 %

    ```
    // 先登录mysql
    mysql -u root -p 
    // 进入 mysql 表
    use mysql; 
    // 查看当前情况
    select host,user from user; 
    // 如果不符合要求，更新字段
    update user set host=’%’ where user=’root’; 
    
    // 最后退出mysql后，重启
    sudo /etc/init.d/mysql restart
    ```

	4. 可能是防火墙的问题

    查看端口列表：

    `sudo iptables -L -n //查看列表`

    如果 3306 不是 ACCEPT 状态，说明是有问题的，具体解决可以参考这个：[Ubuntu 开启mysql远程连接](https://dzer.me/2016/05/04/ubuntu-%E5%BC%80%E5%90%AFmysql%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5/)


## Mysql 常用的命令行

1. 启动：

   `sudo /etc/init.d/mysqld start`

2. 关闭：

   `sudo /etc/init.d/mysqld stop`

3. 重启 ：

   `sudo /etc/init.d/mysql restart`

4. 本地登录：

   `mysql  -uroot  -p`

5. 查看字符集

   `mysql>show variables like '%char%';`



其他参考：

https://blog.csdn.net/ynnmnm/article/details/45097857

https://www.jianshu.com/p/3111290b87f4