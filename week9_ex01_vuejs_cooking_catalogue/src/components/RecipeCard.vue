<script setup>
defineProps({
  recipe: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="recipe-render-card">
    <div class="card-image-wrapper">
      <img 
        :src="recipe.image || 'https://images.unsplash.com/photo-1495521821757-a1efb6729352?w=500'" 
        alt="Recipe Rendering"
        @error="$event.target.src = 'https://images.unsplash.com/photo-1495521821757-a1efb6729352?w=500'"
      />
    </div>

    <div class="card-content-body">
      <h3>{{ recipe.title }}</h3>
      <p class="recipe-desc-text">{{ recipe.description }}</p>

      <div class="ingredients-section">
        <h4>Ingredients:</h4>
        <div class="tags-container">
          <span v-for="(ingredient, index) in recipe.ingredients" :key="index" class="ingredient-tag">
            {{ ingredient }}
          </span>
        </div>
      </div>

      <div class="steps-section">
        <h4>Instructions:</h4>
        <p class="steps-text">{{ recipe.steps }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.recipe-render-card {
  background: var(--surface-bg);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
  border: 1px solid var(--border-element);
  display: flex;
  flex-direction: column;
}

.card-image-wrapper {
  width: 100%;
  height: 200px;
  background-color: #e2e8f0;
}

.card-image-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.card-content-body {
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.card-content-body h3 {
  margin: 0;
  font-size: 1.2rem;
  font-weight: 700;
  line-height: 1.3;
}

.recipe-desc-text {
  margin: 0;
  font-size: 0.9rem;
  color: var(--text-muted);
  line-height: 1.5;
}

.ingredients-section h4, .steps-section h4 {
  margin: 0 0 0.5rem 0;
  font-size: 0.85rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--text-muted);
}

.tags-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

.ingredient-tag {
  background: #f1f5f9;
  color: var(--text-primary);
  font-size: 0.8rem;
  font-weight: 600;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  border: 1px solid #e2e8f0;
}

.steps-text {
  margin: 0;
  font-size: 0.9rem;
  line-height: 1.5;
  background: #f8fafc;
  padding: 0.75rem;
  border-radius: 6px;
  border-left: 3px solid var(--brand-primary);
}
</style>