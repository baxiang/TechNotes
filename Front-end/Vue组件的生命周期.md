一个组件（实例）从开始到最后的消亡所经历的各种状态，就是一个组件的(实例）生命周期
生命周期函数定义：从组件的被创建，到组件挂载到页面上运行，再到页面关闭组件被卸载，这三个阶段总是伴随着组件的各种各样的事件，这些事件，统称为组件的生命周期函数
![image.png](https://upload-images.jianshu.io/upload_images/143845-c2d7c47128514cab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


开发人员可以通过Vue 提供的钩子函数，让我们写的代码参与Vue的生命周期里面来，让我们的代码 在合适的阶段起到响应的作用。
注意：
1 Vue在执行过程中会自动调用`生命周期钩子函数`,我们主需要提供这些钩子函数即可。
2 钩子函数的名称都是Vue中规定好的。

beforeCreate 组件刚刚被创建
created 组件创建完成
breforeMount 挂载之前
mounted 挂载之后
beforeDestory 组件销毁前调用
destoryed 组件销毁后调用
```
<script>
  export default {
    name: "HelloVue",
    beforeCreate() {
      console.log("beforeCreate")
    },
    created() {
      console.log("created")
    },
    beforeMount() {
      console.log("beforeMount")
    },
    mounted() {
      console.log("mounted")
    }
  }
</script>
```
####挂载阶段（进入页面）

####更新阶段（当数据发生变化）
####卸载阶段（实例卸载）

