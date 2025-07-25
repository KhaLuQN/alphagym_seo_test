<template>
  <div>
    <h1>Test SSR</h1>
    <p>{{ now }}</p>
  </div>
</template>

<script setup>
const now = new Date().toString();
</script>
