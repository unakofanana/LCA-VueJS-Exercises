<script setup>
import { ref } from 'vue'
import RecipeCard from './components/RecipeCard.vue'

// Form Fields State
const title = ref('')
const description = ref('')
const ingredients = ref('')
const steps = ref('')
const imageUrl = ref('')

// Pre-seeded Recipe List with a reliable live image link
const recipes = ref([
  {
    id: 1,
    title: 'Classic Garlic Butter Pizza',
    description: 'A crispy, golden homemade crust brushed with aromatic garlic butter and loaded with melted mozzarella cheese.',
    ingredients: ['Pizza dough', 'Garlic butter', 'Mozzarella cheese', 'Fresh basil'],
    steps: 'Roll out dough, spread garlic butter, add cheese, and bake at 220°C for 12 minutes.',
    image: 'https://images.unsplash.com/photo-1513104890138-7c749659a591?w=600&auto=format&fit=crop&q=80'
  }
])

// Submit Handler
const handleSubmit = () => {
  if (!title.value.trim() || !description.value.trim()) return

  // Fallback image array to assign a delicious random food look if user leaves it blank
  const foodPlaceholders = [
    'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?w=600', // Salad/Healthy bowl
    'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?w=600', // Pizza
    'https://images.unsplash.com/photo-1567620905732-2d1ec7ab7445?w=600', // Pancakes
    'https://images.unsplash.com/photo-1484723091739-30a097e8f929?w=600'  // French Toast
  ]
  
  const selectedImage = imageUrl.value.trim() 
    ? imageUrl.value.trim() 
    : foodPlaceholders[Math.floor(Math.random() * foodPlaceholders.length)]

  // Structure the new recipe item
  const newRecipe = {
    id: Date.now(),
    title: title.value,
    description: description.value,
    ingredients: ingredients.value.split(',').map(item => item.trim()).filter(item => item),
    steps: steps.value,
    image: selectedImage
  }

  // Add to active display stream
  recipes.value.push(newRecipe)

  // Clear Form Inputs
  title.value = ''
  description.value = ''
  ingredients.value = ''
  steps.value = ''
  imageUrl.value = ''
}
</script>

<template>
  <div class="app-container">
    <header class="main-header">
      <h1>🍳 Culinary Recipe Catalogue</h1>
      <p>Week 9 - Exercise 01 Prototype Layout</p>
    </header>

    <div class="catalogue-workspace-grid">
      <section class="form-card-panel">
        <h2>Add a New Recipe</h2>
        <form @submit.prevent="handleSubmit" class="recipe-entry-form">
          <div class="form-input-group">
            <label>Recipe Title</label>
            <input v-model="title" type="text" placeholder="e.g., Homemade Garlic Butter Pizza" required />
          </div>

          <div class="form-input-group">
            <label>Brief Description</label>
            <textarea v-model="description" rows="3" placeholder="Describe your dish..." required></textarea>
          </div>

          <div class="form-input-group">
            <label>Ingredients (Separate entries with commas)</label>
            <input v-model="ingredients" type="text" placeholder="Flour, Sugar, Butter, Love" required />
          </div>

          <div class="form-input-group">
            <label>Cooking Steps</label>
            <textarea v-model="steps" rows="3" placeholder="1. Mix ingredients. 2. Bake until golden..." required></textarea>
          </div>

          <div class="form-input-group">
            <label>Custom Image URL (Optional - Leaves blank for automatic placeholder)</label>
            <input v-model="imageUrl" type="url" placeholder="https://example.com/food-pic.jpg" />
          </div>

          <button type="submit" class="submit-action-btn">Add to Catalogue</button>
        </form>
      </section>

      <section class="display-stream-panel">
        <h2>Your Saved Recipes</h2>
        <div v-if="recipes.length === 0" class="empty-notice">
          <p>No recipes logged in the catalogue yet.</p>
        </div>
        <div v-else class="recipe-cards-flex-grid">
          <RecipeCard v-for="recipe in recipes" :key="recipe.id" :recipe="recipe" />
        </div>
      </section>
    </div>
  </div>
</template>

<style>
:root {
  --surface-bg: #ffffff;
  --body-bg: #f1f5f9;
  --text-primary: #1e293b;
  --text-muted: #64748b;
  --brand-primary: #2563eb;
  --border-element: #cbd5e1;
}

body {
  margin: 0;
  background-color: var(--body-bg);
  font-family: 'Inter', system-ui, sans-serif;
  color: var(--text-primary);
}

.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem 1rem;
}

.main-header {
  margin-bottom: 2rem;
  border-bottom: 2px solid var(--border-element);
  padding-bottom: 1rem;
}

.main-header h1 { margin: 0; font-size: 2rem; font-weight: 800; }
.main-header p { margin: 0.25rem 0 0 0; color: var(--text-muted); font-weight: 500; }

.catalogue-workspace-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  align-items: start;
}

@media (min-width: 900px) {
  .catalogue-workspace-grid {
    grid-template-columns: 420px 1fr;
  }
}

.form-card-panel {
  background: var(--surface-bg);
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
}

.form-card-panel h2, .display-stream-panel h2 {
  margin-top: 0;
  font-size: 1.25rem;
  font-weight: 700;
  margin-bottom: 1.25rem;
}

.recipe-entry-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-input-group {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

.form-input-group label {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--text-primary);
}

.form-input-group input, .form-input-group textarea {
  padding: 0.6rem 0.75rem;
  border: 1px solid var(--border-element);
  border-radius: 6px;
  font-family: inherit;
  font-size: 0.9rem;
}

.form-input-group input:focus, .form-input-group textarea:focus {
  outline: 2px solid var(--brand-primary);
  border-color: transparent;
}

.submit-action-btn {
  background: var(--brand-primary);
  color: white;
  border: none;
  padding: 0.75rem;
  font-weight: 700;
  font-size: 0.95rem;
  border-radius: 6px;
  cursor: pointer;
  margin-top: 0.5rem;
}

.submit-action-btn:hover { background: #1d4ed8; }

.recipe-cards-flex-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 1.5rem;
}

.empty-notice {
  background: #e2e8f0;
  padding: 3rem;
  text-align: center;
  border-radius: 8px;
  color: var(--text-muted);
}
</style>