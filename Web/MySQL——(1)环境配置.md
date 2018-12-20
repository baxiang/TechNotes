##Ubuntu 安装

 ##### 使用apt-get
```
sudo apt-get install mysql-server
```
 #####查看安装状态
```
sudo netstat -tap | grep mysql
tcp6       0      0 [::]:mysql              [::]:*                  LISTEN      1313/mysqld
```
 #####配置远程访问
将mysqld.cnf 中的bind-address = 127.0.0.1注释
```
 sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf  
```
 ##### 查看端口
```
netstat -an|grep 3306
```
开启了远程访问的端口是
```
tcp6       0      0 :::3306                 :::*                    LISTEN
```
####开启安全组
阿里云服务器设置安全组规则
![image.png](https://upload-images.jianshu.io/upload_images/143845-71f1b08554f823b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
腾讯云设置安全组规则
![image.png](https://upload-images.jianshu.io/upload_images/143845-b295de67b3adb43f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



 #####添加远程登录的用户

添加一个用户名是root且密码是123456的远程访问用户
```
grant all on *.* to root@'%' identified by '123456'; 
flush privileges; ##刷新
```
 #####查看当前的用户信息
```
use mysql
select user,host,authentication_string from user;
```
 #####重启MySQL服务
```
  /etc/init.d/mysql restart  
```
 #####卸载
```
sudo apt-get remove --purge mysql-\* 
sudo find  / -name mysql -print  ## 查找包含的文件 逐一删除
sudo rm -rf /etc/mysql /var/lib/mysql
sudo apt-get autoremove
sudo apt-get autoclean
```

## CentOS 安装
使用yum 在CentOS 7 上安装mysql-server会失败,CentOS 7 已经将MySQL数据库软件从默认的程序列表中移除。据说是因为甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险。
```
yum install mysql-server
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
No package mysql-server available.
Error: Nothing to do
```
查看是否安装旧版本
```
# rpm -qa | grep mysql
mysql-community-common-5.6.42-2.el7.x86_64
mysql-community-release-el7-5.noarch
mysql-community-libs-5.6.42-2.el7.x86_64
mysql-community-server-5.6.42-2.el7.x86_64
mysql-community-client-5.6.42-2.el7.x86_64
```
卸载旧版本
```
# rpm -e --nodeps mysql-community-common-5.6.42-2.el7.x86_64
# rpm -e --nodeps mysql-community-release-el7-5.noarch
# rpm -e --nodeps mysql-community-libs-5.6.42-2.el7.x86_64
# rpm -e --nodeps mysql-community-server-5.6.42-2.el7.x86_64
# rpm -e --nodeps mysql-community-client-5.6.42-2.el7.x86_64
```
安装5.6版本
```
#wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
#rpm -ivh mysql-community-release-el7-5.noarch.rpm
#yum install mysql-community-server
#service mysqld restart 
```
安装5.7版本
```
# wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.22-1.el7.x86_64.rpm-bundle.tar
# mkdir mysql
# tar -xvf mysql-5.7.22-1.el7.x86_64.rpm-bundle.tar -C mysql
```
注意5.7的安装顺序 依次安装rpm包 依赖关系依次为common→libs→client→server
```
# rpm -ivh mysql-community-common-5.7.22-1.el7.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-common-5.7.22-1.e################################# [100%]
# rpm -ivh mysql-community-libs-5.7.22-1.el7.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-libs-5.7.22-1.el7################################# [100%]
# rpm -ivh mysql-community-client-5.7.22-1.el7.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-client-5.7.22-1.e################################# [100%]
# rpm -ivh mysql-community-server-5.7.22-1.el7.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-server-5.7.22-1.e################################# [100%]
# systemctl start mysqld.service
```

####增加新的远程登录用户
```
create user 'baxiang'@'%' identified by 'baxiang';
```
##MAC OS安装
```brew安装
$ brew install mysql
==> Downloading https://homebrew.bintray.com/bottles/mysql-5.7.21.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring mysql-5.7.21.sierra.bottle.tar.gz
==> /usr/local/Cellar/mysql/5.7.21/bin/mysqld --initialize-insecure --user=baxiang --basedir=/usr/local/Cellar/mysql/5.7.21 --datad
==> Caveats
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -uroot

To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start
==> Summary
🍺  /usr/local/Cellar/mysql/5.7.21: 323 files, 233.9MB
```
##设置新密码
```
 mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No: y// 设置新密码

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0//设置密码的安全等级
Please set the password for root here.

New password:

Re-enter new password:

Estimated strength of the password: 50
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y//这里删除默认无密码用户Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n //是否禁止用户远程登录

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y//是否删除MySQL自带的test数据库
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```
##tar.gz 安装方式
解压安装包
```
sudo tar -zxf mysql-5.7.24-macos10.14-x86_64.tar.gz -C /usr/local/
```
```
sudo mv mysql-5.7.24-macos10.14-x86_64 mysql
```
```
sudo chown -R baxiang:admin mysql
```
初始化MySQL用户 主要为了生成用户登录的临时密码 吗，此刻我们登录root登录密码是v=f(2;YRENlY
```
cd /usr/local/mysql/bin
sudo mysqld --initialize --user=mysql
Password:
2018-12-15T08:42:25.598745Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-12-15T08:42:25.607047Z 0 [Warning] Setting lower_case_table_names=2 because file system for /usr/local/mysql/data/ is case insensitive
2018-12-15T08:42:25.810336Z 0 [Warning] InnoDB: New log files created, LSN=45790
2018-12-15T08:42:25.844271Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2018-12-15T08:42:25.908498Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 59194e28-0045-11e9-ae26-c32d9a7a9888.
2018-12-15T08:42:25.921646Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2018-12-15T08:42:25.924583Z 1 [Note] A temporary password is generated for root@localhost: v=f(2;YRENlY
```
启动MySQL服务器
```
sudo /usr/local/mysql/support-files/mysql.server start
Starting MySQL
.Logging to '/usr/local/mysql/data/baxiangs-Mac-mini.local.err'.
 SUCCESS!
```
修改密码,此时需要输入我们刚才的临时密码
```
./mysqladmin -u root -p password 新密码
```
设置 vim .bash_profile
```
export PATH=$PATH:/usr/local/mysql/bin
```

##登录Mysql 数据库
```
mysql -uroot -p
```
登录远程服务器
```
mysql -uroot -h192.168.1.xx -p
```
##MySQL登录参数
|参数|描述|
|:-----:|:-------:|
|-D --database=name |打开指定数据库|
|--delimiter = name |指定分隔符|
|-h --host = name |服务器名称|
|-p,--password[=name]|密码|
|-P,--port=3306|端口号|
|--prompt=name|设置提示符的名称mysql -uroot -p --prompt=\D\d\h\u 代表当前的日期，数据库，服务器名称，登录用户|
|-u,--user=name|用户名|
|-V --version|输出版本信息,或者登录成功以后 SELECT VERSION();|
## MySQL退出
```
mysql> exit
mysql> quit
mysql> \q
```
##Mysql 配置文件
```
mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.24, for macos10.14 (x86_64) using  EditLine wrapper

Connection id:		4
Current database:
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		5.7.24 MySQL Community Server (GPL)
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	latin1
Db     characterset:	latin1
Client characterset:	utf8
Conn.  characterset:	utf8
UNIX socket:		/tmp/mysql.sock
Uptime:			3 hours 11 min 3 sec
```
增加配置文件sudo vim my.cnf,默认字符集设置成utf-8
```
[client]
  default-character-set=utf8
[mysqld]
  character-set-server=utf8
```
重新启动
```
sudo ./mysql.server restart
```
