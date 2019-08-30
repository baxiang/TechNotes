####概述
前端路由
路由就是根据不同的url地址来展示不同的内容或页面.
Vue Router 是 [Vue.js](http://cn.vuejs.org) [](http://cn.vuejs.org) 官方的路由管理器
github: https://github.com/vuejs/vue-router
中文官方网站[https://router.vuejs.org/zh/](https://router.vuejs.org/zh/)
中文API文档 https://router.vuejs.org/zh/api/
####vue-router安装与结构
```
npm isntall vue-router --save
```
vue-router是Vue的一个插件，需要在Vue的全局应用中通过Vue.use()将他纳入到Vue实例中
创建项目 `router-demo` 选择Manually select features
```
$vue create router-demo

Vue CLI v3.9.2
? Please pick a preset: Manually select features
? Check the features needed for your project:
 ◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
❯◉ Router
 ◯ Vuex
 ◯ CSS Pre-processors
 ◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing
```
会在当前项目src目录下自动创建router.js文件
```
import Vue from 'vue'
import Router from 'vue-router'
import Home from './views/Home.vue'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'home',
      component: Home
    },
    {
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (about.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import(/* webpackChunkName: "about" */ './views/About.vue')
    }
  ]
})
```
一条路由的实现需要三部分：
name 名称
path, 路径
component 组件
启动路由器
在main.js 入口文件中启动路由器
####Helloworld
接着使用上面的
1. 创建组件Bar.vue
```
<template>
    <div >
        <h1>Bar</h1>
        <router-link to="foo">跳转到Foo</router-link>
    </div>
</template>

<script>
    export default {
        name: 'Bar',
    }
</script>

```
2. 创建组件Foo.vue
```
<template>
    <div>
        <h1>Foo</h1>
        <router-link to="bar">跳转到Bar</router-link>
    </div>
</template>

<script>
    export default {
        name: "Foo",
    }
</script>

<style scoped>

</style>
```
3. 创建文件router,在其目录下创建index.js文件
```
import Router from 'vue-router'
import Vue from 'vue'
Vue.use(Router)

import Bar from '../components/Bar'
import Foo from '../components/Foo'

export default new Router({
    routes: [
        {
          path:"/",
          redirect:"/foo"
        },
        {
            path: '/foo',
            component: Foo
        },
        {
            path: '/bar',
            component: Bar
        }
    ]
});

```
4. 将App.vue修改
```
<template>
    <div id="app">
        <img alt="Vue logo" src="./assets/logo.png">
        <router-view></router-view>
        </div>
</template>

<style>
    #app {
        text-align: center;
        margin-top: 60px;
    }
</style>
```
5. 在main.js注册路由器
```
import Vue from 'vue'
import App from './App.vue'
import router from './router/index'

Vue.config.productionTip = false;

new Vue({
  router,
  render: h => h(App),
}).$mount('#app')

```
####动态路由
创建组件User.vue
```
<template>
    <div>
        {{$route.params.id}}
    </div>
</template>
```
修改router目录的index文件
```
import Router from 'vue-router'
import Vue from 'vue'
Vue.use(Router)
import User from  '../components/User'

export default new Router({
    routes: [
        {
            path:'/user/:id',
            component:User
        }
    ]
});

```
访问的http://localhost:8080/#/user/12347
![图片.png](https://upload-images.jianshu.io/upload_images/143845-9fda681fcd5214e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

设置“路径参数”,将User.vue 修改成
```
<template>
    <div>
        {{$route.query.id}}
    </div>
</template>
```
增加路由配置
```
        {
            path:'/user',
            component:User
        }
```
访问路径 http://localhost:8080/#/user?id=2
####嵌套路由router-view
创建组件文件Profile.vue
```
<template>
    <div>
        用户画子界面
    </div>
</template>

<script>
    export default {
        name: "Profile"
    }
</script>

<style scoped>

</style>
```
修改路由文件router/index.js
```
import Router from 'vue-router'
import Vue from 'vue'
Vue.use(Router)
import User from  '../components/User'
import Profile from '../components/Profile'
export default new Router({
    routes: [

        {
            path:'/user',
            component:User,
            children:[
                {
                    path:"profile",
                    component:Profile
                }

            ]
        },
        {
            path:'/user/:id',
            component:User
        }

    ]
});

```
同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。
访问http://localhost:8080/#/user/profile
![图片.png](https://upload-images.jianshu.io/upload_images/143845-7562e86b11bae0e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####路由组件传参
 方式 1: 路由路径携带参数(param/query)
配置路由
```
children: [
{
path: 'mdetail/:id',
component: MessageDetail
}
]
```
路由路径
参数传递 注意需要加上冒号`:`
```
<router-link :to="'/home/message/mdetail/'+m.id">{{m.title}}</router-link>
```
路由组件中读取请求参数,获取传递数据 注意是`route` 而不是router
```
this.$route.params.id
```
方式2 <router-view>属性携带数据
```
<router-view :msg="msg"></router-view>
```
####缓存路由组件对象
1) 默认情况下, 被切换的路由组件对象会死亡释放, 再次回来时是重新创建的
2) 如果可以缓存路由组件对象, 可以提高用户体验
```
<keep-alive>
<router-view></router-view>
</keep-alive>
```
####路由导航
相关 API
1) this.$router.push(path): 相当于点击路由链接(可以返回到当前路由界面)
```
// 字符串
router.push('home')
// 对象
router.push({ path: 'home' })
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```
注意：如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：
```
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

2) this.$router.replace(path): 用新路由替换当前路由(不可以返回到当前路由界面)
3) this.$router.back(): 请求(返回)上一个记录路由
4) this.$router.go(-1): 请求(返回)上一个记录路由
5) this.$router.go(1): 请求下一个记录路由
####路由元数据
定义路由的时候可以配置 meta 字段：
```
import Router from 'vue-router'
import Vue from 'vue'
Vue.use(Router)
import User from  '../components/User'
import Profile from '../components/Profile'

const router = new Router({
    routes: [
        {
            path:'/user',
            component:User,
            children:[
                {
                    path:"profile",
                    component:Profile
                }
            ]
        },
        {
            path:'/user/:id',
            component:User,
            meta:{
                requireAuth:true
            }
        }

    ]
});

router.beforeEach((to, from, next) => {
    if (to.matched.some(record => record.meta.requireAuth)) {
        next()
    }
})

export default router
```
####路由组件传参
使用route的props进行解耦.提高组件的复用,同时不改变url

修改user.vue组件文件如下
```
<template>
    <div>
        <p>姓名:{{this.name}}</p>
    </div>
</template>

<script>
    export default {
        name: "User",
        props:["name"]
    }
</script>

<style scoped>

</style>
```
修改路由文件router/index.js 文件如下
```
import Router from 'vue-router'
import Vue from 'vue'
Vue.use(Router)
import User from  '../components/User'

const router = new Router({
    routes: [
        {
            path:"/user/profile",
            component:User,
            props:{name:"tony"}
        },
        {
            path:"/user/search",
            component:User,
            props: (route)=>({name:route.query.name})
        },
        {
            path:"/user/:name",
            component:User,
            props:true
        },
    ]
});

function searchName(route){
    return {name:(route.query.name)}
}

export default router
```
函数返回的是对象类型的 ，props: (route)=>({name:route.query.name}) 等价于
```
function searchName(route){
    return {name:(route.query.name)}
}
```
访问接口http://localhost:8081/#/user/search?name=tony
访问接口http://localhost:8081/#/user/profile
访问接口http://localhost:8081/#/user/tony
![图片.png](https://upload-images.jianshu.io/upload_images/143845-3bfb325789998338.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####路由懒加载
当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。
结合 Vue 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。
首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：
```
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```
第二，在 Webpack 2 中，我们可以使用[动态 import](https://github.com/tc39/proposal-dynamic-import)
[](https://github.com/tc39/proposal-dynamic-import)

语法来定义代码分块点 (split point)：

```
import('./Foo.vue') // 返回 Promise

```
注意
如果您使用的是 Babel，你将需要添加 [`syntax-dynamic-import`](https://babeljs.io/docs/plugins/syntax-dynamic-import/)
[](https://babeljs.io/docs/plugins/syntax-dynamic-import/)
插件，才能使 Babel 可以正确地解析语法。
结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。
```
const Foo = () => import('./Foo.vue')
```
在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`：
```
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})

```
######把组件按组分块
有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)
[](https://webpack.js.org/guides/code-splitting-require/#chunkname)
，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。
```
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。


####组件渲染
在index.html处有个id="app"的div，在App.vue中也有一个id="app"的div。页面渲染的就是这个组件。
![图片.png](https://upload-images.jianshu.io/upload_images/143845-04467059c55ccb41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 在这个App.vue中有个router-view标签，默认在这渲染当路由为"/"指向的组件，路由设置在src/router/index.js中

![图片.png](https://upload-images.jianshu.io/upload_images/143845-df2e6177d82ae353.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 这样就可以使用HelloWorld组件了。

![图片.png](https://upload-images.jianshu.io/upload_images/143845-363629e2b67ee71c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这张图可以更清楚得让大家明白router-view标签的作用
![图片.png](https://upload-images.jianshu.io/upload_images/143845-39fd1ba01b41d241.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*注意，当有多个组件的时候，就不是component属性而是components，记得注册的组件上面都要用import导入

当组件里有自己的子组件的情况下，就需要使用到router-link标签了，它其实是一个a标签
![图片.png](https://upload-images.jianshu.io/upload_images/143845-c0ffa60c7cc2dda1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


那么router-view和router-link的区别是什么呢？

    目前来说，router-link和router-view结合使用，可以在组件中通过点击渲染子组件；
    单独使用router-view就可以在当前组件中重根据name属性选择渲染组件；
    单独使用router-link就可以整个切换到to属性指定路由的组件；

最后，这个单页面开发的意思就是，这个项目只有index.html这个页面，我们通过使用不同的组件像拼装机器人一样获得不同的页面展示，而src下的App.vue就是所有组件的父组件，所有其它组件都通过路由渲染到它里面的router-view中，然后再将App.vue渲染到index.html中。


