<template>
  <view class="container">
    <view class="header">
      <text class="title">Uni-app API Demo</text>
      <text class="subtitle">Explore various Uni-app capabilities</text>
    </view>

    <view class="card">
      <!-- HTTP Request Section -->
      <view class="section">
        <text class="section-title">HTTP Request</text>
        <text class="section-description">Using uni.request() to fetch data</text>
        
        <view class="api-controls">
          <button 
            @click="fetchPost" 
            class="api-button"
            :disabled="loading"
          >
            <view class="button-content">
              <text v-if="!loading">🌐 Fetch Post</text>
              <text v-else>⏳ Loading...</text>
            </view>
          </button>
          
          <button 
            @click="resetData" 
            class="api-button reset"
            :disabled="loading"
          >
            <text>🔄 Reset</text>
          </button>
        </view>

        <view v-if="loading" class="loading-indicator">
          <activity-indicator size="large" color="#007AFF" />
          <text>Fetching data from API...</text>
        </view>

        <view v-else-if="error" class="error-message">
          <text class="error-icon">⚠️</text>
          <text class="error-text">{{ error }}</text>
        </view>

        <view v-else-if="postData" class="api-response">
          <view class="response-card">
            <view class="data-row">
              <text class="data-label">ID:</text>
              <text class="data-value">{{ postData.id }}</text>
            </view>
            <view class="data-row">
              <text class="data-label">Title:</text>
              <text class="data-value">{{ postData.title }}</text>
            </view>
            <view class="data-row">
              <text class="data-label">Body:</text>
              <text class="data-value">{{ postData.body }}</text>
            </view>
          </view>
        </view>
      </view>

      <!-- Toast Message Section -->
      <view class="section">
        <text class="section-title">Toast Messages</text>
        <text class="section-description">Using uni.showToast() for notifications</text>
        
        <view class="button-group">
          <button @click="showSuccessToast" class="toast-button success">
            <text>✅ Success Toast</text>
          </button>
          <button @click="showErrorToast" class="toast-button error">
            <text>❌ Error Toast</text>
          </button>
          <button @click="showLoadingToast" class="toast-button info">
            <text>⏳ Loading Toast</text>
          </button>
        </view>
      </view>

      <!-- Local Storage Section -->
      <view class="section">
        <text class="section-title">Local Storage</text>
        <text class="section-description">Using uni.setStorage()/uni.getStorage()</text>
        
        <view class="storage-controls">
          <input 
            v-model="storageInput" 
            class="storage-input" 
            placeholder="Enter value to store"
          />
          <view class="storage-buttons">
            <button @click="saveToStorage" class="storage-button save">
              <text>💾 Save</text>
            </button>
            <button @click="getFromStorage" class="storage-button load">
              <text>📂 Retrieve</text>
            </button>
            <button @click="clearStorage" class="storage-button clear">
              <text>🗑️ Clear</text>
            </button>
          </view>
          <text v-if="storageValue" class="storage-value">
            Stored value: {{ storageValue }}
          </text>
        </view>
      </view>

      <!-- Image Upload Section -->
      <view class="section">
        <text class="section-title">Image Upload</text>
        <text class="section-description">Using uni.chooseImage()</text>
        
        <button @click="chooseImage" class="upload-button">
          <text>📷 Choose Image</text>
        </button>
        
        <view v-if="imagePath" class="image-preview">
          <image :src="imagePath" class="preview-image" mode="aspectFit" />
          <text class="image-info">Selected image: {{ imageName }}</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Post {
  id: number;
  title: string;
  body: string;
  userId: number;
}

// HTTP Request Section
const loading = ref(false);
const error = ref('');
const postData = ref<Post | null>(null);

const fetchPost = async () => {
  loading.value = true;
  error.value = '';
  
  try {
    const res = await uni.request({
      url: 'https://jsonplaceholder.typicode.com/posts/1',
      method: 'GET'
    });
    
    if (res.statusCode === 200) {
      postData.value = res.data as Post;
      showToast('Post fetched successfully!', 'success');
    } else {
      error.value = `Request failed with status: ${res.statusCode || 'Unknown'}`;
      showToast('Failed to fetch post', 'error');
    }
  } catch (err) {
    error.value = `Error: ${err instanceof Error ? err.message : String(err)}`;
    showToast('Network error occurred', 'error');
  } finally {
    loading.value = false;
  }
};

const resetData = () => {
  postData.value = null;
  error.value = '';
  showToast('Data reset', 'none');
};

// Toast Messages Section
const showToast = (title: string, icon: 'success' | 'loading' | 'error' | 'none' = 'none') => {
  uni.showToast({
    title,
    icon,
    duration: 2000
  });
};

const showSuccessToast = () => showToast('Operation successful!', 'success');
const showErrorToast = () => showToast('Something went wrong', 'error');
const showLoadingToast = () => {
  uni.showToast({
    title: 'Processing...',
    icon: 'loading',
    duration: 2000
  });
};

// Local Storage Section
const storageInput = ref('');
const storageValue = ref('');

