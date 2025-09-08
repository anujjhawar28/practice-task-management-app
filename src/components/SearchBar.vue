<script setup>
import { ref, watch } from "vue";
const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  }
})
const emit = defineEmits(["update:modelValue"])

const inputValue = ref(props.modelValue);
let debounceTimeout = null

watch(() => props.modelValue, (newVal) => {
  inputValue.value = newVal
})

watch(inputValue, (newVal) => {
  clearTimeout(debounceTimeout)
  debounceTimeout = setTimeout(() => {
    emit("update:modelValue", newVal)
  }, 300)
})
</script>

<template>
  <input placeholder="Search tasks...." class="search-input" v-model="inputValue" type="text"/>
</template>

<style scoped>
.search-input {
  flex: 1;
  padding: 8px 12px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 50% !important;
}
</style>