<template>
  <div
    class="flex flex-col items-center justify-center min-h-screen transition-colors duration-300"
    :class="{
      'bg-gray-900 text-gray-100': isDark,
      'bg-white text-gray-900': !isDark,
    }"
  >
    <!-- Theme Switcher Button -->
    <!-- <button
      @click="cycleTheme"
      class="absolute top-4 right-4 p-2 rounded-full hover:bg-opacity-20 transition-colors"
      :class="{ 'hover:bg-white': isDark, 'hover:bg-black': !isDark }"
    >
      <SunIcon v-if="theme === 'light'" class="w-6 h-6" />
      <MoonIcon v-else-if="theme === 'dark'" class="w-6 h-6" />
      <ComputerDesktopIcon v-else class="w-6 h-6" />
    </button> -->
    <!-- Theme Switcher Button -->
    <button
      @click="cycleTheme"
      class="fixed top-4 right-4 p-3 rounded-full transition-colors duration-300 shadow-lg z-50"
      :class="{
        'bg-gray-800 hover:bg-opacity-80': isDark,
        'bg-white hover:bg-opacity-80': !isDark,
      }"
    >
      <SunIcon v-if="theme === 'light'" class="w-6 h-6 sm:w-8 sm:h-8" />
      <MoonIcon v-else-if="theme === 'dark'" class="w-6 h-6 sm:w-8 sm:h-8" />
      <ComputerDesktopIcon v-else class="w-6 h-6 sm:w-8 sm:h-8" />
    </button>
    <div class="mb-5">
      <Clock :is-dark="isDark" />
    </div>
    <h1 class="text-2xl font-bold mb-4 text-center">Countdown Timer</h1>
    <!-- Dropdown untuk memilih menit -->
    <select
      v-model="selectedMinutes"
      class="p-2 mb-4 border rounded transition-colors w-full max-w-xs"
      :class="{
        'bg-gray-800 border-gray-700': isDark,
        'bg-white border-gray-300': !isDark,
      }"
    >
      <option v-for="minute in availableMinutes" :key="minute" :value="minute">
        {{ minute }} menit
      </option>
    </select>
    <div class="mt-5">
      <GlowButton v-if="isDark" :is-dark="isDark" @click="startTimer"
        >Start</GlowButton
      >
      <button
        v-else
        class="bg-blue-500 text-white px-8 py-4 text-xl font-semibold rounded-xl shadow-xl transition-all duration-300 ease-in-out transform hover:bg-blue-600 hover:scale-105 active:scale-95 w-full max-w-xs"
        @click="startTimer"
      >
        Start
      </button>
    </div>
    <!-- Tampilan Countdown -->
    <div
      v-if="remainingTime > 0"
      class="text-[100px] sm:text-[200px] font-bold text-center"
    >
      {{ formatTime(remainingTime) }}
    </div>
    <!-- Pesan saat timer selesai -->
    <div
      v-else-if="remainingTime === 0"
      class="text-[100px] sm:text-[200px] font-bold text-green-600 text-center"
    >
      Timer selesai!
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from "vue";
import {
  SunIcon,
  MoonIcon,
  ComputerDesktopIcon,
} from "@heroicons/vue/24/outline";
import Clock from "./components/Clock.vue";
import GlowButton from "./components/GlowButton.vue";

// Theme logic
const theme = ref("auto"); // Set default to "auto"
const isDark = ref(false);

const updateDarkMode = () => {
  const currentHour = new Date().getHours();
  const isSystemDark = window.matchMedia(
    "(prefers-color-scheme: dark)"
  ).matches;
  if (theme.value === "auto") {
    // Set dark mode between 18:00 and 06:00 based on local time
    isDark.value = currentHour >= 18 || currentHour < 6;
    document.documentElement.classList.toggle("dark", isDark.value);
  } else {
    isDark.value = theme.value === "dark";
    document.documentElement.classList.toggle("dark", isDark.value);
  }
};

const cycleTheme = () => {
  theme.value = {
    auto: "light",
    light: "dark",
    dark: "auto",
  }[theme.value];
  localStorage.setItem("theme", theme.value);
  updateDarkMode();
};

// Initialize theme
onMounted(() => {
  const savedTheme = localStorage.getItem("theme") || "auto"; // Default to "auto"
  theme.value = savedTheme;
  updateDarkMode();
  // Listen for system theme changes only if the theme is set to "auto"
  window
    .matchMedia("(prefers-color-scheme: dark)")
    .addEventListener("change", (e) => {
      if (theme.value === "auto") updateDarkMode();
    });
});

// Timer logic
const availableMinutes = [1, 2, 5, 10, 15, 30, 60];
const selectedMinutes = ref(availableMinutes[5]);
const remainingTime = ref(0);
let timerInterval = null;

const startTimer = () => {
  if (timerInterval) clearInterval(timerInterval);
  remainingTime.value = selectedMinutes.value * 60;
  timerInterval = setInterval(() => {
    if (remainingTime.value > 0) {
      remainingTime.value--;
    } else {
      clearInterval(timerInterval);
    }
  }, 1000);
};

const formatTime = (time) => {
  const minutes = Math.floor(time / 60);
  const seconds = time % 60;
  return `${String(minutes).padStart(2, "0")}:${String(seconds).padStart(
    2,
    "0"
  )}`;
};
</script>

<style>
/* Add responsive styles */
@media (max-width: 640px) {
  h1 {
    font-size: 1.5rem; /* Reduce font size for smaller screens */
  }
  select {
    width: 100%; /* Make dropdown full width on mobile */
  }
  button {
    width: 100%; /* Make buttons full width on mobile */
  }
}
</style>
