####data()
data()定义全局数据声明,它本身就是一个函数类型
```
<script>
  export default {
    name: "HelloData",
    data() {
      return {}
    }
  }
</script>
```
####模板语法
支持表达式
```
<div>
  {{number+1}}
</div>
</template>

<script>
    export default {
        name: "HelloTemplate",
      data(){
          return{
            number:1,
          }
      }
    }
</script>
```
支持系统指令
```
<div v-html="rawHtml"></div>
 data(){
          return{
            number:1,
            rawHtml:'<strong>Hello HTML</strong>'
          }
      }
```
v-bind 绑定data值 动态改变属性值
```
<template>
<div>
  <div :class="currColor">背景修改</div>
  <button @click="changeColor">修改</button>
</div>
</template>

<script>
    export default {
        name: "HelloTemplate",
      data(){
          return{
            currColor:"redColor"
          }
      },
      methods:{
          changeColor(){
            this.currColor = "greenColor"
          }
      }
    }
</script>

<style scoped>
  .redColor{
    background-color: red;
  }
  .greenColor{
    background-color: green;
  }
</style>

```
#####computed
```
<div>
  {{msg}}
  <div>{{reverseStr}}</div>
</div>
</template>

<script>
  export default {
    name: "HelloData",
    data() {
      return {
        msg:"Hello Data"
      }
    },
    computed:{
      reverseStr(){
        return  this.msg.split("").reverse().join("")
      }
    },
  }
</script>
```
####methods
事件处理
```
<div>
  {{msg}}
  <div>{{reverseStr}}</div>
  <button @click="sayHello">sayHello</button>
</div>
</template>

<script>
  export default {
    name: "HelloData",
    data() {
      return {
        msg:"Hello Data"
      }
    },
    computed:{
      reverseStr(){
        return  this.msg.split("").reverse().join("")
      }
    },
    methods:{
      sayHello(){
         console.log("Hello Data")
      }
    }
  }
```
