VSCode（Visual Studio Code）是由微软研发的一款免费、开源的跨平台文本（代码）编辑器，算是目前前端开发几乎完美的软件开发工具。
官网为：[https://code.visualstudio.com/](https://code.visualstudio.com/)
##通用插件
####Code Runner
![图片.png](https://upload-images.jianshu.io/upload_images/143845-911348ec3e60d146.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Code Spell Checker
单词拼写检查
![图片.png](https://upload-images.jianshu.io/upload_images/143845-d0b2791f34376243.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Settings Sync
![图片.png](https://upload-images.jianshu.io/upload_images/143845-879524ff5afe5e09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Git History
![图片.png](https://upload-images.jianshu.io/upload_images/143845-3627e53a1d4ae9a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####carbon.now.sh
代码分享插件
![图片.png](https://upload-images.jianshu.io/upload_images/143845-79d9dd98a01f15b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
顶部代码注释
![图片.png](https://upload-images.jianshu.io/upload_images/143845-d3d8f29624f88797.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
实时调试Live
![图片.png](https://upload-images.jianshu.io/upload_images/143845-844ecdb43e4d7897.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##前端常用插件
**Vetur**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-2b04f97fbe8d38cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**auto rename tag（自动修改标签）**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-13c23acb2fb6ea05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Prettier - Code formatter（代码格式化）**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-816b08e9e1786a37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**HTML CSS Support**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-a174f8403d8d54ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**HTML boilerplate**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-fb3af9ad043cd49e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Debugger for Chrome**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-c25e79b522ef2697.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**ESLint**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-4972702c454e42ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/143845-d16f6723f703b03d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##Vue开发设置
“文件 -> 首选项 -> 用户代码片段”
```
{
    "Print to console": {
        "prefix": "vue",
        "body": [
            "<template>",
            "  <div></div>",
            "</template>",
            "",
            "<script>",
            "export default {",
            "  name: '',",
            "  data () {",
            "    return {",
            "      ",
            "    };",
            "  },",
            "  methods: {}",
            "}",
            "</script>",
            "",
            "<style  scoped>",
            "</style>"
    ],
        "description": "Log output to console"
    }
}

```
新建.vue文件，输入vue，按回车，页面结构自动生成

settings.json
```
{
  "editor.fontFamily": "Fira Code",
  "editor.fontSize": 18,
  "files.autoSave": "onFocusChange",
  "editor.tabSize": 2,
  // #每次保存的时候自动格式化
  "editor.formatOnSave": true,
  // #每次保存的时候将代码按eslint格式进行修复
  "eslint.autoFixOnSave": true,
  // 添加 vue 支持
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  // #让prettier使用eslint的代码格式进行校验
  "prettier.eslintIntegration": true,
  // #去掉代码结尾的分号
  "prettier.semi": false,
  // #使用带引号替代双引号
  "prettier.singleQuote": true,
  // #让函数(名)和后面的括号之间加个空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  // #这个按用户自身习惯选择
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  // #让vue中的js按编辑器自带的ts格式进行格式化
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "wrap_attributes": "force-aligned"
      // #vue组件中html代码格式化样式
    }
  }
}

```

