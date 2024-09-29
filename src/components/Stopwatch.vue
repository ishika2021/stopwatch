<template>
  <div class="stopwatch-wrapper">
    <div class="clock-display">
      <span class="clock">{{ clock }}</span>
      <div class="action-btns">
        <button class="btn" @click="stopTimer">Stop</button>
        <button class="btn start-stop" @click="handleTimer">
          {{ timerState }}
        </button>
        <button class="btn" @click="resetTimer">Reset</button>
      </div>
    </div>
    <div class="divider"></div>
    <div class="pel-display">
      <h1>PEL</h1>
      <div class="pel-time-wrapper">
        <div
          v-if="previusElapsedTime.length"
          v-for="(time, index) in previusElapsedTime"
          :key="index"
          class="time"
        >
          <span># {{ index + 1 }}</span>
          <div>{{ time }}</div>
        </div>
        <div v-else>
          <h3>No Entries!!</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
const minutes = ref(0);
const seconds = ref(0);
const miliseconds = ref(0);
const timerState = ref("Start");
const timer = ref(null);
const previusElapsedTime = ref([]);

const clock = computed(() => {
  let m = minutes.value;
  let s = seconds.value;
  let ms = miliseconds.value;

  if (minutes.value < 10) {
    m = "0" + minutes.value;
  }
  if (seconds.value < 10) {
    s = "0" + seconds.value;
  }
  if (miliseconds.value < 10) {
    ms = "0" + miliseconds.value;
  }
  return `${m}:${s}:${ms}`;
});

const updateTimerState = () => {
  const state = timerState.value === "Start" ? "Pause" : "Start";
  timerState.value = state;
};

const handleTimer = () => {
  updateTimerState();
  if (timerState.value === "Pause") {
    startTimer();
  } else {
    pauseTimer();
  }
};

const startTimer = () => {
  timer.value = setInterval(() => {
    if (miliseconds.value < 59) {
      miliseconds.value++;
    } else if (seconds.value < 59) {
      seconds.value++;
      miliseconds.value = 0;
    } else if (minutes.value < 59) {
      minutes.value++;
      seconds.value = 0;
    }
  }, 100);
};

const pauseTimer = () => {
  if (timer.value) {
    clearInterval(timer.value);
  }
};

const stopTimer = () => {
  if (timerState.value === "Pause") {
    if (timer.value) {
      clearInterval(timer.value);
    }
    previusElapsedTime.value.push(clock.value);
    timerState.value = "Start";
    miliseconds.value = 0;
    seconds.value = 0;
    minutes.value = 0;
  }
};

const resetTimer = () => {
  if (timerState.value === "Start") {
    previusElapsedTime.value = [];
    miliseconds.value = 0;
    seconds.value = 0;
    minutes.value = 0;
  }
};
</script>

<style>
@import "./Stopwatch.css";
</style>
