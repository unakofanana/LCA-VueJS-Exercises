<script setup>
import { ref, computed } from 'vue'
import FoodStallCard from './components/FoodStallCard.vue'

// Default list of Food Festival stalls
const stalls = ref([
  {
    id: 1,
    name: "Cape Town Taco Truck",
    type: "Street Food",
    signatureDish: "Spicy Birria Tacos",
    priceRange: "$$",
    image: "https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?w=500&auto=format&fit=crop&q=60",
    description: "Authentic Mexican street tacos made with slow-cooked shredded beef and crispy grilled corn tortillas.",
    isBookmarked: false
  },
  {
    id: 2,
    name: "The Green Goddess",
    type: "Vegan",
    signatureDish: "Loaded Falafel Bowl",
    priceRange: "$$",
    image: "https://images.unsplash.com/photo-1512621776951-a57141f2eefd?w=500&auto=format&fit=crop&q=60",
    description: "100% plant-based nutritious bowls featuring crunchy homemade falafels, beetroot hummus, and tahini.",
    isBookmarked: false
  },
  {
    id: 3,
    name: "Waffle Wonderland",
    type: "Dessert",
    signatureDish: "Nutella & Strawberry Deluxe",
    priceRange: "$",
    image: "https://images.unsplash.com/photo-1562376502-6f769499c886?w=500&auto=format&fit=crop&q=60",
    description: "Fluffy, golden Belgian waffles stacked high with fresh berries, premium ice cream, and warm chocolate drizzles.",
    isBookmarked: false
  }
])

// Search and filter state tracking
const searchQuery = ref('')
const selectedType = ref('All')
const foodTypes = ['All', 'Street Food', 'Vegan', 'Dessert']

// Form model binding for adding new vendor entries
const newName = ref('')
const newType = ref('Street Food')
const newDish = ref('')
const newPrice = ref('$$')
const newDescription = ref('')

// Computed filtering processor logic
const filteredStalls = computed(() => {
  return stalls.value.filter(stall => {
    const matchesSearch = stall.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
                          stall.signatureDish.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesType = selectedType.value === 'All' || stall.type === selectedType.value
    return matchesSearch && matchesType
  })
})

// Toggle itinerary bookmark state
const toggleBookmark = (id) => {
  const stall = stalls.value.find(s => s.id === id)
  if (stall) stall.isBookmarked = !stall.isBookmarked
}

// Total itinerary counter tracker
const totalBookmarked = computed(() => {
  return stalls.value.filter(s => s.isBookmarked).length
})

// Submission handler to add new food vendors dynamically
const handleAddStall = () => {
  if (!newName.value || !newDish.value) return

  stalls.value.push({
    id: Date.now(),
    name: newName.value,
    type: newType.value,
    signatureDish: newDish.value,
    priceRange: newPrice.value,
    image: "https://images.unsplash.com/photo-1555396273-367ea4eb4db5?w=500&auto=format&fit=crop&q=60", // Default vendor booth placeholder
    description: newDescription.value || "Fresh festival vendor serving up specialized hot meals daily.",
    isBookmarked: false
  })

  // Form field input cleaning reset
  newName.value = ''
  newDish.value = ''
  newDescription.value = ''
}
</script>

<template>
  <div class="fest-wrapper">
    <header class="fest-header">
      <div class="header-content">
        <h1>🎪 Street Food Fest Hub</h1>
        <p>Explore elite local food vendors, map out your flavor journey, and save your favorites.</p>
      </div>
      <div class="itinerary-badge">
        📍 My Food Trail: <strong>{{ totalBookmarked }} Stalls Saved</strong>
      </div>
    </header>

    <main class="fest-layout">
      <aside class="sidebar-controls">
        <div class="panel-card">
          <h3>Search Stalls</h3>
          <input 
            v-model="searchQuery" 
            type="text" 
            placeholder="Search vendor or dish..." 
            class="fest-input" 
          />
          
          <label class="section-label">Filter Category</label>
          <div class="category-stack">
            <button 
              v-for="type in foodTypes" 
              :key="type"
              @click="selectedType = type"
              :class="['filter-pill', { active: selectedType === type }]"
            >
              {{ type }}
            </button>
          </div>
        </div>

        <div class="panel-card">
          <h3>Register New Vendor</h3>
          <form @submit.prevent="handleAddStall" class="vendor-form">
            <div class="form-group">
              <label>Vendor / Truck Name</label>
              <input v-model="newName" type="text" required placeholder="e.g., Smokey BBQ Bros" class="fest-input" />
            </div>
            <div class="form-group">
              <label>Cuisine Style</label>
              <select v-model="newType" class="fest-input">
                <option value="Street Food">Street Food</option>
                <option value="Vegan">Vegan</option>
                <option value="Dessert">Dessert</option>
              </select>
            </div>
            <div class="form-group">
              <label>Signature Menu Item</label>
              <input v-model="newDish" type="text" required placeholder="e.g., Pulled Beef Sliders" class="fest-input" />
            </div>
            <div class="form-group">
              <label>Price Category</label>
              <select v-model="newPrice" class="fest-input">
                <option value="$">$ (Budget Friendly)</option>
                <option value="$$">$$ (Standard Fare)</option>
                <option value="$$$$">$$$ (Gourmet Delicacy)</option>
              </select>
            </div>
            <div class="form-group">
              <label>Vendor Teaser Summary</label>
              <textarea v-model="newDescription" placeholder="Describe the vibe and dishes..." class="fest-input box-area"></textarea>
            </div>
            <button type="submit" class="submit-vendor-btn">Launch Booth</button>
          </form>
        </div>
      </aside>

      <section class="stalls-display-area">
        <div v-if="filteredStalls.length === 0" class="no-vendors-fallback">
          <p>No food stalls match your active search filters.</p>
        </div>
        <div v-else class="stalls-grid">
          <FoodStallCard 
            v-for="stall in filteredStalls" 
            :key="stall.id" 
            :stall="stall"
            @toggle-save="toggleBookmark"
          />
        </div>
      </section>
    </main>
  </div>
</template>