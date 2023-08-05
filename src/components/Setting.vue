<script setup>
import { ref } from "vue";
let emit = defineEmits(["setting", "settingVal"]);
let initial = defineProps(["value"]);

let DIFFICULTY = ref(0);
let SUMBOXS = ref(0);
let DURATIONS = ref(0);

DIFFICULTY.value = initial.value[0];
SUMBOXS.value = initial.value[1];
DURATIONS.value = 1;

function changeSetting(id, val) {
  switch (id) {
    case 1:
      DIFFICULTY.value = val;
      break;
    case 2:
      SUMBOXS.value = val;
      break;
    case 3:
      DURATIONS.value = val;
      break;
    default:
      break;
  }
  sendVal();
}

function reset() {
  SUMBOXS.value = 1;
  DURATIONS.value = 1;
  DIFFICULTY.value = 1;
  sendVal();
}

let sendVal = () => {
  emit("settingVal", DIFFICULTY.value, SUMBOXS.value, DURATIONS.value);
};
</script>

<template>
  <ul class="list-menu">
    <li>
      DIFFICULTY
      <ul class="sub-list">
        <li @click="changeSetting(1, 1)" :class="{ active: DIFFICULTY == 1 }">
          Easy
        </li>
        <li @click="changeSetting(1, 2)" :class="{ active: DIFFICULTY == 2 }">
          Medium
        </li>
        <li @click="changeSetting(1, 3)" :class="{ active: DIFFICULTY == 3 }">
          Hard
        </li>
      </ul>
    </li>
    <li>
      HOW MANY BOX
      <ul class="sub-list">
        <li @click="changeSetting(2, 4)" :class="{ active: SUMBOXS == 4 }">
          4
        </li>
        <li @click="changeSetting(2, 5)" :class="{ active: SUMBOXS == 5 }">
          5
        </li>
        <li @click="changeSetting(2, 6)" :class="{ active: SUMBOXS == 6 }">
          6
        </li>
        <li @click="changeSetting(2, 7)" :class="{ active: SUMBOXS == 7 }">
          7
        </li>
        <li @click="changeSetting(2, 8)" :class="{ active: SUMBOXS == 8 }">
          8
        </li>
      </ul>
    </li>
    <li>
      DURATIONS (Minutes)
      <ul class="sub-list">
        <li @click="changeSetting(3, 1)" :class="{ active: DURATIONS == 1 }">
          1
        </li>
        <li @click="changeSetting(3, 2)" :class="{ active: DURATIONS == 2 }">
          2
        </li>
        <li @click="changeSetting(3, 3)" :class="{ active: DURATIONS == 3 }">
          3
        </li>
        <li>+</li>
      </ul>
    </li>
  </ul>
  <div class="value">{{ DIFFICULTY }} {{ SUMBOXS }} {{ DURATIONS }}</div>
  <div class="menu-wrapper">
    <p @click="reset()">RESET</p>
    <p @click="$emit('setting')">BACK</p>
  </div>
</template>

<style scoped>
.list-menu li {
  margin: 1rem 0;
  padding: 0;
}

.menu-wrapper {
  display: flex;
  justify-content: space-between;
  color: var(--golden-dark);
  opacity: 0.8;
  font-size: 0.85rem;
  font-weight: 400;
}

.menu-wrapper > * {
  cursor: pointer;
  margin: 0.5rem;
}

.sub-list {
  justify-content: center;
  display: flex;
  list-style: none;
  column-gap: 20px;
  text-transform: uppercase;
}

.sub-list > li {
  padding: 10px;
  border: 2px dashed var(--golden-light);
}

.sub-list > li:hover {
  background: var(--golden-light);
}

.active {
  background: var(--golden);
}
</style>