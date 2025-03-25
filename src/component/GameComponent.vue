<template>
  <div class="table">
    <div class="row" v-for="(row, rowIndex) in gameField" :key="rowIndex">
      <div class="cell" v-for="(cell, cellIndex) in row" :key="cellIndex" @click="takeTurn(rowIndex, cellIndex)">
        {{ cell }}
      </div>
    </div>
  </div>
  <div class="results" v-if="winner">
    <p>{{ resultText }}</p>
    <button @click="restart">Restart</button>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const gameField = ref([['', '', ''], ['', '', ''], ['', '', '']]);
const currentPlayer = ref('X');
const winner = ref(null)

const resultText = computed(() => {
  if (winner.value === 'draw') {
    return `It's a draw!`
  } else {
    return `You win: ${winner.value}`
  }
})

function checkResult() {
  const field = gameField.value;

  const isWinningLine = (a, b, c) =>
    a && a === b && b === c;

  for (let i = 0; i < 3; i++) {
    if (isWinningLine(field[i][0], field[i][1], field[i][2])) {
      winner.value = currentPlayer.value;
      return;
    }

    if (isWinningLine(field[0][i], field[1][i], field[2][i])) {
      winner.value = currentPlayer.value;
      return;
    }
  }
  if (isWinningLine(field[0][0], field[1][1], field[2][2]) ||
      isWinningLine(field[0][2], field[1][1], field[2][0])) {
    winner.value = currentPlayer.value;
    return;
  }

  if (field.flat().every(cell => cell)) {
    winner.value = "draw";
  }
}

function takeTurn(rowIndex, cellIndex) {
  if (winner.value) return;
  if (gameField.value[rowIndex][cellIndex]) return;
  gameField.value[rowIndex][cellIndex] = currentPlayer.value;

  checkResult();
  currentPlayer.value = currentPlayer.value === 'X' ?  'O' : 'X'
}

function restart() {
  gameField.value = [['', '', ''], ['', '', ''], ['', '', '']]
  currentPlayer.value = ('X')
  winner.value = null;
}

</script>

<style scoped>
  .row {
    display: flex;
  }

  .cell {
    background-color: white;
    border: 1px solid brown;
    width: 60px;
    height: 60px;
    color: black;
    font-size: 32px;
    display: flex;
    align-items: center;
    justify-content: center; 
  }

  .results {
    margin-top: 16px;
    font-size: 16px;
  }
</style>