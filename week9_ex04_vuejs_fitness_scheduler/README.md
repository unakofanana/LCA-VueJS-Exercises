# 🏋️‍♂️ LCA Week 9 - Exercise 04: Fitness Class Scheduler

A responsive, reactive frontend web application built using **Vue.js (Composition API)** and **Vite** for the Life Choices Academy technical curriculum. This application enables fitness coaches and administrators to schedule sessions, manage client bookings, monitor class capacities in real time, and persist data locally.

---

## 🚀 Features

* **Dynamic Class Scheduling:** Add custom fitness sessions detailing the Class Name, Instructor, Start Time, and Maximum Capacity.
* **Real-Time Booking System:** Clients can reserve spots instantly. The app dynamically tracks remaining slots and disables bookings once a class reaches full capacity.
* **Data Persistence:** Integrated with browser `localStorage` so scheduled classes and active bookings remain saved even after a page refresh.
* **Conditional UI Rendering:** Displays custom clean notification banners when the schedule is completely empty.
* **Component-Driven Architecture:** Separated cleanly into modular, reusable Vue pieces (`App.vue`, `ClassForm.vue`, and `ClassCard.vue`).
* **Responsive Styling:** Scaled beautifully across mobile, tablet, and desktop viewports using CSS Grid and Flexbox layout frameworks.

---

## 🛠️ Tech Stack

* **Framework:** Vue 3 (`<script setup>` syntax)
* **Build Tool:** Vite
* **Languages:** JavaScript, HTML5, CSS3
* **Version Control:** Git & GitHub

---

## ⚙️ Local Setup Instructions

Follow these quick steps to get the development environment running locally on your machine:

1. **Navigate into the exercise directory:**
```bash
   cd week9_ex04_vuejs_fitness_scheduler