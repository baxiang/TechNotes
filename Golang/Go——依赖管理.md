##vendor
在Go1.5 release的版本的发布vendor目录被添加到除了GOPATH和GOROOT之外的依赖目录查找的解决方法。
查找依赖包路径的解决
当前包下的vendor目录
先上级的目录查找，直到找到scr的vendor目录
在GOPATH下面查找依赖包
在GOROOT目录下查找
##dep
dep安装方式的安装方式是:执行 curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
```
$ curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5140  100  5140    0     0   3346      0  0:00:01  0:00:01 --:--:--  3346
ARCH = amd64
OS = linux
Will install into /home/baxiang/go/bin
Fetching https://github.com/golang/dep/releases/latest..
Release Tag = v0.5.0
Fetching https://github.com/golang/dep/releases/tag/v0.5.0..
Fetching https://github.com/golang/dep/releases/download/v0.5.0/dep-linux-amd64..
Setting executable permissions.
Moving executable to /home/baxiang/go/bin/dep
```
MacOS 使用 Homebrew安装
```
$ brew install dep
$ brew upgrade dep
```
Linux Apt安装
```
sudo apt install go-dep
```
通过go get 按照
```
go get -u github.com/golang/dep/cmd/dep
````
######dep常用命令
```
$ dep
Dep is a tool for managing dependencies for Go projects

Usage: "dep [command]"

Commands:

  init     Set up a new Go project, or migrate an existing one
  status   Report the status of the project's dependencies
  ensure   Ensure a dependency is safely vendored in the project
  version  Show the dep version information
  check    Check if imports, Gopkg.toml, and Gopkg.lock are in sync

Examples:
  dep init                               set up a new project
  dep ensure                             install the project's dependencies
  dep ensure -update                     update the locked versions of all dependencies
  dep ensure -add github.com/pkg/errors  add a dependency to the project

Use "dep help [command]" for more information about a command.

```
######初始化
文件目录结构
```
$ tree
.
├── Gopkg.lock
├── Gopkg.toml
└── vendor

1 directory, 2 files
```
![图片.png](https://upload-images.jianshu.io/upload_images/143845-32d90156c64565e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在Gopkg.toml编辑
```
[[constraint]]
     name = "github.com/pkg/errors"
     source = "github.com/pkg/errors"
     version = "0.8.1"

[[constraint]]
     name = "github.com/gorilla/websocket"
     source = "github.com/gorilla/websocket"
     version = "1.4.0"

[[constraint]]
     name = "github.com/skip2/go-qrcode"
     source = "github.com/skip2/go-qrcode"
     branch = "master"
```
##go mod
1.  升级golang 版本到 1.12 [Go下载](https://links.jianshu.com/go?to=https%3A%2F%2Fgolang.org%2Fdl%2F)
设置环境变量
```
export GO111MODULE=on
bogon:~ baxiang$ source ~/.bash_profile
bogon:~ baxiang$ echo $GO111MODULE
auto
```
go mod 命令
```
go mod help
Go mod provides access to operations on modules.

Note that support for modules is built into all the go commands,
not just 'go mod'. For example, day-to-day adding, removing, upgrading,
and downgrading of dependencies should be done using 'go get'.
See 'go help modules' for an overview of module functionality.

Usage:

	go mod <command> [arguments]

The commands are:

	download    download modules to local cache
	edit        edit go.mod from tools or scripts
	graph       print module requirement graph
	init        initialize new module in current directory
	tidy        add missing and remove unused modules
	vendor      make vendored copy of dependencies
	verify      verify dependencies have expected content
	why         explain why packages or modules are needed

Use "go help mod <command>" for more information about a command.
```
go mod init [module]：初始化.mod 包管理文件到当前工程。
go mod vendor：vendor版本的解决方案，将依赖复制到vendor下面。
go mod tidy：移除未用的模块，以及添加缺失的模块。
go mod verify：验证所有模块是否正确。
######初始化模块
```
go mod init
go: modules disabled inside GOPATH/src by GO111MODULE=auto; see 'go help modules'
```

在当前目录下，命令行运行 go mod init + 模块名称 初始化模块

```
export GO111MODULE=on
$  go mod init server
go: creating new go.mod: module mServer
go: copying requirements from Gopkg.lock
```

运行完后，会在当前项目目录下生成一个**go.mod** 文件，这是一个关键文件，之后的包的管理都是通过这个文件管理。
除了go.mod之外，go命令还维护一个名为go.sum的文件，其中包含特定模块版本内容的预期加密哈希，go.sum 文件不需要手工维护。


####go.mod工作机制
main.go文件：

```
package main

