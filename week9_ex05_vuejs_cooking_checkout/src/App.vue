<script setup>
import { ref, computed } from 'vue'
import CourseCard from './components/CourseCard.vue'
import CartPanel from './components/CartPanel.vue'

// Local Course Catalogue Data Base
const courses = ref([
  { id: 1, title: 'Mastering French Pastries', price: 750, image: '🥐', duration: '4 Weeks', description: 'Learn the fine art of baking perfect croissants, eclairs, and tarts from scratch.', soldOut: false },
  { id: 2, title: 'Gourmet Sushi & Sashimi', price: 950, image: '🍣', duration: '2 Weeks', description: 'Master knife skills, rice preparation, and traditional rolling techniques.', soldOut: false },
  { id: 3, title: 'Artisan Bread Making', price: 600, image: '🥖', duration: '3 Weeks', description: 'Understand wild yeast, sourdough starters, and wood-fired oven techniques.', soldOut: true },
  { id: 4, title: 'Authentic Italian Pasta Masterclass', price: 800, image: '🍝', duration: '3 Weeks', description: 'Roll, shape, and cook traditional pasta shapes paired with regional sauces.', soldOut: false }
])

// Shopping Cart State
const cart = ref([])
const appliedDiscount = ref(0) // Percentage value

// Adds course to cart or increases quantity if already inside
const addToCart = (course) => {
  if (course.soldOut) return
  
  const existingItem = cart.value.find(item => item.id === course.id)
  if (existingItem) {
    existingItem.quantity++
  } else {
    cart.value.push({ ...course, quantity: 1 })
  }
}

// Adjusts item quantity safely
const updateQuantity = ({ id, quantity }) => {
  const item = cart.value.find(item => item.id === id)
  if (item) {
    item.quantity = Math.max(1, quantity)
  }
}

// Drops item entirely from array
const removeFromCart = (id) => {
  cart.value = cart.value.filter(item => item.id !== id)
}

// Empties out array entirely
const clearCart = () => {
  cart.value = []
  appliedDiscount.value = 0
}

// Applies valid coupon codes
const applyCoupon = (code) => {
  if (code.trim().toUpperCase() === 'SAVE10') {
    appliedDiscount.value = 10 // 10% off
    return true
  }
  return false
}

// Checkout Calculations
const subtotal = computed(() => {
  return cart.value.reduce((sum, item) => sum + (item.price * item.quantity), 0)
})

const discountAmount = computed(() => {
  return (subtotal.value * appliedDiscount.value) / 100
})

const taxAmount = computed(() => {
  // Calculated at 15% VAT standard on the discounted subtotal
  return (subtotal.value - discountAmount.value) * 0.15
})

const grandTotal = computed(() => {
  return (subtotal.value - discountAmount.value) + taxAmount.value
})
</script>

<template>
  <div class="checkout-app-wrapper">
    <header class="app-navbar">
      <div class="navbar-brand">
        <span class="brand-logo">🍳</span>
        <h1>Cooking Masterclass Checkout</h1>
      </div>
      <p class="navbar-tagline">Internal Prototype Design Environment</p>
    </header>

    <main class="checkout-layout-grid">
      <section class="catalogue-column">
        <h2 class="section-title">Available Culinary Courses</h2>
        <div class="courses-display-grid">
          <CourseCard 
            v-for="course in courses" 
            :key="course.id" 
            :course="course" 
            @add-to-cart="addToCart"
          />
        </div>
      </section>

      <section class="cart-column">
        <CartPanel 
          :cart="cart"
          :subtotal="subtotal"
          :discount="discountAmount"
          :tax="taxAmount"
          :total="grandTotal"
          @update-quantity="updateQuantity"
          @remove-item="removeFromCart"
          @clear-cart="clearCart"
          @apply-coupon="applyCoupon"
        />
      </section>
    </main>
  </div>
</template>

<style>
/* Global Resets and Variables */
:root {
  --bg-main: #f8fafc;
  --panel-bg: #ffffff;
  --text-dark: #0f172a;
  --text-muted: #64748b;
  --primary: #1e293b;
  --accent-red: #ef4444;
  --accent-green: #10b981;
  --border-color: #e2e8f0;
}

body {
  margin: 0;
  background-color: var(--bg-main);
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  color: var(--text-dark);
}

.checkout-app-wrapper {
  max-width: 1300px;
  margin: 0 auto;
  padding: 2rem 1rem;
}

.app-navbar {
  margin-bottom: 2.5rem;
  border-bottom: 2px solid var(--border-color);
  padding-bottom: 1.5rem;
}

.navbar-brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.brand-logo {
  font-size: 2.2rem;
}

.navbar-brand h1 {
  font-size: 2rem;
  font-weight: 800;
  margin: 0;
  letter-spacing: -0.025em;
}

.navbar-tagline {
  margin: 0.25rem 0 0 0;
  color: var(--text-muted);
  font-size: 0.95rem;
  font-weight: 500;
}

.checkout-layout-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2.5rem;
  align-items: start;
}

@media (min-width: 1024px) {
  .checkout-layout-grid {
    grid-template-columns: 1fr 450px;
  }
}

.section-title {
  font-size: 1.4rem;
  font-weight: 700;
  margin-top: 0;
  margin-bottom: 1.5rem;
}

.courses-display-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}
</style>