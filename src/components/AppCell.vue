<template>
  <div class="app-cell">
    <span>
      <template v-if="isAnimated">
        <span
          v-for="n of 5"
          :key="n"
          :style="{ '--offset': displayValue[n - 1] === '•' ? 10 : 9 - +(displayValue[n - 1]) }"
          v-text="TEXT"
        />
      </template>
      <template v-else>
        {{ displayValue }}
      </template>
    </span>
  </div>
</template>

<script setup lang="ts">
import { computed, inject, Ref, ref, toRef, watch } from 'vue'

const TEXT = '9\n8\n7\n6\n5\n4\n3\n2\n1\n0\n•'
const props = withDefaults(defineProps<{ value: number }>(), { value: 0 })

const localValue = toRef(props.value)
const isNextTick = ref(false)
const isAnimated = computed(() => localValue.value !== props.value)
const fromValue = computed(() => `${ props.value }`.padStart(5, '•'))
const toValue = computed(() => `${ localValue.value }`.padStart(5, '•'))
const displayValue = computed(() => isNextTick.value ? fromValue.value : toValue.value)

const animated = inject<Ref<boolean>>('animated')

function animate () {
  if (!animated?.value) {
    return localValue.value = props.value
  }
  setTimeout(() => {
    localValue.value = props.value
    isNextTick.value = false
  }, 550)
  setTimeout(() => {
    isNextTick.value = true
  }, 100)
}

watch(() => props.value, animate)
</script>

<style lang="scss">
.app-cell {
  display: flex;
  align-items: center;
  justify-content: center;

  width: 64px;
  height: 64px;

  border: 2px solid var(--border-color);
  background-color: var(--card-color);
  border-radius: 8px;

  transition: scale ease-in-out 250ms;

  &:hover {
    scale: 0.8;
  }

  font-family: "JetBrains Mono", monospace;

  & > span {
    position: relative;
    overflow: hidden;
    max-height: 32px;
    line-height: 32px;
    font-weight: 500;

    &::after {
      left: -4px;
      position: absolute;
      width: calc(100% + 8px);
      height: 100%;
      top: 0;
      display: block;
      content: ' ';
      box-shadow: inset 0 0 4px 4px var(--card-color);
    }
  }

  & > span > span {
    position: relative;
    display: inline-block;
    max-height: 32px;
    white-space: pre;
    line-height: 32px;
    top: calc(var(--offset, 0) * -32px);
    transition: top ease-in-out 500ms;
  }
}
</style>