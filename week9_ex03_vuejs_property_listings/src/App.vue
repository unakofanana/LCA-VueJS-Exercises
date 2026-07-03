<template>
  <div class="container">

    <!-- HEADER -->
    <header class="header">
      <h1>Homes & Beyond</h1>
      <p>Total Listings: {{ filteredProperties.length }}</p>

      <input
        v-model="search"
        type="text"
        placeholder="Search by title or location..."
        class="search"
      />

      <button @click="toggleSort" class="sort-btn">
        Sort: {{ sortOrder === 'asc' ? 'Low → High' : 'High → Low' }}
      </button>
    </header>

    <!-- GRID -->
    <div class="grid">
      <div
        v-for="property in sortedProperties"
        :key="property.id"
        class="card"
        :class="{ unavailable: !property.available }"
      >

        <div class="badge" v-if="!property.available">
          Not Available
        </div>

        <img :src="property.image" class="img" />

        <h3>{{ property.title }}</h3>
        <p>{{ property.location }}</p>
        <p><strong>R {{ property.price }}</strong></p>
        <p>Type: {{ property.type }}</p>

        <button @click="toggleFav(property)" class="fav-btn">
          {{ favourites.includes(property.id) ? "★ Saved" : "☆ Save" }}
        </button>

      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, computed } from "vue";

/* DATA */
const search = ref("");
const sortOrder = ref("asc");
const favourites = ref([]);

const properties = ref([
  {
    id: 1,
    title: "Sea View Apartment",
    location: "Cape Town",
    price: 1200,
    type: "Apartment",
    available: true,
    image: "https://placehold.co/300x200"
  },
  {
    id: 2,
    title: "City Studio",
    location: "Johannesburg",
    price: 800,
    type: "Studio",
    available: false,
    image: "https://placehold.co/300x200"
  },
  {
    id: 3,
    title: "Luxury Villa",
    location: "Durban",
    price: 2500,
    type: "Villa",
    available: true,
    image: "https://placehold.co/300x200"
  },
  {
    id: 4,
    title: "Modern Flat",
    location: "Pretoria",
    price: 950,
    type: "Apartment",
    available: true,
    image: "https://placehold.co/300x200"
  }
]);

/* FILTER */
const filteredProperties = computed(() => {
  return properties.value.filter(p =>
    p.title.toLowerCase().includes(search.value.toLowerCase()) ||
    p.location.toLowerCase().includes(search.value.toLowerCase())
  );
});

/* SORT */
const sortedProperties = computed(() => {
  return [...filteredProperties.value].sort((a, b) => {
    return sortOrder.value === "asc"
      ? a.price - b.price
      : b.price - a.price;
  });
});

/* TOGGLE SORT */
function toggleSort() {
  sortOrder.value = sortOrder.value === "asc" ? "desc" : "asc";
}

/* FAVOURITES */
function toggleFav(property) {
  if (favourites.value.includes(property.id)) {
    favourites.value = favourites.value.filter(id => id !== property.id);
  } else {
    favourites.value.push(property.id);
  }
}
</script>

<style>
.container {
  max-width: 1000px;
  margin: auto;
  padding: 20px;
  font-family: Arial;
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.search {
  padding: 8px;
  width: 60%;
  margin: 10px;
}

.sort-btn {
  padding: 8px 12px;
  cursor: pointer;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 15px;
}

.card {
  border: 1px solid #ddd;
  padding: 10px;
  position: relative;
  border-radius: 8px;
}

.img {
  width: 100%;
  height: 140px;
  object-fit: cover;
}

.badge {
  position: absolute;
  top: 10px;
  left: 10px;
  background: red;
  color: white;
  padding: 5px;
  font-size: 12px;
}

.fav-btn {
  margin-top: 10px;
  padding: 6px;
  cursor: pointer;
}

.unavailable {
  opacity: 0.5;
}
</style>
