<script setup lang="ts">
import { shallowRef, type Component } from 'vue'
import GameHome from './components/GameHome.vue'
import MainPage from './components/MainPage.vue'

const pages: Record<string, Component> = {
  home: GameHome,
  main: MainPage,
}

const currentPage = shallowRef<Component>(pages.home)

const openPage = (page: string) => {
  currentPage.value = pages[page]
}
</script>

<template>
  <div class="game-main">
    <component :is="currentPage" @goPage="openPage"></component>
  </div>
</template>

<style scoped>
.game-main {
  width: 100vw;
  height: 100vh;
  background-color: var(--theme-grey-green);
  /* padding: 4rem; */
  justify-items: center;
}
</style>
