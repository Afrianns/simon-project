<script setup>
import { ref } from "vue";
let level = ref(1);
</script>
<template>
  <Transition>
    <footer>
      <div class="level">
        <h2>LEVEL. {{ level }}</h2>
      </div>
    </footer>
  </Transition>
</template>

<style scoped>
footer {
  position: absolute;
  bottom: 0;
  margin: 1rem 0;
}
</style>