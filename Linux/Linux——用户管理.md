####用户权限存储文件
|权限文件地址|权限信息|
|----|----|
|/etc/group |存储当前系统中所有用户组信息|
|/etc/gshadow |存储当前系统中用户组的密码信息|
|/etc/passwd |存储当前系统中所有用户的信息|
|/etc/shadow |储存当前系统中所有用户的密码信息|
|/etc/nologin|默认不存在，当创建这个名称的文件则禁止非root用户登录|
**/etc/passwd**
从文件名称看是存储密码相关的，但是这个已经是历史，心在主要存储的使用户名称
```
# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
```
1、账号名称
2、原先用来保存密码的，现在密码都放在/etc/shadow中，所以这里显示x
3、UID，也就是使用者ID。默认的系统管理员的UID为0，我们添加用户的时候最好使用1000以上的UID，1-1000范围的UID最好保留给系统用。
4、GID，也就是群组ID
5、关于账号的一些说明信息（暂时可以忽略）
6、账号的家目录，家目录就是你登陆系统后默认的那个目录
7、账号使用的shell
总结起来就是   ``用户名:密码:UID:GID:用户全名:home 目录:shell``
**/etc/group**
```
# cat /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:
```
其数据含义是``组名称:组密码:GID:用户组类的用户名``
####useradd
在centos版本中adduser和useradd这两个命令是一样的。
增加用户
```
useradd(选项)(要创建的用户名)
```
选项
```
-c<备注>：加上备注文字。备注文字会保存在passwd的备注栏位中
-d<登入目录>：指定用户登入时的启始目录；
-D：变更预设值；
-e<有效期限>：指定帐号的有效期限；
-f<缓冲天数>：指定在密码过期后多少天即关闭该帐号；
-g<群组>：指定用户所属的群组；
-G<群组>：指定用户所属的附加群组；
-m：自动建立用户的登入目录；
-M：不要自动建立用户的登入目录；
-n：取消建立以用户名称为名的群组；
-r：建立系统帐号；
-s<shell>：指定用户登入后所使用的shell；
-u<uid>：指定用户
```
####usermod
用于修改用户的基本信息
```
usermod(选项)(用户名称)
```
选项
```
 -c, --comment 注释            GECOS 字段的新值
  -d, --home HOME_DIR           用户的新主目录
  -e, --expiredate EXPIRE_DATE  设定帐户过期的日期为 EXPIRE_DATE
  -f, --inactive INACTIVE       过期 INACTIVE 天数后，设定密码为失效状态
  -g, --gid GROUP               强制使用 GROUP 为新主组
  -G, --groups GROUPS           新的附加组列表 GROUPS
  -a, --append GROUP            将用户追加至上边 -G 中提到的附加组中，
                                并不从其它组中删除此用户
  -h, --help                    显示此帮助信息并推出
  -l, --login LOGIN             新的登录名称
  -L, --lock                    锁定用户帐号
  -m, --move-home               将家目录内容移至新位置 (仅于 -d 一起使用)
  -o, --non-unique              允许使用重复的(非唯一的) UID
  -p, --password PASSWORD       将加密过的密码 (PASSWORD) 设为新密码
  -R, --root CHROOT_DIR         chroot 到的目录
  -s, --shell SHELL             该用户帐号的新登录 shell
  -u, --uid UID                 用户帐号的新 UID
  -U, --unlock                  解锁用户帐号
  -Z, --selinux-user  SEUSER       用户账户的新 SELinux 用户映射
```
####passwd
如果是普通用户执行passwd只能修改自己的密码。如果新建用户后，要为新用户创建密码，则用passwd用户名，注意要以root用户的权限来创建。
语法
```
passwd [选项...] <帐号名称>
```
选项
```
-k, --keep-tokens       保持身份验证令牌不过期
  -d, --delete            删除已命名帐号的密码(只有根用户才能进行此操作)
  -l, --lock              锁定指名帐户的密码(仅限 root 用户)
  -u, --unlock            解锁指名账户的密码(仅限 root 用户)
  -e, --expire            终止指名帐户的密码(仅限 root 用户)
  -f, --force             强制执行操作
  -x, --maximum=DAYS      密码的最长有效时限(只有根用户才能进行此操作)
  -n, --minimum=DAYS      密码的最短有效时限(只有根用户才能进行此操作)
  -w, --warning=DAYS      在密码过期前多少天开始提醒用户(只有根用户才能进行此操作)
  -i, --inactive=DAYS     当密码过期后经过多少天该帐号会被禁用(只有根用户才能进行此操作)
  -S, --status            报告已命名帐号的密码状态(只有根用户才能进行此操作)
  --stdin                 从标准输入读取令牌(只有根用户才能进行此操作)
```
####userdel
用于删除给定的用户
```
userdel(选项)(用户名)
```
选项
```
-f：强制删除用户，即使用户当前已登录
-r：删除用户的同时，删除与用户相关的所有文件
```
####su
 切换用户名
