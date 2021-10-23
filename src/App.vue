<template>
  <div class="container-time">
    <vue-countdown
      class="countdown"
      :time="state.time"
      v-slot="{ minutes, seconds }"
      @end="onCountdownEnd"
      @progress="(data) => (state.time = data.totalMilliseconds)"
    >
      {{ fmt(minutes) }} : {{ fmt(seconds) }}
    </vue-countdown>
  </div>
  <div class="container-btn">
    <timer-button label="1m" @click="startCountdown(1, 0)" />
    <timer-button label="2m" @click="startCountdown(2, 1)" />
    <timer-button label="3m" @click="startCountdown(3, 2)" />
    <p>{{ state.count[0] }}</p>
    <p>{{ state.count[1] }}</p>
    <p>{{ state.count[2] }}</p>
  </div>
  <div class="container-clear">
    <button @click="clearCount">clear</button>
  </div>
  <div class="container-sum">{{ state.count.reduce((a, b) => a + b) }}</div>
</template>

<script lang="ts">
import { defineComponent, reactive } from "vue";
import TimerButton from "./components/TimerButton.vue";
import VueCountdown from "@chenfengyuan/vue-countdown";

export default defineComponent({
  components: {
    TimerButton,
    VueCountdown,
  },
  setup() {
    const state = reactive({
      count: [0, 0, 0],
      time: 0,
    });

    const fmt = (num: number) => {
      return num.toString().padStart(2, "0");
    };

    const startCountdown = (minutes: number, index: number) => {
      state.time = minutes * 60 * 1000;
      state.count[index]++;
    };

    // eslint-disable-next-line @typescript-eslint/no-var-requires
    const base64Sound = require("./sound.js");
    const dataURI = "data:audio/mp3;base64," + base64Sound.default;
    const sound = new Audio(dataURI);
    const onCountdownEnd = () => {
      sound.play();
    };

    const clearCount = () => {
      // eslint-disable-next-line @typescript-eslint/no-unused-vars
      state.count = state.count.map((_) => 0);
    };

    return {
      state,
      fmt,
      startCountdown,
      onCountdownEnd,
      clearCount,
    };
  },
});
</script>

<style lang="scss">
html {
  height: 100%;
  width: 100%;
}
body {
  height: 100%;
  width: 100%;
  margin: 0;
}
#app {
  display: grid;
  grid-template-areas:
    "time time time"
    "btn btn btn"
    "clear clear sum";
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.countdown {
  color: white;
  font-size: 4.5rem;
  font-weight: 700;
}
.container-time {
  grid-area: time;
}
.container-btn {
  grid-area: btn;
  display: grid;
  grid-template-rows: auto auto;
  grid-template-columns: 1fr 1fr 1fr;
  justify-items: center;
  p {
    color: white;
    margin: 0;
    margin-top: 0.25rem;
  }
}
.container-clear {
  grid-area: clear;
  margin-top: 1.5rem;
  button {
    width: 70%;
    text-decoration: none;
    color: #668ad8;
    border: solid 2px #668ad8;
    text-align: center;
    overflow: hidden;
    font-weight: bold;
    transition: 0.4s;
    cursor: pointer;
    background: transparent;
  }
}
.container-sum {
  grid-area: sum;
  color: white;
  margin-top: 1.5rem;
}
</style>
