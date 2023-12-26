
<template>
  <div class="max-w-3xl mx-auto py-10 px-2">
    <h1 class="text-3xl sm:text-4xl font-bold text-center mb-12">Vazifalar Menedjeri</h1>
    <InputField @newTask="addTask" />
    <CurrentTime />
    <div v-if="tasks.length">
      <TaskGroup v-if="tasksBugun.length" :tasks="tasksBugun" title="Bugun" />
      <TaskGroup v-if="tasksErtaga.length" :tasks="tasksErtaga" title="Ertaga" />
      <TaskGroup v-if="tasksKeyin.length" :tasks="tasksKeyin" title="Keyin" />
    </div>
    <div v-if="tasks.length" class="w-full h-[1px] bg-gray-300 my-8"></div>
    <div v-if="tasks.length" class="text-right my-4 text-gray-500 italic text-lg">
      <p>Bajarilganlar: {{ tasksDone }}</p>
      <p>Bajarilmaganlar: {{ tasksNotDone }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import InputField from './components/InputField.vue';
import CurrentTime from './components/CurrentTime.vue';
import TaskGroup from './components/TaskGroup.vue';

const tasks = ref([
  // { id: 1, title: "Buy groceries", deadline: "bugun", completed: false, date: '12.27.2023', time: '12:00' },
  // { id: 2, title: "Buy asdf", deadline: "ertaga", completed: true, date: '12.27.2023', time: '12:00' },
  // { id: 3, title: "Buy asdfwr234", deadline: "bugun", completed: false, date: '12.27.2023', time: '12:00' },
  // { id: 4, title: "Buy groceries", deadline: "keyin", completed: false, date: '12.27.2023', time: '12:00' },
])

const addTask = (addedTask) => {
  const newTask = {
    id: tasks.value.length + 1,
    title: addedTask.title,
    deadline: addedTask.deadline,
    time: addedTask.time,
    date: addedTask.date,
    completed: false
  };
  tasks.value.push(newTask);
};

const sortChecks = (tasks) => {
  return tasks.sort((a, b) => a.completed - b.completed);
};

const tasksBugun = computed(() => sortChecks(tasks.value.filter(task => task.deadline == 'bugun')));
const tasksErtaga = computed(() => sortChecks(tasks.value.filter(task => task.deadline == 'ertaga')));
const tasksKeyin = computed(() => sortChecks(tasks.value.filter(task => task.deadline == 'keyin')));
const tasksDone = computed(() => tasks.value.filter(task => task.completed == true).length);
const tasksNotDone = computed(() => tasks.value.length - tasks.value.filter(task => task.completed == true).length);
</script>