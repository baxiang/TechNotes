官方地址[http://mint-ui.github.io/#!/zh-cn](http://mint-ui.github.io/#!/zh-cn)
安装
```
# Vue 1.x
npm install mint-ui@1 -S
# Vue 2.0
npm install mint-ui -S
```
main.js导入MintUI 和样式
```
import MintUI from 'mint-ui'
import 'mint-ui/lib/style.css'
```
使用tabbar
```
<template>

  <mt-tabbar >
    <mt-tab-item id="tab1">
      <img slot="icon" src="../assets/logo.png">
      tab1
    </mt-tab-item>
    <mt-tab-item id="tab2">
      <img slot="icon" src="../assets/logo.png">
      tab2
    </mt-tab-item>
    <mt-tab-item id="tab3">
      <img slot="icon" src="../assets/logo.png">
      tab3
    </mt-tab-item>
    <mt-tab-item id="tab4">
      <img slot="icon" src="../assets/logo.png">
      tab4
    </mt-tab-item>
  </mt-tabbar>

</template>

<script>
  import { Tabbar, TabItem } from 'mint-ui';
  import Vue from 'vue'

  Vue.component(Tabbar.name, Tabbar);
  Vue.component(TabItem.name, TabItem);
    export default {
        name: "MintUI",
      mounted() {

      }
    }
</script>
```
