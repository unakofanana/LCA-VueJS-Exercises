# 🍳 Exercise 5: Cooking Masterclass Checkout System

A functional single-page e-commerce checkout prototype built with **Vue 3 (Composition API)** and **Vite** for the Week 9 Frontend Web Development curriculum.

## 🎯 Completed Requirements Checklist

- [x] **Masterclass Catalog:** Renders available cooking courses with titles, chef names, descriptions, and pricing dynamically.
- [x] **Interactive Cart Management:** Users can add classes to the cart stream without duplicating items.
- [x] **Quantity Adjustment:** Includes functional `+` and `-` controls to scale seat bookings, automatically removing items if the count drops below 1.
- [x] **Live Financial Computations:** Dynamically updates calculations in real-time:
  - Subtotal of all selected seats.
  - 15% VAT/Tax calculation.
  - Final Grand Total.
- [x] **Checkout Simulation:** A "Proceed to Checkout" action that alerts the user with an order confirmation and clears the cart back to zero.

## 🛠️ Tech Stack & Components Used
- **Framework:** Vue 3 (Script Setup)
- **Build Tool:** Vite
- **Architecture:** Component-driven (`App.vue`, `CourseCard.vue`, `CartPanel.vue`)

## 🚀 How to Run Locally
1. Install dependencies: `npm install`
2. Start development server: `npm run dev`