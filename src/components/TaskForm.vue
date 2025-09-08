<script setup>
import { ref } from 'vue';

const title = ref()
const priority = ref("Medium")
const dueDate = ref()
const description = ref()
const subtasks = ref([""])
const statuses = ref(["High", "Medium", "Low"])

const emit = defineEmits(["add-task"])
const submitForm = () => {
  if (!title.value) return
  emit('add-task', {
    title: title.value,
    description: description.value,
    dueDate: dueDate.value,
    subtasks: subtasks.value.filter((s) => s.trim()),
    priority: priority.value
  })
  resetForm();
}

// Resetting Form
const resetForm = () => {
  title.value = "";
  description.value = "";
  priority.value = "Medium";
  dueDate.value = "";
  subtasks.value = [""]
}

const removeSubtask = (i) => {
  if (subtasks.value[i] && subtasks.value[i].trim() !== "") {
    subtasks.value.splice(i, 1)
  }
}

const addSubtask = () => {
  const last = subtasks.value[subtasks.value.length - 1];
  if (last && last.trim() !== "") {
    subtasks.value.push("")
  }
}
</script>

<template>
  <form class="task-form" @submit.prevent="submitForm">
    <input v-model="title" placeholder="Task title" required />
    <textarea v-model="description" placeholder="Description" />

    <div class="form-row">
      <div class="priority">
        <label>Priority: </label>
        <select v-model="priority">
          <option v-for="(status, index) in statuses" :key="index">{{ status }}</option>
        </select>
      </div>

      <div class="due">
        <label>Due: </label>
        <input type="date" v-model="dueDate" />

      </div>
    </div>

    <div class="subtasks">
      <label>Subtasks: </label>
      <div class="subtask" v-for="(sub, index) in subtasks" key="index">
        <input v-model="subtasks[index]" placeholder="Enter subtask" />
        <button type="button" @click="removeSubtask(index)" v-if="subtasks[index].trim()">X</button>
      </div>
      <button type="button" @click="addSubtask">+ Add Subtask</button>
    </div>

    <button type="submit">Add Task</button>
  </form>
</template>

<style scoped>
.task-form {
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 5px;
  margin-bottom: 20px;
  background: #fafafa;
}

.task-form input,
.task-form textarea,
.task-form select {
  width: 100%;
  margin-bottom: 10px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.form-row {
  display: flex;
  gap: 10px;
  align-items: center;
}

.priority,
.due {
  display: flex;
  align-items: center;
  width: 100%;
  gap: 5px;
}

.subtasks {
  margin-bottom: 10px;
}

.subtask {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-bottom: 5px;
}

button {
  cursor: pointer;
  padding: 10px 10px;
  border-radius: 5px;
  border: none;
  background-color: #0077cc;
  color: #fafafa;
}

button:hover {
  background-color: #005fa3;
}
</style>
