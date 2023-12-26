<template>
  <div class="my-2 text-center italic">
    <span class="text-gray-500">Hozirgi vaqt: {{ currentTime }}</span>
  </div>
</template>
<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const currentTime = ref(new Date().toLocaleString());
const updateTime = () => {
  setInterval(() => {
    currentTime.value = new Date().toLocaleString();
  }, 1000);
};

let interval;

onMounted(() => {
  interval = updateTime();
});

onBeforeUnmount(() => {
  clearInterval(interval);
});
</script>