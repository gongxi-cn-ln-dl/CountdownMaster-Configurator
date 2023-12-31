<script setup>
import { ref, inject } from 'vue'

// 从父组件注入事件处理函数
const addEvent = inject('addEvent');

// 用于存储用户输入的数据
const inputDate = ref('');
const inputEventName = ref('');

// 处理添加事件的逻辑
function postEvent() {
  if (inputEventName.value && inputDate.value) {
    addEvent(inputEventName.value, inputDate.value);
    // 清空输入字段
    inputEventName.value = '';
    inputDate.value = '';
  }
}
</script>

<template>
  <div>
    <el-row :gutter="20" justify="space-between" style="margin-top: 10px;">
      <!-- 事件名称输入框 -->
      <el-col :span="6">
        <el-input
          v-model="inputEventName"
          placeholder="事件名称"
        />
      </el-col>

      <!-- 日期选择器 -->
      <el-col :span="10">
        <el-date-picker
          v-model="inputDate"
          type="date"
          placeholder="选择日期"
          value-format="YYYY-MM-DD"
        />
      </el-col>

      <!-- 添加事件按钮 -->
      <el-button type="primary" @click="postEvent">添加事件</el-button>
    </el-row>
  </div>
</template>
