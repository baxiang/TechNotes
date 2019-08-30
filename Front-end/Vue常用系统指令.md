####插值表达式
 数据绑定最常见的形式，其中最常见的是使用插值表达式，写法是{{}} 中写表达式
```
   <span>Message: {{ msg }}</span>
```
 标签将会被替代为对应数据对象上 msg 属性（msg定义在data对象中）的值。无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。
插值表达式主要展示Vue对象中data中的数据，合法的数据表达式
1直接写变量
2字符串拼接
3数值运算
4三元表达式
5内置函数

```
<div id="app">
    <p>姓名：{{name}}</p>
    <p>年龄：{{age+1}}</p>
    <p>{{age>18? "成年":"未成年"}}
    <p>{{name.split("").reverse().join("")}}</p>
</div>
<script>
    let vm = new Vue({
        el:'#app',
        data:{
            name:"小明",
            age:20
        }
    })

```
####v-text
v-text 和插值表达式一样，也是用来展示数据，它的使用方式在标签的属性中使用，而插值表达式在innerHTML位置使用
```
<div id="app">
    <p v-text="'姓名:'+name"></p>
    <p v-text="'年龄:'+(age+1)"></p>
</div>
<script>
    let vm = new Vue({
        el:'#app',
        data:{
            name:"小明",
            age:20
        }
    })
</script>
```
####v-html
插值表达式和v-text会将数据解释为纯文本，而非HTML 为了输出真正的 HTML ，你需要使用 v-html 指令：
```
<div id="app">
    <p v-html="html">{{html}}</p>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
           html:"<h1> Hello Vue</h1>"

        }
    })
</script>
```
####v-bind 属性/样式绑定
可以给html元素或者组件动态地绑定一个或多个属性/样式，动态绑定style和class。缩写形式是在属性和样式前面加一个冒号`:`，原始写法v-bind:class,缩写形式 `:class`
```
<div id="app">
    <img v-bind:src="pic">
</div>
<script>
     new Vue({
        el:'#app',
        data:{
           pic:"avatar.jpg"
        }
    })
</script>
```
通过冒号简写模式
```
<div id="app">
    <img :src="pic">
</div>
```
动态修改样式 
``` 
<template>
<div v-bind:class="{'active':isActive,'text-danger':hasError}">
   class
</div>
</template>

<script>
    export default {
        name: "HelloBind",
      data(){
          return{
            isActive:true,
            hasError:true
          }
      }
    }
</script>
```   
![image.png](https://upload-images.jianshu.io/upload_images/143845-c6cac60ab7201fb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<div data-v-278940af="" class="active text-danger">
   class
</div>
```
修改hasError为false
```
data(){
          return{
            isActive:true,
            hasError:false
          }
      }
```
属性就变成了
```
<div data-v-278940af="" class="active">
   class
</div>
```
通过类来绑定
```
<div :class="classStyle">
   class
</div>
</template>

<script>
    export default {
        name: "HelloBind",
      data(){
          return{
            classStyle:{
              active:true,
              text_danger:true
            }
          }
      }
    }
</script>
```
通过数组形式绑定
```
<div :class="['active','text_danger']">
   class
</div>
```
####v-if 条件渲染
```
if(ok){
 <h1>Yes</h1>
}else{
 <h2>NO</h2>
}
```
```
<template>
  <div v-if="isShow">YES</div>
  <div v-else>NO</div>
</template>

<script>
    export default {
        name: "HelloIF",
        data(){
          return{
            isShow: false
          }
        }
    }
</script>
```
####v-show
```
<template>
  <div>
    <div v-show="isShow">Show</div>
  </div>

</template>

<script>
    export default {
        name: "HelloIF",
        data(){
          return{
            isShow: true
          }
        }
    }
</script>
```
v-if 是真实的条件渲染，因为它会确保条件块在切换当中适当地销毁与重建条件块内的事件监听器和子组件。
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。
 v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。因此，如果需要频繁切换使用 v-show 较好，如果在运行时条件不大可能改变则使用 v-if 较好。
####v-for 列表渲染
遍历数组
```
 item in Array
 (item, index) in Array
```
遍历对象
```
value in Object

```
使用 item in Array 遍历数组
```
<div id="app">
    <ui>
        <li v-for="s in list">{{s.id}}-{{s.name}}</li>
    </ui>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
          list:[
              {id:11,name:"xiaoming"},
              {id:22,name:"xiaohong"},
              {id:33,name:"tony"}
          ]
        }
    })
