webpack是一个现代JavasScript应用程序的模块打包器(module bunder) 
官方网站[https://www.webpackjs.com/](https://www.webpackjs.com/)
![image.png](https://upload-images.jianshu.io/upload_images/143845-814fabf25c4788e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
webpack的两大特点：1模块化 2打包
作用：
1将sass/less 等预编译的css语言转换成浏览器识别的css文件
2能够将多个预编译文件打包成一个文件
3 打包image/styles/assets/scrips/等前端常用的文件
4 搭建开发环境开启服务器
5 监视文件改动，热部署。
6 将单文件组件(*.vue)类型的文件，转化成浏览器识别的内容
####基本使用

webpack的两种使用方式：1命令行 2 配置文件 `webpack.config.js`
####package
创建webpacktest目录 在webpacktest下执行
```
mkdir webpacktest
cd webpacktest
nmp init
```
生成package.json 文件
```
{
  "name": "webpacktest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
安装webpack
```
  npm i -D webpack webpack-cli
```
当前webpacktest目录下文件结构
```
 tree -L 1
.
├── node_modules
├── package-lock.json
└── package.json

1 directory, 2 files
```
创建helloworld.js文件
```
function hello() {
    console.log("hello world")
}
```
安装
```
$ ./node_modules/.bin/webpack helloworld.js
Hash: 4f3b3b08a01b5cace0ff
Version: webpack 4.34.0
Time: 322ms
Built at: 2019-06-15 13:33:42
  Asset       Size  Chunks             Chunk Names
main.js  930 bytes       0  [emitted]  main
Entrypoint main = main.js
[0] ./helloworld.js 52 bytes {0} [built]

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
```
默认输出目录在当前目录底下的默认会创建dist目录，通过-o 指定输出目录
```
./node_modules/.bin/webpack helloworld.js -o ./test/main.js --mode development
```
使用webpack的时候应该提供mode 分别是生产模式production 或开发模式development,通过修改--mode来指定模式，默认是生产模式
```
$ ./node_modules/.bin/webpack helloworld.js --mode development
Hash: d18edbdae5a62805073b
Version: webpack 4.34.0
Time: 75ms
Built at: 2019-06-15 13:41:21
  Asset      Size  Chunks             Chunk Names
main.js  3.83 KiB    main  [emitted]  main
Entrypoint main = main.js
[./helloworld.js] 52 bytes {main} [built]
```
####修改package.json的scripts
在package.json文件的scripts增加如下内容
```
"scripts": {
    "build": "webpack ./src/main.js --mode development"
  },
```
在终端中执行执行命令
```
npm run build
```
webpack打包处理的过程：
1 运行webpack的打包命令 
2 webpack 找到我们指定的入口文件main.js
3 webpack 分析main.js 中的代码，当遇到imort $....语法的时候，那么webpack就会导入模块代码
####配置文件webpack.config.js
在项目的根目录底下创建webpack.config.js,注意不要使用ES6中的模块化语法import/export
```
const path = require('path')

module.exports = {

    // 入口
    entry: path.join(__dirname,'./src/main.js'),

    //出口
    output: {
        path: path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },

    //模式
    mode:'development'


}
```
将package.json修改成
```
 "scripts": {
    "build": "webpack"
  },
```
终端执行编译`npm run build`
####webpack-dev-server
1开启服务器
2 自动监视文件变化 热部署
安装
```
npm i -D webpack-dev-server
```
在package.json添加
```
"scripts": {
    "dev":"webpack-dev-server"
  },
```
终端执行命令 npm run dev
```
> webpack-dev-server

ℹ ｢wds｣: Project is running at http://localhost:8080/
ℹ ｢wds｣: webpack output is served from /
ℹ ｢wds｣: Content not from webpack is served from /Users/baxiang/Web/webpacktest
ℹ ｢wdm｣: wait until bundle finished: /
ℹ ｢wdm｣: Hash: bc2343c1f33feabb1023
Version: webpack 4.34.0
Time: 917ms
```
html-webpack-plugin
####加载器loader
webpack自身处理普通的JS文件，而对于非JS文件，都需要对应的loader来进行特殊的处理，也就是每种类型的文件，都需要需要专门的loader来处理。
一般缺少loader的报错日志
```
Module parse failed: Unexpected character '#' (1:0)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loade
```
上面的报错就是告知缺少css loader,需要安装css的loader
```
npm i -D style-loader css-loader
```
