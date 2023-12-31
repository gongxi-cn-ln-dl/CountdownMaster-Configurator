<script setup>
import { ref, inject } from 'vue';
import AddEvents from './AddEvents.vue';

// 从父组件注入函数和对象
const addKeyValuePair = inject('addKeyValuePair');
const delEvent = inject('delEvent');
const profileObject = inject('profileObject');
const hasOpenedJsonFile = inject("hasOpenedJsonFile");
const saveJsonFile = inject("saveJsonFile");

// 处理文件更改事件
const handleChange = async (file) => {
  try {
    const content = await readFileContent(file.raw);
    const jsonObject = JSON.parse(content);
    Object.entries(jsonObject).forEach(([key, value]) => {
      addKeyValuePair(key, value);
    });
    hasOpenedJsonFile.value = true;
  } catch (error) {
    console.error(error);
  }
};

// 读取文件内容
const readFileContent = (file) => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.onerror = () => reject(new Error('Failed to read the file.'));
    reader.readAsText(file);
  });
};
</script>

<template>
  <el-row v-if="!hasOpenedJsonFile">
    <el-col :span="24">
      <el-upload
        action="#"
        :show-file-list="false"
        :on-change="handleChange"
        accept=".json"
      >
        <el-button>打开 events.json</el-button>
      </el-upload>
    </el-col>
  </el-row>
  <div v-if="hasOpenedJsonFile">
    <el-row :gutter="20">
      <!-- 窗口布局控制滑块 -->
      <el-col :span="24" v-for="(property, index) in ['width', 'height', 'left', 'top']" :key="index">
        <div class="slider-container">
          <span class="slider-label">{{ property }}</span>
          <el-slider v-model="profileObject.window[property]" :step="0.01" :min="0" :max="1" show-input/>
        </div>
      </el-col>
    </el-row>

    <el-divider/>

    <!-- 事件列表 -->
    <el-row v-for="(event, index) in profileObject.events" :key="index" :gutter="20" justify="space-between" style="margin-top: 5px;">
      <el-col :span="5">
        <el-input v-model="event.eventName" placeholder="事件名称"/>
      </el-col>
      <el-date-picker v-model="event.time" type="date" placeholder="选择日期" value-format="YYYY-MM-DD"/>
      <el-button disabled>修改颜色</el-button>
      <el-button type="danger" @click="delEvent(event.eventName)">删除事件</el-button>
    </el-row>

    <AddEvents/>
    <el-divider/>
    <el-button @click="saveJsonFile">保存更改</el-button>
  </div>
</template>

<style>
.slider-container {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
}

.slider-label {
  margin-right: 10px;
  white-space: nowrap;
}
</style>
