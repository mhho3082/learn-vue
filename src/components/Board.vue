<script setup>
const ids = [...Array(9).keys()];
const props = defineProps({ state: Array });
const emit = defineEmits(["buttonClick"]);
</script>

<template>
  <div class="base">
    <button v-for="id in ids" key="id" @click="() => $emit('buttonClick', id)">
      {{ props.state[id] || "" }}
    </button>
  </div>
</template>

<style scoped lang="scss">
.base {
  display: grid;
  grid: repeat(3, 1fr) / repeat(3, 1fr);
  gap: 5px 5px;
}

button {
  position: relative;
  cursor: pointer;
  counter-increment: el;
  width: min(25vw, 100px);
  height: min(25vw, 100px);
  font-size: 3em;
  font-family: "Courier New", Courier, monospace;

  /* Flip the grid into a numpad (for easier keyboard input) */
  &:nth-child(-n + 6) {
    grid-row: 2;
  }

  &:nth-child(-n + 3) {
    grid-row: 3;
  }

  /* Add numbering */
  &::before {
    content: counter(el);
    position: absolute;
    left: 10%;
    top: 10%;
    font-size: 0.5em;
    display: block;
    color: #888;
  }
}
</style>
