
![Screenshot from 2017-09-06 23-52-49.png](http://upload-images.jianshu.io/upload_images/143845-8e2616d97da80b4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##安装shadowsocks-qt5
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
##配置shadowsocks-qt5
connection(链接)-> add(添加)
![Screenshot from 2017-09-06 23-40-21.png](http://upload-images.jianshu.io/upload_images/143845-d1cd1d20c3c1ca2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
sudo apt-get install python-pip
sudo pip install genpac

```

##配置Network Proxy
```
genpac -p "SOCKS5 127.0.0.1:1080" --output="autoproxy.pac"
```
会在在/home/用户名/下生成autoproxy.pac，打开SystemSetting->Network->Network Proxy，将Method改为Automatic，Configuration Url填”file:///home/用户名/autoproxy.pac”，然后Apply System Wide即可 。

####设置自动代理
genpac，基于 gfwlist 的代理自动配置(Proxy Auto-config)文件生成工具，支持自定义规则
github 上有详细的内容。[https://github.com/JinnLynn/genpac](https://github.com/JinnLynn/genpac)


````
$ sudo pip install genpac
[sudo] baxiang 的密码：
WARNING: Running pip install with root privileges is generally not a good idea. Try `pip install --user` instead.
Collecting genpac
  Downloading https://files.pythonhosted.org/packages/48/fb/b8f9cce57c4ea856e7dd1f9fb917df2896d7e62d43c50ed1af2e50a4e57d/genpac-2.1.0.tar.gz (102kB)
    100% |████████████████████████████████| 112kB 498kB/s 
Installing collected packages: genpac
  Running setup.py install for genpac ... done
Successfully installed genpac-2.1.0
$mkdir vpnPAC
$cd vpnPAC
$sudo genpac --proxy="SOCKS5 127.0.0.1:1086" --gfwlist-proxy="SOCKS5 127.0.0.1:1086" -o autoproxy.pac --gfwlist-url="https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt"


打开 network ，网络代理中方法选择自动配制 URL 文件选项为上面生成的文件比如：
```
file:///home/用户名/vpnPAC/autoproxy.pac
````
![image.png](https://upload-images.jianshu.io/upload_images/143845-edc0324ef51bb6c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Fedora安装shadowsocks-qt5
https://github.com/shadowsocks/shadowsocks-qt5

1.  使用`dnf`添加shadowsocks的Copr源： `sudo dnf copr enable librehat/shadowsocks`
2.  使用`dnf`更新cache并安装：
```
sudo dnf update
sudo dnf install shadowsocks-qt5
```
如果使用传统的`yum`包管理工具的话，需要从[Copr](https://copr.fedoraproject.org/coprs/librehat/shadowsocks/)下载相应版本的repo文件放到`/etc/yum.repos.d/`下，然后通过`yum`安装：
```
sudo yum update
sudo yum install shadowsocks-qt5
```
具体参考文章https://github.com/shadowsocks/shadowsocks-qt5/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97
