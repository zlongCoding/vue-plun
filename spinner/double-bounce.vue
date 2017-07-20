<template>
  <div class="zl-spinner-double-bounce" :style="{
    width: spinnerSize,
    height: spinnerSize
  }">
  <div class="zl-spinner-double-bounce-bounce1" :style="{ backgroundColor: spinnerColor }"></div>
  <div class="zl-spinner-double-bounce-bounce2" :style="{ backgroundColor: spinnerColor }"></div>
</div>
</template>

<script>
  import common from './common.vue';

  export default {
    name: 'double-bounce',

    mixins: [common]
  };
</script>

<style lang="css">

  .zl-spinner-double-bounce{
    position: relative;
  }
  .zl-spinner-double-bounce-bounce1 , .zl-spinner-double-bounce-bounce2{
    width: 100%;
    height: 100%;
    border-radius: 50%;
    opacity: 0.6;
    position: absolute;
    top: 0;
    left: 0;
    animation: zl-spinner-double-bounce 2.0s infinite ease-in-out;
  }
  .zl-spinner-double-bounce-bounce2{
    animation-delay:-1.0s;
  }
  @keyframes zl-spinner-double-bounce {
    0%, 100% {
      transform: scale(0.0);
    }

    50% {
      transform: scale(1.0);
    }
  }
</style>
