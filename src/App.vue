<template>
  <div class="app">
    <!-- HEADER -->
    <header>
      <h1>Cooking Masterclass Catalogue</h1>
      <p>Wishlist: {{ wishlistCount }}</p>
    </header>

    <!-- COURSES -->
    <div class="courses">
      <div
        class="course-card"
        v-for="course in courses"
        :key="course.id"
      >
        <h2>{{ course.title }}</h2>

        <p><strong>Chef:</strong> {{ course.chef }}</p>
        <p><strong>Price:</strong> R{{ course.price }}</p>
        <p><strong>Level:</strong> {{ course.level }}</p>

        <!-- AVAILABILITY -->
        <p v-if="course.available">Available</p>
        <p v-else>Sold Out</p>

        <!-- SAVE BUTTON (wishlist) -->
        <button
          v-if="course.available"
          @click="addToWishlist"
        >
          Save to Wishlist
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

/* Wishlist counter */
const wishlistCount = ref(0)

/* Course data */
const courses = ref([
  {
    id: 1,
    title: "Italian Pasta Masterclass",
    chef: "Chef Marco Rossi",
    price: 450,
    level: "Beginner",
    available: true
  },
  {
    id: 2,
    title: "Advanced Sushi Techniques",
    chef: "Chef Yuki Tanaka",
    price: 650,
    level: "Advanced",
    available: false
  },
  {
    id: 3,
    title: "French Pastry Essentials",
    chef: "Chef Claire Dubois",
    price: 550,
    level: "Intermediate",
    available: true
  }
])

/* FUNCTION: increases wishlist */
const addToWishlist = () => {
  wishlistCount.value++
}
</script>

<style>
.app {
  font-family: Arial, sans-serif;
  padding: 20px;
}

header {
  text-align: center;
  margin-bottom: 20px;
}

.courses {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
}

.course-card {
  border: 1px solid #ccc;
  padding: 15px;
  width: 250px;
  border-radius: 8px;
  background: #fff;
}

button {
  margin-top: 10px;
  padding: 8px;
  cursor: pointer;
}
</style>