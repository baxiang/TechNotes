####自定义属性
组件props中声明的属性
创建hello工程
```
vue create hello
```
修改app.vue 如下
```
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```
修改HelloWorld.vue如下
```
<template>
  <div >
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    props: {
      msg: String
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >

</style>

```
####原生属性
没有声明的属性，默认自动挂载到组件根元素上，设置`inheritAttrs`为false可以关闭自动挂载
####特殊属性class,style
挂载到组件的根元素上，支持字符串，对象，数组等多种语法
