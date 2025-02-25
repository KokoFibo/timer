<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const time = ref(new Date().toLocaleTimeString());

let interval;

onMounted(() => {
  interval = setInterval(() => {
    time.value = new Date().toLocaleTimeString();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<template>
  <div
    class="text-7xl font-mono p-4 bg-gray-800 text-white rounded-lg shadow-lg"
  >
    {{ time }}
  </div>
</template>
