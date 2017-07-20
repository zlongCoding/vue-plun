<template>
	<transition name="vodal-fade" >
		<div class=" vodal-zoom-enter-active vodal-zoom-leave-active" >
		<div class="vodal-mask" v-show="show"   @click="dialog_end" />
		<div class="vodal-dialog"  v-bind:class="'dialog'+show" :style="dialogStyle">
			<span class="vodal-close" @click="dialog_end" ></span>
			<div class="header">{{headerTitle}}</div>
			<div class="body">{{content}}</div>
			<div class="dialog-button">
			<button class="vodal-confirm-btn" @click="dialog_success">确认</button>
				<button class="vodal-cancel-btn" @click="dialog_end" v-if="dialogclose">关闭</button>
			</div>
		</div>
	</div>
</div>
</transition>
</template>
<script>
	export default{
		name:'dialog',
		props:{
			show:{
				type:Boolean,
				required:false
			},
			content:{
                type:String,
                default:'请填写内容'
			},
			headerTitle:{
				type:String,
				default:'请填写Title'
			},
			width:{
				type:Number,
				default:400
			},
			height:{
				type:Number,
				default:240
			},
			measure:{
				type:String,
				default:'px'
			},
			dialogclose:{
				type:Boolean,
				default:true
			}
		},
		computed:{
			dialogStyle(){
				return{
					width:`${this.width+this.measure}`,
					height:`${this.height+this.measure}`
				}
			},

		},
		methods:{
			dialog_success(){				
                this.$emit('showsuccess', false);
			},
			dialog_end(){
                this.$emit('show', false);
			}
		}
	}
</script>
<style scoped>
	.vodal-dialog{
		position: absolute;
		top:0;
		left: 0;
		right:0;
		bottom: 0;
		margin: auto;
		z-index: 101;
		padding: 15px;
		background: #fff;
		border-radius: 3px;
		box-shadow: 0 1px 3px rgba(0,0,0,.2);
	}
	.vodal-mask {
		position: absolute;
		background: rgba(0, 0, 0, .3);
	}
	.vodal, .vodal-mask {
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 100;
	}
	.vodal-close{
		position:absolute;
		cursor: pointer;
		top:16px;
		right: 16px;
		width: 16px;
		height: 16px;
	}
	.vodal-close:before,
	.vodal-close:after {
		position: absolute;
		content:'';
		height: 2px;
		width: 100%;
		top:50%;
		left:0;
		margin-top: -1px;
		background: #999;
		border-radius: 100%;
		--webkit-transition: background .2s;
		transition: background .2s;
	}


	.vodal-close:before{
		--webkit-transform: rotate(45deg);
		transform: rotate(45deg);
	}

	.vodal-close:after{
		--webkit-transform: rotate(-45deg);
		transform: rotate(-45deg);
	}
	.vodal-close:hover:before,
	.vodal-close:hover:after {
		background: #333;
	}

	.header {
		font-size: 16px;
		padding-bottom: 10px;
		border-bottom: 1px solid #e9e9e9;
	}
	.body {
		padding-top: 15px;
	}


	.dialog-button{
		position: absolute;
		bottom: 16px;
		width: 100%;
		right:0;
		text-align: center;
	}
	.vodal-cancel-btn, .vodal-confirm-btn {

		font: inherit;
		width: 70px;
		padding: 4px 15px;
		border-radius: 3px;
		transition: background .2s;
		border: 1px solid #03a9f4;
	}
	.vodal-confirm-btn {
		color: #fff;
		margin-right: 20px;
		background: #03a9f4;
	}
	.vodal-cancel-btn {
		color: #03a9f4;

		background: #fff;
	}

@-webkit-keyframes vodal-fade-enter {
    from {
        opacity: 0;
    }
}

@keyframes vodal-fade-enter {
    from {
        opacity: 0;
    }
}

.vodal-fade-enter-active {
    -webkit-animation: vodal-fade-enter both ease-in;
    animation: vodal-fade-enter both ease-in;
}

@-webkit-keyframes vodal-fade-leave {
    to {
        opacity: 0
    }
}

@keyframes vodal-fade-leave {
    to {
        opacity: 0
    }
}

.vodal-fade-leave-active {
    -webkit-animation: vodal-fade-leave both ease-out;
    animation: vodal-fade-leave both ease-out;
}
.dialogfalse{
    -webkit-animation: vodal-zoom-leave  .5s both;
    animation: vodal-zoom-leave .5s both;
}
@keyframes vodal-zoom-leave{
		from{
		--webkit-transform: scale3d(1,1,1);
		transform: scale3d(1,1,1);
	}
	to{
		--webkit-transform:scale3d(0,0,0);
		transform:scale3d(0,0,0);
	}
}
@-webkit-keyframes vodal-zoom-leave{
from{
		--webkit-transform: scale3d(1,1,1);
		transform: scale3d(1,1,1);
	}
	to{
		--webkit-transform:scale3d(0,0,0);
		transform:scale3d(0,0,0);
	}
}
.dialogtrue{
   -webkit-animation: vodal-zoom-enter .5s both;
    animation: vodal-zoom-enter .5s both;
}
@keyframes vodal-zoom-enter{
	from{
		--webkit-transform: scale3d(.3,.3,.3);
		transform: scale3d(.3,.3,.3);
	}
	to{
    --webkit-transform:scale3d(1,1,1);
		transform:scale3d(1,1,1);
	}
}
@-webkit-keyframes vodal-zoom-enter{
	from{
		--webkit-transform:scale3d(.3,.3,.3);
		transform:scale3d(.3,.3,.3);
	}
	to{
    --webkit-transform:scale3d(1,1,1);
		transform:scale3d(1,1,1);
	}
}
</style>