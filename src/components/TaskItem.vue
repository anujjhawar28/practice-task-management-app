<script setup>
import { ref } from "vue"

defineProps({
  task: {
    type: Object,
  }
})

const emit = defineEmits(['toggle-task', 'delete-task'])
const showDetails = ref(false)
</script>

<template>
  <div class="task-item">
    <div class="header">
      <div class="flex">
        <input type="checkbox" :checkbox="task.completed" @change="emit('toggle-task')">
        <span :class="{ done: task.completed }">{{ task.title }}</span>
        <span class="badge" :class="task.priority.toLowerCase()">{{ task.priority }}</span>
      </div>

      <div class="flex">
        <button @click="showDetails = !showDetails">{{showDetails ? "Hide" : "Show"}}</button>
        <button @click="emit('delete-task')">Delete</button>
      </div>
    </div>

    <div v-if="showDetails" class="details">
      <p><strong>Description: </strong> {{task.description || '-'}}</p>
      <p><strong>Due: </strong> {{task.dueDate || 'No Date'}}</p>
      <ul>
        <li v-for="(sub, i) in task.subtasks" :key="i">
          {{sub}}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.task-item {
  border: 1px solid #ddd;
  padding: 10px;
  margin: 8px 0px;
  border-radius: 5px;
  background-color: white;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}

.flex {
  display: flex;
  align-items: center;
  gap: 10px;
}

.done {
  text-decoration: line-through;
  color: #666
}

.badge {
  padding: 2px 5px;
  border-radius: 4px;
  font-size: 12px;
  color: white;
}

.badge.high {
  background-color: red;
}
.badge.medium {
  background-color: orange;
}
.badge.low {
  background-color: green;
}
.details {
  margin-top: 8px;
  font-size: 14px;
  color: #444;
}
button {
  background-color: #ccc;
  border: none;
  border-radius: 4px;
  padding: 4px 8px;
  cursor: pointer;
}
button:hover {
  background-color: #aaa;
}
</style>
