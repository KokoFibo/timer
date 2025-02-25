<template>
  <button class="glowing-button" @click="handleClick">
    <slot></slot>
  </button>
</template>

<script setup>
// const handleClick = () => {
//   alert("Button clicked!");
// };
</script>

<style scoped>
.glowing-button {
  position: relative;
  padding: 15px 30px;
  font-size: 18px;
  color: #fff;
  background-color: #0275d8;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  overflow: hidden;
  box-shadow: 0 0 10px #0275d8, 0 0 20px #0275d8, 0 0 30px #0275d8,
    0 0 40px #0275d8;
  transition: all 0.3s ease;
}

.glowing-button:hover {
  background-color: #5bc0de;
  box-shadow: 0 0 15px #5bc0de, 0 0 25px #5bc0de, 0 0 35px #5bc0de,
    0 0 45px #5bc0de;
  transform: scale(1.1);
}

.glowing-button::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.3);
  transform: scale(0);
  transition: transform 0.5s ease;
}

.glowing-button:hover::before {
  transform: scale(2);
  opacity: 0;
}
</style>
