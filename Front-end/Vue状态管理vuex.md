####概述
官方网站
[https://vuex.vuejs.org/zh/](https://vuex.vuejs.org/zh/)
####项目中安装vuex
```
npm install vuex -S
```
####Helloworld

在项目的src目录下新建一个目录store，在该目录下新建一个index.js文件，我们用来创建vuex实例，然后在该文件中引入vue和vuex，创建Vuex.Store实例保存到变量store中，最后使用export default导出store：
```
import Vue from 'vue'; //首先引入vue
import Vuex from 'vuex'; //引入vuex
Vue.use(Vuex) 

export default new Vuex.Store({
    state: { 
        // state 类似 data
        //这里面写入数据
    },
    getters:{ 
        // getters 类似 computed 
        // 在这里面写方法
    },
    mutations:{ 
        // mutations 类似methods
        // 写方法对数据做出更改(同步操作)
    },
    actions:{
        // actions 类似methods
        // 写方法对数据做出更改(异步操作)
    }
})

```

 在main.js文件中引入该文件，在文件里面添加 import store from ‘./store’;，再在vue实例全局引入store对象；
```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```
####State
vuex中的数据源，我们需要保存的数据就保存在这里，可以在页面通过 this.$store.state来获取我们定义的数据；

创建store目录 在新创建的store目录下创建index.js
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 1
  }
})
```
设置数据和获取数据
```
<template>
<div>
  {{msg}}
  <button @click="change">change</button>
</div>
</template>

<script>
    export default {
        name: "HelloVuex",
      data(){
          return{
            msg:234
          }
      },
      methods:{
          change(){
            this.$store.dispatch('inc',100)
            this.msg = this.$store.state.num;
          }
      }
    }
</script>
```
