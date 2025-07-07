<template>
  
  <view>
    <text>
      Hello, this is the index page!
    </text>
    <button @click="goToAbout">Go To About Page</button>
  </view>
</template>

<script setup lang="ts">

  // go to about page
  const goToAbout = () => {
    uni.navigateTo({
      url: '/pages/index/about'
    })
  }

</script>