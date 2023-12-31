<script setup>
import { ref, inject } from 'vue';
import DesktopSimulator from './DesktopSimulator.vue';

// 从父组件注入 hasOpenedJsonFile
const hasOpenedJsonFile = inject("hasOpenedJsonFile");
</script>

<template>
  <div id="bd">
    <DesktopSimulator v-if="hasOpenedJsonFile" />
  </div>
</template>

<style scoped>
#bd {
  margin: 5px;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: transparent;
  color: rgb(0, 0, 0);
  display: flex;
  justify-content: flex-start;
  flex-direction: column;
  align-items: stretch;
}
</style>