```
su(选项)(切换用户名)
```
选项
```
-c<指令>或--command=<指令>：执行完指定的指令后，即恢复原来的身份；
-f或--fast：适用于csh与tsch，使shell不用去读取启动文件；
-l或--login：改变身份时，也同时变更工作目录，以及HOME,SHELL,USER,logname。此外，也会变更PATH变量；
-m,-p或--preserve-environment：变更身份时，不要变更环境变量；
-s<shell>或--shell=<shell>：指定要执行的shell；
--help：显示帮助；</pre>
显示帮助；
--version；显示版本信息。</pre>
```

####groupadd
创建一个新的工作组
```
groupadd(选项)(用户组名)
```
选项
```
-g：指定新建工作组的ID号
-r：创建系统工作组，系统工作组的组ID小于500；
-K：覆盖配置文件“/ect/login.defs”；
-o：允许添加组ID号不唯一的工作组。
```
####groupdel
删除用户组
```
groupdel (用户组名)
```
#####gpasswd
管理工作组
用法
```
gpasswd [选项] 组
```

选项：
```
  -a, --add USER                向组 GROUP 中添加用户 USER
  -d, --delete USER             从组 GROUP 中添加或删除用户
  -h, --help                    显示此帮助信息并推出
  -Q, --root CHROOT_DIR         要 chroot 进的目录
  -r, --delete-password         remove the GROUP's password
  -R, --restrict                向其成员限制访问组 GROUP
  -M, --members USER,...        设置组 GROUP 的成员列表
  -A, --administrators ADMIN,...	设置组的管理员列表
```

#### newgrp 
切换用户所在用户组命令 登入另一个群组。 
用法：
```
newgrp [-] [组]
```
####id
显示指定用户信息，包括用户编号，用户名
####groups
显示每个输入的用户名所在的全部组，如果没有指定用户名则默认为当前进程用户(当用户组数据库发生变更时可能导致差异)。
用法：
```
groups [选项]... [用户名]...
```
####sudo
在Ubuntu或者fedora偏向于日常使用，所有在除root用户以为存在管理员角色可以执行sudo命令 但是在centos等服务器版本的一般除了root其他用户是无法使用sudo命令的
```
$ sudo cat /etc/passwd
[sudo] test 的密码：
test 不在 sudoers 文件中。此事将被报告。
```
修改/etc/sudoers
```
## Allow root to run any commands anywhere
root	ALL=(ALL) 	ALL

## Allows members of the 'sys' group to run networking, software,
## service management apps and more.
# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL
```
wheel 用户无需密码使用sudo权限，一般不建议开启
```
## Same thing without a password
# %wheel	ALL=(ALL)	NOPASSWD: ALL
```
因此我们可以执行将当前用户增加到wheel组
```
usermod -g wheel test
```
