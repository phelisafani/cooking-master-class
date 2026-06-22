<template>
  <div class="app-container">
    <header class="main-header">
      <div class="header-content">
        <h1 class="logo">🍳 Cooking Masterclass</h1>
        <div class="wishlist-counter">
          <span>❤️ Wishlist:</span> 
          <span class="count-badge">{{ wishlist.length }}</span>
        </div>
      </div>
    </header>

    <main class="content-wrapper">
      <section class="controls-bar">
        <h2>Available Workshops</h2>
        <div class="filter-buttons">
          <button @click="filterSelection = 'all'" :class="{ active: filterSelection === 'all' }">All</button>
          <button @click="filterSelection = 'available'" :class="{ active: filterSelection === 'available' }">Available</button>
        </div>
      </section>

      <section class="courses-grid">
        <CourseCard 
          v-for="course in filteredCourses" 
          :key="course.id"
          :course="course"
          :isWishlisted="wishlist.includes(course.id)"
          @toggle-wishlist="handleWishlistToggle"
        />
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { courses } from './coursesData';
import CourseCard from './components/CourseCard.vue';

const allCourses = ref(courses);
const wishlist = ref([]);
const filterSelection = ref('all');

const handleWishlistToggle = (courseId) => {
  const index = wishlist.value.indexOf(courseId);
  if (index > -1) {
    wishlist.value.splice(index, 1);
  } else {
    wishlist.value.push(courseId);
  }
};

const filteredCourses = computed(() => {
  if (filterSelection.value === 'available') {
    return allCourses.value.filter(c => c.isAvailable);
  }
  return allCourses.value;
});
</script>

<style>
body {
  margin: 0;
  font-family: 'Inter', sans-serif;
  background-color: #f8f9fa;
}
.main-header {
  background-color: #1d3557;
  color: white;
  padding: 1rem 2rem;
  position: sticky;
  top: 0;
  z-index: 100;
}
.header-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.wishlist-counter {
  background: rgba(255, 255, 255, 0.1);
  padding: 8px 16px;
  border-radius: 30px;
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: bold;
}
.count-badge {
  background: #e63946;
  padding: 2px 8px;
  border-radius: 50%;
}
.content-wrapper { max-width: 1200px; margin: 2rem auto; padding: 0 1rem; }
.controls-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem; }
.filter-buttons button { background: #e2e8f0; border: none; padding: 8px 16px; margin-left: 8px; border-radius: 6px; cursor: pointer; font-weight: 500; }
.filter-buttons button.active { background: #1d3557; color: white; }
.courses-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
}
</style>