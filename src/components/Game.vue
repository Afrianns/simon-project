<script setup>
import { ref, watch, onMounted, computed, reactive } from "vue";
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
let gap = ref(false);
let patternID = ref([]);
let res = ref(false);
let start = defineProps(["start"]);
let wrongStep = ref(false);

function startGame() {
  for (let i = 0; i <= repeat.value - 1; i++) {
    repeatTask(i);
  }
}

// const Toast = Swal.mixin({
//   toast: true,
//   position: 'top-end',
//   showConfirmButton: false,
//   timer: 3000,
//   timerProgressBar: true,
//   didOpen: (toast) => {
//     toast.addEventListener('mouseenter', Swal.stopTimer)
//     toast.addEventListener('mouseleave', Swal.resumeTimer)
//   }
// })

// Toast.fire({
//   icon: 'success',
//   title: 'Signed in successfully'
// })

function repeatTask(i) {
  setTimeout(() => {
    val.value = Math.ceil(Math.random() * items.length);
    for (const item of items) {
      item["pattern"] = false;
    }
    setTimeout(() => {
      drawPatter(val.value);
    }, 100);
  }, 1200 * i);
}

function drawPatter(patternId) {
  for (const item of items) {
    if (patternId == item["id"]) {
      item["pattern"] = true;
    } else {
      item["pattern"] = false;
    }
  }
  patternID.value.push(val.value);
}

let sorted = 0;

function boxClick(id) {
  patternID.value.forEach((a, b) => {
    if (b == sorted) {
      console.log(a == id, id, a);
      if (a == id) {
        sorted++;
      } else {
        wrongStep.value = true;
        setTimeout(() => {
          wrongStep.value = false;
        }, 1000);
        console.log("wrong step");
      }
    }
  });
}

if (start.start) {
  setTimeout(() => {
    startGame();
  }, 2000);
}

console.log(start.start);
</script>

<template>
  {{ start.start }}{{ patternID }}
  <div class="wrapper-menu" :class="{ red: wrongStep }">
    <TransitionGroup name="boxs">
      <div class="wrapper">
        <span
          v-for="item in items"
          :key="item.id"
          :class="{ box: start.start, clicked: item['pattern'] }"
          :style="{ background: item.color }"
          @click="boxClick(item.id)"
          >{{ item["pattern"] }}{{ item.id }}</span
        >
      </div>
    </TransitionGroup>
  </div>
</template>

<style scoped>
body {
  background: red;
}
.red {
  animation: wrongAnim 1s ease-in-out;
}

@keyframes wrongAnim {
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
  border: 4px solid orange;
}

.clicked {
  animation: clicked 1s ease;
}

@keyframes clicked {
  0% {
    opacity: 1;
    border: 0 solid salmon;
  }
  20% {
    opacity: 0.7;
    border: 4px solid salmon;
  }
  80% {
    border: 4px solid salmon;
    opacity: 0.7;
  }
  100% {
    border: 0 solid salmon;
    opacity: 1;
  }
}
</style>
