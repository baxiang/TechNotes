##概述
Vue的官方教程地址[https://cn.vuejs.org/v2/guide/](https://cn.vuejs.org/v2/guide/)
GitHub的地址：[https://github.com/vuejs/vue](https://github.com/vuejs/vue)
![image.png](https://upload-images.jianshu.io/upload_images/143845-b7ef8ed2e6a2f28f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
渐进式(progressive)是指可以一步一步的阶段性的学习和使用Vue.js,不需要一开始就是使用到所有技能点。

####前端渲染&后端渲染
1. 如果是在浏览器端进行渲染工作，则称为前端渲染
   1. 首先向服务器请求页面
   2. 将页面的基本结构渲染出来
   3. 通过ajax请求向后台请求数据
   4. 利用模板引擎将获取到数据渲染到页面中
2. 如果是在服务器端进行的渲染工作，则称为后端渲染
   1. 首先向服务器请求页面
   2. 服务器会先去根目录读取页面文件
   3. 将数据读取到
   4. 将数据渲染到读取到的页面中
   5. 将渲染好数据的页面返回给浏览器
   6. 浏览器拿到的就是已经渲染好数据的页面！
####SPA
单页Web应用（single page web application，SPA）： SPA 是一种特殊的 Web 应用，是加载单个 HTML 页面并在用户与应用程序交互时动态更新该页面的。它将所有的活动局限于一个 Web 页面中，仅在该 Web 页面初始化时加载相应的 HTML 、 JavaScript 、 CSS 。一旦页面加载完成， SPA 不会因为用户的操作而进行页面的重新加载或跳转，而是利用 JavaScript 动态的变换 HTML（采用的是 div 切换显示和隐藏），从而实现UI与用户的交互。在 SPA 应用中，应用加载之后就不会再有整页刷新。相反，展示逻辑预先加载，并有赖于内容Region（区域）中的视图切换来展示内容。

要实现单页面应用，现在已经有很多现成的框架了，它们都是很全面的开发平台，为单页面应用开发提供了必需的页面模板、路径解析和处理、后台服务 api 访问、 DOM 操作等功能。

![image](https://upload-images.jianshu.io/upload_images/143845-67261e447504c958.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1) 有良好的交互体验
能提升页面切换体验，用户在访问应用页面是不会频繁的去切换浏览页面，从而避免了页面的重新加载；
2) 前后端分离开发
单页Web应用可以和 RESTful 规约一起使用，通过 REST API 提供接口数据，并使用 Ajax 异步获取，这样有助于分离客户端和服务器端工作。更进一步，可以在客户端也可以分解为静态页面和页面交互两个部分；
3) 减轻服务器压力
服务器只用出数据就可以，不用管展示逻辑和页面合成，吞吐能力会提高几倍；
4) 共用一套后端程序代码
不用修改后端程序代码就可以同时用于 Web 界面、手机、平板等多种客户端；

##安装npm
npm(Node Package Manager): node包管理工具
nodejs中集成了npm 因此需要安装nodejs, 那么npm就自动安装好.官方地址是[https://nodejs.org/en/](https://nodejs.org/en/)
![image.png](https://upload-images.jianshu.io/upload_images/143845-007667da1eb01b87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-084668e7202d69c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

查看当前npm版本
```
$ npm --version
6.9.0
```
淘宝npm镜像
中国大陆用户，建议将 NPM 源设置为[国内的镜像](https://npm.taobao.org/)，可以大幅提升安装速度
```
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```
####npm常用命令
```
npm install --registry=https://registry.npm.taobao.org
```
`--registry=https://registry.npm.taobao.org`提高大陆地区npm安装插件速度
```
# 生成 package.json 文件（需要手动选择配置）
npm init
# 生成 package.json 文件（使用默认配置）
npm init -y
# 一键安装 package.json 下的依赖包
npm i
# 在项目中安装包名为 xxx 的依赖包（配置在 dependencies 下）
npm i xxx
# 在项目中安装包名为 xxx 的依赖包（配置在 dependencies 下）
npm i xxx --save
# 在项目中安装包名为 xxx 的依赖包（配置在 devDependencies 下）
npm i xxx --save-dev
# 全局安装包名为 xxx 的依赖包
npm i -g xxx
# 运行 package.json 中 scripts 下的命令
npm run xxx
```
##常用开发IDE
####vscode
下载地址[https://code.visualstudio.com/Download](https://code.visualstudio.com/Download)

![image.png](https://upload-images.jianshu.io/upload_images/143845-41ffe4359a0430f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####webstorm
下载地址[https://www.jetbrains.com/webstorm/](https://www.jetbrains.com/webstorm/)
![image.png](https://upload-images.jianshu.io/upload_images/143845-9173800cbd1f2ac3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####浏览器调试插件vue.js devtools
GitHub下载地址[https://github.com/vuejs/vue-devtools](https://github.com/vuejs/vue-devtools)
![](https://upload-images.jianshu.io/upload_images/143845-b8ac76d89cb2761a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##安装Vue CLI脚手架
官方地址[https://cli.vuejs.org/zh/](https://cli.vuejs.org/zh/)
脚手架快速生成项目的目录结构，安装了vue的脚手架以后，直接通过一条命令就可以生成一个vue项目的目录结构。
个人开发的不同阶段和当前项目的使用版本情况，分别介绍CLI2和3的安装，推荐安装最新版本CLI3
安装Cli2 
```
$ npm install -g vue-cli
```
安装Cli3
`npm uninstall -g vue-cli` 是安装了CLI2版本的 需要卸载掉CLI2的版本，如果是首次安装不需要执行这个命令
```
npm uninstall -g vue-cli
npm install -g @vue/cli
```
查看当前脚手架的版本信息
```
vue --version
3.8.2
```
##helloworld
```
vue create 项目名称
```
因为vue-cli 用的是 npm 源，设置 npm 源可以提升创建速度：
```
npm config set registry https://registry.npm.taobao.org
```
现在默认创建hello-world项目
```
vue create hello-world
Vue CLI v3.8.2
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint)
  Manually select features
🎉  Successfully created project hello-world.
👉  Get started with the following commands:
 $ cd hello-world
 $ npm run serve
cd hello-world
npm run serve
```
选择自定义插件创建hello-world

![图片.png](https://upload-images.jianshu.io/upload_images/143845-1c87649a59338afc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过空格键选择需要安装的插件
![图片.png](https://upload-images.jianshu.io/upload_images/143845-ad238ac88693226c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) 
```
```
Pick a linter / formatter config: (Use arrow keys)
 Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◉ Lint on save
 ◯ Lint and fix on commit

```
Save this as a preset for future projects
```
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
❯ In dedicated config files 
  In package.json 
```
是否保存这个项目的配置信息
```
Save this as a preset for future projects
```


CLI v3.热部署项目使用的命令是
```
npm run serve
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-54d9721a81354b3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用脚手架CLI v2创建项目
```
$ vue init webpack first-vue

? Project name first-vue
? Project description A Vue.js project
? Author baxiang <baxiang@roobo.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests Yes
? Pick a test runner jest
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recom
mended) npm

   vue-cli · Generated "my-project".
```
CLI v2运行项目
```
npm run dev
 DONE  Compiled successfully in 4486ms                                                                                                                             12:14:53 PM

 I  Your application is running here: http://localhost:8080  
```
##src目录结构
```
├── node_modules     # 项目依赖包目录
├── public
│   ├── favicon.ico  # ico图标
│   └── index.html   # 首页模板
├── src 
│   ├── assets       # 样式图片目录
│   ├── components   # 组件目录
│   ├── views        # 页面目录
│   ├── App.vue      # 父组件
│   ├── main.js      # 入口文件
│   ├── router.js    # 路由配置文件
│   └── store.js     # vuex状态管理文件
├── .gitignore       # git忽略文件
├── .postcssrc.js    # postcss配置文件
├── babel.config.js  # babel配置文件
├── package.json     # 包管理文件
└── yarn.lock        # yarn依赖信息文件      
```

####vue不同构建版本的说明
1完整版(运行时+编译器）
2 运行时(体积比完整版下30%)
3 `import vue from 'vue'` 默认导入的是运行时版本
4 如果需要使用完整版 需要在webpack.base.conf.js
@符号表示src路径
```
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
```
##package.json
package.json文件就是用来描述一个包的信息,在进行代码分享的时候，不需要分享node_modules，只需要分享自己的代码和pacakge.json即可，另外的程序员拿到代码之后，自己根据pacakge.json下载所有的依赖项即可！
#### package.json属性
必须包含两个属性 name version
name: 包名    不能有中文，不能有空格，不能有大写字母，不能有特殊字符！
version:  版本信息 
description： 描述信息
author: 作者
keywords: 关键词 方便在npm网站上进行搜索
license: 开源协议 自己指定
scripts: 放的就是一些shell命令，这些命令可以通过 `npm run 命令别名` 进行执行
    可以省略run执行的命令别名： start stop restart test config例如`npm start`

####dependencies 和 devDependencies 的说明
这两个属性中保存的都是当前包所有的依赖信息。
dependencies： 运行时依赖项，在将代码上传到服务器时，这个包仍被需要
devDependencies： 开发时依赖项，这个依赖项只需要在开发时时候，上传到服务器的时候不需要！

`npm install`  这条命令会自动根据package.json中保存的包信息进行下载 （devDependencies+dependencies）

只下载运行时依赖项可以使用命令`npm install --production`

1. 将依赖项的信息保存到dependencies
   `npm install 包 --save`
   `npm install 包 -S`
2. 将依赖项的信息保存到devDependencies
   `npm install 包 --save-dev`
   `npm install 包 -D`
