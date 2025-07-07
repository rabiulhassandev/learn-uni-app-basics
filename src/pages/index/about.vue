<template>
  <view class="about-container">
    <view class="header">
      <text class="title">About Our Company</text>
      <text class="subtitle">Our Story and Mission</text>
    </view>

    <view class="content-card">
      <image 
        src="https://images.pexels.com/photos/3453047/pexels-photo-3453047.jpeg?cs=srgb&dl=pexels-denniz-futalan-339724-3453047.jpg&fm=jpg" 
        class="about-image"
        mode="aspectFill"
      />
      
      <text class="section-title">Who We Are</text>
      <text class="about-text">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam in dui mauris. Vivamus hendrerit arcu sed erat molestie vehicula. Sed auctor neque eu tellus rhoncus ut eleifend nibh porttitor.
      </text>
      
      <text class="section-title">Our Mission</text>
      <text class="about-text">
        Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
      </text>

      <view class="stats-container">
        <view class="stat-item">
          <text class="stat-number">5+</text>
          <text class="stat-label">Years Experience</text>
        </view>
        <view class="stat-item">
          <text class="stat-number">50+</text>
          <text class="stat-label">Happy Clients</text>
        </view>
        <view class="stat-item">
          <text class="stat-number">100+</text>
          <text class="stat-label">Projects Completed</text>
        </view>
      </view>
    </view>

    <button class="back-button" @click="goBack">
      <text class="button-text">Go Back</text>
    </button>
  </view>
</template>

<script setup lang="ts">
// go back to previous page
const goBack = () => {
  uni.navigateBack({
    delta: 1
  });
};
</script>

<style lang="scss">
.about-container {
  padding: 24rpx;
  background-color: #f8f9fa;
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

.content-card {
  background-color: #ffffff;
  border-radius: 16rpx;
  padding: 32rpx;
  margin-bottom: 32rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.about-image {
  width: 100%;
  height: 300rpx;
  border-radius: 12rpx;
  margin-bottom: 24rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #2c3e50;
  margin: 24rpx 0 12rpx;
  display: block;
}

.about-text {
  font-size: 28rpx;
  color: #34495e;
  line-height: 1.6;
  margin-bottom: 16rpx;
  display: block;
}

.stats-container {
  display: flex;
  justify-content: space-between;
  margin: 32rpx 0;
}

.stat-item {
  text-align: center;
  flex: 1;
}

.stat-number {
  font-size: 36rpx;
  font-weight: bold;
  color: #007AFF;
  display: block;
}

.stat-label {
  font-size: 24rpx;
  color: #7f8c8d;
  display: block;
  margin-top: 4rpx;
}

.back-button {
  background-color: #007AFF;
  border-radius: 8rpx;
  padding: 20rpx;
  width: 100%;
  margin-top: 16rpx;
}

.button-text {
  color: white;
  font-size: 28rpx;
  font-weight: 500;
  text-align: center;
}

/* Add some animation for better UX */
.content-card {
  transition: transform 0.3s ease;
  &:active {
    transform: scale(0.98);
  }
}
</style>