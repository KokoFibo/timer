<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const time = ref("");

let interval;

const updateTime = () => {
  time.value = new Date().toLocaleTimeString([], { hour12: false });
};

onMounted(() => {
  updateTime(); // Set awal waktu
  interval = setInterval(updateTime, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<template>
  <div
    class="whitespace-nowrap text-4xl sm:text-7xl font-mono p-4 rounded-lg shadow-lg w-full max-w-xs sm:max-w-md mx-auto"
  >
    {{ time }}
  </div>
</template>
