<template>
    
    <h1>Counter Page</h1>



    <div style="text-align: center; margin-top: 20px;">
      <p>Counter: {{ counter }}</p>
      <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px;">
        <button @click="counter++" style="background: red; color: white">Increment</button>
        <button @click="counter--" style="background: green; color: white">Decrement</button>
      </div>
      <button @click="goBack" style="margin-top: 10px;">Go Back</button>
    </div>
</template>

<script setup lang="ts">
    import { ref } from 'vue';

    const counter = ref(0);


    // go back to previous page
    const goBack = () => {
        uni.navigateBack({
            delta: 1 // Go back one page
        });
    }
</script>