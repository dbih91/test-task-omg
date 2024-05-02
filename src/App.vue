<template>
  <header class="app-header">
    <h1>
      OMG Numbers - Test Task
    </h1>
    <nav>
      <button @click="animated = !animated">
        {{ animated ? '✔️' : '❌' }} Animated
      </button>
      <button @click="waterfall = !waterfall">
        {{ waterfall ? '✔️' : '❌' }} Waterfall
      </button>
    </nav>
  </header>
  <app-numbers />
  <footer class="app-footer">
    <b>Author: Petrov Danil (179danil@gmail.com)</b>
    <a href="https://github.com/dbih91">github.com/DIdH91</a>
  </footer>
</template>

<script setup lang="ts">
import AppNumbers from './components/AppNumbers.vue'
import { provide, ref } from 'vue'

const animated = ref(true)
const waterfall = ref(true)

provide('animated', animated)
provide('waterfall', waterfall)
</script>

<style lang="scss">
.app-header {
  display: flex;
  justify-content: space-between;
  width: 100%;
  position: sticky;
  top: 0;
  background-color: var(--card-color);
  border-bottom: 2px solid var(--border-color);
  z-index: 1;
  box-shadow: 0 0 16px var(--background-color);
  padding-block: 8px;
  padding-inline: max(calc(50% - 428px), 16px);

  nav {
    display: flex;
    align-items: center;
    gap: 16px;
  }
}

.app-footer {
  display: flex;
  justify-content: space-between;
  width: 100%;
  background-color: var(--card-color);
  border-top: 2px solid var(--border-color);
  z-index: 1;
  box-shadow: 0 0 16px var(--background-color);
  padding-block: 8px;
  padding-inline: max(calc(50% - 428px), 16px);
}
</style>
