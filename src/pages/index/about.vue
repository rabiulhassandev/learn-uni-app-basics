<template>
    
    <h1>About Us Page</h1>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere at architecto officiis consequuntur fuga minus?</p>

    <button @click="goBack">Go Back</button>
</template>

<script setup lang="ts">
    // go back to previous page
    const goBack = () => {
        uni.navigateBack({
            delta: 1 // Go back one page
        });
    }
</script>