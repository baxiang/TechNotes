###序言
    iOS组件化现阶段成为开发者讨论的热点问题，个人觉得这是iOS技术日渐成熟的表现，就跟一个人一样，最初是想着如何吃饱，现在开始琢磨如何变着花样吃好。不太想蹭组件化的热度，暂且不要关心这些字眼的意义，实际上不管是组件化还是模块化我们所要解决的问题是一致的：效率。就跟现今社会一样从农业 —工业—信息实际上就是社会生产效率的提高。所以评判我们开发工作成效第一标准应该是否提高了效率，而不是又加了多少班，代码如何的高深莫测。
   组件化/模块化实际上在计算机领域一直在使用，包括我们使用的电脑就是按照CPU、内存、显存、硬盘等等各种模块组织到一起。iOS开发中也是按照MVC,MVVM等等各种各样的把代码分拆组合。关于采用何种代码架构我觉得真心没有必要过于迷信他人，说什么好就赶紧跟随推崇，从来不冷静思考我上文所说的问题：是否真的提高了效率？
    模块化我们首要做的就是代码的结构的组织调整，关于如何组织代码结构目前流行的就是按照功能和内容。任何团队也不会自诩说自己的代码组织结构是最好方案，因为我们每个团队不管是外部还是内部环境都是不一样的，就跟市面不存在一模一样的APP一样，只要提高了自己团队开发效率的就是最优的，所以我今天主要跟大家探讨是如何用CocoaPods来组织我们的模块化。
###模块说明书——PodSpec
     关于CocoaPods或者是Carthage大家肯定想到是管理我们的第三方代码。目前我们使用的热门第三方库都支持CocoaPods,关于CocoaPods的使用方法CocoaPods官方教程真的很清晰，所以我们不明白一定要看官网的教程说明：https://guides.cocoapods.org/    我们当初在使用CocoaPods踩了很多坑，都是谷歌搜索看其他人技术博客，互相复制粘贴搞得一知半解。直到我们看完官方的教程清晰很多：

