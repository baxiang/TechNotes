注：本文是《Go语言核心编程》（李文塔/著）个人读书笔记
####编译环境
编译go源代码
Go1.5起Go的编译器完全使用Go重写，要源码安装Go需要有Go的编译环境，需要下载 1.4 版本使用C语言编写的Go编译器源码，通过 Linux自带的gcc先编译出 一个 Go 环境，然后拿这个Go环境编译新版本的Go环境 。
重要的环境变量
\$GOROOT
`$GOROOT` 是 Go 的安装根目录 。Linux 下的环境默认是／usr/local/go，如果`$GOROOT`位于上述位置，则不需要显式地设置`$GO ROOT` 环境变量；如果不是默认安装目录，则需要显式地设置`$GOROOT` 环境变量
\$GOPATH
\$GOPATH 是 Go 语言编程的工作目录（workspace)如果没有设置 GOPATH 环境变量，则 Linux 下系统默认是 $HOME/go。
####第三包管理
vendor
Go1.5引入了vendor机制，手动设置环境变量GO15VENDOREXPERIMENT= 1，编译器才能启用vendor,从 Go 1.6 起，默认开启vendor目录查找。查找第三方包的流程：
1如果当前包下有 vendor 目 录 ，则从其下查找第三方的包，如果没有找到，则继续执行下一步操作 。
2如果当前包目录下没有 vendor 目录，则沿当前包目录向上逐级目录查找 vendor 目录，直到找到$GOPATH/src 下的 vendor 目录，只要找到 vendor 目录就去其下查找第三方的 包，如果没有则继续执行下一步操作。
3在 GOPATH 下面查找依赖包 。
4在 GOROOT 目录下查找依赖包 。
vendor 将原来放在$GOPATH/src 的第三方包放到当前工程的 vendor 目录中进行管理。 它为工程独立的管理自己所依赖第三方包提供了保证 ，多个工程独立地管理自己的第三方依赖包， 它们之间不会相互影响 。 vendor将原来包共享模式转换为每个工程独立维护的模式， vendor的另一个好处是保证了工程目录下代码的完整性，将工程代码复制到其他 Go 编译环境，不需要再去下载第三方包 ，直接就能编译就行了。
vendor有一个重要的问题没有解决第三包的版本管理，go get -u 更新第三方包。 默认的是将工程的默认分支的最新版本拉取到本地。
####dep
安装dep
```
go get - u github.com/golang/dep/cmd/dep
```
使用 `dep init` 命令初始化工程,该命令可以用于新项目，也可以用于己经存在的项目 。Gopkg.toml 可以由用户自由配置，包括依赖的 source、branch、version 等。 Gopkg.lock 仅描述工程当前第三方包版本视图。
dep ensure
手动更新 Gopkg.toml 需要运行 `dep ensure `来重新生成 Gopkg.lock 并更新vendor，
