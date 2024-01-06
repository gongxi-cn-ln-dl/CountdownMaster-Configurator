<script setup>
import { reactive, ref } from 'vue';
import { provide } from 'vue';

import DesktopContainer from './components/DesktopContainer.vue';
import VariablePanelVue from './components/VariablePanel.vue';

// 使用 reactive 创建响应式对象
const profileObject = reactive({ events: [] });
const hasOpenedJsonFile = ref(false);

// 添加键值对
function addKeyValuePair(key, value) {
  profileObject[key] = value;
}

// 添加事件
function addEvent(eventName, time) {
  profileObject.events.push({ eventName, time });
}

// 删除事件
function delEvent(key) {
  profileObject.events = profileObject.events.filter((t) => t.eventName !== key);
}

// 保存为 JSON 文件
function saveJsonFile() {
  const fileName = 'events.json';
  const content = JSON.stringify(profileObject);

  const element = document.createElement('a');
  const blob = new Blob([content], { type: 'text/plain' });
  const fileUrl = URL.createObjectURL(blob);

  element.href = fileUrl;
  element.download = fileName;
  document.body.appendChild(element);
  element.click();
  document.body.removeChild(element);
  URL.revokeObjectURL(fileUrl);
}

// 提供依赖
provide('profileObject', profileObject);
provide('hasOpenedJsonFile', hasOpenedJsonFile);
provide('addKeyValuePair', addKeyValuePair);
provide('addEvent', addEvent);
provide('delEvent', delEvent);
provide('saveJsonFile', saveJsonFile);

const dialogVisible = ref(false);

// 处理按键事件
const handleKeyPress = (event) => {
  if (event.shiftKey && event.key === 'O') {
    dialogVisible.value = true;
  }
};

window.addEventListener('keydown', handleKeyPress);
window.addEventListener('dblclick',() => {
  dialogVisible.value = true;
})

</script>

<template>
  <DesktopContainer />
  <el-dialog v-model="dialogVisible" title="EventConfigurator" width="35%" draggable>
    <VariablePanelVue />
  </el-dialog>
</template>

<style scoped>
</style>
