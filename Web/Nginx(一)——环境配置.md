##nginx官方地址
nginx官方地址https://nginx.org/en/download.html
Nginx 服务器三种版本的下载，分别是开发版（Mainline version）、稳定版本（Stable version）和过期版本（Legacy versions）。
“CHANGES-x.xx”链接，记录的是对应版本的功能变更日志。包括新增功能、功能的优化和功能缺陷的修复等。
紧接着“CHANGES-x.xx”链接后面的“nginx-x.x.x”链接，是 Nginx服务器的 Linux版本下载地址。
“pgp”链接，记录的是提供下载的版本使用PGP加密自由软件GnuPG计算后的签名。PGP可以理解为Pretty Good Privacy。这些数据可以用于下载文件的验证。
“nginx/Windows-x.x.x”链接，是 Nginx 服务器的Windows版本下载地址。
##CentOS安装
#### yum 安装
```
rpm -ivh https://nginx.org/packages/centos/7/x86_64/RPMS/nginx-1.14.0-1.el7_4.ngx.x86_64.rpm
```
yum 安装nginx
```
yum install -y nginx
````
## ubuntu 安装
```
apt-get install Nginx
```
##Mac 使用brew安装
```
$ brew install nginx
==> Installing dependencies for nginx: openssl, pcre
==> Installing nginx dependency: openssl
==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2p.high_sierra.
######################################################################## 100.0%
==> Pouring openssl--1.0.2p.high_sierra.bottle.tar.gz
==> Caveats
A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

openssl is keg-only, which means it was not symlinked into /usr/local,
because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.

If you need to have openssl first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile

For compilers to find openssl you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl/include"

For pkg-config to find openssl you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig"

==> Summary
🍺  /usr/local/Cellar/openssl/1.0.2p: 1,793 files, 12.3MB
==> Installing nginx dependency: pcre
==> Downloading https://homebrew.bintray.com/bottles/pcre-8.42.high_sierra.bottl
######################################################################## 100.0%
==> Pouring pcre--8.42.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/pcre/8.42: 204 files, 5.3MB
==> Installing nginx
==> Downloading https://homebrew.bintray.com/bottles/nginx-1.15.2.high_sierra.bo
######################################################################## 100.0%
==> Pouring nginx--1.15.2.high_sierra.bottle.tar.gz
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  nginx
==> Summary
🍺  /usr/local/Cellar/nginx/1.15.2: 23 files, 1.4MB
==> Caveats
==> openssl
A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

openssl is keg-only, which means it was not symlinked into /usr/local,
because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.

If you need to have openssl first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile

For compilers to find openssl you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl/include"

For pkg-config to find openssl you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig"

==> nginx
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  nginx
```
