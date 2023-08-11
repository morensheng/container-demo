<template>
  <div class="container">
    <div
      class="cube"
      id="main"
      ref="mainTag"
      @mousedown="handleMouseDown"
      @mousemove="handleMouseMove"
      @mouseup="handleMouseUp"
      @mouseleave="handleMouseUp"
    >
      <div class="face front">front</div>
      <div class="face back">back</div>
      <div class="face top">top</div>
      <div class="face bottom">bottom</div>
      <div class="face left">left</div>
      <div class="face right">right</div>

      <div class="minibox box1">
        <div class="face front"></div>
        <div class="face back"></div>
        <div class="face top"></div>
        <div class="face bottom"></div>
        <div class="face left"></div>
        <div class="face right"></div>
      </div>

      <div class="minibox box2">
        <div class="face front"></div>
        <div class="face back"></div>
        <div class="face top"></div>
        <div class="face bottom"></div>
        <div class="face left"></div>
        <div class="face right"></div>
      </div>

      <div class="minibox box3">
        <div class="face front"></div>
        <div class="face back"></div>
        <div class="face top"></div>
        <div class="face bottom"></div>
        <div class="face left"></div>
        <div class="face right"></div>
      </div>

      <div class="minibox box4">
        <div class="face front"></div>
        <div class="face back"></div>
        <div class="face top"></div>
        <div class="face bottom"></div>
        <div class="face left"></div>
        <div class="face right"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDragging: false,
      initialMouseX: 0,
      initialMouseY: 0,
      rotateX: -30,
      rotateY: 60
    }
  },
  methods: {
    handleMouseDown(e) {
      this.isDragging = true
      this.initialMouseX = e.clientX - this.rotateY
      this.initialMouseY = e.clientY - this.rotateX
    },
    handleMouseMove(e) {
      if (!this.isDragging) return

      this.rotateX = e.clientY - this.initialMouseY
      this.rotateY = e.clientX - this.initialMouseX

      this.$refs.mainTag.style.transform = `rotateX(${this.rotateX}deg) rotateY(${this.rotateY}deg)`
    },
    handleMouseUp() {
      this.isDragging = false
    }
  }
}
</script>

<!-- <script>
window.onload = function () {
  var oBox = document.querySelector('#main')

  var y = 60
  var x = -30
  oBox.onmousedown = function (ev) {
    var oEvent = ev || event
    var disX = oEvent.clientX - y
    var disY = oEvent.clientY - x
    document.onmousemove = function (ev) {
      var oEvent = ev || event
      x = oEvent.clientY - disY
      y = oEvent.clientX - disX
      oBox.style.transform = 'rotateX(' + x + 'deg) rotateY(' + y + 'deg)'
    }
    document.onmouseup = function () {
      document.onmousemove = null
      document.onmouseup = null
    }
    return false
  }
}
</script> -->

<style scoped>
.container {
  width: 300px;
  height: 300px;
  perspective: 800px;
  perspective-origin: center;
  margin: 100px auto;
  user-select: none;
  cursor: pointer;
}

.cube {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transform: rotateX(-30deg) rotateY(60deg);
}

.cube > .face {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 128, 255, 0.3);
  border: 1px solid rgba(0, 128, 255, 0.6);
  box-sizing: border-box;
  opacity: 0.8;
  display: flex;
  justify-content: center;
  align-items: center;
}

.cube > .front {
  transform: translateZ(150px);
}

.cube > .back {
  transform: translateZ(-150px) rotateY(180deg);
}

.cube > .top {
  transform: rotateX(90deg) translateZ(150px);
}

.cube > .bottom {
  transform: rotateX(-90deg) translateZ(150px);
}

.cube > .left {
  transform: rotateY(-90deg) translateX(0px) translateZ(150px);
}

.cube > .right {
  transform: rotateY(90deg) translateX(0px) translateZ(150px);
}

.minibox {
  width: 50px;
  height: 50px;
  position: absolute;
  bottom: 0;
  right: 0;
  transform-style: preserve-3d;
  transform: rotateX(0deg) rotateY(0deg);
}
.box1 {
  transform: translateZ(-125px);
}
.box2 {
  transform: translateZ(-75px);
}
.box3 {
  transform: translateX(-50px) translateZ(-125px);
}
.box4 {
  transform: translateY(-50px) translateZ(-75px);
}
.minibox > .face {
  position: absolute;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  opacity: 0.8;
}
.box1 > .face {
  background: rgba(255, 0, 55, 0.3);
  border: 1px solid rgba(255, 0, 55, 0.6);
}
.box2 > .face {
  background: rgba(0, 255, 34, 0.3);
  border: 1px solid rgba(0, 255, 34, 0.6);
}
.box3 > .face {
  background: rgba(255, 255, 0, 0.3);
  border: 1px solid rgba(255, 255, 0, 0.6);
}
.box4 > .face {
  background: rgba(166, 0, 255, 0.3);
  border: 1px solid rgba(166, 0, 255, 0.6);
}

.minibox > .front {
  transform: translateZ(25px);
}

.minibox > .back {
  transform: translateZ(-25px);
}

.minibox > .top {
  transform: rotateX(90deg) translateZ(25px);
}

.minibox > .bottom {
  transform: rotateX(-90deg) translateZ(25px);
}

.minibox > .left {
  transform: rotateY(-90deg) translateX(0px) translateZ(25px);
}

.minibox > .right {
  transform: rotateY(90deg) translateX(0px) translateZ(25px);
}
</style>
