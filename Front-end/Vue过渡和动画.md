####transition
```
<template>
<div>
  <button @click="show =!show">Toggle</button>
  <transition name ="fade">
     <p v-if="show">Hello Vue</p>
  </transition>
</div>
</template>

<script>
    export default {
        name: "HelloTransition",
      data(){
          return{
            show:true
          }
      }
    }
</script>
增加样式效果
```
<style scoped>
  .fade-enter-active, .fade-leave-active{
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

</style>
```
####进场动画类
v-enter(开始状态)
v-enter-active(中间状态)
v-enter-to(结束状态)
####离场动画类
v-leave(开始状态)
v-leave-active(中间状态)
v-leave-to(结束状态)
####动画列表
trainsition-group
v-move
####anmiate.css
####js实现动画

@before-enter
@enter
@after-enter
@before-leave
@leave
@after-leave
```
<div id="app">
    <transition
            @before-enter="beforeEnter"
            @enter="enter"
            @after-enter="afterEnter"
            @before-leave="beforeLeave"
            @leave="leave"
            @after-leave="afterLeave"
    >
        <div v-if="isShow">显示</div>
    </transition>

    <button @click="isShow =!isShow">切换</button>
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            isShow: false
        },
        methods:{
            beforeEnter(el){
                el.style.paddingLeft ="100px"
                console.log('beforeEnter')
            },
            enter(el,done){
                console.log('enter')
                let step = 1
                let interval = setInterval(()=>{
                    el.style.paddingLeft =(100-step)+"px"
                    step++
                    //判断结束时机
                    if (step==100){
                        clearInterval(interval)
                        done()
                    }
                },10)

            },
            afterEnter(el){
                el.style.paddingLeft="0px"
                console.log('afterEnter')
            },
            beforeLeave(el){
                console.log('beforeLeave')

            },
            leave(el,done){
                console.log('leave')
                done()
            },
            afterLeave(el){
                console.log('afterLeave')
            }
        }
    })
</script>
```

