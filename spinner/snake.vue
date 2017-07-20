<template>
	<div 
	    class="zl_spinner_snake" 
        :style="{
           'border-top-color':spinnerColor,
           'border-left-color':spinnerColor,
           'border-bottom-color':spinnerColor,
           'height':spinnerSize,
           'width':spinnerSize
         }"></div>
</template>

<script>
	 import common from './common.vue';
	 export default {
	 	name: 'snake',
	 	mixins:[common]
	 };
</script>
<style lang="css">
	.zl_spinner_snake{
		animation:zl_spinner_rotate 0.8s infinite linear;
		border:4px solid transparent;
		border-radius: 50%;
	}
	@keyframes zl_spinner_rotate {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}
</style>