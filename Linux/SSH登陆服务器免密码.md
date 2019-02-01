但是后来发现原本好使的SSH再次登陆服务器时却提示：Permission denied (publickey).的错误。解决办法是用 ssh-add privateKey
ssh-add 永久将私钥添加到 Keychain
      我们配置完SSH之后执行 ssh-add privateKey 将 SSH 的私钥添加进去，但是发现了一个问题就是每次重启电脑后都需要重新 ssh-add，显然每次重启后都需要重新添加让我等程序员肯定受不了，解决办法就是在添加 ssh 私钥的时候使用如下命令： ssh-add -K privateKey，即可一劳永逸将私钥添加进 Mac 本身的钥匙串中，即 Keychain。下面简单解释下原理。

首先得了解一件事：ssh-add 这个命令不是用来永久性的记住你所使用的私钥的。实际上，它的作用只是把你指定的私钥添加到 ssh-agent 所管理的一个 session 当中。而 ssh-agent 是一个用于存储私钥的临时性的 session 服务，也就是说当你重启之后，ssh-agent 服务也就重置了，session 会话也就失效了。

既然 ssh-agent 是个临时的，那么对于 Mac 来说，哪里可以永久存储的，显然就是 Keychain 了，在执行 ssh-add -K privateKey 后可


Apple updated its [Technical Notes](https://developer.apple.com/library/content/technotes/tn2449/_index.html#//apple_ref/doc/uid/DTS40017589) to indicate that since 10.12.2, macOS includes version 7.3p1 of OpenSSH and its new behaviors.

In `~/.ssh` create `config` file with the following content:

```
Host * (asterisk for all hosts or add specific host)
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile <key> (e.g. ~/.ssh/userKey)
```
添加远程登录公钥
```
ssh-copy-id -i ~/.ssh/id_rsa.pub 远程用户名@IP地址
```
查看 远程登录的公钥信息
```
cat ~/.ssh/authorized_keys
```

## umperServer 免密登录

用了JumperServer后，每次要连远程服务器，需要选一次私钥文件，输入两次密码，输入一次ip，非常麻烦，能不能一个简短的指令就搞定呢？答案是可以的

简化步骤：

1.  将jumperServer 私钥转换为 无密码私钥
2.  ssh-add 将私钥添加到 Keychain
3.  设置命令 alias，简化命令

### [](https://ifengkou.github.io/JumperServer%20%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95.html#1-%E5%B0%86jumperServer-%E7%A7%81%E9%92%A5%E8%BD%AC%E6%8D%A2%E4%B8%BA-%E6%97%A0%E5%AF%86%E7%A0%81%E7%A7%81%E9%92%A5 "1\. 将jumperServer 私钥转换为    无密码私钥")1\. 将jumperServer 私钥转换为 无密码私钥

使用openssl将私钥转换为无密码私钥

```
# 指令
openssl rsa -in server.key -out server2.key
# 实际
openssl rsa -in shenlongguang_aliyun.pem -out shenlongguang_aliyun_passwordless.pem

```

### [](https://ifengkou.github.io/JumperServer%20%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95.html#2-ssh-add-%E5%B0%86%E7%A7%81%E9%92%A5%E6%B7%BB%E5%8A%A0%E5%88%B0-Keychain "2\. ssh-add 将私钥添加到 Keychain")2\. ssh-add 将私钥添加到 Keychain

```
ssh-add -k /Users/sloong/Documents/company/analysys/jumperServerKeys/shenlongguang_aliyun_passwordless.pem

```

如果出现下面异常，则是私钥文件的权限问题：

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/Users/sloong/Documents/company/analysys/jumperServerKeys/shenlongguang_aliyun_passwordless.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.

# 修改文件权限
chmod 700 shenlongguang_aliyun_passwordless.pem

```

### [](https://ifengkou.github.io/JumperServer%20%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95.html#3-%E8%AE%BE%E7%BD%AE%E5%91%BD%E4%BB%A4-alias%EF%BC%8C%E7%AE%80%E5%8C%96%E8%BE%93%E5%85%A5 "3\. 设置命令 alias，简化输入")3\. 设置命令 alias，简化输入

修改 `~/.zshrc` 文件(bash 也差不多），增加 下列 alias：

```
alias ssh-add-ali='ssh-add -k /Users/sloong/Documents/company/analysys/jumperServerKeys/shenlongguang_aliyun_passwordless.pem'
alias ssh-ali='ssh shenlongguang@123.56.25.ip'

```

这样执行 `ssh-add-ali` 就可以直接添加私钥到 Keychain（重启后执行一次即可，重启后会失效）。下次需要ssh 到远程服务器 只需要执行 `ssh-ali` 即可，ip／密码什么都不用输入

```
~ ssh-add-uc
Identity added: /Users/sloong/xxx/shenlongguang_aliyun_passwordless.pem)
~ ssh-ali
Last login: Wed May  3 09:25:25 2017 from 218.76.1.ip

Welcome to aliyun Elastic Compute Service!
......
```








