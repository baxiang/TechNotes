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

