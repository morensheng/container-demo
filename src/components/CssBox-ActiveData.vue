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

      <!-- 动态加载小盒子 -->
      <div class="minibox" v-for="(box, index) in miniboxes" :key="index" :style="boxStyles(box)">
        <div class="face front" :style="faceStyles(box, 'front')"></div>
        <div class="face back" :style="faceStyles(box, 'back')"></div>
        <div class="face top" :style="faceStyles(box, 'top')"></div>
        <div class="face bottom" :style="faceStyles(box, 'bottom')"></div>
        <div class="face left" :style="faceStyles(box, 'left')"></div>
        <div class="face right" :style="faceStyles(box, 'right')"></div>
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
      rotateY: 60,

      miniboxes: [
        {
          x: 0,
          y: 0,
          z: -125,
          long: 50,
          width: 50,
          height: 50,
          backgroundColor: 'rgba(255, 0, 55, 0.3)',
          borderColor: 'rgba(255, 0, 55, 0.6)'
        },
        {
          x: 0,
          y: 0,
          z: -50,
          long: 100,
          width: 100,
          height: 100,
          backgroundColor: 'rgba(255, 55, 0, 0.3)',
          borderColor: 'rgba(255, 55, 0, 0.6)'
        },
        {
          x: 220,
          y: 180,
          z: 0,
          long: 80,
          width: 80,
          height: 120,
          backgroundColor: 'rgba(195, 150, 50, 0.3)',
          borderColor: 'rgba(195, 150, 50, 0.6)'
        },
        {
          x: 0,
          y: 220,
          z: 50,
          long: 100,
          width: 120,
          height: 80,
          backgroundColor: 'rgba(25, 150, 50, 0.3)',
          borderColor: 'rgba(125, 150, 50, 0.6)'
        }
      ]
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
    },

    boxStyles(box) {
      return {
        transform: `translate3d(${box.x}px, ${box.y}px, ${box.z}px)`,
        width: `${box.width}px`,
        height: `${box.height}px`
      }
    },
    faceStyles(box, faceName) {
      const commonStyles = {
        position: 'absolute',
        width: '100%',
        height: '100%',
        background: box.backgroundColor,
        borderColor: box.borderColor
      }

      // 适配正方体
      // const sideStyles = {
      //   front: { transform: `translateZ(${box.long / 2}px)` },
      //   back: { transform: `translateZ(-${box.long / 2}px)` },
      //   top: { transform: `rotateX(90deg) translateZ(${Math.max(box.width, box.height) / 2}px)` },
      //   bottom: { transform: `rotateX(-90deg) translateZ(${Math.max(box.width, box.height) / 2}px)` },
      //   left: { transform: `rotateY(-90deg) translateZ(${Math.max(box.long, box.height) / 2}px)` },
      //   right: { transform: `rotateY(90deg) translateZ(${Math.max(box.long, box.height) / 2}px)` }
      // }

      const sideStyles = {
        front: { transform: `translateZ(${box.long / 2}px)` },
        back: { transform: `translateZ(-${box.long / 2}px)` },
        top: { transform: `rotateX(90deg) translateY(0%) translateZ(${Math.max(box.width, box.height) / 2}px)` },
        bottom: { transform: `rotateX(-90deg) translateY(0%) translateZ(${Math.max(box.width, box.height) / 2}px)` },
        left: { transform: `rotateY(-90deg) translateX(0%) translateZ(${Math.max(box.width, box.height) / 2}px)` },
        right: { transform: `rotateY(90deg) translateX(0%) translateZ(${Math.max(box.width, box.height) / 2}px)` }
      }

      return { ...commonStyles, ...sideStyles[faceName] }
    }
  }
}
</script>

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
  position: absolute;
  transform-style: preserve-3d;
}

.minibox > .face {
  position: absolute;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 1px solid transparent;
}
</style>
