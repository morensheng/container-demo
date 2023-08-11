<template>
  <div class="app">
    <ThreeBox v-if="false" />
    <CssBox v-if="false" />
    <CssBoxData />
  </div>
</template>

<script>
import ThreeBox from './components/ThreeBox.vue'
import CssBox from './components/CssBox.vue'
import CssBoxData from './components/CssBox-ActiveData.vue'

export default {
  components: {
    ThreeBox,
    CssBoxData,
    CssBox
  }
}
</script>

<style>
.app {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #f0f0f0;
}
</style>
