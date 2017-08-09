<template>
	<label class="zl-switch">
		<input type="checkbox" @change="zlChange" ref="input"/>
		<span class="zl-span" ref="core"><span :class="{'zl-check':value}"></span></span>
		<transition>
			<div class="zl-div" v-if="!value"><span>{{zlNo}}</span></div>
		</transition>
		<transition>
			<div class="zl-div-yes" v-if="value"><span>{{zlYes}}</span></div>
		</transition>

	</label>
</template>
<script>
	export default {
		name:'zlSwitch',
		methods: {
			zlChange () {
				this.$emit("input" , this.$refs.input.checked)
				this.value = this.$refs.input.checked;
			},
			setBackgroundColor() {
				let newColor = this.value ? this.onColor : this.offColor;
				this.$refs.core.style.borderColor = newColor;
				this.$refs.core.style.backgroundColor = newColor;
			}
		},
		watch: {
			value(){
				this.$refs.input.checked = this.value;
				if (this.onColor || this.offColor) {
					this.setBackgroundColor();
				}
			}
		},
	   data(){
	   	return {
	   		value:null
	   	}
	   },
	   created(){
	   	   this.value = this.checked;
	   },
		props: {
			checked:{
				type: Boolean,
				default: true
			},
			zlNo: {
				type: String,
				default: ""
			},
			zlYes: {
				type: String,
				default: ""
			},
			offColor:{
				type:String,
				default:'#bfcbd9'
			},
			onColor:{
				type:String,
				default:'#20a0ff'
			}
		}
	}
</script>
<style scoped>
	.zl-switch{
		display: inline-block;
		position: relative;
		font-size: 14px;
		line-height: 22px;
		height: 22px;
		vertical-align: middle;
	}
	.zl-switch input {
		display: none;
	}
	.zl-span{
		margin:0;
		display: inline-block;
		position: relative;
		width: 46px;
		height: 22px;
		border:1px solid #bfcbd9;
		background: #bfcbd9;
		box-sizing: border-box;
		cursor: pointer;
		border-radius: 12px;
	}
	.zl-span span{
		position: absolute;
		top: 0;
		left: 0;
		transition: transform .3s;
		width: 16px;
		height: 16px;
		background: #fff;
		border-radius: 100%;
		transform:translate(2px,2px);
	}
	.zl-check{
		transform: translate(26px , 2px) !important;
	}
	.zl-div, .zl-div-yes{
		transition: .2s;
		width: 46px;
		left: 0;
		top:0;
		cursor: pointer;
		position: absolute;
		display: inline-block;
		font-size: 14px;
		height: 22px;
	}
	.zl-div-yes span, .zl-div span{
		line-height:1;
		top:4px;
		color:#fff;
		position: absolute; 
	}
	.zl-div-yes span{
		left: 6px;
	}
	.zl-div span {
		left: 20px;
	}
</style>