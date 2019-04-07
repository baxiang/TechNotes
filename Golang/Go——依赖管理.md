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
####go mod
## 准备工作

1.  升级golang 版本到 1.12 [Go下载](https://links.jianshu.com/go?to=https%3A%2F%2Fgolang.org%2Fdl%2F)
2.  添加环境变量 `GO111MODULE`为 on
```
export GO111MODULE=on
```

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

## 创建一个项目
首先，在`$GOPATH/src`路径外的你喜欢的地方创建一个目录，cd 进入目录，新建一个hello.go文件，内容如下

```
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Hello, world!")
}

```

## 初始化模块

```
 go mod init
go: modules disabled inside GOPATH/src by GO111MODULE=auto; see 'go help modules'
```
设置环境变量
```
export GO111MODULE=on
bogon:~ baxiang$ source ~/.bash_profile
bogon:~ baxiang$ echo $GO111MODULE
auto
```
在当前目录下，命令行运行 go mod init + 模块名称 初始化模块

```
export GO111MODULE=on
$  go mod init mServer
go: creating new go.mod: module mServer
go: copying requirements from Gopkg.lock

```

运行完后，会在当前项目目录下生成一个**go.mod** 文件，这是一个关键文件，之后的包的管理都是通过这个文件管理。

> 官方说明：除了go.mod之外，go命令还维护一个名为go.sum的文件，其中包含特定模块版本内容的预期加密哈希
> go命令使用go.sum文件确保这些模块的未来下载检索与第一次下载相同的位，以确保项目所依赖的模块不会出现意外更改，无论是出于恶意、意外还是其他原因。 go.mod和go.sum都应检入版本控制。
> **go.sum** 不需要手工维护，所以可以不用太关注。

生成出来的文件包含模块名称和当前的go版本号

```
module hello

go 1.12

```
设置golang.org/x/sys 无法使用
```
go: converting Gopkg.lock: stat golang.org/x/sys@9eb1bfa1ce65ae8a6ff3114b0aaf9a41a6cf3560: unrecognized import path "golang.org/x/sys" (https fetch: Get https://golang.org/x/sys?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
go: converting Gopkg.lock: stat google.golang.org/appengine@v1.5.0: unrecognized import path "google.golang.org/appengine" (https fetch: Get https://google.golang.org/appengine?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
go: converting Gopkg.lock: stat golang.org/x/crypto@a5d413f7728c81fb97d96a2b722368945f651e78: unrecognized import path "golang.org/x/crypto" (https fetch: Get https://golang.org/x/crypto?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
go: converting Gopkg.lock: stat golang.org/x/text@v0.3.0: unrecognized import path "golang.org/x/text" (https fetch: Get https://golang.org/x/text?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
go: converting Gopkg.lock: stat golang.org/x/image@3fc05d484e9f77dd51816890e05f2602e4ca4d65: unrecognized import path "golang.org/x/image" (https fetch: Get https://golang.org/x/image?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
```

**注意**：子目录里是不需要init的，所有的子目录里的依赖都会组织在根目录的go.mod文件里

## 来吧，搞点小事情，看看go.mod怎么工作的

接下来，让你的项目依赖一下第三方包
以大部分人都熟悉的beego为例吧！
修改Hello.go文件：

```
package main

import "github.com/astaxie/beego"

func main() {
    beego.Run()
}
```
按照过去的做法，要运行hello.go需要执行`go get` 命令 下载beego包到 $GOPATH/src，但是，使用了新的包管理就不在需要这样做了。
直接 `go run hello.go`

稍等片刻… go 会自动查找代码中的包，下载依赖包，并且把具体的依赖关系和版本写入到go.mod和go.sum文件中。
查看go.mod，它会变成这样：

```
module hello

go 1.12

require github.com/astaxie/beego v1.11.1

```

`require` 关键子是引用，后面是包，最后v1.11.1 是引用的版本号

这样，一个使用Go包管理方式创建项目的小例子就完成了。

那么，接下来，几个小问题来了

### 问题一：依赖的包下载到哪里了？还在GOPATH里吗？

**不在。**
使用Go的包管理方式，依赖的第三方包被下载到了`$GOPATH/pkg/mod`路径下。
如果你成功运行了本例，可以在您的`$GOPATH/pkg/mod` 下找到一个这样的包 `github.com/astaxie/beego@v1.11.1`

使用 vendor 目录
如果你不喜欢 go mod 的缓存方式，你可以使用 go mod vendor 回到 godep 或 govendor 使用的 vendor 目录进行包管理的方式。

当然这个命令并不能让你从godep之类的工具迁移到 go modules，它只是单纯地把 go.sum 中的所有依赖下载到 vendor 目录里，如果你用它迁移 godep 你会发现 vendor 目录里的包会和 godep 指定的产生相当大的差异，所以请务必不要这样做。

使用 go build -mod=vendor 来构建项目，因为在 go modules 模式下 go build 是屏蔽 vendor 机制的，所以需要特定参数重新开启 vendor 机制:
```
go build -mod=vendor
./hello
hello world!
```
构建成功。当发布时也只需要和使用 godep 一样将 vendor 目录带上即可

### 问题二： 依赖包的版本是怎么控制的？

在上一个问题里，可以看到最终下载在`$GOPATH/pkg/mod` 下的包 `github.com/astaxie/beego@v1.11.1` 最后会有一个版本号 1.11.1，也就是说，`$GOPATH/pkg/mod`里可以保存相同包的不同版本。

版本是在go.mod中指定的。

如果，在go.mod中没有指定，go命令会自动下载代码中的依赖的最新版本，本例就是自动下载最新的版本。

如果，在go.mod用require语句指定包和版本 ，go命令会根据指定的路径和版本下载包，
指定版本时可以用`latest`，这样它会自动下载指定包的最新版本；

**依赖包的版本号是什么？** 是包的发布者标记的版本号，格式为 vn.n.n (n代表数字)，本例中的beego的历史版本可以在其代码仓库release看到[Releases · astaxie/beego · GitHub](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fastaxie%2Fbeego%2Freleases)

如果包的作者还没有标记版本，默认为 v0.0.0

## go get 命令

获取依赖的特定版本，用来升级和降级依赖。可以自动修改 `go.mod` 文件，而且依赖的依赖版本号也可能会变。在 `go.mod` 中使用 `exclude` 排除的包，不能 `go get` 下来。

与以前不同的是，新版 `go get` 可以在末尾加 `@` 符号，用来指定版本。

它要求仓库必须用 `v1.2.0` 格式打 tag，像 `v1.2` 少个零都不行的，必须是[语义化](https://semver.org/lang/zh-CN/)的、带 `v` 前缀的版本号。

```
go get github.com/gorilla/mux    # 匹配最新的一个 tag
go get github.com/gorilla/mux@latest    # 和上面一样
go get github.com/gorilla/mux@v1.6.2    # 匹配 v1.6.2
go get github.com/gorilla/mux@e3702bed2 # 匹配 v1.6.2
go get github.com/gorilla/mux@c856192   # 匹配 c85619274f5d
go get github.com/gorilla/mux@master    # 匹配 master 分支

```

`latest` 匹配最新的 tag。

### 问题三： 可以把项目放在$GOPATH/src下吗？

**可以。**
但是go会根据GO111MODULE的值而采取不同的处理方式
默认情况下，`GO111MODULE=auto` 自动模式

*   **auto** 自动模式下，项目在`$GOPATH/src`里会使用`$GOPATH/src`的依赖包，在$GOPATH/src外，就使用go.mod 里 require的包
*   **on** 开启模式，1.12后，无论在`$GOPATH/src`里还是在外面，都会使用go.mod 里 require的包
*   **off** 关闭模式，就是老规矩。

### 问题三： 依赖包中的地址失效了怎么办？比如 golang.org/x/… 下的包都无法下载怎么办？

在go快速发展的过程中，有一些依赖包地址变更了。
以前的做法

1.  修改源码，用新路径替换import的地址
2.  git clone 或 go get 新包后，copy到$GOPATH/src里旧的路径下
    无论什么方法，都不便于维护，特别是多人协同开发时。

使用go.mod就简单了，在go.mod文件里用 replace 替换包，例如

`replace golang.org/x/text => github.com/golang/text latest`

这样，go会用 github.com/golang/text 替代golang.org/x/text，原理就是下载github.com/golang/text 的最新版本到 `$GOPATH/pkg/mod/golang.org/x/text`下。

### 问题四： init生成的go.mod的模块名称有什么用？

本例里，用 go mod init hello 生成的go.mod文件里的第一行会申明
`module hello`

因为我们的项目已经不在`$GOPATH/src`里了，那么引用自己怎么办？就用模块名+路径。

例如，在项目下新建目录 utils，创建一个tools.go文件:

```
package utils

import “fmt”

func PrintText(text string) {
    fmt.Println(text)
}

```

在根目录下的hello.go文件就可以 import “hello/utils” 引用utils

```
package main
import (
"hello/utils"

"github.com/astaxie/beego"
)
func main() {
    utils.PrintText("Hi")
    beego.Run()
}
```
####问题五：以前老项目如何用新的包管理
1.  如果用auto模式，把项目移动到`$GOPATH/src`外
2.  进入目录，运行 `go mod init + 模块名称`
3.  `go build` 或者 `go run` 一次

