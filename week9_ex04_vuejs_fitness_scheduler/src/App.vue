<template>
  <div class="app-container">
    <header class="app-header">
      <h1>🏋️‍♂️ LCA Fitness Class Scheduler</h1>
      <p>Manage your daily sessions and client bookings seamlessly.</p>
    </header>
    
    <main class="app-content">
      <section class="form-section">
        <h2>Schedule a New Class</h2>
        <ClassForm @add-class="handleAddClass" />
      </section>

      <section class="list-section">
        <h2>Current Class Schedule</h2>
        
        <div v-if="fitnessClasses.length === 0" class="alert-banner">
          <p>No fitness sessions scheduled yet. Fill out the form to create your first class!</p>
        </div>

        <div v-else class="classes-grid">
          <ClassCard 
            v-for="item in fitnessClasses" 
            :key="item.id" 
            :classItem="item"
            @book-spot="handleBookSpot"
            @delete-class="handleDeleteClass"
          />
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ClassForm from './components/ClassForm.vue'
import ClassCard from './components/ClassCard.vue'

// Core state array holding all fitness classes
const fitnessClasses = ref([])

// Adds a new class to the schedule array
const handleAddClass = (newClass) => {
  fitnessClasses.value.push({
    id: Date.now(),
    ...newClass,
    bookedSpots: 0 // New classes always start with 0 bookings
  })
}

// Increments the booked spots if capacity permits
const handleBookSpot = (classId) => {
  const targetClass = fitnessClasses.value.find(c => c.id === classId)
  if (targetClass && targetClass.bookedSpots < targetClass.capacity) {
    targetClass.bookedSpots++
  }
}

// Removes a class from the list completely
const handleDeleteClass = (classId) => {
  fitnessClasses.value = fitnessClasses.value.filter(c => c.id !== classId)
}
</script>

<style>
/* Global Layout Styles */
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f4f6f9;
  color: #333;
}
.app-container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 40px 20px;
}
.app-header {
  text-align: center;
  margin-bottom: 40px;
}
.app-header h1 {
  color: #1d3557;
  margin-bottom: 5px;
}
.app-header p {
  color: #666;
  margin: 0;
}
.app-content {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 40px;
}
@media (max-width: 768px) {
  .app-content {
    grid-template-columns: 1fr;
  }
}
h2 {
  color: #1d3557;
  border-bottom: 2px solid #e2e8f0;
  padding-bottom: 10px;
  margin-top: 0;
}
.alert-banner {
  background-color: #e2e8f0;
  border-left: 4px solid #64748b;
  padding: 15px;
  border-radius: 4px;
  color: #475569;
}
.classes-grid {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
</style>