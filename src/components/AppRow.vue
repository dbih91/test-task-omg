<template>
  <div
    ref="root"
    class="app-row"
  >
    <app-cell
      v-for="cell of cells"
      v-memo="[cell.value]"
      :key="cell.index"
      :value="cell.value"
    />
  </div>
</template>

<script
  setup
  lang="ts"
>
import { ref } from 'vue'
import AppCell from './AppCell.vue'
import random from '../utils/random.ts'
import { useElementVisibility } from '@vueuse/core'

const cells = ref(new Array(12).fill(0).map((_, index) => ({ index, value: random(0xffff)})))
const root = ref<HTMLDivElement | null>(null)
const visible = useElementVisibility(root)

function updateRandomCell () {
  cells.value[random(11)].value = random(0xffff)
}

defineExpose({
  visible,
  updateRandomCell
})
</script>

<style lang="scss">
.app-row {
  display: flex;
  flex-flow: row nowrap;
  gap: 8px;
}
</style>