![image.png](http://upload-images.jianshu.io/upload_images/143845-07e01bcaaefad99e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
AFNetworking的github上有一个AFNetworking.podspec的文件，
我们在使用CocoaPod管理使用第三方代码的时候cocoapod是如何保证我们只通过一个pod 'xxxxx' 就可以下载到需要的第三方库文件，配置好引用的系统库，这个就是podspec文件功劳。只要一个第三库支持cocopod肯定有一个后缀是podspec的文件，
![image.png](http://upload-images.jianshu.io/upload_images/143845-a9339da3ab6e722e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
而不管是github上cocoapod和本地~/.cocoapods/repos/master/都只是一个podspec文件，只是把它转换成json格式了，但是文件内容都是一摸一样的。
```
├── Specs
    └── [SPEC_NAME]
        └── [VERSION]
            └── [SPEC_NAME].podspec
```
![image.png](http://upload-images.jianshu.io/upload_images/143845-17431e651c1d08a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们把podspec文件上传给cocoapod的master上，当别人配置好CocoaPods就会把我们的podspec.json下载到~/.cocoapods/repos/master/下，用户要使用AFNetworking查找本地~/.cocoapods/repos/master/ 找到这个AFNetworking.podspec.json文件根据内容下载配置。

![image.png](http://upload-images.jianshu.io/upload_images/143845-022967abf48bc578.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
podspec文件就是充当了一个我们源代码模块的说明书，告诉开发者我们模块名称，我们用途，可以在哪下载源代码 ，需要如何配置。
创建一个podspec文件
```
$ pod spec create test
```
就会生成一个test.podspec的文件 当然我们这个只是演示， pod spec create后面的文件名称一定是有意义的名称。一般跟我们的模块名称一样。

podspec配置内容官方的说明教程最全最详细：https://guides.cocoapods.org/syntax/podspec.html
最简单的配置大致样子，看名字照猫画虎分分钟就会配置，
![image.png](http://upload-images.jianshu.io/upload_images/143845-c2df32b0730be686.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其中最重要的就是存放源代码地址.source，同时大部分是按照tag 来区分发布版本的
```
s.source   = { :git => 'https://github.com/AFNetworking/AFNetworking.git', :tag => s.version, :submodules => true }
```
当然pod 还提供了一个一条龙服务的命令：pod lib create 会帮我们创建一个跟项目名称相同podspec文件 测试project 工程 测试框架。
```
pod lib create XXXX
```
![image.png](http://upload-images.jianshu.io/upload_images/143845-9f136791730d004c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/143845-8313c5de035d668b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当我们完成配置说明书我们需要验证一下我们podspec和我们代码是否有问题。命令执行完看到 passed validation.就证明通过了验证。验证未通过会提示有几个Error或者几个Warnings都是不行的，但是我们可以设置 --allow-warnings来忽略警告，通过--verbose查看错误具体信息
```
pod spec lint test.podspec 
```

当我们通过 pod spec lint验证后，我们就可以发布我们cocoapod了到cocoapod的spec仓库了，实际是一个Git仓库，它的远程地址在在GitHub上：https://github.com/CocoaPods/Specs
但是我们如果是第一次在这台电脑发布代码库，需要为当前电脑注册trunk

![image.png](http://upload-images.jianshu.io/upload_images/143845-5df3a3c6e22d41ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
pod trunk register yangyucug@gmail.com 'baxiang'
```
此时会给我们注册的邮箱发送一份确认注册邮件，需要我们确认链接

![image.png](http://upload-images.jianshu.io/upload_images/143845-73f317ecea44ebcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

验证我们是否注册成功
```
$ pod trunk me
```
![image.png](http://upload-images.jianshu.io/upload_images/143845-8bca2f05b4c39a81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

podspec最终发布命令是：
```
pod trunk push  test .podspec
```
trunk 发布也是把我们podspec文件最终推送到了CocoaPods Master上，让其他开发者使用你的成果，但是前提是开发者pod update命令更新了本地master本地仓库。

目前我们在项目中使用cocoapod上传脚本，主要为了保证代码的提交完整和代码仓的tag提交。前面已经说过目前cocoapod都是根据代码仓的tag来区分不同的发布版本的。

```
#!/bin/bash

cd $(dirname $0)

diff=`git diff`


if [ ${#diff} != 0 ];
then
    echo "还有东西没有提交"
    exit 1
fi

echo "--------tag list--------"
git tag -l
echo "--------tag list--------"

echo "根据上面的tag输入新tag"
read thisTag

# 获取podspec文件名
podSpecName=`ls|grep ".podspec$"|sed "s/\.podspec//g"`
echo $podSpecName

# 修改版本号
sed -i "" "s/s.version *= *[\"\'][^\"]*[\"\']/s.version=\"$thisTag\"/g" $podSpecName.podspec


pod lib lint --allow-warnings


# 验证失败退出
if [ $? != 0 ];then
    exit 1
fi


git commit $podSpecName.podspec -m "update podspec"
git push
git tag -m "update podspec" $thisTag
git push --tags

pod trunk push $podSpecName.podspec --allow-warnings
```
一个tag对应一个版本 都会包含一个podspec 文件，也就意味着我们可以根据需求或者开发进度编译出不同的工程ipa文件。一般我们都在分支上开发新功能，测试的时候才会合并到主干上，我们可以根据不同的功能点分拆成不同的podspec 模块来，这样可以方便测试，也可以满足产品脑子突然短路这个版本不上线这个功能点，这一切只需要我们修改podspec 配置文件就可以了。

![image.png](http://upload-images.jianshu.io/upload_images/143845-d63b1424cb26f129.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/143845-b2b376037376fa3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过2份不同的配置文件我们编译出来的就是截然不同的2个framework 文件。

### 编译静态库[cocoapods-packager]
可以通过安装cocoapod的编译插件配合我们的cocoapod 文件来编译我们静态库文件
安装cocoapods-packager
```
sudo gem install cocoapods-packager
```
编译库文件
```
//pod package NAME [SOURCE]
pod package Roobo_Plus.podspec  --force --embedded
```

![](http://upload-images.jianshu.io/upload_images/143845-70f368b3e836164b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是使用--library编译出的.a静态库文件，pod package插件有问题的，根部不会生成头文件。所以只能编译framwork的库文件。
###创建仓库—— Repo
存放说明书的地方就是仓库啦，cocoapod的仓库就跟一本字典一样，通过索引查找找到我们需要的podspec说明书，然后按照说明书配置，我们已经知道所有的开源第三方源代码podspec文件都存储在 cocoapod的一个名叫**[Specs](https://github.com/CocoaPods/Specs)**的github仓库里，而我们将创建自己的仓库，本质是没什么区别的，只是我们的仓库名字和我们的远程地址不一样的。
而创建自己cocoapod仓库的命令就是
```
//REPO_NAME 仓库名称
//SOURCE_URL 仓库远程地址
pod repo add REPO_NAME SOURCE_URL
```
上传过我们开源代码到cocoapod同学都遇到过那个速度真是泪崩，创建自有仓库的优势就可以把我们cocoapod代码迁移到国内服务器，当然这一切跟github没有半毛钱关系，但在中国大家都懂的原因，甚至发生过无法访问的情况，如果你的用户对象主要在国内，可以在国内建立自有的仓库。
![image.png](http://upload-images.jianshu.io/upload_images/143845-dc67815e1d22b881.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
RooboSpecs 就是我们iOS 团队为了国内的企业用户建立的仓库，我们仓库服务器地址在开源中国码云上不管是我们这边上传podspec文件 还是对方下载更新我们的库文件那速度杠杠滴。大大节约了时间。
![image.png](http://upload-images.jianshu.io/upload_images/143845-eca8a4c16125d671.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看出这个自有的仓库跟cocoapod mster一模一样的。只是地址不一样。
我们在使用自有的仓库的时候需要在Podfile文件顶部添加自建仓库的地址
```
source 'https://git.oschina.net/roobospecs/roobospecs.git'
source 'https://github.com/CocoaPods/Specs.git'
```
  目前我们在私有仓库这块运用的较多：
（1）把第三方的稳定不需要改动代码编译成静态库形式，做成私有cocospod加速工程的编译速度
（2）修改了第三方的源代码或者第三方源代码停止维护更新了，我们需要自己维护。
（3）跨团队核心代码的安全，对于我们核心代码一般都会采用静态库，主需要更新保证同步，不需要邮件或者其他同步工具，其他开发者也不需要繁琐的删了换换了删的。
(4) 降低代码的耦合度，明晰代码全责，根据内容和功能划分不同的模块，每个人所写代码模块可以自由开发和维护，砍掉一个业务模块代码照样运行，更新一个功能模块 对上层的业务模块毫不影响。
  实际上明白了上面创建自有仓库的过程，你就会创建私有仓，如果这个仓库地址是一个内网地址或者一个Private的，你就无法下载这个仓库到本地~/.cocoapods/repos/底下。
  即使我们自建的仓库地址是公开下载的，但是我们设置的podspec的source属性也就是我们的源代码地址是一个私有地址 ，我们的代码也还是无法被非授权人下载的。所以私有仓库就是要保证你的代码无法被仓库查找到，查找到了也无法下载。不就OK了,所以一直厌恶把一些很简单的事情说的那么高深莫测。
推送podspec需要添加仓库名称和 podspec文件地址
```
pod repo push RooboSpecs xxxxxx.podspec
```
