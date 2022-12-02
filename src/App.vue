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
  <button @click.prevent="resetBoard">Reset</button>
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
import { ref } from 'vue'

enum Mark {
  O = 0,
  X = 1,
}

const currentPlayer = ref<Mark>(0)
const grid = ref<Mark[][]>([])

const num: number = 0

function resetBoard() {
  grid.value = [
    [null, null, null],
    [null, null, null],
    [null, null, null],
  ]
}

resetBoard()

function select(row, col) {
  if (grid.value[row][col] == null) {
    grid.value[row][col] = currentPlayer.value
    currentPlayer.value = 1 - currentPlayer.value
  }
}
</script>
