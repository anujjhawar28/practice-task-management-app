<script setup>
import {computed} from "vue";

const props = defineProps({
  modelValue: String,
  tasks: Array
})
const emit = defineEmits(["update:modelValue"])
const filters = ['all', 'completed', 'pending']

const allCount = computed(() => props.tasks.length)
const completedCount = computed(() => props.tasks.filter(f => f.completed).length)
</script>

<template>
  <div class="filter-bar">
    <button v-for="f in filters" :key="f" :class="{ active: f === modelValue }" @click="$emit('update:modelValue', f)">
      {{ f }} 
      <span v-if="f === 'all'"> ({{allCount}}) </span>
      <span v-else-if="f === 'completed'"> ({{completedCount}}) </span>
      <span v-else> ({{allCount - completedCount}}) </span>
    </button>
  </div>
</template>

<style scoped>
.filter-bar {
  margin: 15px 0px;
  display: flex;
  justify-content: center;
  gap: 10px;
}

button {
  padding: 6px 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f5f5f5;
  cursor: pointer;
}

button.active {
  background-color: #0077cc;
  color: white;
}

button:hover {
  background-color: #0087cc;
  color: white;
}
</style>