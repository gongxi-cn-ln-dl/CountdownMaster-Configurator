<script setup>
import { inject, computed, reactive } from 'vue';

// 注入 profileObject
const profileObject = inject("profileObject");

// 计算样式百分比
const percentageWidth = computed(() => `${profileObject.window.width * 100}%`);
const percentageHeight = computed(() => `${profileObject.window.height * 100}%`);
const percentageLeft = computed(() => `${profileObject.window.left * 100}%`);
const percentageTop = computed(() => `${profileObject.window.top * 100}%`);

// 创建响应式样式对象
const styleObject = reactive({
  width: percentageWidth,
  height: percentageHeight,
  left: percentageLeft,
  top: percentageTop,
  position: 'absolute'
});

// 计算距离事件的天数
function calculateDaysRemaining(eventTime) {
  const currentDate = new Date();
  const targetDate = new Date(eventTime);
  const timeDiff = targetDate - currentDate;

  if (timeDiff >= 0) {
    return Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
  } else {
    return -1;
  }
}
</script>

<template>
  <div id="desktop-simulator" :style="styleObject">
    <div v-for="event in profileObject.events" :key="event.eventName">
      <div class="countdownWindow" v-if="calculateDaysRemaining(event.time) >= 0">
        <span class="eventName" :style="{ color: event.titleColor || '#000' }">{{ event.eventName }}</span>
        <br />
        <span class="daysRemaining" :style="{ color: event.countdownColor || '#000' }">{{ calculateDaysRemaining(event.time) }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
#desktop-simulator {
  position: absolute;
}

.countdownWindow {
  position: relative;
  margin-top: 5px;
  margin-right: 0;
  background: #ffffff33;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  flex: 1 1 300px;
  color: #000000;
  display: block;
}
.eventName {
  font-size: 32px;
  font-weight: bold;
}
.daysRemaining {
  font-size: 40px;
  font-weight: bold;
}
</style>
