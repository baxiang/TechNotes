##安装oclint
```
 brew install oclint
```
##安装 xcpretty
用于对xcodebuild的输出进行格式化
```
$ gem install xcpretty
```


##oclint分析脚本
```
#!/bin/bash
source $HOME/.bash_profile
export LC_ALL=en_US.UTF-8
myworkspace=PuddingPlus.xcworkspace # 替换workspace的名字
myscheme=PuddingPlus # 替换scheme的名字
xcodebuild -workspace $myworkspace -scheme $myscheme clean&&
xcodebuild -workspace $myworkspace -scheme $myscheme \
-configuration Debug \
| tee xcodebuild.log \
| xcpretty -r json-compilation-database -o compile_commands.json&&
oclint-json-compilation-database -e Pods -- \
-report-type pmd -o oclint_result.pmd \
-rc LONG_LINE=200 \
-max-priority-1=100000 \
-max-priority-2=100000 \
-max-priority-3=100000; \
rm compile_commands.json;

```
##脚本内容分析
### xcodebuild 编译工程
```
xcodebuild -workspace $myworkspace -scheme $myscheme clean&&
xcodebuild -workspace $myworkspace -scheme $myscheme \
-configuration Debug
```
获取xcodebuild.log
```
tee xcodebuild.log
```
 compile_commands.json
```
xcpretty -r json-compilation-database -o compile_commands.json
```
clint-json-compilation-database

```
$ oclint-json-compilation-database -help
usage: oclint-json-compilation-database [-h] [-v] [-debug] [-i INCLUDES]
                                        [-e EXCLUDES]
                                        [oclint_args [oclint_args ...]]

OCLint for JSON Compilation Database (compile_commands.json)

positional arguments:
  oclint_args           arguments that are passed to OCLint invocation

optional arguments:
  -h, --help            show this help message and exit
  -v                    show invocation command with arguments
  -debug, --debug       invoke OCLint in debug mode
  -i INCLUDES, -include INCLUDES, --include INCLUDES
                        extract files matching pattern
  -e EXCLUDES, -exclude EXCLUDES, --exclude EXCLUDES
                        remove files matching pattern
```
通过 -e 选项来忽略Cocoapods 来pod文件，通过--来分割 oclint-json-compilation-database 的参数与 oclint_args。oclint_args 就是 oclint 命令的参数。 -report-type 文件类型
```
oclint-json-compilation-database -e Pods -- -report-type pmd -o oclint_result.pmd
```
##Jenkins集成静态代码分析
安装插件PMD Plugin

![image.png](http://upload-images.jianshu.io/upload_images/143845-77e94c9d3a8a771b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
项目配置
![image.png](http://upload-images.jianshu.io/upload_images/143845-43350af3c38e4ab1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
分析结果

![image.png](http://upload-images.jianshu.io/upload_images/143845-c404909939ee89f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

