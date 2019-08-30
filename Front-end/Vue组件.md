组件 (Component) 封装可重用的代码，便于分工合作，代码的管理和复用。所有的 Vue 组件同时也都是 Vue 的实例。
注意：
1如果组件命令使用驼峰命名方法，使用组件的时候需要改成连字符的方式
2 模板template属性指定组件模板，模板只能有一个根节点
总结起来就是：

`Vue组件 = Vue实例 = new Vue(options)`
####全局组件创建
```
<div id="app">
    <!--使用组件-->
    <index></index>
</div>
<script>
    // 创建组件
    Vue.component("index", {
        template: '<div>这是首页</div>'
    })
    new Vue({
        el: "#app",
        data: {}
    })

</script>
```
####局部组件的创建
```
<div id="app">
    <!--使用组件-->
    <index></index>
</div>
<script>
    let product = {
        template: '<div>这里是一个商品列表</div>'
    }
    // 创建组件
    Vue.component("index", {
        template: '<div>这是首页:<product/></div>',
        components:{
            product
        }
    })

    new Vue({
        el: "#app",
        data: {}
    })

</script>
```
