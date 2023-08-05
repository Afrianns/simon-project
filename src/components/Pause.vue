<script setup>
// let propsData = defineEmits(["propParam"]);

// function backToMenu() {
//   propsData("propParam");
// }
</script>
<template>
  <div class="backdrop"></div>
  <div class="menu">
    <slot></slot>
  </div>
</template>

<style scoped>
.backdrop {
  position: absolute;
  z-index: 1;
  width: 100%;
  height: 100%;
  top: 0;
  opacity: 0.5;
  background-color: #f2ead3;
}
.menu {
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  position: absolute;
  display: flex;
  align-items: center;
  z-index: 2;
  box-shadow: 5px 12px 25px #a7967c1f;
}
</style>