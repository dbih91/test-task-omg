<template>
  <main class="app-main">
    <AppRow
      v-for="row of rows"
      ref="rowInstances"
      :key="row"
      />
  </main>
</template>

<script
  setup
  lang="ts"
>
import AppRow from './AppRow.vue'
import random from '../utils/random.ts'
import { inject, onMounted, Ref, ref } from 'vue'

const rows = new Array(random(0xff, 0x80)).fill(0).map((_, index) => index)
const rowInstances = ref<InstanceType<typeof AppRow>[] | null>(null)
const waterfall = inject<Ref<boolean>>('waterfall')

function sleep (milliseconds: number) {
  return new Promise<void>((resolve) => setTimeout(resolve, milliseconds))
}

function getVisibleRowInstances () {
  return rowInstances.value?.filter((rowInstance) => rowInstance.visible) ?? []
}

async function *changeNumbersWaterfall () {
  const visibleRowInstances = getVisibleRowInstances()
  const milliseconds = Math.floor(1000 / visibleRowInstances.length)

  for (const visibleRowInstance of visibleRowInstances) {
    yield visibleRowInstance
    await sleep(milliseconds)
  }

  return
}

onMounted(() => {
  setInterval(async () => {
    if (rowInstances.value) {
      if (waterfall?.value) {
        for await (const rowInstance of changeNumbersWaterfall()) {
          rowInstance.updateRandomCell()
        }
      } else {
        for (const rowInstance of getVisibleRowInstances()) {
          rowInstance.updateRandomCell()
        }
      }
    }
  }, 1000)
})
</script>

<style lang="scss">
.app-main {
  display: flex;
  flex-flow: column;
  gap: 8px;
  margin-inline: auto;
}
</style>