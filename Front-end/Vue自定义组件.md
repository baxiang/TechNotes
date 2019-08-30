####创建组件
```
<template>
  <p>{{time}}</p>
</template>

<script>
    export default {
        name: "CustomComponent",
        data(){
           return{
             time:10
           }
        },
      mounted() {
          var vm = this;
          var t = setInterval(function () {
              vm.time --;
              if (vm.time==0){
                 clearTimeout()
              }
          },1000)
      }

    }
</script>

<style scoped>

</style>
```
导入组件
```
 import CustomComponent from "./CustomComponent";
```
声明组件
```
export default {
    name: "HelloComponents",
    components: {
      CustomComponent
    }
  }
```
在需要组件的HelloComponent中模板中使用组件
```
<template>
  <div>
    <custom-component></custom-component>
  </div>
</template>
```
