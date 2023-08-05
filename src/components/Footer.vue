<script setup>
import { ref } from "vue";
let level = defineProps(["level"]);
</script>
<template>
  <Transition>
    <footer>
      <div class="level">
        <h2>LEVEL. {{ level.level }}</h2>
      </div>
    </footer>
  </Transition>
</template>

<style scoped>
footer {
  width: fit-content;
  /* position: absolute;
  bottom: 0; */
  margin: 1rem auto;
}
</style>