<template>
  <transition name="zl-fade" v-if="showhidden">
    <div class="zl-img-view" @click="hiddenImg">
      <div class="zl-img-layer"></div>
      <img :src="zlImgSrc"/>
    </div>
  </transition>
</template>
<script>
  export default {
    props:{
      zlImgSrc:{
        type:String,
        default:''
      },
      showhidden:{
        type:Boolean,
        default:false
      }
    },
    methods:{
      hiddenImg(){
        this.$emit('imghidden' , false)
      }
    }
  }
</script>
<style lang="css" scoped>
  .zl-fade-enter-active{    
    animation:zl-fade-enterleave .3s ease;
  }
  
  .zl-fade-leave-active{    
    animation:zl-fade-leave .2s ease forwards;
  }
  @keyframes zl-fade-enterleave {
    0% {
      opacity: 0;
    }
    100% {
      
    }
  }

  @keyframes zl-fade-leave {
    0% {
      
    }
    100% {
      opacity: 0;
    }
  }

  .zl-img-view .zl-img-layer{
    position: absolute;
    left:0;
    top:0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: rgba(0,0,0,.7);
    z-index: 9998;
  }
  .zl-img-view{
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
  }
  .zl-img-view img{
    display: block;
    max-width: 100%;
    position: absolute;
    left: 0;
    right: 0;
    margin: auto;
    z-index: 9999;
    top: 0;
    bottom: 0;
  }
</style>