</script>
```
使用 (item, index) in Array 遍历数组

```
<div id="app">
    <ui>
        <li v-for="(s,index) in list">{{index}}-{{s.name}}</li>
    </ui>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
          list:[
              {id:1,name:"xiaoming"},
              {id:2,name:"xiaohong"},
              {id:3,name:"tony"}
          ]
        }
    })
</script>
```
使用value in Object遍历对象 value就是对象属性的值，obj就是需要遍历的对象
```
<div id="app">
    <ui>
        <li v-for="s in stu">{{s}}</li>
    </ui>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
          stu:{id:1,name:"xiaoming",age:20}
        }
    })
</script>
```
使用(value, key, index) in Object遍历对象
```
<div id="app">
    <ui>
        <li v-for="(value,key,index) in stu">{{index}}-{{key}}-{{value}}</li>
    </ui>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
          stu:{id:1,name:"xiaoming",age:20}
        }
    })
</script>
```
######重点v-for的key属性使用
注意使用v-for渲染数据的时候，一定要记得将key属性加上，并且保证这个key具有唯一且不重复的，它的作用就是用来唯一标示数据的每一项，提高渲染性能。
注意：以下变动不会触发视图更新
      1. 通过索引给数组设置新值
      2. 通过length改变数组
    解决：
```
      1. Vue.set(arr, index, newValue)
      2. arr.splice(index, 1, newValue)
```
####v-model双向数据绑定
实现双向数据绑定，这个指令给input/select/textarea/标签和组件使用
```
<div id="app">
   <div>用户名：<input type="text" v-model="name"></div>
    <div>密码：<input type="text" value="password"></div>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
         name:"admin",
         password:"123456"
        }
    })
</script>
```
v-model 用在checkbox 中用来控制复选框的选中状态
####v-on
作用：绑定事件监听，表达式可以是一个方法的名字或一个内联语句，
  如果没有修饰符也可以省略，用在普通的html元素上时，只能监听 原生
  DOM 事件。用在自定义元素组件上时，也可以监听子组件触发的自定义事件。
 2、常用事件：
      v-on:click //点击
      v-on:keydown
      v-on:keyup
      v-on:mousedown
      v-on:mouseover
      v-on:submit
      ....

 v-on:click 按钮点击事件
```
<div id="app">
   <div>{{name}}</div>
    <button v-on:click="login">登录</button>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
         name:"admin",
        },
        methods:{
            login () {
                this.name ="登录成功"
            }
        }
    })
</script>
```
一般情况都是简写@click
```
 <button @click="login">登录</button>
```
$event
```
<div id="app">
   <div>{{name}}</div>
    <button @click="login($event)">登录</button>
</div>
<script>
     new Vue({
        el:'#app',
        data:{
         name:"admin",
        },
        methods:{
            login (e) {
                console.log(e)
            }
        }
    })
</script>
```
事件修饰符：增强事件功能，.stop 阻止冒泡 .prevent 阻止默认行为
```
<div id="app">
   <a href="http://www.google.com" @click.prevent="stop">阻止跳转</a>
</div>
<script>
     new Vue({
        el:'#app',
        methods:{
            stop () {
                console.log("阻止跳转")
            }
        }
    })
</script>
```
按键修饰符
    触发像keydown这样的按键事件时，可以使用按键修饰符指定按下特殊的键后才触发事件
当按下回车键时才触发kd1事件
```
<input type="text" @keydown.enter="kd1">  
```
由于回车键对应的keyCode是13，也可以使用如下替代
```
<input type="text" @keydown.13="kd1">  当按下回车键时才触发kd1事件
```
 但是如果需要按下字母a（对应的keyCode=65）才触发kd1事件，有两种写法：
   1、由于默认不支持a这个按键修饰符，需要Vue.config.keyCodes.a = 65 添加一个对应,所以这种写法为：
```
      Vue.config.keyCodes.a = 65
      <input type="text" @keydown.a="kd1">  这样即可触发
```
2、也可以之间加上a对应的数字65作为按键修饰符
```
<input type="text" @keydown.65="kd1">  这样即可触发
```
键盘上对应的每个按键可以通过 http://keycode.info/ 获取到当前按下键所对应的数字
```
<div id="app">
    <div>{{msg}}</div>
  <input v-model="msg" @keyDown.13="submit">
</div>
<script>
     new Vue({
        el:'#app',
         data:{
            msg:""
         },
        methods:{
            submit () {
                console.log(this.msg)
            }
        }
    })
</script>
```


