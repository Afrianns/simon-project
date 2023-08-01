<script setup>
import { ref } from "vue";
import Game from "./components/Game.vue";
import Nav from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
import Pause from "./components/Pause.vue";

let isStart = ref(false);
let healthNumber = ref(0);
let level = ref(1);
let pause = ref(false);
let resume = ref(false);
let restart = ref(false);

let startGame = () => {
  isStart.value = !isStart.value;
};

let healthbar = (param) => {
  healthNumber.value = param;
};

let levelUP = (param) => {
  level.value = param;
  console.log(param);
};

let pauseGame = () => {
  pause.value = !pause.value;
};

let menuGame = (param) => {
  resume.value = !resume.value;
  if (param == 2) {
    pause.value = !pause.value;
    isStart.value = false;
    restart.value = false;
    level.value = 1;
  }
  if (param == 3) {
    pause.value = !pause.value;
    isStart.value = false;
    restart.value = true;
    level.value = 1;
    setTimeout(() => {
      startGame();
    }, 1000);
  }
};
</script>

<template>
  <div class="backdrop" v-show="pause"></div>
  <Pause v-if="pause" @propParam="menuGame" />
  <Transition mode="out-in">
    <div class="navs-wrapper" :class="{ 'start-game': isStart }" v-if="isStart">
      <Nav
        v-show="isStart"
        :health="healthNumber"
        @pause="pauseGame"
        :resume="resume"
      />
    </div>
    <div v-else class="navs-wrapper" :class="{ 'start-game': isStart }">
      <h2 class="title-head">Welcome to my</h2>
      <h1 class="title-game">Simon Game</h1>
    </div>
  </Transition>
  <div>
    <div class="wrapper-menu menu" v-if="!isStart && !restart">
      <ul class="list-menu">
        <li @click="startGame()">START</li>
        <li>SETTING</li>
        <li>ABOUT</li>
      </ul>
    </div>
    <div v-if="isStart">
      <Game :start="isStart" @wrongStep="healthbar" @levelUp="levelUP" />
    </div>
  </div>
  <Footer v-show="isStart" :level="level" />
</template>

<style scoped>
.navs-wrapper {
  top: 10%;
  margin-bottom: 1.5rem;
  text-align: center;
  width: 100%;
  position: absolute;
}
.start-game {
  top: 2%;
}
.title-head {
  font-family: "Starllet";
  font-size: 2rem;
  line-height: 17px;
}
.title-game {
  font-family: "Holen Vintage";
  font-size: 3rem;
}
</style>
