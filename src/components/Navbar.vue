<script setup>
</script>
<template>
  <Transition mode="out-in">
    <div class="nav">
      <div class="healthbar">
        <img src="../assets/icons/shield.svg" alt="shield icon" width="35" />
        <div class="hp">
          <h2>HP</h2>
          <div class="bar">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>
      <div class="timer">
        <h2>04:00</h2>
      </div>
      <div class="pause">
        <h2>Pause</h2>
        <img src="../assets/icons/pause.svg" width="35" alt="pause icon" />
      </div>
    </div>
  </Transition>
</template>


<style scoped>
.nav {
  position: absolute;
  top: 0;
  width: 100%;
  margin: 1rem 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: var(--golden-dark-black);
}

.healthbar,
.pause {
  display: flex;
  align-items: center;
  gap: 8px;
}

.pause {
  cursor: pointer;
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
.hp {
  text-align: left;
}
</style>
