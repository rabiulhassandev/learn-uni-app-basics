<template>
  
  <view>
    <text>
      Hello, this is the index page!
    </text>
    <button @click="goToAbout">Go To About Page</button>

    <p>
      Lets work with Data: 
    </p>
    <button @click="goToCounter">Go To Counter Page</button>
  </view>
</template>

<script setup lang="ts">

  // go to about page
  const goToAbout = () => {
    uni.navigateTo({
      url: '/pages/index/about'
    })
  }

  // go to counter page
  const goToCounter = () => {
    uni.navigateTo({
      url: '/pages/index/counter'
    })
  }

</script>