import (
	"github.com/gin-contrib/pprof"
	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()
	pprof.Register(router)
	router.Run(":8000")
}

```
按照过去的做法，要运行main.go需要执行`go get` 命令 下载gin包到 $GOPATH/src。
```
go get -u github.com/gin-gonic/gin
```
go 会自动查找代码中的包，下载依赖包，并且把具体的依赖关系和版本写入到go.mod和go.sum文件中。
查看go.mod，它会变成这样：

```
module baxiang.cn/GoNote

go 1.12

replace (
	golang.org/x/text => github.com/golang/text v0.3.0
	golang.org/x/text v0.3.0 => github.com/golang/text v0.3.0
)

require (
	github.com/gin-contrib/pprof v1.2.0
	github.com/gin-contrib/sse v0.0.0-20190301062529-5545eab6dad3 // indirect
	github.com/gin-gonic/gin v1.3.0
	github.com/golang/protobuf v1.3.1 // indirect
	github.com/json-iterator/go v1.1.6 // indirect
	github.com/mattn/go-isatty v0.0.7 // indirect
	github.com/ugorji/go v1.1.4 // indirect
)

```
require关键子是引用，后面是包，最后v1.3.0 是引用的版本号
使用Go mod依赖的第三方包被默认下载到`$GOPATH/pkg/mod`路径下。

####go mod使用vendor目录
如果你不喜欢 go mod 的缓存方式，你可以使用vendor命令回到 godep 或 govendor 使用的 vendor 目录进行包管理的方式。
```
go mod vendor 
```
当然这个命令并不能让你从godep之类的工具迁移到 go modules，它只是单纯地把 go.sum 中的所有依赖下载到 vendor 目录里，如果你用它迁移 godep 你会发现 vendor 目录里的包会和 godep 指定的产生相当大的差异，所以请务必不要这样做。
使用 go build -mod=vendor 来构建项目，因为在 go modules 模式下 go build 是屏蔽 vendor 机制的，所以需要特定参数重新开启 vendor 机制:
```
go build -mod=vendor
```
构建成功。当发布时也只需要和使用 godep 一样将 vendor 目录带上即可

####依赖包的版本管理
```
:gin-gonic baxiang$ tree  -L 1
.
├── gin@v0.0.0-20190328061400-ce20f107f5dc
├── gin@v1.1.4
└── gin@v1.3.0

3 directories, 0 files
```
在上一个问题里，可以看到最终下载在`$GOPATH/pkg/mod` 下的`github.com/gin-gonic` 的gin包@ vx.x.x，代表着不同的version，`$GOPATH/pkg/mod`里可以保存相同包的不同版本。

版本是在go.mod中指定的。

如果，在go.mod中没有指定，go命令会自动下载代码中的依赖的最新版本，本例就是自动下载最新的版本。

如果，在go.mod用require语句指定包和版本 ，go命令会根据指定的路径和版本下载包，
指定版本时可以用`latest`，这样它会自动下载指定包的最新版本；如果包的作者还没有标记版本，默认为 v0.0.0

####go get 命令
新版 `go get` 可以在末尾加 `@` 符号，用来指定版本。
它要求仓库必须用 `v1.2.0` 格式打 tag，像 `v1.2` 少个零都不行的，必须是[语义化](https://semver.org/lang/zh-CN/)的、带 `v` 前缀的版本号。其中`latest` 匹配最新的 tag。
```
go get github.com/gorilla/mux    # 匹配最新的一个 tag
go get github.com/gorilla/mux@latest    # 和上面一样
go get github.com/gorilla/mux@v1.6.2    # 匹配 v1.6.2
go get github.com/gorilla/mux@e3702bed2 # 匹配 v1.6.2
go get github.com/gorilla/mux@c856192   # 匹配 c85619274f5d
go get github.com/gorilla/mux@master    # 匹配 master 分支

```
####replace
 golang.org/x/… 等包在中国大陆区域无法下载
依赖包地址变更
在go.mod文件里用 replace 替换包地址
```
replace (
	golang.org/x/text => github.com/golang/text latest
	golang.org/x/text v0.3.0 => github.com/golang/text v0.3.0
)

```
go mod会用 github.com/golang/text 替代golang.org/x/text，原理就是下载github.com/golang/text 的最新版本到 `$GOPATH/pkg/mod/golang.org/x/text`下。
####goproxy.io
goproxy.io [ Github ](https://github.com/goproxyio/goproxy)可以给go modules 设置全局代理,下载golang/x的包文件 也就就不需要设置replace
Linux 和mac OS
```shell
export GOPROXY=https://goproxy.io
```
powershell (windows)
```
$env:GOPROXY = "https://goproxy.io"
```



