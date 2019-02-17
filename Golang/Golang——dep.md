####安装方式
执行 curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
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
####dep
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

#### 初始化
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
