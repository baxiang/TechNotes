##Linux的压缩包安装方式
Golang的官方下载地址https://golang.org/dl/
![image.png](https://upload-images.jianshu.io/upload_images/143845-e6b046e71caf9552.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1. 下载最新版本的压缩包
```
wget https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz
```
2. 解压安装包 安装包的解压路径放在/usr/local 
```
tar -C /usr/local -xzf go1.10.3.linux-amd64.tar.gz
```
3. 配置环境变量.打开profile文件
```
vim /etc/profile
```
4 在文档最后，添加
```
export PATH=$PATH:/usr/local/go/bin
```
5 wq保存退出，然后运行
```
source /etc/profile
```
##Linux的源码安装方式
如果是使用源码安装需要安装1.4版本的，然后在安装后续升级的版本。
```
ubuntu@VM-0-12-ubuntu:~$ wget https://dl.google.com/go/go1.4-bootstrap-20171003.tar.gz
--2018-03-09 02:21:03--  https://dl.google.com/go/go1.4-bootstrap-20171003.tar.gz
Resolving dl.google.com (dl.google.com)... 203.208.51.36, 203.208.51.33, 203.208.51.41, ...
Connecting to dl.google.com (dl.google.com)|203.208.51.36|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11009739 (10M) [application/octet-stream]
Saving to: ‘go1.4-bootstrap-20171003.tar.gz’

go1.4-bootstrap-20171003.tar.gz 100%[=====================================================>]  10.50M  2.39MB/s    in 3.5s    

2018-03-09 02:21:07 (3.04 MB/s) - ‘go1.4-bootstrap-20171003.tar.gz’ saved [11009739/11009739]

ubuntu@VM-0-12-ubuntu:~$ tar -xf go1.4-bootstrap-20171003.tar.gz 
ubuntu@VM-0-12-ubuntu:~$ cd go/src
ubuntu@VM-0-12-ubuntu:~/go/src$ ./make.bash 
# Building C bootstrap tool.
```
安装1.4成功
```
---
Installed Go for linux/386 in /home/ubuntu/go
Installed commands in /home/ubuntu/go/bin
ubuntu@VM-0-12-ubuntu:~/go/src$ cd ../
ubuntu@VM-0-12-ubuntu:~/go$ cd ../
ubuntu@VM-0-12-ubuntu:~$ ls
download  go  go1.4-bootstrap-20171003.tar.gz
ubuntu@VM-0-12-ubuntu:~$ mv go go1.4
```
安装最新版本的1.10
```
wget https://studygolang.com/dl/golang/go1.10.src.tar.gz
--2018-03-09 02:25:05--  https://studygolang.com/dl/golang/go1.10.src.tar.gz
Resolving studygolang.com (studygolang.com)... 59.110.219.94
Connecting to studygolang.com (studygolang.com)|59.110.219.94|:443... connected.
HTTP request sent, awaiting response... 303 See Other
Location: https://dl.google.com/go/go1.10.src.tar.gz [following]
--2018-03-09 02:25:06--  https://dl.google.com/go/go1.10.src.tar.gz
Resolving dl.google.com (dl.google.com)... 203.208.51.34, 203.208.51.46, 203.208.51.36, ...
Connecting to dl.google.com (dl.google.com)|203.208.51.34|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 18300467 (17M) [application/octet-stream]
Saving to: ‘go1.10.src.tar.gz’

go1.10.src.tar.gz               100%[=====================================================>]  17.45M  2.39MB/s    in 6.4s    

2018-03-09 02:25:12 (2.74 MB/s) - ‘go1.10.src.tar.gz’ saved [18300467/18300467]

ubuntu@VM-0-12-ubuntu:~$ tar -xf go1.10.src.tar.gz 
ubuntu@VM-0-12-ubuntu:~$ cd go/src
ubuntu@VM-0-12-ubuntu:~/go/src$ ./all.bash 
```
1.10安装成功
```
ALL TESTS PASSED
---
Installed Go for linux/386 in /home/ubuntu/go
Installed commands in /home/ubuntu/go/bin
*** You need to add /home/ubuntu/go/bin to your PATH.
```
设置安装路径
```
ubuntu@VM-0-12-ubuntu:~$ sudo mv go /usr/local/go1.10
ubuntu@VM-0-12-ubuntu:~$ vim /etc/profile
ubuntu@VM-0-12-ubuntu:~$ source /etc/profile
ubuntu@VM-0-12-ubuntu:~$ go version
go version go1.10 linux/386
ubuntu@VM-0-12-ubuntu:~$ vim main.go
ubuntu@VM-0-12-ubuntu:~$ go run main.go 
hello, world
```

##MAC 安装Go
####安装Homebrew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
#### 卸载Homebrew
```
/usr/bin/ruby -e "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/master/uninstall](https://raw.githubusercontent.com/Homebrew/install/master/uninstall))"
```


#### Homebrew安装Go
```
brew install go
==> Downloading https://homebrew.bintray.com/bottles/go-1.9.2.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring go-1.9.2.high_sierra.bottle.tar.gz
==> Caveats
A valid GOPATH is required to use the `go get` command.
If $GOPATH is not specified, $HOME/go will be used by default:
  https://golang.org/doc/code.html#GOPATH

You may wish to add the GOROOT-based install location to your PATH:
  export PATH=$PATH:/usr/local/opt/go/libexec/bin
==> Summary
🍺  /usr/local/Cellar/go/1.9.2: 7,646 files, 293.9MB
```
##配置GOPATH
GOPATH其实就是一个指定的目录。在Golang开发时需要将其放在这个指定的目录下进行开发。而这个目录是由名为 GOPATH 的环境变量所控制，默认为 $HOME/go,GOPATH可以根据自己的项目目录设置 ，但是不能是 Go 的安装目录。当你使用 go get 来获取第三方的 package 时，程序也会自动将 package 下载到 GOPATH 目录中。
```
vim .bash_profile
```
增加bin配置
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
 退出vim，执行下面的命令完成对golang环境变量的配置。
```
source ~/.bash_profile
```

####验证安装是否成功
```
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```
接着通过 go 工具运行它：
```
$ go run hello.go
hello, world
```
## GOPATH目录结构
src - 存放 Go 程序的源代码。
pkg - 编译后的 package 对象文件。
bin - 可执行命令
src 目录通常包含多个版本控制仓库用来跟踪一个或多个源码包的开发。这个目录是你开发程序的主目录，所有的源码都是放在这个目录下面进行开发。通常的做法是一个目录为一个独立的项目。例如 $GOPATH/src/hello 就表示 hello 这个应用/包。
因此，每当开发一个新项目时，都需要在 $GOPATH/src/ 下新建一个文件夹用作开发。当你在引用其他包的时候，Go 程序也会以 $GOPATH/src/ 目录作为根目录进行查找。当然了， src 目录允许存在多级目录，例如在 src 下面新建了目录 $GOPATH/src/github.com/golang/bx/hello，那么这个包路径就是 github.com/golang/bx/hello，包名称为最后一个目录 hello。
## GO命令
```
go help
Go is a tool for managing Go source code.

Usage:

	go command [arguments]

The commands are:

	build       compile packages and dependencies
	clean       remove object files
	doc         show documentation for package or symbol
	env         print Go environment information
	bug         start a bug report
	fix         run go tool fix on packages
	fmt         run gofmt on package sources
	generate    generate Go files by processing source
	get         download and install packages and dependencies
	install     compile and install packages and dependencies
	list        list packages
	run         compile and run Go program
	test        test packages
	tool        run specified go tool
	version     print Go version
	vet         run go tool vet on packages

Use "go help [command]" for more information about a command.

Additional help topics:

	c           calling between Go and C
	buildmode   description of build modes
	filetype    file types
	gopath      GOPATH environment variable
	environment environment variables
	importpath  import path syntax
	packages    description of package lists
	testflag    description of testing flags
	testfunc    description of testing functions

Use "go help [topic]" for more information about that topic.
```
####go get
用于动态获取远程仓库中的代码包及其依赖，并进行安装。这个命令实际上分为两步：第一步是下载源码包，第二步是执行 go install 安装。下载源码包的 go 工具会自动根据不同的域名调用不同的源码工具。例如下载 Github 上的代码包时则是利用系统的 Git 工具。
所以为了 go get 能正常工作，你必须确保安装了合适的源码管理工具。另外，go get 支持自定义域名的功能，具体参见 go help remote。
常用参数：
```
-u - 强制使用网络去更新包和它的依赖包
-d - 只下载不安装
-fix - 在获取源码之后先运行fix，然后再去做其他的事情
-t - 同时也下载需要为运行测试所需要的包
-v - 显示执行的命令
```
####go run
直接从源码文件运行编译任务并执行编译文件。
####go build
这个命令主要用来编译代码，Go 是一门编译型语言，因此需要将源码编译为二进制文件之后才能执行。该命令即可将源码文件编译为对应的二进制文件。若有必要，会同时编译与之相关联的包。
如果是普通包（后续介绍），当执行 go build 之后，它不会产生任何文件。如果需要在 $GOPATH/pkg 下生成相应的文件，则需要执行 go install 命令完成。
如果是 main 包，当执行 go build 之后，它就会在当前目录下生成一个可执行文件。如果需要在 $GOPATH/bin 下生成相应的文件，需要执行 go install，或者使用 go build -o 路径。
如果某个项目文件夹下有多个文件，而你只想编译某个文件，可以在 go build 之后加上文件名，例如 go build hello.go，go build 命令默认会编译当前目录下的所有 go 文件。
可以通过 -o NAME 的方式指定 go build 命令编译之后的文件名，默认是 package 名（非 main 包），或者是第一个源文件的文件名（main 包）。
go build 会忽略目录下以“_”或“.”开头的 go 文件。
####go clean
移除当前源码包和关联源码包里面编译生成的文件。
####go fmt
Go 语言有标准的书写风格，不按照此风格的代码将不能编译通过，为了减少浪费在排版上的时间，go fmt 命令可以帮你格式化你写好的代码文件，使你写代码的时候不需要关心格式，你只需要在写完之后执行 go fmt filename.go，你的代码就被修改成了标准格式。
####go install
编译和安装包及其依赖。在内部实际上分成了两步操作：第一步是生成结果文件(可执行文件或者包)，第二步会把编译好的结果移到 $GOPATH/pkg 或者 $GOPATH/bin 中。
####go test
读取源码目录下面名为 *_test.go 的文件，生成并自动运行测试用的可执行文件。
```
-bench regexp - 执行相应的 benchmarks。
-cover - 开启测试覆盖率。
-run regexp - 只运行 regexp 匹配的函数。
-v - 显示测试的详细命令。
```
####go tool
一些有用的工具集合。常用的有：
```
go tool fix . 用来修复以前老版本的代码到新版本。
go tool vet directory|file 用来分析当前目录的代码是否都是正确的代码，例如函数里面提前 return 导致出现了无用代码之类的。
```
####godoc
为 go 程序自动提取和生成文档，或者查看某个 package 的文档。
例如，查看 fmt 的文档，使用 godoc fmt 即可。
支持查看包中的某个函数，例如 godoc fmt Printf。
支持使用 -http 参数开启一个本地服务，用于展示 golang.org 的上的官方文档。例如：godoc -http=:8080。
