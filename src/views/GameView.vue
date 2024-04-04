<template>
  <v-container>
    <v-row>
      <v-col cols="6">
        <v-text-field v-model="sizeX" label="Size X" type="number" max="64" @input="generateGrid"></v-text-field>
      </v-col>
      <v-col cols="6">
        <v-text-field v-model="sizeY" label="Size Y" type="number" max="64" @input="generateGrid"></v-text-field>
      </v-col>
    </v-row>

    <div class="grid-container" :style="{ gridTemplateColumns: `repeat(${sizeX}, 36px)`, gridTemplateRows: `repeat(${sizeY}, 36px)` }">
      <div v-for="(row, rowIndex) in grid" :key="rowIndex" class="grid-row">
        <div
          v-for="(col, colIndex) in row"
          :key="colIndex"
          :id="`cell-${rowIndex}-${colIndex}`"
          class="grid-cell"
          @mouseover="toggleColor(rowIndex, colIndex)"
        ></div>
      </div>
    </div>
  </v-container>
</template>

<script>
import { ref, reactive } from 'vue';

export default {
  setup() {
    const sizeX = ref(0);
    const sizeY = ref(0);
    const grid = reactive([]);

    const generateGrid = () => {
      grid.length = 0;
      for (let i = 0; i < sizeY.value; i++) {
        const row = [];
        for (let j = 0; j < sizeX.value; j++) {
          row.push(true);
        }
        grid.push(row);
      }
    };

    const toggleColor = (rowIndex, colIndex) => {
      const el = document.querySelector(`#cell-${rowIndex}-${colIndex}`)
      
      if (el.classList.contains('blue')) {
        el.classList.remove('blue')
      } else {
        el.classList.add('blue')
      }
    };

    return { sizeX, sizeY, grid, generateGrid, toggleColor };
  }

}
</script>

<style scoped>
.grid-container {
  display: flex;
  flex-direction: column;
  background-color: #333;
}

.grid-row {
  display: flex;
}

.grid-cell {
  min-width: 36px;
  min-height: 36px;
  border: 1px solid transparent;
  border-radius: 5px;
  background-color: white;
}

.blue {
  background-color: blue;
}
</style>
