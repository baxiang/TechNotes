
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

##安装genpac
```
sudo apt-get install python-pip
sudo pip install genpac

```

##配置Network Proxy
```
genpac -p "SOCKS5 127.0.0.1:1080" --output="autoproxy.pac"
```
会在在/home/用户名/下生成autoproxy.pac，打开SystemSetting->Network->Network Proxy，将Method改为Automatic，Configuration Url填”file:///home/用户名/autoproxy.pac”，然后Apply System Wide即可 。
