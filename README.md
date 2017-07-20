# dialog 
Vue dialog原生样式的插件<br/>


```
  npm i dialog-all --save

```

 
  如果你安装了淘宝镜像 可以使用cnpm i dialog-all --save <br/>


  如果没有安装建议安装一个毕竟国内的影响心情 [点击安装](https://npm.taobao.org/)

 或者

```
 <template>
  <div class="hello">
  <button @click="aaa">fasfsfsfsfs</button>
    <dia-logs :show="show" v-on:show = 'showHidden' :dialogclose = "false" v-on:showsuccess="successEnd" :width="'500'" :height="'500'" :measure="'px'"></dia-logs>
  </div>
</template>

<script>
import diaLogs from './diaLogs.vue'
export default {
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      show:false
    }
  },
  components:{
    diaLogs
  },
  methods:{
    showHidden(content){
        this.show = content;
    },
    successEnd(content){
      this.show = content;
    },
    aaa(){
      this.show = true
    }
  }
}
</script>
<style scoped>
</style>


```


```
几个值得作用
v-on:show = 'showHidden' 监听点击取消和错号空白处的事件
:show="show" 是否显示   默认false

v-on:showsuccess="successEnd" 监听点击确认的时候的事件

:width="'500'" 设置宽度 默认 400

:height="'500'"  设置高度  默认 400


:measure="'px'" 设置单位   默认px

:dialogclose = "false" 取消按钮是否显示 默认true
```




 如果有什么bug欢迎指出  [欢迎指正](https://github.com/zhanglongdream/dialog/issues/4)

  
   最后帮忙来个start呗，thanks ☺☺;
