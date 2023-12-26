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
const newTaskDate = ref('');
const newTaskTime = ref('');
const newTaskDeadline = ref('bugun');

const emit = defineEmits(['newTask']);

const timePattern = /\b(?:[0-1]?\d|2[0-3]):[0-5]\d\b/
const datePattern = /\b(0[1-9]|[12][0-9]|3[01])\.(0[1-9]|1[0-2])\.\d{4}\b/

const emitNewTask = () => {
  if (newTaskTitle.value.trim()) {
    const newTaskData = {}

    let tempTitle = newTaskTitle.value;

    if (newTaskTitle.value.toLowerCase().includes('bugun')) {
      newTaskData.deadline = 'bugun'
      newTaskTime.value = ('0' + (new Date().getHours() + 1) % 24).slice(-2) + ':00'
      newTaskData.time = newTaskTime.value
      tempTitle = tempTitle.replace('bugun', '');
      tempTitle = tempTitle.replace('Bugun', '');
    } else if (newTaskTitle.value.toLowerCase().includes('ertaga')) {
      tempTitle = tempTitle.replace('ertaga', '');
      tempTitle = tempTitle.replace('Ertaga', '');
      newTaskData.deadline = 'ertaga'
      newTaskTime.value = '09:00'
      newTaskData.time = newTaskTime.value
    } else {
      newTaskData.deadline = newTaskDeadline.value
      newTaskTime.value = ('0' + (new Date().getHours() + 1) % 24).slice(-2) + ':00'
      newTaskData.time = newTaskTime.value
    }

    const dateMatch = tempTitle.match(datePattern);
    if (dateMatch) {
      newTaskDate.value = dateMatch[0];
      tempTitle = tempTitle.replace(datePattern, '').trim();
      newTaskData.deadline = 'keyin'
      newTaskTime.value = '09:00'
      newTaskData.time = newTaskTime.value
    }
    newTaskData.date = newTaskDate.value

    const timeMatch = tempTitle.match(timePattern);
    if (timeMatch) {
      newTaskTime.value = timeMatch[0];
      tempTitle = tempTitle.replace(timePattern, '').trim();
    }
    newTaskData.time = newTaskTime.value

    newTaskData.title = tempTitle.trim();


    emit('newTask', newTaskData);
    newTaskTitle.value = '';
    newTaskDeadline.value = 'bugun';
    newTaskTime.value = '';
    newTaskDate.value = '';
  }
};


</script>
