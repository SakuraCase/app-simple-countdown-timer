<template>
  <button class="btn">
    {{ label }}
  </button>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({
  props: {
    label: String,
  },
})
export default class TimerButton extends Vue {
  label!: string;
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.btn {
  display: inline-block;
  text-decoration: none;
  color: #668ad8;
  width: 3.5rem;
  height: 3.5rem;
  line-height: 3.5rem;
  border-radius: 50%;
  border: solid 2px #668ad8;
  text-align: center;
  overflow: hidden;
  font-weight: bold;
  transition: 0.4s;
  cursor: pointer;
  background: transparent;
}
</style>
