<script setup>
import { ref } from "vue";
import Game from "./components/Game.vue";
import Nav from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
import Pause from "./components/Pause.vue";
import Setting from "./components/Setting.vue";

let isStart = ref(false);
let healthNumber = ref(4);
let level = ref(1);
let pause = ref(false);
let resume = ref(false);
let restart = ref(false);
let healthEmpty = ref(false);
let isTimeout = ref(false);
let isSetting = ref(false);

let DIFFICULTY = ref(1);
let SUMBOXS = ref(4);
let DURATIONS = ref(1);

let sound = ref(true);

let startGame = () => {
  isStart.value = !isStart.value;
};

let setting = () => {
  isSetting.value = !isSetting.value;
};

let healthbar = (param) => {
  healthNumber.value = param;
  if (healthNumber.value == 0) {
    pauseGame();
    healthEmpty.value = true;
  }
  console.log(healthEmpty.value, healthNumber.value);
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
  isTimeout.value = false;
  if (param == 2) {
    pauseGame();
    healthEmpty.value = false;
    isStart.value = false;
    level.value = 1;
  }
  if (param == 3) {
    pauseGame();
    healthEmpty.value = false;
    isStart.value = false;
    restart.value = true;
    level.value = 1;
    healthNumber.value = 0;
    setTimeout(() => {
      restart.value = false;
      startGame();
    }, 1000);
  }
};

function timeout() {
  pause.value = !pause.value;
  isTimeout.value = true;
}

function setSetting(DIFF, BOXS, DUR) {
  DIFFICULTY.value = DIFF;
  SUMBOXS.value = BOXS;
  DURATIONS.value = DUR;
  console.log(DIFF, BOXS);
}

function setSound(status) {
  sound.value = status;
}
</script>

<template>
  <Transition>
    <Pause class="pause" v-if="pause" @propParam="menuGame">
      <ul class="list-menu" v-if="!healthEmpty && !isTimeout">
        <li @click="menuGame(1)">RESUME</li>
        <li @click="menuGame(2)">MENU</li>
        <li @click="menuGame(3)">RESTART</li>
        <li>ABOUT</li>
      </ul>
      <ul class="list-menu" v-else>
        <li class="title" v-show="isTimeout">YOU GOT LEVEL. {{ level }}</li>
        <li class="title" v-show="healthEmpty">YOU LOST</li>
        <li @click="menuGame(2)">MENU</li>
        <li @click="menuGame(3)">PLAY AGAIN</li>
        <li>ABOUT</li>
      </ul>
    </Pause>
  </Transition>
  <Transition mode="out-in">
    <div class="navs-wrapper" :class="{ 'start-game': isStart }" v-if="isStart">
      <Nav
        v-show="isStart"
        :health="healthNumber"
        @pause="pauseGame"
        :resume="resume"
        :pause="pause"
        @setSound="setSound"
        @timeout="timeout"
        :duration="DURATIONS"
        :sound="sound"
      />
    </div>
    <div
      v-else-if="!isStart && !isSetting"
      class="navs-wrapper"
      :class="{ 'start-game': isStart }"
    >
      <div class="title">
        <h2 class="title-head">Welcome to my</h2>
        <h1 class="title-game">Simon Game</h1>
      </div>
    </div>
    <div v-else-if="isSetting" class="navs-wrapper">
      <div class="title">
        <h2 class="title-head">Setting in my</h2>
        <h1 class="title-game">Simon Game</h1>
      </div>
    </div>
  </Transition>
  <div>
    <div class="wrapper-menu menu" v-if="!isStart && !restart && !isSetting">
      <ul class="list-menu">
        <li @click="startGame()">START</li>
        <li @click="setting()">SETTING</li>
        <li>ABOUT</li>
      </ul>
    </div>
    <div v-if="isStart">
      <Game
        :start="isStart"
        :pause="pause"
        :timeout="isTimeout"
        :SetValue="[DIFFICULTY, SUMBOXS]"
        :sound="sound"
        @wrongStep="healthbar"
        @levelUp="levelUP"
      />
    </div>
  </div>

  <Trasition>
    <div class="wrapper-menu menu" v-if="isSetting">
      <Setting
        @setting="setting"
        @settingVal="setSetting"
        :value="[DIFFICULTY, SUMBOXS, DURATIONS]"
      />
    </div>
  </Trasition>
  <Footer v-show="isStart" :level="level" />
</template>

<style scoped>
.pause {
  margin: auto;
}
.navs-wrapper {
  top: 10%;
  text-align: center;
  width: 100%;
  height: 10rem;
}
.start-game {
  top: 2%;
}

.title {
  margin: 4rem 0 1rem;
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

.list-menu li:hover {
  color: var(--golden-dark);
  background: #f3f3f3;
}

.list-menu li {
  margin: 1rem auto;
  padding: 0.5rem 1rem;
  width: fit-content;
}

.list-menu {
  border-radius: 10px;
  width: 45rem;
  height: fit-content;
  padding: 1.5rem;
  background: var(--primary-light);
}
</style>
