<template>
  <div class="pd-select-box" v-show="parentHidden" >
 

<div class="pd-select-opcity" @click="showMove"></div>
    <ul class="pd-select-title">
     <li class="pd-select-title-left" @click="showMove">取消</li>
     <li class="pd-select-title-right" @click="showAll">确定</li>
   </ul>
   <div class="pd-select-item" ref="allContent"  >
    <div class="pd-select-line"></div>
    <div class="pd-select-list">
      <ul class="pd-select-ul" ref="list">
        <li class="pd-select-list-item" v-for="el,index in renderData " :class="{'hidden':setHidden(el.index)}" :key="index">{{el.value}}</li>
      </ul>
    </div>
    <ul class="pd-select-wheel" ref="wheel">
      <li class="pd-select-wheel-item" :class="{'hidden':setHidden(el.index)}" :style="setWheelItemDeg(el.index)" :index="el.index" v-for="el,index in renderData " :key="index">{{el.value}}</li>
    </ul>
  </div>
</div>
</template>
<script>
  export default{
    name: 'pdSelectItem',
    data () {
      return {
        spin: {start: -9, end: 9, branch: 9},
        finger: {startY: 0, lastY: 0, startTime: 0, lastTime: 0, transformY: 0},
        indexSend : null,
      }
    },
    props: {
      listData: {
        type: Array,
        required: true
      },
      type: {
        type: String,
        default: 'line'
      },
      parentHidden:{
        type:Boolean,
        default: false
      },
      value: {}
    },
    computed: {
      renderData () {
        let temp = []
        for (let k = this.spin.start; k <= this.spin.end; k++) {
          let data = {
            value: this.getSpinData(k),
            index: k
          }
          temp.push(data)
        }
        return temp
      }
    
    },
    mounted () {
      /* 事件绑定 */
      this.$refs.allContent.addEventListener('touchstart', this.itemTouchStart)
      this.$refs.allContent.addEventListener('touchmove', this.itemTouchMove)
      this.$refs.allContent.addEventListener('touchend', this.itemTouchEnd)
      /* 初始化状态 */
      let index = this.listData.indexOf(this.value)
      if (index === -1) {
        console.warn('当前初始值不存在，请检查后listData范围！！')
        this.setListTransform()
        this.getPickValue(0)
      } else {
        let move = index * 34
        /* 因为往上滑动所以是负 */
        this.setStyle(-move)
        this.setListTransform(-move, -move)
      }
    },
    methods: {
      /* 根据type 控制滚轮显示效果 */
      setHidden (index) {
        if (this.type === 'line') {
          return index < 0 || index > this.listData.length - 1
        } else {
          return false
        }
      },
      setWheelItemDeg (index) {
        return {
          transform: `rotate3d(1, 0, 0, ${-index * 20 % 360}deg) translate3d(0px, 0px, 100px)`
        }
      },
      showMove(){
         this.$emit('show', false);
      },
      showAll(){
        let pickValue = this.getSpinData(this.indexSend);
        this.$emit('input', pickValue)
        this.showMove();
      },
      setWheelDeg (updateDeg, type, time = 1000) {
        if (type === 'end') {
          this.$refs.wheel.style.webkitTransition = `transform ${time}ms cubic-bezier(0.19, 1, 0.22, 1)`
          this.$refs.wheel.style.webkitTransform = `rotate3d(1, 0, 0, ${updateDeg}deg)`
        } else {
          this.$refs.wheel.style.webkitTransition = ''
          this.$refs.wheel.style.webkitTransform = `rotate3d(1, 0, 0, ${updateDeg}deg)`
        }
      },
      setListTransform (translateY = 0, marginTop = 0, type, time = 1000) {
        if (type === 'end') {
          this.$refs.list.style.webkitTransition = `transform ${time}ms cubic-bezier(0.19, 1, 0.22, 1)`
          this.$refs.list.style.webkitTransform = `translateY(${translateY - this.spin.branch * 34}px)`
          this.$refs.list.style.marginTop = `${-marginTop}px`
          this.$refs.list.setAttribute('scroll', translateY)
        } else {
          this.$refs.list.style.webkitTransition = ''
          this.$refs.list.style.webkitTransform = `translateY(${translateY - this.spin.branch * 34}px)`
          this.$refs.list.style.marginTop = `${-marginTop}px`
          this.$refs.list.setAttribute('scroll', translateY)
        }
      },
      itemTouchStart (event) {
        let finger = event.changedTouches[0]
        this.finger.startY = finger.pageY
        this.finger.startTime = event.timestamp || Date.now()
        this.finger.transformY = this.$refs.list.getAttribute('scroll')
        event.preventDefault()
      },
      itemTouchMove (event) {
        let finger = event.changedTouches[0]
        this.finger.lastY = finger.pageY
        this.finger.lastTime = event.timestamp || Date.now()
        /* 设置css */
        let move = this.finger.lastY - this.finger.startY
        this.setStyle(move)
        event.preventDefault()
      },
      itemTouchEnd (event) {
        let finger = event.changedTouches[0]
        this.finger.lastY = finger.pageY
        this.finger.lastTime = event.timestamp || Date.now()
        let move = this.finger.lastY - this.finger.startY
        /* 计算速度 */
        /* 速度计算说明
         * 当时间小于300毫秒 最后的移动距离等于 move + 减速运动距离
         * */
         let time = this.finger.lastTime - this.finger.startTime
         let v = move / time
         /* 减速加速度a */
         let a = 1.8
         /* 设置css */
         if (time <= 300) {
          move = v * a * time
          time = 1000 + time * a
          this.setStyle(move, 'end', time)
        } else {
          this.setStyle(move, 'end')
        }
      },
      /* 设置css */
      setStyle (move, type, time) {
        const singleHeight = 34
        const deg = 20
        const singleDeg = deg / singleHeight
        let currentListMove = this.finger.transformY
        let updateMove = move + Number(currentListMove)
        /* 根据滚轮类型 line or cycle 判断 updateMove最大距离 */
        if (this.type === 'line') {
          if (updateMove > 0) {
            updateMove = 0
          }
          if (updateMove < -(this.listData.length - 1) * singleHeight) {
            updateMove = -(this.listData.length - 1) * singleHeight
          }
        }
        let updateDeg = -updateMove * singleDeg
        let spinAim = Math.round(updateDeg / 20)
        let margin = Math.round(updateMove / singleHeight) * singleHeight // 如果不这么写 会导致没有滚动效果
        /* 计算touchEnd移动的整数距离 */
        let endMove = margin
        let endDeg = Math.round(updateDeg / deg) * deg
        if (type === 'end') {
          this.setListTransform(endMove, margin, type, time)
          this.setWheelDeg(endDeg, type, time)
          /* 设置$emit 延迟 */
          setTimeout(() => this.getPickValue(endMove), 500)
        } else {
          this.setListTransform(updateMove, margin)
          this.setWheelDeg(updateDeg)
        }
        this.updateSpin(spinAim)
      },
      /* 更新spin */
      updateSpin (spinAim) {
        this.spin.start = this.spin.branch * -1 + spinAim
        this.spin.end = this.spin.start + this.spin.branch * 2
      },
      /* 获取spin 数据 */
      getSpinData (index) {
        index = index % this.listData.length
        return this.listData[index >= 0 ? index : index + this.listData.length]
      },
      /* 获取选中值 */
      getPickValue (move) {
        this.indexSend = Math.abs(move / 34)
       
      }
    },
    beforeDestroy () {
      this.$refs.allContent.removeEventListener('touchstart', this.itemTouchStart)
      this.$refs.allContent.removeEventListener('touchmove', this.itemTouchMove)
      this.$refs.allContent.removeEventListener('touchend', this.itemTouchEnd)
    }
  }
