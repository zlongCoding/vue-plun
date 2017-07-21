<template>
  <div class="zl-imgBig" ref="zlimg" v-if="showContent">
    <div v-on:mousemove="moveImg" :style="{'width':imgWidth+'px' , 'height':imgHeight+'px'}" class="zl-imgshowonc" v-on:mouseleave="leaveImg" v-on:mouseenter="enterImg">
      <img :src="showImg" :style="{'width':imgWidth+'px' , 'height':imgHeight+'px'}"/>
      <div class="zl-imgCover" :style="{'left':coverLet+'px','top':coverTop+'px','height':coverWidth+'px','width':coverHeight+'px'}" v-show="contentImg" ref="coverimg"></div>
    </div>
    <div class="zl-showimg" :style="{'width':imgWidth+'px' , 'height':imgHeight+'px','left':leftImg+'px','top': topImg + 'px'}" v-show="contentImg">
      <img :style="{'width':(imgWidth*bigSize)+'px' , 'height':(imgHeight*bigSize)+'px' , 'right':backPstLeft+'px','top':backPstTop+'px'}" :src="Imglarger" />
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        showImg:'', //小图
        Imglarger:'',  //大图
        showContent:true, //组件是否显示
        imgWidth:200, //小图的宽度 大图的高度 这两个是相等的
        imgHeight:200, //小图的高度 大图的宽度 这两个是相等的
        contentImg:false, //小图上面的看大图的区域是否显示，如果不显示则大图无法显示
        leftImg:210,   //大图距离小图的距离因为是left最好是在本身小图的基础上加px
        topImg:0,   //大图和小图等高 默认
        coverLet:1, 
        coverTop:1,
        coverHeight:100, //小图上面显示区域的高度
        coverWidth:100,//小图上面显示区域的宽度
        offsetTopImg:0,
        offsetLeftImg:0,
        backPstLeft:0,
        backPstTop:0,
        bigSize:2  //放大的倍数
      }
    },
    methods:{
      moveImg(event) {

        const coverTopImg = this.$refs.coverimg.offsetTop;
        const coverLeftImg = this.$refs.coverimg.offsetLeft;
        const eventX = event.clientX-this.offsetLeftImg;
        const eventY = event.clientY-this.offsetTopImg;
        this.coverLet = eventX-this.coverLet/2;;
        this.coverTop = eventY-this.coverTop/2;
        if((coverTopImg+this.coverTop) <= this.imgHeight && (coverLeftImg+this.coverWidth) <= this.imgWidth ){
          this.backPstLeft =  (coverLeftImg*this.bigSize);
          this.backPstTop = -(coverTopImg*this.bigSize);
        }
      },
      leaveImg(){
        this.contentImg = false;
      },
      enterImg() {
        this.contentImg = true;
      }
    },
    mounted(){
      if(this.Imglarger === ''){
        this.Imglarger = this.showImg;
      } 
      this.offsetTopImg = this.$refs.zlimg.offsetTop;
      this.offsetLeftImg = this.$refs.zlimg.offsetLeft;
    }
  }
</script>
<style>
  .zl-imgBig{
    position: relative;
  }
  .zl-imgBig .zl-showimg{
    position: absolute;
    overflow: hidden;
  }
  .zl-showimg img{
    position: relative;
  }
  .zl-imgshowonc{
    position: relative;
    overflow: hidden;
  }
  .zl-imgCover{
    position: absolute;
    overflow: hidden;
    visibility: visible;
    background: rgb(223, 223, 223);
    opacity: 0.6;
    overflow: hidden;
    visibility: visible;
    cursor: move;
    background-repeat: no-repeat;
  }
</style>