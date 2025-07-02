<script setup lang="ts">
import { onUnmounted } from 'vue';

declare global {
  interface Window {
    uni: any;
  }
}

function receiveMessage(event: MessageEvent) {
  // 可以在这里处理接收到的消息
  console.log('收到消息', event.data);
}

function clickEvent(action: string) {
  switch (action) {
    case 'switchTab':
      window.uni.switchTab({
        url: '/pages/tabBar/API/API'
      });
      break;
    case 'navigateBack':
      window.uni.navigateBack({
        delta: 1
      });
      break;
    case 'postMessage':
      window.uni.postMessage({
        data: {
          action: 'postMessage',
          msg: '我从H5发送消息到UniApp'
        }
      });
      break;
    case 'getEnv':
      window.uni.getEnv(function (res) {
        alert('当前环境：' + JSON.stringify(res));
      });
      break;
    default:
      window.uni[action]({
        url: '/pages/component/button/button'
      });
      break;
  }
}
onUnmounted(() => {
  // 移除消息监听器
  window.removeEventListener('message', receiveMessage);
});
</script>

<template>
  <view class="btn-list">
    <button class="btn" @click="clickEvent('navigateBack')">返回上一页</button>
    <button class=" btn btn-yellow" @click="clickEvent('postMessage')">发送消息到UniApp(PM)</button>
    <button class="btn btn-yellow" @click="clickEvent('getEnv')">getEnv</button>
    <button class="btn" @click="clickEvent('navigateTo')">navigateTo</button>
    <button class="btn" @click="clickEvent('redirectTo')">redirectTo</button>
    <button class="btn" @click="clickEvent('reLaunch')">reLaunch</button>
    <button class=" btn btn-red" @click="clickEvent('switchTab')">switchTab</button>
  </view>
</template>

<style scoped>
.btn {
  display: block;
  margin: 20px auto;
  padding: 5px;
  background-color: #007aff;
  border: 0;
  color: #ffffff;
  height: 40px;
}

.btn-red {
  background-color: #dd524d;
}

.btn-yellow {
  background-color: #f0ad4e;
}
</style>
