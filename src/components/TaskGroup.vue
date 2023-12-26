<template>
  <div class="py-3">
    <h2 class="text-2xl font-bold mb-2">{{ title }}</h2>
    <div v-for="task in tasks" :key="task.id">
      <div class="flex items-center justify-between select-none"
        :class="{ 'line-through': task.completed, 'text-gray-500': task.completed }">
        <div class="flex items-center gap-3">
          <input :id="task.id" type="checkbox" v-model="task.completed" @change="update(task)"
            class="w-5 h-5 text-[#89d7ad] bg-gray-100 border-gray-300 focus:ring-blue-500 focus:ring-offset-2 focus:ring-2 outline-none">
          <label :for="task.id" class="text-xl">{{ task.title }}</label>
        </div>
        <div class="flex items-center gap-3 text-xl">
          <span>{{ task.date }}</span>
          <span>{{ task.time }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  tasks: Array,
  title: String
});

const emit = defineEmits(['updateTask']);

const update = (task) => {
  emit('updateTask', { ...task });
};



</script>