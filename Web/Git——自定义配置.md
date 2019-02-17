Git 使用一系列的配置文件来存储你定义的偏好，它首先会查找/etc/gitconfig文件，该文件含有 对系统上所有用户及他们所拥有的仓库都生效的配置值（译注：gitconfig是全局配置文件）， 如果传递--system选项给git config命令， Git 会读写这个文件。
接下来 Git 会查找每个用户的~/.gitconfig文件，你能传递--global选项让 Git读写该文件。
最后 Git 会查找由用户定义的各个库中 Git 目录下的配置文件（.git/config），该文件中的值只对属主库有效。 以上阐述的三层配置从一般到特殊层层推进，如果定义的值有冲突，以后面层中定义的为准，例如：在.git/config和/etc/gitconfig的较量中， .git/config取得了胜利。虽然你也可以直接手动编辑这些配置文件，但是运行git config命令将会来得简单些。
```
$ git config --help

```
