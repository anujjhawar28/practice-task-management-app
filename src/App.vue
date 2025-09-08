<script setup>
import { ref, computed, watch, onMounted } from 'vue';
import TaskForm from './components/TaskForm.vue';
import TaskList from "./components/TaskList.vue";
import FilterBar from "./components/FilterBar.vue";
import SearchBar from './components/SearchBar.vue';

const tasks = ref([])
const filter = ref("all")
const searchQuery = ref()
const addTask = (task) => {
  tasks.value.push({
    id: Date.now(),
    ...task,
    completed: false
  })
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

const toggleTask = (id) => {
  const task = tasks.value.find(t => t.id === id);
  if(task) task.completed = !task.completed
}

const filterTasks = computed(() => {
  let result = tasks.value;

  if(filter.value === "completed") {
    result = result.filter(r => r.completed)
  }
  else if (filter.value === 'pending') {
    result = result.filter(r => !r.completed)
  }

  if(searchQuery.value?.trim()) {
    const q = searchQuery.value.toLowerCase();

    result = result.filter(t => {
      const iTitle = t.title?.toLowerCase().includes(q)
      const iDescription = t.description?.toLowerCase().includes(q)
      const iSubtasks = Array.isArray(t.subtasks) ? t.subtasks.some(s => s?.toLowerCase().includes(q)) : false

      return iTitle || iDescription | iSubtasks
    })
  }

  return result;
})

watch(tasks, () => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value))
}, {
  deep: true
})

onMounted(() => {
  const saved = localStorage.getItem("tasks")
  if(saved) {
    tasks.value = JSON.parse(saved)
  }
})
</script>

<template>
  <div class="app">
    <h1>Task Manager</h1>
    <TaskForm @add-task="addTask"/>

    <div class="toolbar">
      <SearchBar v-model="searchQuery"/>
      <FilterBar :tasks="tasks" v-model="filter"/>
    </div>

    <TaskList :tasks="filterTasks" @delete-task="deleteTask" @toggle-task="toggleTask" />

  </div>
</template>

<style scoped>
.app {
  max-width: 800px;
  margin: auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
h1 {
  text-align: center;
}
.toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  margin: 15px;
}
</style>