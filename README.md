# iosSelect

Vue ios select原生样式的插件<br/>

按照ios select下拉框开发的一个插件，经测试完全可用

```
  npm i select-ios --save

```

 
  如果你安装了淘宝镜像 可以使用cnpm i select-ios --save <br/>


  如果没有安装建议安装一个毕竟国内的影响心情 [点击安装](https://npm.taobao.org/)
  

```
使用方法  
<template>
  <div>
    <div @click="showSelect">选择的值是{{month}}</div>
      <selet-ios :listData="listData" v-model="month" :parentHidden="Hidden" v-on:show="showHidden" type ="other"></selet-ios>
  </div>
</template>
<script>
   import seletIos from 'select-ios'
  export default{
    name: 'pdIosSelect',
    data () {
      return {
        listData: Array.from({length: 12}, (value, index) => 1 + index),
        month: 2,
        Hidden:false,
      }
    },
    components: {
      seletIos
    },
    methods:{
      showSelect(){
       this.Hidden = true;
      },
      showHidden(content){
        this.Hidden = content;
      }
    }
  }
👌 结束
</script>
```


其中v-model可以指定select初始化的值，因为api不多就不写了，简单的业务这个是可以胜任的

```
主要是v-model是下拉框初始化的值 
    :listData是传入到下拉框中的值
    v-on:show 是控制取消 ， 确定按钮的显示和隐藏 ， 采用了父组件传递子组件然后子组件传递父组件
    type 可以无限滚动  值可以是随意的如果不需要不加就行
    parentHidden 传递给子组件的值
```




 如果有什么bug欢迎指出  [欢迎指正](https://github.com/zhanglongdream/vue-plun/issues/2)

  
   最后帮忙来个start呗，thanks ☺☺;
