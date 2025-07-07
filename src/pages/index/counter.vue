<template>
  <view class="counter-container">
    <view class="header">
      <text class="title">Counter App</text>
      <text class="subtitle">Simple Increment/Decrement</text>
    </view>

    <view class="counter-card">
      <view class="counter-display">
        <text class="counter-value">{{ counter }}</text>
        <text class="counter-label">Current Value</text>
      </view>

      <view class="button-group">
        <button 
          @click="decrement" 
          class="action-button decrement"
          :disabled="counter <= 0"
        >
          <text class="button-icon">−</text>
          <text>Decrement</text>
        </button>
        
        <button 
          @click="increment" 
          class="action-button increment"
        >
          <text class="button-icon">+</text>
          <text>Increment</text>
        </button>
      </view>

      <view class="history" v-if="history.length > 0">
        <text class="history-title">Recent Changes:</text>
        <view class="history-items">
          <view 
            v-for="(item, index) in history.slice().reverse().slice(0, 3)" 
            :key="index" 
            class="history-item"
            :class="item.type"
          >
            <text class="history-value">{{ item.value }}</text>
            <text class="history-time">{{ formatTime(item.timestamp) }}</text>
          </view>
        </view>
      </view>
    </view>

    <button class="back-button" @click="goBack">
      <text>← Back to Home</text>
    </button>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const counter = ref(0);
const history = ref<Array<{
  value: number;
  type: 'increment' | 'decrement';
  timestamp: Date;
}>>([]);

const increment = () => {
  counter.value++;
  history.value.push({
    value: counter.value,
    type: 'increment',
    timestamp: new Date()
  });
};

const decrement = () => {
  if (counter.value > 0) {
    counter.value--;
    history.value.push({
      value: counter.value,
      type: 'decrement',
      timestamp: new Date()
    });
  }
};

const formatTime = (date: Date) => {
  return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
};

const goBack = () => {
  uni.navigateBack({
    delta: 1
  });
};
</script>

<style lang="scss">
.counter-container {
  padding: 24rpx;
  background-color: #f5f7fa;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  margin-bottom: 32rpx;
  text-align: center;
}

.title {
  font-size: 40rpx;
  font-weight: bold;
  color: #2c3e50;
}

.subtitle {
  font-size: 28rpx;
  color: #7f8c8d;
  display: block;
  margin-top: 8rpx;
}

.counter-card {
  background-color: #ffffff;
  border-radius: 16rpx;
  padding: 32rpx;
  margin-bottom: 24rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
  flex: 1;
}

.counter-display {
  text-align: center;
  margin: 32rpx 0;
}

.counter-value {
  font-size: 72rpx;
  font-weight: bold;
  color: #007AFF;
  display: block;
  line-height: 1;
}

.counter-label {
  font-size: 28rpx;
  color: #7f8c8d;
  display: block;
  margin-top: 8rpx;
}

.button-group {
  display: flex;
  gap: 16rpx;
  margin: 40rpx 0;
}

.action-button {
  flex: 1;
  padding: 24rpx;
  border-radius: 12rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s ease;
  
  &:active {
    transform: scale(0.96);
  }
  
  &.increment {
    background-color: #4cd964;
    color: white;
  }
  
  &.decrement {
    background-color: #ff3b30;
    color: white;
    
    &:disabled {
      background-color: #ffcdd2;
      opacity: 0.7;
    }
  }
}

.button-icon {
  font-size: 40rpx;
  font-weight: bold;
  margin-bottom: 8rpx;
}

.history {
  margin-top: 40rpx;
  border-top: 1rpx solid #eee;
  padding-top: 24rpx;
}

.history-title {
  font-size: 28rpx;
  color: #7f8c8d;
  display: block;
  margin-bottom: 16rpx;
}

.history-items {
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.history-item {
  background-color: #f9f9f9;
  border-radius: 8rpx;
  padding: 16rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
  
  &.increment {
    border-left: 4rpx solid #4cd964;
  }
  
  &.decrement {
    border-left: 4rpx solid #ff3b30;
  }
}

.history-value {
  font-weight: bold;
  color: #2c3e50;
}

.history-time {
  font-size: 24rpx;
  color: #7f8c8d;
}

.back-button {
  background-color: #007AFF;
  color: white;
  border-radius: 8rpx;
  padding: 20rpx;
  width: 100%;
  text-align: center;
  font-weight: 500;
}
</style>