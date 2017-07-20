# paginate
vue翻页插件


```
  npm i pagination_vue --save

```

 
  如果你安装了淘宝镜像 可以使用cnpm i pagination_vue --save <br/>


  如果没有安装建议安装一个毕竟国内的影响心情 [点击安装](https://npm.taobao.org/)
  

```
    <pagination :lastpage="20" :currentpage = "currentpage" :centerside = '1' @callbackpage="callbackpage" :side="1"></pagination>

    callbackpage(page){
        this.currentpage = page;
        console.log(this.currentpage)
     },




里面有几个值只要设置一下就行了

nexttext 下一页显示的字段

prevtext 上一页显示的字段
currentpage 当前页
lastpage 总页数

centerside 中间数量
side 边缘数量

callbackpage 当前点击的页数



```






 如果有什么bug欢迎指出  [欢迎指正](https://github.com/zhanglongdream/vue-plun/issues/3)

  
   最后帮忙来个start呗，thanks ☺☺;