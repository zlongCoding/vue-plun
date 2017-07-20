<template>
	<div class="zl-msgbox-wapper">
		<div class="v-modal" v-show="value"></div>
		<transition name="msgbox-bounce">
			<div class="zl-msgbox" v-show="value">
				<div class="zl-msgbox-header" v-if="title !== '' ">
					<div class="zl-msgbox-title">{{title}}</div>
				</div>
				<div class="zl-msgbox-content">
					<!-- <div class="zl-msgbox-message" v-html="message"></div> -->
					<div class="zl-msgbox-input">
						<!-- <input v-model="inputValue" :placeholder="inputPlaceholder" ref="input"/> -->
						<div class="zl-msgbox-errormsg" :style="{visibility : !!editorErrorMessage ? 'visible' : 'hidden'}">{{editorErrorMessage}}</div>
					</div>
				</div>
				<div class="zl-msgbox-btns">
					<input type="bottom" class="zl-msgbox-cancel" v-show="showCancelButton" @click="handleAction('cancel')" v-model="cancelButtonText" readonly="readonly" />
					<input type="bottom" class="zl-msgbox-confirm" v-show="showConfirmButton" @click="handleAction('confirm')" v-model="confirmButtonText" readonly="readonly" :class="showCancelButton ? '' : 'zl-msgbox-confirmall' "/>

				</div>
			</div>
		</transition>
	</div>
</template>
<script>
	export default {
		props:{
			value:{
				type:Boolean,
				default:false
			},
			title:{
				type:String,
				default:'请输入title',
			},
			editorErrorMessage:{
				type:String,
				default:'请填写提示文字'
			},
			showCancelButton:{
				type:Boolean,
				default:true
			},
			showConfirmButton:{
				type:Boolean,
				default:true
			},
			cancelButtonText:{
				type:String,
				default:'取消'
			},
			confirmButtonText: {
				type:String,
				default:'确定'
			}
		},
		methods:{
			handleAction(content){
				if(content === 'cancel'){
					this.$emit("callbackall" , false)
				}else{
					this.$emit("callbackall" , true)
				}
			}
		},
		mounted(){
			console.log(this.showCancelButton)
		}
	}
</script>
<style lang="css" scoped>
	.zl-msgbox{
		position: fixed;
		top:50%;
		left: 50%;
		transform: translate3d(-50%,-50%,0);
		background-color: #fff;
		width: 85%;
		border-radius: 3px;
		font-size: 16px;
		overflow: hidden;
		transition: .2s;
		backface-visibility: hidden;
		--webkit-user-select: none;
	}
	.zl-msgbox-header{
		padding:15px 0 0 ;
	}
	.zl-msgbox-content{
		padding: 10px 20px 15px;
		border-bottom: 1px solid #ddd;
		min-height: 36px;
		position: relative;
	}
	.zl-msgbox-input{
		padding-top: 15px;
	}
	.zl-msgbox-input input{
		border:1px solid #dedede;
		border-radius: 5px;
		padding: 4px 5px;
		width:100%;
		appearance: none;
		outline: none;
	}
	.zl-msgbox-input input.invalid{
		border-color: #ff4949;
	}
	.zl-msgbox-input input.invalid:focus{
		border-color:#ff4949;
	}
	.zl-msgbox-errormsg{
		color: #000;
		font-size: 12px;
		min-height: 18px;
		margin-top: 2px;
	}
	.zl-msgbox-title{
		text-align: center;
		padding-left: 0;
		margin-bottom: 0;
		font-size: 16px;
		font-weight: bold;
		color:#333;
	}
	.zl-msgbox-message{
		color:#999;
		margin:0;
		text-align: center;
		line-height: 36px;
	}
	.zl-msgbox-btns{
		display: -webkit-box;
		display: -webkit-flex;
		display: -ms-flex;
		display: flex;
		height: 40px;
		line-height: 40px;
	}
	.zl-msgbox-btn{
		line-height: 35px;
		display: block;
		background-color: #fff;
		flex: 1;
		margin:0;
		border: 0;
		-webkit-tap-highlight-color:transparent;
	}


	.zl-msgbox-cancel{
		width: 50%;
		padding: 0px;
		text-align: center;
		border: none;
		border-right: 1px solid #eee;
		background-color: #fff;
		font-size: 14px;
		-webkit-tap-highlight-color:transparent;
		outline: none;
	}
	.zl-msgbox-confirm{
		width: 50%;
		font-size: 14px;
		color:#26a2ff;
		outline: none;
		text-align: center;
		-webkit-tap-highlight-color:transparent;
	}
	.zl-msgbox-confirm:active{
		color:#26a2ff;
	}
	.msgbox-bounce-enter{
		opacity: 0;
		transform: translate3d(-50%,-50%,0) scale(0.7);
	}
	.msgbox-bounce-leave-active{
		opacity: 0;
		transform: translate3d(-50%,-50%,0) scale(0.9);
	}
	.v-modal-enter {
		animation: v-modal-in .2s ease;
	}

	.v-modal-leave {
		animation: v-modal-out .2s ease forwards;
	}

	@keyframes v-modal-in {
		0% {
			opacity: 0;
		}
		100% {
		}
	}

	@keyframes v-modal-out {
		0% {
		}
		100% {
			opacity: 0;
		}
	}

	.v-modal {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		opacity: 0.5;
		background: #000;
	}
.zl-msgbox-confirmall{
	width: 100%;
}
</style>