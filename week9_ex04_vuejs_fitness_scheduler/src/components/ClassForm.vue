<template>
  <form @submit.prevent="submitForm" class="form-container">
    <div class="form-group">
      <label>Class Name</label>
      <input v-model="className" type="text" placeholder="e.g., Boxing Basics" required />
    </div>

    <div class="form-group">
      <label>Instructor / Coach</label>
      <input v-model="instructor" type="text" placeholder="e.g., Coach Ashley" required />
    </div>

    <div class="form-group">
      <label>Start Time</label>
      <input v-model="classTime" type="time" required />
    </div>

    <div class="form-group">
      <label>Maximum Capacity</label>
      <input v-model="capacity" type="number" min="1" required />
    </div>
    
    <button type="submit" class="btn-submit">Add to Schedule</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-class'])

const className = ref('')
const instructor = ref('')
const classTime = ref('')
const capacity = ref(12)

const submitForm = () => {
  if (!className.value || !instructor.value || !classTime.value) return

  // Package data and emit to parent
  emit('add-class', {
    name: className.value,
    instructor: instructor.value,
    time: classTime.value,
    capacity: Number(capacity.value)
  })

  // Reset form inputs
  className.value = ''
  instructor.value = ''
  classTime.value = ''
  capacity.value = 12
}
</script>

<style scoped>
.form-container {
  background: white;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}
.form-group {
  margin-bottom: 15px;
}
.form-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 5px;
  font-size: 14px;
  color: #4a5568;
}
input {
  width: 100%;
  padding: 10px;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  box-sizing: border-box;
  font-size: 14px;
}
input:focus {
  outline: none;
  border-color: #457b9d;
}
.btn-submit {
  width: 100%;
  background-color: #457b9d;
  color: white;
  border: none;
  padding: 12px;
  font-weight: bold;
  font-size: 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}
.btn-submit:hover {
  background-color: #1d3557;
}
</style>