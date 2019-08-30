#####事件修饰符
stop 阻止事件冒泡，防止多个事件触发
```
<template>
<div>
  <div @click="clickDiv">
    <button @click.stop="clickBtn">点击</button>
  </div>
</div>
</template>

<!--事件点击处理-->
<script>
    export default {
        name: "HelloClick",
        methods:{
           clickDiv(){
             console.log("div click")
           },
           clickBtn(){
             console.log("btn click")
           }
        }
    }

```
