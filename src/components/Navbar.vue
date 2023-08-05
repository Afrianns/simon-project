<script setup>
import { ref, watch } from "vue";
let propsData = defineProps(["health", "resume", "pause", "duration", "sound"]);
let pauseEmit = defineEmits(["pause", "timeout", "setSound"]);
let healthbars = ref([
  { id: 1, isEmpty: false },
  { id: 2, isEmpty: false },
  { id: 3, isEmpty: false },
  { id: 4, isEmpty: false },
]);
let minutes = ref(0);
let seconds = ref(0);
let distance = ref(1);
let sound = ref(true);

distance.value = propsData.duration;
sound.value = propsData.sound;
let pauseGame = ref(false);
distance.value = distance.value * 60000;
let isPause = ref(false);

// watch evertime user wrong click step & decrease the health bar
watch(
  () => propsData.health,
  (a) => {
    if (a >= 0) {
      healthbars.value.forEach((healthbar, idx) => {
        if (idx == a) {
          healthbar["isEmpty"] = true;
        }
      });
    }
  }
);

// function calculate minutes
let getTimeMinutes = () => {
  return (minutes.value = Math.floor(
    (distance.value % (1000 * 60 * 60)) / (1000 * 60)
  ));
};

// function calculate seconds
let getTimeSeconds = () => {
  return (seconds.value = Math.floor((distance.value % (1000 * 60)) / 1000));
};

// initial minutes and seconds value
seconds.value = getTimeSeconds();
minutes.value = getTimeMinutes();

// countdown time
let idx = setInterval(() => {
  if (!isPause.value) {
    distance.value -= 1000;

    seconds.value = getTimeSeconds();
    minutes.value = getTimeMinutes();

    if (distance.value <= 0) {
      breakInteval();
    }
  }
}, 1000);

watch(
  () => propsData.resume,
  () => {
    pause();
  }
);

// function clear interval when time hit 00:00
function breakInteval() {
  pauseEmit("timeout");
  clearInterval(idx);
}

function pause() {
  isPause.value = !isPause.value;
  pauseGame.value = !pauseGame.value;
  pauseEmit("pause", true);
}

function ChangeSound() {
  sound.value = !sound.value;
  pauseEmit("setSound", sound.value);
}
</script>
<template>
  <Transition mode="out-in">
    <div class="nav">
      <div class="healthbar">
        <img src="../assets/icons/shield.svg" alt="shield icon" width="35" />
        <div class="hp">
          <h2>HP</h2>
          <div class="bar">
            <span
              v-for="healthbar in healthbars"
              :key="healthbar.id"
              :class="{ 'bar-empty': healthbar.isEmpty }"
            ></span>
          </div>
        </div>
      </div>
      <div class="timer">
        <h2>
          {{ minutes.toString().padStart(2, 0) }}:{{
            seconds.toString().padStart(2, 0)
          }}
        </h2>
      </div>
      <div class="control-wrapper">
        <div class="sound" @click="ChangeSound()">
          <img
            src="../assets/icons/sound-on.svg"
            v-if="sound"
            width="30"
            alt="pause icon"
          />
          <img
            src="../assets/icons/sound-off.svg"
            width="30"
            v-else
            alt="play icon"
          />
        </div>
        <div class="pause" @click.prevent="pause()">
          <h2>Pause</h2>
          <img
            src="../assets/icons/pause.svg"
            v-if="!propsData.pause"
            width="35"
            alt="pause icon"
          />
          <img
            src="../assets/icons/play.svg"
            v-else
            width="35"
            alt="play icon"
          />
        </div>
      </div>
    </div>
  </Transition>
</template>


<style scoped>
.nav {
  margin: 2rem 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: var(--golden-dark-black);
}

.healthbar,
.pause,
.control-wrapper {
  display: flex;
  align-items: center;
}

.pause,
.sound {
  cursor: pointer;
}
.healthbar {
  gap: 8px;
}

.healthbar h2 {
  font-size: 0.75rem;
  padding-bottom: 0.5rem;
}

.bar {
  gap: 2px;
  display: flex;
}
.bar span {
  width: 25px;
  height: 10px;
  background: var(--golden);
}
.bar .bar-empty {
  background: none;
  border: 1px solid var(--golden);
}
.hp {
  text-align: left;
}

.control-wrapper {
  gap: 20px;
}
</style>
