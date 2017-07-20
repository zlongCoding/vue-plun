<template>
	<div>
		<ul v-if="lastpage > 1" class="pagination_web">
			<li v-if="activePrev"><a @click="prev" href="javascript:void(0)">{{prevtext}}</a></li>
			<li v-else class="disabled_pagination"><span>{{ prevtext }}</span></li>
			<li  :class="{active_pagination: isActive(headEnd)}">
				<span v-if='isActive(headEnd)'>{{headEnd}}</span>
				<a v-else href="javascript:void(0)" @click='turn(headEnd)'>{{headEnd}}</a>
			</li>
			<li v-if="headEllipsis" class="disabled_pagination"><span>...</span></li>
			<li v-for="n in centerCount" :class="{active_pagination : isActive(n+ centerOffset)}">
				<span v-if="isActive(n+ centerOffset)">{{n+ centerOffset}}</span>
				<a v-else @click="turn(n + centerOffset)" href="javascript:void(0)">{{n+ centerOffset}}</a>
			</li>
			<li v-if="tailEllipsis" class="disabled_pagination"><span>...</span></li>
			<li v-for="n in tailShowCount" :class="{active_pagination: isActive(n+tailOffset)}">
				<span v-if="isActive(n+tailOffset)">{{ n + tailOffset}}</span>
				<a v-else href="javascript:void(0)" @click="turn(n + tailOffset)" >{{ n + tailOffset}}</a>
			</li>
			<li v-if="activeNext">
				<a @click="next" href="javascript:void(0)">{{nexttext}}</a>
			</li>
			<li v-else class="disabled_pagination"><span>{{nexttext}}</span></li>
		</ul>
	</div>
</template>
<script>
	export default{
		data(){
			return{
				popStateListening:false,
			}
		},
		props:{
			side:{
				type:Number,
				default:1,
			},
			centerside:{
				type:Number,
				default:4,
			},
			lastpage:{
				type:Number,
				required:true
			},
			currentpage:{
				type:Number,
				required:true
			},
			pagename:{
				type:String,
				default:'page'
			},
			prevtext:{
				type:String,
				default:'«'
			},
			nexttext: {
				type: String,
				default: '»'
			},
			urlsync: {
				type: Boolean,
				default: false
			}
		},
		computed:{
			showCount: function () {
				return Math.min(this.lastpage, (this.centerside + this.side) * 2 + 1);
			},
			halfCenterCount: function () {
				return parseInt((this.showCount - this.side * 2) / 2);
			},
			headEnd: function () {
				return Math.min(this.lastpage, this.side);
			},
			tailShowCount: function () {
				return Math.min(this.side, Math.max(this.lastpage - this.side, 0));
			},
			tailOffset: function () {
				return this.lastpage - this.tailShowCount;
			},
			headEllipsis: function () {
				return this.lastpage > this.showCount && this.currentpage > this.side + this.halfCenterCount + 1;
			},
			tailEllipsis: function () {
				return this.lastpage > this.showCount && this.currentpage < this.lastpage - this.halfCenterCount - this.side;
			},
			centerCount: function () {
				return this.lastpage > this.side * 2
				? this.lastpage <= this.showCount
				? this.lastpage - this.side * 2
				: this.showCount - this.side * 2
				: 0;
			},
			centerOffset: function () {
				return !this.tailEllipsis
				? this.lastpage - this.showCount + this.side
				: this.headEllipsis
				? this.currentpage - this.halfCenterCount - 1
				: this.side;
			},
			activePrev: function () {
				return this.currentpage > 1;
			},
			activeNext: function () {
				return this.currentpage < this.lastpage;
			}
		},
		methods:{
			turn: function(page){
				if(this.urlsync) {
					var url = window.location.href,
					params = this.queryParams(url),
					state = {'page': page};
					if (!this.popStateListening) {
						var that = this;
						this.popStateListening = true;
						window.addEventListener("popstate", function() {
							that.$emit('callbackpage', window.history.state.page);
						});
					}
					if (!window.history.state) {
						window.history.replaceState({'page': this.currentpage}, '', url);
					}
					if (!params.length) {
						window.history.pushState(state, '', url + '?' + this.pagename + '=' + page);
					} else if (params[this.pagename]) {
						window.history.pushState(state, '', url.replace(new RegExp('(' + this.pagename + '=)\\d+'), '$1' + page));
					} else {
						window.history.pushState(state, '', url + '&' + this.pagename + '=' + page);
					}
				}

				this.$emit('callbackpage', page);
			},
			prev: function(){
				this.turn(this.currentpage - 1);
			},
			next: function(){
				this.turn(this.currentpage + 1);
			},
			isActive:function(item){
				return this.currentpage == item;
			},
			queryParams:function(url){
				var vars = [] , hash , index = url.indexOf('?');
				if(index < 0){
					return []
				}else{
					var hashIndex = url.slice(index+1).split('&');
					for(var i = 0 ; i < hashIndex.length ; i++){
						hash = hashIndex[i].split('=');
						vars.push(hash[0]);
						vars[hash[0]] = hash[1];
					}
					return vars;
				}
			}
		}
	}
</script>
<style scoped>
	.pagination_web>li:first-child>a, .pagination_web>li:first-child>span {
		margin-left: 0;
		border-top-left-radius: 4px;
		border-bottom-left-radius: 4px;
	}
	.pagination_web>li>a, .pagination_web>li>span {
		position: relative;
		float: left;
		padding: 6px 12px;
		margin-left: -1px;
		line-height: 1.42857143;
		color: #428bca;
		text-decoration: none;
		background-color: #fff;
		border: 1px solid #ddd;
	}
	.pagination_web>li {
		display: inline;
	}
	.pagination_web {
		display: inline-block;
		padding-left: 0;
		margin: 20px 0;
		border-radius: 4px;
	}
	.pagination_web>.active_pagination>a, .pagination_web>.active_pagination>span, .pagination_web>.active_pagination>a:hover, .pagination_web>.active_pagination>span:hover, .pagination_web>.active_pagination>a:focus, .pagination_web>.active_pagination>span:focus {
		z-index: 2;
		color: #fff;
		cursor: default;
		background-color: #428bca;
		border-color: #428bca;
	}
	.pagination_web>li>a, .pagination_web>li>span {
		position: relative;
		float: left;
		padding: 6px 12px;
		margin-left: -1px;
		line-height: 1.42857143;
		color: #428bca;
		text-decoration: none;
		background-color: #fff;
		border: 1px solid #ddd;
	}
	.pagination_web>li>a:hover, .pagination_web>li>span:hover, .pagination_web>li>a:focus, .pagination_web>li>span:focus {
		color: #2a6496;
		background-color: #eee;
		border-color: #ddd;
	}
	.pagination_web>.disabled_pagination, .pagination_web>.disabled_pagination>span:hover, .pagination_web>.disabled_pagination>span:focus{
		color: #777;
		cursor: not-allowed;
		background-color: #fff;
		border-color: #ddd;
	}
</style>