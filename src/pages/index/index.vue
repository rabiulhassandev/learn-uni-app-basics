<template>
  <view class="container">
    <view class="header">
      <text class="title">Uni-app Starter</text>
      <text class="subtitle">Explore different features</text>
    </view>

    <view class="card welcome-card">
      <text class="welcome-text">Hello, welcome to the index page!</text>
    </view>

    <view class="feature-grid">
      <!-- About Page Card -->
      <view class="feature-card" @click="goToAbout">
        <view class="card-icon">
          <text class="icon">ℹ️</text>
        </view>
        <text class="card-title">About</text>
        <text class="card-description">Learn about this application</text>
      </view>

      <!-- Counter Page Card -->
      <view class="feature-card" @click="goToCounter">
        <view class="card-icon">
          <text class="icon">🔢</text>
        </view>
        <text class="card-title">Counter</text>
        <text class="card-description">Work with reactive data</text>
      </view>

      <!-- API Call Page Card -->
      <view class="feature-card" @click="goToApiCall">
        <view class="card-icon">
          <text class="icon">🌐</text>
        </view>
        <text class="card-title">API Demo</text>
        <text class="card-description">Fetch remote data</text>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
// Navigation functions
const goToAbout = () => {
  uni.navigateTo({
    url: '/pages/index/about'
  });
};

const goToCounter = () => {
  uni.navigateTo({
    url: '/pages/index/counter'
  });
};

const goToApiCall = () => {
  uni.navigateTo({
    url: '/pages/index/api-call'
  });
};
</script>

<style lang="scss">
.container {
  padding: 24rpx;
  background-color: #f5f7fa;
  min-height: 100vh;
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

.welcome-card {
  background-color: #ffffff;
  border-radius: 16rpx;
  padding: 32rpx;
  margin-bottom: 32rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.welcome-text {
  font-size: 32rpx;
  color: #34495e;
  text-align: center;
}

.feature-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24rpx;
}

.feature-card {
  background-color: #ffffff;
  border-radius: 16rpx;
  padding: 32rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  
  &:active {
    transform: scale(0.98);
    box-shadow: 0 2rpx 6rpx rgba(0, 0, 0, 0.1);
  }
}

.card-icon {
  width: 80rpx;
  height: 80rpx;
  background-color: #e3f2fd;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 20rpx;
}

.icon {
  font-size: 40rpx;
}

.card-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #2c3e50;
  text-align: center;
  display: block;
  margin-bottom: 8rpx;
}

.card-description {
  font-size: 26rpx;
  color: #7f8c8d;
  text-align: center;
  display: block;
}

/* Responsive layout for larger screens */
@media (min-width: 768px) {
  .feature-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>