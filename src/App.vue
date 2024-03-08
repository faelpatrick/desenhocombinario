<template>
  <v-container>
    <v-row>
      <v-col cols="4">
        <v-text-field v-for="(input, index) in binaryInputs" :key="index" v-model="binaryInputs[index]" :maxlength="8"
          :rules="[rules]" label="Insira os binários" variant="underlined" dense></v-text-field>
      </v-col>
      <v-col cols="8">
        <div class="grid">
          <div v-for="(row, rowIndex) in binaryInputs" :key="`row-${rowIndex}`" class="row">
            <div v-for="(bit, columnIndex) in row.split('')" :key="`col-${columnIndex}`"
              :class="{ 'bit': true, 'on': bit === '1' }"></div>
          </div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, watch } from 'vue'

const binaryInputs = ref(Array(8).fill(''))

const rules = [
  value => value.length <= 8 || 'Máximo 8 dígitos',
  value => /^([01]{0,8})$/.test(value) || 'Somente 0s e 1s são permitidos',
]

binaryInputs.value.forEach((_, index) => {
  watch(() => binaryInputs.value[index], (newValue) => {
    // Filtra o valor para manter apenas 0s e 1s
    binaryInputs.value[index] = newValue.replace(/[^0-1]/g, '');
  });
});
</script>

<style>
.grid .row {
  display: flex;
}

.bit {
  width: 65px;
  height: 65px;
  border: 1px solid #000;
}

.on {
  background-color: #000;
}
</style>