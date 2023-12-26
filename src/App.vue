
<template>
  <div class="max-w-3xl mx-auto py-10 px-2">
    <h1 class="text-3xl sm:text-4xl font-bold text-center mb-12">Vazifalar Menedjeri</h1>
    <InputField @newTask="addTask" />
    <CurrentTime />
    <div>
      <TaskGroup :tasks="tasksBugun" @updateTask="handleUpdate" title="Bugun" />
      <TaskGroup :tasks="tasksErtaga" @updateTask="handleUpdate" title="Ertaga" />
      <TaskGroup :tasks="tasksKeyin" @updateTask="handleUpdate" title="Keyin" />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import InputField from './components/InputField.vue';
import CurrentTime from './components/CurrentTime.vue';
import TaskGroup from './components/TaskGroup.vue';

const tasks = ref([
  { id: 1, title: "Buy groceries", deadline: "bugun", completed: false, date: '12.27.2023', time: '12:00' },
  { id: 2, title: "Buy asdf", deadline: "ertaga", completed: false, date: '12.27.2023', time: '12:00' },
  { id: 3, title: "Buy asdfwr234", deadline: "bugun", completed: false, date: '12.27.2023', time: '12:00' },
  { id: 3, title: "Buy groceries", deadline: "keyin", completed: false, date: '12.27.2023', time: '12:00' },
])

function handleUpdate(taskUpd) {
  const index = tasks.value.findIndex(task => task.id == taskUpd.id)

  if (index != -1) {
    tasks.value[index] = taskUpd
  }
}

const addTask = (addedTask) => {
  const newTask = {
    id: tasks.value.length + 1,
    title: addedTask.title,
    deadline: addedTask.deadline,
    completed: false
  };
  tasks.value.push(newTask);
};

const sortChecks = (tasks) => {
  return tasks.sort((a, b) => a.completed - b.completed);
};

const tasksBugun = computed(() => sortChecks(tasks.value.filter(task => task.deadline === 'bugun')));
const tasksErtaga = computed(() => sortChecks(tasks.value.filter(task => task.deadline === 'ertaga')));
const tasksKeyin = computed(() => sortChecks(tasks.value.filter(task => task.deadline === 'keyin')));


</script>