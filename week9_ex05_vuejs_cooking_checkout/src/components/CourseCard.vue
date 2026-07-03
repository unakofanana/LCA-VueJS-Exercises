<script setup>
defineProps({
  course: {
    type: Object,
    required: true
  }
})

defineEmits(['add-to-cart'])
</script>

<template>
  <div class="course-card-box" :class="{ 'is-sold-out': course.soldOut }">
    <div class="card-hero-row">
      <span class="course-emoji-art">{{ course.image }}</span>
      <span v-if="course.soldOut" class="sold-out-pill">Sold Out</span>
      <span v-else class="duration-badge">{{ course.duration }}</span>
    </div>

    <div class="card-body-details">
      <h3>{{ course.title }}</h3>
      <p class="course-description">{{ course.description }}</p>
    </div>

    <div class="card-footer-action-row">
      <span class="course-rate-label">R {{ course.price.toLocaleString() }}</span>
      <button 
        class="add-action-btn"
        :disabled="course.soldOut"
        @click="$emit('add-to-cart', course)"
      >
        {{ course.soldOut ? 'Unavailable' : 'Add to Cart' }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.course-card-box {
  background: var(--panel-bg);
  border: 1px solid var(--border-color);
  border-radius: 12px;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.course-card-box:hover:not(.is-sold-out) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(15, 23, 42, 0.08);
}

.course-card-box.is-sold-out {
  opacity: 0.6;
  background-color: #f1f5f9;
}

.card-hero-row {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
}

.course-emoji-art {
  font-size: 2.5rem;
}

.duration-badge {
  background: #f1f5f9;
  color: var(--text-dark);
  font-size: 0.75rem;
  font-weight: 600;
  padding: 0.35rem 0.65rem;
  border-radius: 20px;
}

.sold-out-pill {
  background: var(--accent-red);
  color: white;
  font-size: 0.75rem;
  font-weight: 700;
  padding: 0.35rem 0.65rem;
  border-radius: 20px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.card-body-details h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.15rem;
  font-weight: 700;
  line-height: 1.4;
}

.course-description {
  margin: 0;
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.5;
  margin-bottom: 1.5rem;
}

.card-footer-action-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid var(--border-color);
  padding-top: 1rem;
  margin-top: auto;
}

.course-rate-label {
  font-size: 1.25rem;
  font-weight: 800;
  color: var(--text-dark);
}

.add-action-btn {
  background-color: var(--primary);
  color: white;
  border: none;
  padding: 0.6rem 1.1rem;
  border-radius: 6px;
  font-weight: 600;
  font-size: 0.85rem;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.add-action-btn:hover:not(:disabled) {
  background-color: #0f172a;
}

.add-action-btn:disabled {
  background-color: var(--text-muted);
  cursor: not-allowed;
}
</style>