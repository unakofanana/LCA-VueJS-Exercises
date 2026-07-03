<template>
  <div class="class-card" :class="{ 'fully-booked': isFull }">
    <div class="card-info">
      <h3>{{ classItem.name }}</h3>
      <p>👤 <strong>Instructor:</strong> {{ classItem.instructor }}</p>
      <p>⏰ <strong>Time:</strong> {{ classItem.time }}</p>
      <p>🎟️ <strong>Booked Status:</strong> 
        <span class="status-badge" :class="{ 'danger': isFull }">
          {{ classItem.bookedSpots }} / {{ classItem.capacity }} Slots Filled
        </span>
      </p>
    </div>
    
    <div class="card-actions">
      <button 
        @click="emit('book-spot', classItem.id)" 
        :disabled="isFull"
        class="btn-book"
      >
        {{ isFull ? 'Fully Booked' : 'Book a Spot' }}
      </button>
      
      <button @click="emit('delete-class', classItem.id)" class="btn-cancel">
        Remove
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  classItem: Object
})

const emit = defineEmits(['book-spot', 'delete-class'])

// Computed utility to check if class capacity has been reached
const isFull = computed(() => {
  return props.classItem.bookedSpots >= props.classItem.capacity
})
</script>

<style scoped>
.class-card {
  background: white;
  border-left: 5px solid #457b9d;
  padding: 20px;
  border-radius: 6px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.2s;
}
.class-card.fully-booked {
  border-left-color: #e63946;
  background-color: #fef2f2;
}
.card-info h3 {
  margin: 0 0 10px 0;
  color: #1d3557;
}
.card-info p {
  margin: 5px 0;
  font-size: 14px;
  color: #4a5568;
}
.status-badge {
  background: #e2e8f0;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: bold;
}
.status-badge.danger {
  background: #fecaca;
  color: #dc2626;
}
.card-actions {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
button {
  padding: 8px 16px;
  font-weight: bold;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  font-size: 13px;
}
.btn-book {
  background-color: #2a9d8f;
  color: white;
}
.btn-book:disabled {
  background-color: #cbd5e1;
  color: #94a3b8;
  cursor: not-allowed;
}
.btn-cancel {
  background-color: #e63946;
  color: white;
}
.btn-cancel:hover {
  background-color: #b91c1c;
}
</style>