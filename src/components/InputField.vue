<template>
  <div class="flex flex-col sm:flex-row gap-2">

    <input type="text" placeholder="Yangi vazifani qo'shish" v-model="newTaskTitle" @keyup.enter="emitNewTask"
      class="w-full text-xl h-12 py-[10px] px-[15px] outline-none border-[3px] border-[#b0b0b0] focus:border-[#616161] rounded-lg text-[#616161]">

    <button @click="emitNewTask"
      class="w-full sm:w-12 h-12 flex items-center justify-center text-3xl font-bold border-[3px] border-[#51af7c] bg-[#89d7ad] outline-none rounded-lg select-none active:bg-[#89d7adaa]">
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M12 4v16m8-8H4"></path>
      </svg>
    </button>

  </div>
</template>
<script setup>
import { ref } from 'vue'

const newTaskTitle = ref('');
const newTaskDeadline = ref('bugun');

const emit = defineEmits(['newTask']);

const emitNewTask = () => {
  if (newTaskTitle.value.trim()) {
    const newTaskData = {}
    if (newTaskTitle.value.toLowerCase().includes('bugun')) {
      newTaskData.title = newTaskTitle.value.replace('bugun', '')
      newTaskData.deadline = 'bugun'
    } else if (newTaskTitle.value.toLowerCase().includes('ertaga')) {
      newTaskData.title = newTaskTitle.value.replace('ertaga', '')
      newTaskData.deadline = 'ertaga'
    }
    console.log(newTaskData)
    emit('newTask', newTaskData);
    newTaskTitle.value = '';
    newTaskDeadline.value = 'bugun';
  }
};


</script>
