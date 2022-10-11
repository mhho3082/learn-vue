<script setup>
import Board from "@/components/Board.vue";
import { ref, computed, watch, onMounted } from "vue";

const history = ref([]);
history.value.push(new Array(9).fill(null));
const step = ref(0);

const player = computed(() => {
  return ai.value ? "O" : step.value % 2 ? "X" : "O";
});

const handlePress = (id) => {
  makeMove(id);
};

onMounted(() =>
  // Add keyboard binding
  window.addEventListener("keypress", (e) => {
    // Note: e.charCode is depreciated, e.key is used instead

    // bind '1' - '9' to the board
    // bind '+' and '-' to time machine
    if (e.key.charCodeAt(0) >= 49 && e.key.charCodeAt(0) <= 57) {
      makeMove(e.key.charCodeAt(0) - 49);
    } else if (e.key === "+") {
      if (step.value < history.value.length) {
        step.value++;
      }
    } else if (e.key === "-") {
      if (step.value > 0) {
        step.value--;
      }
    }
  })
);

const makeMove = (id) => {
  let state = history.value[step.value].slice();

  // Handle human input
  if (isWon.value || state[id]) {
    return;
  }
  state[id] = player.value;

  // Handle AI input
  if (ai.value && !checkWin(state) && !state.every((e) => e)) {
    state[aiThinkMove(state)] = "X";
  }

  history.value.splice(step.value + 1);
  history.value.push(state);
  step.value++;
};

const fullMap = computed(() => {
  return history.value[step.value].every((cell) => cell);
});

const winSituations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];
const checkWin = (state) => {
  const good = winSituations.filter(
    (e) =>
      state[e[0]] && state[e[0]] === state[e[1]] && state[e[0]] === state[e[2]]
  );
  return good.length ? state[good[0][0]] : null;
};
const isWon = computed(() => {
  return checkWin(history.value[step.value]);
});

const ai = ref(true);
// Reset board history if AI toggled
watch(ai, () => {
  history.value.splice(1);
  step.value = 0;
});
const aiThinkMove = (state) => {
  // A basic, naive AI
  // Cannot nicely handle paradoxes
  // (But this is tic-tac-toe, so who cares)
  let temp = state.slice();
  let won;
  let decision = [];
  let level = 0;
  for (let i = 0; i < 9; i++) {
    if (temp[i]) {
      continue;
    }

    temp[i] = "X";
    won = checkWin(temp);
    temp[i] = null;
    if (won) {
      if (level < 2) {
        decision = [];
        level = 2;
      }
      decision.push(i);
      continue;
    }

    temp[i] = "O";
    won = checkWin(temp);
    temp[i] = null;
    if (won) {
      if (level < 1) {
        decision = [];
        level = 1;
      }
      if (level === 1) {
        decision.push(i);
      }
      continue;
    }

    if (level === 0) {
      decision.push(i);
    }
  }
  return decision.length === 1
    ? decision[0]
    : decision[Math.floor(Math.random() * decision.length)];
};

const title = computed(() => {
  return isWon.value === "O"
    ? `${ai.value ? "You" : "O"} have won!`
    : isWon.value === "X"
    ? `${ai.value ? "The AI" : "X"} have won!`
    : fullMap.value
    ? `It's a draw`
    : ai.value
    ? `It's your turn`
    : `It's ${player.value}'s turn`;
});
</script>

<template>
  <div class="base">
    <p class="title">
      {{ title }}
    </p>

    <button class="ai" @click="ai = !ai">
      AI:
      <span v-if="ai" class="on">ON</span>
      <span v-else class="off">OFF</span>
    </button>

    <Board :state="history[step]" @button-click="(id) => handlePress(id)" />

    <div class="time-machine">
      <button
        v-for="(_, i) in history"
        @click="step = i"
        :class="{
          'current-step': step === i,
          'win-step': checkWin(history[i]),
        }"
      >
        {{ i ? `Step ${i}` : "Start" }}
      </button>
    </div>
  </div>
</template>

<style scoped lang="scss">
.base {
  display: grid;
  grid: auto auto / auto auto;
  grid-auto-flow: row;
  justify-content: center;
}

.title {
  font-size: 2em;
  width: 100%;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
}

.ai {
  width: 100%;
  color: #666;
  box-sizing: border-box;
  border: none;
  background-color: transparent;
  font-size: 1em;
  font-family: Arial, Helvetica, sans-serif;

  * {
    cursor: pointer;
    padding: 0.25em;
    border-radius: 20%;
  }

  .on {
    color: green;
    &:hover {
      background-color: lightgreen;
    }
  }

  .off {
    color: red;
    &:hover {
      background-color: pink;
    }
  }
}

.time-machine {
  width: clamp(6ch, 4em, 8ch);
  margin-left: 10px;
  margin-top: 5px;
  display: block;

  button {
    width: 100%;
    margin-bottom: 5px;
    cursor: pointer;
  }

  .current-step {
    font-weight: bold;
  }

  .win-step {
    color: green;
  }
}
</style>
