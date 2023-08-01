<script setup>
import { ref, watch, onMounted, computed, reactive } from "vue";
import Swal from "sweetalert2";

let start = defineProps(["start"]);
let changeState = defineEmits(["wrongStep", "levelUp"]);

let items = reactive([
  { id: 1, color: "#ADE4DB", pattern: false },
  { id: 2, color: "#6DA9E4", pattern: false },
  { id: 3, color: "#F6BA6F", pattern: false },
  { id: 4, color: "#FEE8B0", pattern: false },
  { id: 5, color: "#E9A178", pattern: false },
  { id: 6, color: "#E96479", pattern: false },
  { id: 7, color: "#CD5888", pattern: false },
  { id: 8, color: "#C3ACD0", pattern: false },
]);

let val = ref();
let repeat = ref(2);
let patternID = ref([]);
let wrongStep = ref(false);
let correctStep = ref(false);
let healthNumber = ref(4);
let level = ref(1);
let isClickable = ref(false);

let idx = ref("");

function randomPattern() {
  for (let i = 0; i <= repeat.value - 1; i++) {
    val.value = Math.ceil(Math.random() * items.length);
    patternID.value.push(val.value);
  }
}

function startGame() {
  console.log(patternID.value, patternID.value.length);
  isClickable.value = false;
  for (let i = 0; i <= patternID.value.length - 1; i++) {
    repeatTask(i);
  }
}

function repeatTask(i) {
  setTimeout(() => {
    drawPattern(i);
  }, 2000 * i);
}

function drawPattern(i) {
  if (i >= patternID.value.length - 1) {
    setTimeout(() => {
      isClickable.value = true;
    }, 1000);
  }
  for (const item of items) {
    item["pattern"] = false;
  }
  setTimeout(() => {
    for (const item of items) {
      if (patternID.value[i] == item["id"]) {
        item["pattern"] = true;
        return;
      } else {
        item["pattern"] = false;
      }
    }
  }, 10);
}

let sorted = 0;

function boxClick(id) {
  if (healthNumber.value == 0 || !isClickable.value) {
    return;
  }
  for (const i in patternID.value) {
    if (i == sorted) {
      if (patternID.value[i] == id) {
        sorted++;
        correctStep.value = true;
        delayAnim(correctStep);

        if (sorted == patternID.value.length) {
          sorted = 0;
          level.value++;
          changeState("levelUp", level.value);
          randomPattern();
          callStart();
        }
      } else {
        wrongStep.value = true;
        healthNumber.value--;
        changeState("wrongStep", healthNumber.value);
        delayAnim(wrongStep);

        wrongStepAlert();
      }
      break;
    }
  }
}

let delayAnim = (param) => {
  idx.value = setTimeout(() => {
    param.value = false;
    // idx.value = "";
  }, 1000);
};

function wrongStepAlert() {
  const Toast = Swal.mixin({
    toast: true,
    position: "bottom-end",
    showConfirmButton: false,
    timer: 3000,
    timerProgressBar: true,
  });

  Toast.fire({
    icon: "error",
    title: "You Got Wrong Step. <br> Try Again!",
  });
}

if (start.start) {
  randomPattern();
  callStart();
}

function callStart() {
  setTimeout(() => {
    startGame();
  }, 2000);
}
</script>

<template>
  <div
    class="wrapper-menu"
    :class="{
      'wrong-step': wrongStep,
      trans: startGame,
      'correct-step': correctStep,
    }"
  >
    <Transition name="boxs">
      <div class="wrapper" v-if="start.start">
        <!-- {{ start.start }} -->
        <span
          v-for="item in items"
          :key="item.id"
          :class="{
            box: start.start,
            clicked: item['pattern'],
          }"
          :style="{ background: item.color }"
          @click.prevent="boxClick(item.id)"
        ></span>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.trans {
  transition: sizeAnim 2s ease;
}

@keyframes sizeAnim {
  0% {
    height: inherit;
  }
  100% {
    height: 100rem;
  }
}

/* background color when wrong step */
.wrong-step {
  animation: wrong-step 1s ease-in-out;
}

@keyframes wrong-step {
  0% {
    background-color: inherit;
    border-color: var(--golden-light);
  }
  40% {
    background-color: #fff0f0;
    border-color: #ff3407;
  }
  60% {
    background-color: #fff0f0;
    border-color: #ff3407;
  }
  100% {
    border-color: var(--golden-light);
    background-color: inherit;
  }
}
/* background color when correct step */
.correct-step {
  animation: correct-step 1s ease-in-out;
}

@keyframes correct-step {
  0% {
    background-color: inherit;
    border-color: var(--golden-light);
  }
  40% {
    background-color: #f8fff0;
    border-color: #07ff1c;
  }
  60% {
    background-color: #f8fff0;
    border-color: #07ff1c;
  }
  100% {
    border-color: var(--golden-light);
    background-color: inherit;
  }
}

.h1 {
  color: #3c3c3c;
}
.wrapper {
  display: grid;
  width: fit-content;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: 2rem;
  margin: 1.5rem;
}
.box {
  cursor: pointer;
  display: inline-block;
  width: 100px;
  height: 100px;
  transition: all 0.5s ease;
}

.box:hover {
  box-shadow: 0 0 0 4px var(--primary-light), 0 0 0 8px var(--golden-light);
}

.clicked {
  animation: clicked 1s ease;
}

@keyframes clicked {
  0% {
    opacity: 1;
    box-shadow: 0;
    /* border: 0 solid salmon; */
  }
  20% {
    opacity: 0.7;
    box-shadow: 0 0 0 4px var(--primary-light), 0 0 0 7px var(--golden);
    /* border: 4px solid salmon; */
  }
  80% {
    box-shadow: 0 0 0 4px var(--primary-light), 0 0 0 7px var(--golden);
    /* border: 4px solid salmon; */
    opacity: 0.7;
  }
  100% {
    box-shadow: 0;
    /* border: 0 solid salmon; */
    opacity: 1;
  }
}
</style>
