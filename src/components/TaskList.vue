<script setup>
import TaskItem from "./TaskItem.vue"
defineProps({
  tasks: {
    type: Array,
    default: () => []
  }
})

const emit = defineEmits(["delete-task", 'toggle-task'])

const deleteTask = (id) => {
  emit('delete-task', id)
}
</script>

<template>
  <div>
    <p v-if="!tasks.length" class="empty">No tasks found!</p>
    <div v-for="task in tasks" :key="task.id">
      <TaskItem :task="task" @delete-task="deleteTask(task.id)" @toggle-task="$emit('toggle-task', task.id)"/>
    </div>
  </div>
</template>

<style scoped>
.empty {
  text-align: center;
  color: #777;
  font-style: italic;
}
</style>
