<script setup>
import { ref, computed } from "vue";
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};

const winner = computed(() => CalculateWinner(board.value.flat()));

const MakeMove = (x, y) => {
  if (winner.value) return;

  if (board.value[x][y] != "") return;

  board.value[x][y] = player.value;

  player.value = player.value === "X" ? "O" : "X";
};

const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-900 min-h-screen dark:text-white">
    <h1 class="m-9 text-5xl font-bold uppercase text-green-300">
      <span class="text-pink-300">X</span>
      <span class="text-green-300">&</span>
      <span class="text-blue-300">O </span>
      BattleGrid
    </h1>

    <h3 class="text-xl mb-5">Player {{ player }} 's turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`border border-green-300 w-24 h-24 hover:bg-gray-800 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
    </div>

    <div class="text-center">
      <h2
        v-if="winner"
        :class="['text-3xl', 'font-bold', 'mb-8', { blink: player === 'O' }]"
      >
        PLAYER '{{ winner }}' WINS !
      </h2>
      <button
        @click="ResetGame"
        class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300"
      >
        Reset
      </button>
    </div>
  </main>
</template>

<style>
@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.blink {
  animation: blink 1s infinite;
}
</style>
