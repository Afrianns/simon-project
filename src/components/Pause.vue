<script setup>
let propsData = defineEmits(["propParam"]);

function backToMenu() {
  propsData("propParam");
}
</script>
<template>
  <div class="menu">
    <ul class="list-menu">
      <li @click="propsData('propParam', 1)">RESUME</li>
      <li @click="propsData('propParam', 2)">MENU</li>
      <li @click="propsData('propParam', 3)">RESTART</li>
      <li>ABOUT</li>
    </ul>
  </div>
</template>

<style scoped>
.menu {
  position: absolute;
  z-index: 1;
  background: var(--primary-light);
  width: 5vw;
  height: fit-content;
  box-shadow: 5px 12px 25px #a7967c1f;
  padding: 2rem;
}

/* .list-menu > * {
  margin-bottom: 2.5rem;
} */
</style>