const saveToStorage = () => {
  if (!storageInput.value) {
    showToast('Please enter a value', 'error');
    return;
  }
  uni.setStorage({
    key: 'demoStorage',
    data: storageInput.value,
    success: () => {
      showToast('Value saved!', 'success');
      storageInput.value = '';
    }
  });
};

const getFromStorage = () => {
  uni.getStorage({
    key: 'demoStorage',
    success: (res) => {
      storageValue.value = res.data;
      showToast('Value retrieved', 'success');
    },
    fail: () => {
      storageValue.value = '';
      showToast('No stored value found', 'error');
    }
  });
};

const clearStorage = () => {
  uni.removeStorage({
    key: 'demoStorage',
    success: () => {
      storageValue.value = '';
      showToast('Storage cleared', 'success');
    }
  });
};

// Image Upload Section
const imagePath = ref('');
const imageName = ref('');

const chooseImage = () => {
  uni.chooseImage({
    count: 1,
    sizeType: ['compressed'],
    sourceType: ['album', 'camera'],
    success: (res) => {
      imagePath.value = res.tempFilePaths[0];
      imageName.value = res.tempFiles[0].name || 'Selected image';
      showToast('Image selected', 'success');
    },
    fail: () => {
      showToast('Image selection failed', 'error');
    }
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

.card {
  background-color: white;
  border-radius: 16rpx;
  padding: 32rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.section {
  margin-bottom: 40rpx;
  padding-bottom: 32rpx;
  border-bottom: 1rpx solid #eee;
  
  &:last-child {
    margin-bottom: 0;
    padding-bottom: 0;
    border-bottom: none;
  }
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #2c3e50;
  margin-bottom: 8rpx;
}

.section-description {
  font-size: 26rpx;
  color: #7f8c8d;
  display: block;
  margin-bottom: 20rpx;
}

/* HTTP Request Styles */
.api-controls {
  display: flex;
  gap: 16rpx;
  margin-bottom: 24rpx;
}

.api-button {
  flex: 1;
  padding: 20rpx;
  background-color: #007AFF;
  color: white;
  border-radius: 8rpx;
  text-align: center;
  font-size: 28rpx;
  
  &:disabled {
    background-color: #cccccc;
    opacity: 0.7;
  }
  
  &.reset {
    background-color: #ff3b30;
  }
}

.loading-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40rpx 0;
  gap: 16rpx;
}

.error-message {
  background-color: #ffeeee;
  padding: 20rpx;
  border-radius: 8rpx;
  display: flex;
  align-items: center;
  gap: 12rpx;
}

.error-icon {
  font-size: 36rpx;
}

.error-text {
  color: #ff3b30;
  font-size: 28rpx;
}

.response-card {
  background-color: #f9f9f9;
  border-radius: 8rpx;
  padding: 20rpx;
}

.data-row {
  display: flex;
  margin-bottom: 12rpx;
  
  &:last-child {
    margin-bottom: 0;
  }
}

.data-label {
  font-weight: bold;
  width: 120rpx;
  color: #666;
}

.data-value {
  flex: 1;
  color: #333;
}

/* Toast Message Styles */
.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 16rpx;
}

.toast-button {
  flex: 1;
  min-width: 200rpx;
  padding: 20rpx;
  border-radius: 8rpx;
  text-align: center;
  
  &.success {
    background-color: #4cd964;
    color: white;
  }
  
  &.error {
    background-color: #ff3b30;
    color: white;
  }
  
  &.info {
    background-color: #5856d6;
    color: white;
  }
}

/* Storage Styles */
.storage-controls {
  display: flex;
  flex-direction: column;
  gap: 16rpx;
}

.storage-input {
  border: 1rpx solid #ddd;
  border-radius: 8rpx;
  padding: 20rpx;
  font-size: 28rpx;
}

.storage-buttons {
  display: flex;
  gap: 16rpx;
}

.storage-button {
  flex: 1;
  padding: 20rpx;
  border-radius: 8rpx;
  text-align: center;
  
  &.save {
    background-color: #34c759;
    color: white;
  }
  
  &.load {
    background-color: #007AFF;
    color: white;
  }
  
  &.clear {
    background-color: #ff3b30;
    color: white;
  }
}

.storage-value {
  font-size: 28rpx;
  color: #333;
  word-break: break-all;
}

/* Image Upload Styles */
.upload-button {
  width: 100%;
  padding: 20rpx;
  background-color: #5856d6;
  color: white;
  border-radius: 8rpx;
  text-align: center;
  margin-bottom: 16rpx;
}

.image-preview {
  margin-top: 16rpx;
}

.preview-image {
  width: 100%;
  height: 300rpx;
  border-radius: 8rpx;
  background-color: #f9f9f9;
}

.image-info {
  display: block;
  margin-top: 8rpx;
  font-size: 26rpx;
  color: #666;
  text-align: center;
}

/* Responsive adjustments */
@media (min-width: 768px) {
  .button-group {
    flex-wrap: nowrap;
  }
  
  .storage-buttons {
    flex-wrap: nowrap;
  }
}
</style>