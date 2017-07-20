# message-reminder

是一款操作提示插件

```
  npm i message-reminder --save

```

 
  如果你安装了淘宝镜像 可以使用cnpm i message-reminder --save <br/>


  如果没有安装建议安装一个毕竟国内的影响心情 [点击安装](https://npm.taobao.org/)
  

```
使用方法  
<message v-on:callbackall="callbackAll" :value="values" title="我填写了" editorErrorMessage="你确定你填写的吗" :showCancelButton = "false"></message>




methods里面写入

      callbackAll(contetn){
       console.log(contetn)
       if(contetn){
           this.values= false;
       }
     },
👌 结束
</script>


```



```
callbackall  是点击确定 ， 取消按钮的时候的回调 true是确定 false是取消

value  是提示插件是否需要显示

title 填写的头部提示信息

editorErrorMessage 填写的提示内容

showCancelButton 是否显示取消按钮默认是true

```




 如果有什么bug欢迎指出  [欢迎指正](https://github.com/zhanglongdream/vue-plun/issues/1)

  
   最后帮忙来个start呗，thanks ☺☺;
