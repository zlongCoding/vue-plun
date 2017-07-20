<template>
	<span><component :is = "zl_spinner"></component></span>
</template>
<script>
	const zl_spinners = [
        'snake',
        'double',
        'triple',
	];
	const parseSpinner = function(index){
        if({}.toString.call(index) === '[object Number]'){
            if(zl_spinners.length <= index){
               console.warn(`'${index}' 没有找到请查看传入的值 `)
               index = 0;
            }
            return zl_spinners[index];
        }
        if(zl_spinners.indexOf(index) === -1){
           console.warn(`'${index}' 没有找到请查看传入的值`);
           index = zl_spinners[0]
        }
        return index;
	}
	export default {
       name:'zl-spinner',
       computed: {
          zl_spinner(){
          	return `spinner-${parseSpinner(this.type)}`;
          }
       },
       components: {
            SpinnerSnake: require('./spinner/snake.vue'),
            SpinnerDouble: require('./spinner/double-bounce.vue'),
            SpinnerTriple: require("./spinner/triple-bounce.vue"),
            // SpinnerFading: require("./spinner/fading-circle.vue")
       },
       props:{
       	  type:{
       	  	 default:0
       	  },
       	  size:{
       	  	 type:Number,
       	  	 default:28
       	  },
       	  color:{
       	  	 type:String,
       	  	 default:'#ccc'
       	  }
       }
	}
</script>