</script>
<style lang="scss" scoped="">
  html{
    font-family: 'PingFang SC', 'Helvetica Neue', 'Helvetica', 'STHeitiSC-Light', 'Arial', sans-serif;
    line-height: 1.8;
  }
.pd-select-title{
  height:40px;
  background:#f5f5f5;
  width:100%;
  font-size:20px;
  padding: 0 20px;
  color:#066ec1;
}
.pd-select-title-left{
  float:left;
  line-height:40px;
}
.pd-select-title-right{
  text-align:right;
  line-height:40px;
}
  $color-background: #fff;
  $color-checked: #2c97f1;
  $color-text-main: #333;
  $color-text-second: #a8a8a8;
  .pd-select-item{
   overflow: hidden;
   width: 100%;
   text-align: center;
   height: 220px;
   background: $color-background;
   position: fixed;
   bottom: 0;
   width: 100%;
   z-index:999;
 }
 .pd-select-line , .pd-select-list , .pd-select-wheel{
  position: absolute;
  left: 0;
  right: 0;
  top: 93px;
}
.pd-select-line{
  z-index: 993;
}
.pd-select-list-item {
 text-shadow: 0 1px 1px rgba(102, 102, 102, 0.6);
}
.pd-select-list-item , .pd-select-wheel-item{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  line-height: 34px;
  font-size: 18px;
  color: $color-text-main;
}
.pd-select-list-item.hidden ,  .pd-select-wheel-item.hidden{
  visibility: hidden;
  opacity: 0;
}
.pd-select-list{
 overflow: hidden;
 z-index: 992;
 background: $color-background;
}
.pd-select-wheel{
 z-index: 991;
 transform-style: preserve-3d;
 height: 34px;
}
.pd-select-wheel-item{
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  width: 100%;
  color: $color-text-second;
}
.pd-select-ul{
  position: relative;
}
.pd-select-line:after, .pd-select-line:before {
  position: absolute;
  top: 0;
  content: '';
  display: table;
  background: #ddd;
  width: 100%;
  height: 2px;
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
  -webkit-transform-origin: 0 0;
  transform-origin: 0 0;
}
.pd-select-line:before {
  bottom: -1px;
  top: auto;
}
.pd-select-line , .pd-select-list{
 height: 34px;
 transform: translate3d(0px, 0px, 110px);
}
.pd-select-title{
   position: fixed;
   bottom: 220px;
   width: 100%;
   z-index:999;
   height:40px;
  background:#f5f5f5;
  width:100%;
  font-size:20px;
  padding: 0 20px;
  color:#066ec1;
}
.pd-select-title-left{
  float:left;
  line-height:40px;
}
.pd-select-title-right{
  text-align:right;
  line-height:40px;
}
  .pd-select-opcity {
    position: absolute;
    height:100%;
    opacity: .5;
    background: #000;
    width: 100%;
    top:0;
    left:0;
    z-index:99;
  }
</style>