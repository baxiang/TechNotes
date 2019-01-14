```sql
CREATE USER `baxiang`@`%`;

GRANT Create ON *.* TO `baxiang`@`%`;
```

查看当前的uer，其中host 值是localhost，127.0.0.1 是只允许本机登陆,如果需要远程登录需要host设置成%（百分号）
```
mysql> use mysql;
mysql>select Host,User FROM user;
+-------------------------+------+
| Host                    | User |
+-------------------------+------+
| 127.0.0.1               | root |
| ::1                     | root |
| izhp3f2wn461rak5gw97qmz |      |
| izhp3f2wn461rak5gw97qmz | root |
| localhost               |      |
| localhost               | root |
+-------------------------+------+
6 rows in set (0.00 sec)
```
创建远程登录用户
```
CREATE USER 'test'@'%' IDENTIFIED BY 'test';
```
