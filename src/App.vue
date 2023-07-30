<script setup>
import { ref } from "vue";
import Game from "./components/Game.vue";
import Nav from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";

let isStart = ref(false);

let startGame = () => {
  isStart.value = !isStart.value;
};
</script>

<template>
  <Transition mode="out-in">
    <div class="navs-wrapper" :class="{ 'start-game': isStart }" v-if="isStart">
      <Nav v-show="isStart" />
    </div>
    <div v-else class="navs-wrapper" :class="{ 'start-game': isStart }">
      <h2 class="title-head">Welcome to my</h2>
      <h1 class="title-game">Simon Game</h1>
    </div>
  </Transition>
  <div>
    <div class="wrapper-menu menu" v-if="!isStart">
      <ul class="list-menu">
        <li @click="startGame()">START</li>
        <li>SETTING</li>
        <li>ABOUT</li>
      </ul>
    </div>
    <Transition>
      <div v-if="isStart">
        <Game :start="isStart" />
      </div>
    </Transition>
  </div>
  <Footer v-show="isStart" />
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

.list-menu {
  font-family: var(--sans-font);
  color: var(--golden-dark);
  list-style: none;
  text-align: center;
  letter-spacing: 5px;
  font-weight: 400;
  width: fit-content;
  margin: 2rem auto;
}
.list-menu li {
  cursor: pointer;
  margin: 2rem 0;
}
.list-menu li:hover {
  margin: 2rem 0;
  padding: 1rem;
  display: inline;
  width: fit-content;
  color: var(--golden-dark);
  background: #f3f3f3;
}
</style>
