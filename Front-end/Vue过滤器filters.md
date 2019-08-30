过滤数据需要通过参数传递
```
<template>
<div>
  <div>{{msg | trimStr}}</div>
</div>
</template>

<script>
    export default {
        name: "HelloTemplate",
      data(){
          return{
            msg:"Hello world"
          }
      },
      filters:{
          trimStr(str){
            if (!str) return
           return  str.replace(/\s/g,"");
          }
      }
    }
</script>
```
