<template>
  <div class="w-full flex justify-center items-center mb-14">
    <span class="text-3xl">Good luck!</span>
  </div>
  <div class="grid mb-14">
    <template v-for="(row, rowIndex) in grid">
      <div
        class="cell"
        :class="['row' + rowIndex, 'cell' + cellIndex, cell != null ? 'disabled' : '']"
        v-for="(cell, cellIndex) in row"
        @click="select(rowIndex, cellIndex)"
      >
        {{ Mark[cell] }}
      </div>
    </template>
  </div>
  <div class="w-full text-center my-14 text-3xl" v-if="typeof winner == 'number'">
    Winner: {{ Mark[winner] }}! Congrats!
  </div>
  <div class="w-full text-center my-14 text-3xl" v-else-if="typeof winner == 'string'">Tied! Try again!</div>
  <div class="w-full text-center my-14 text-3xl">
    <button
      class="
        border border-gray-700
        bg-gray-700
        text-white
        rounded-md
        px-4
        py-2
        m-2
        transition
        duration-500
        ease
        select-none
        hover:bg-gray-800
        focus:outline-none focus:shadow-outline
      "
      @click.prevent="resetBoard"
    >
      Reset
    </button>
  </div>
</template>

<style>
.grid {
  diplay: inline-grid !important;
  grid-template: 1fr 1fr 1fr / 1fr 1fr 1fr;
  width: 400px;
  margin: auto;
}

.cell {
  text-align: center;
  aspect-ratio: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3em;
}

.cell:not(.disabled):hover {
  background: #f0f0f0;
  cursor: pointer;
}

.cell0,
.cell1 {
  border-right: 1px solid black;
}

.row0,
.row1 {
  border-bottom: 1px solid black;
}
</style>

<script setup lang="ts">
import { ref, watch } from 'vue'

enum Mark {
  O = 0,
  X = 1,
}

const currentPlayer = ref<Mark>(0)
const grid = ref<Mark[][]>([])
const winner = ref<Mark | null | 'tie'>(null)

const num: number = 0

resetBoard()

watch(
  grid,
  () => {
    // Check Row 1
    if (grid.value[0][0] != null && grid.value[0][0] == grid.value[0][1] && grid.value[0][1] == grid.value[0][2]) {
      return (winner.value = grid.value[0][0])
    }
    // Check Row 2
    if (grid.value[1][0] != null && grid.value[1][0] == grid.value[1][1] && grid.value[1][1] == grid.value[1][2]) {
      return (winner.value = grid.value[1][0])
    }
    // Check Row 3
    if (grid.value[2][0] != null && grid.value[2][0] == grid.value[2][1] && grid.value[2][1] == grid.value[2][2]) {
      return (winner.value = grid.value[2][0])
    }
    // Check Col 1
    if (grid.value[0][0] != null && grid.value[0][0] == grid.value[1][0] && grid.value[1][0] == grid.value[2][0]) {
      return (winner.value = grid.value[0][0])
    }
    // Check Col 2
    if (grid.value[0][1] != null && grid.value[0][1] == grid.value[1][1] && grid.value[1][1] == grid.value[2][1]) {
      return (winner.value = grid.value[0][1])
    }
    // Check Col 3
    if (grid.value[0][2] != null && grid.value[0][2] == grid.value[1][2] && grid.value[1][2] == grid.value[2][2]) {
      return (winner.value = grid.value[0][2])
    }
    // Check TL -> BR Diagonal
    if (grid.value[0][0] != null && grid.value[0][0] == grid.value[1][1] && grid.value[1][1] == grid.value[2][2]) {
      return (winner.value = grid.value[0][0])
    }
    // Check TR -> BL Diagonal
    if (grid.value[0][2] != null && grid.value[0][2] == grid.value[1][1] && grid.value[1][1] == grid.value[2][0]) {
      return (winner.value = grid.value[0][2])
    }
    // Check Tie (not tied if there are any nulls)
    for (let row of grid.value) {
      for (let cell of row) {
        if (cell == null) return
      }
    }
    // Made it this far, there are no nulls and no winners, so it's a tie
    winner.value = 'tie'
  },
  { deep: true }
)

function resetBoard() {
  grid.value = [
    [null, null, null],
    [null, null, null],
    [null, null, null],
  ]
  winner.value = null
}

function select(row, col) {
  if (grid.value[row][col] == null && winner.value == null) {
    grid.value[row][col] = currentPlayer.value
    currentPlayer.value = 1 - currentPlayer.value
  }
}
</script>
