####computed
使用v-model实现数据渲染
```
<div id="app">
   <div>姓名{{firstName+lastName}}</div>
   <input type="text" v-model="firstName">
   <input type="text" v-model="lastName">
</div>
<script>
     new Vue({
        el:'#app',
         data:{
            firstName: "",
            lastName: "",
         }
    })
</script>
```
computed写函数，这个函数很特殊，它的函数名作为一个属性来使用。
计算属性是缓存的，当页面中调用一个计算属性多次的时候，后面的计算属性的值，会直接从第一次得到的结果中去获取。所以说它的性能比较高，推荐使用计算属性。
```
<div id="app">
    <div>姓名{{fullName}}</div>
   <input type="text" v-model="firstName">
   <input type="text" v-model="lastName">

</div>
<script>
     new Vue({
        el:'#app',
         data:{
            firstName: "",
            lastName: "",
         },
         computed:{
            fullName(){
                return this.firstName+this.lastName
            }
         }
    })
</script>

```
利用计算属性实现字符串反转
```
<div id="app">
   <p>原始语句{{ msg }}</p>
    <p>计算属性 {{reversedMsg}}</p>

</div>

<script>
     new Vue({
        el: '#app',
        data: {
            msg:"hello vue"
        },
         computed:{
            reversedMsg: function () {
                return this.msg.split("").reverse().join("")
            }
         }
    })
</script>
```
计算商品总价
```
<div id="app">
    <div>
        <span>单价:{{price}} 数量:{{count}}</span>
        <button @click="increase">增加</button>
        <button @click="decrease">减少</button>
        <p>总价:{{totalPrice}}</p>
    </div>

</div>
<script>
     new Vue({
        el:'#app',
         data:{
            price: 5,
            count: 2,
             total:10
         },
         methods:{
            increase(){
               if (this.count>=this.total){
                   alert("超过了库存")
               }else {
                   this.count++;
               }

             },
             decrease(){
                 if (this.count<=0){
                     alert("总数已经是0")
                 }else {
                     this.count--;
                 }
            }
         },
         computed:{
            totalPrice(){
                return this.price*this.count
            }
         }
    })
</script>
```
注意 compued中不能使用异步函数
