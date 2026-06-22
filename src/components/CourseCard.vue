<template>
  <div class="course-card" :class="{ 'sold-out-blur': !course.isAvailable }">
    <div class="image-container">
      <img :src="course.image" :alt="course.title" class="course-image" />
      <span v-if="!course.isAvailable" class="badge sold-out-badge">Sold Out</span>
      <span class="badge level-badge">{{ course.level }}</span>
    </div>

    <div class="course-content">
      <h3 class="course-title">{{ course.title }}</h3>
      <p class="course-chef">By Chef {{ course.chef }}</p>
      
      <div class="course-footer">
        <span class="course-price">R{{ course.price.toFixed(2) }}</span>
        
        <button 
          :disabled="!course.isAvailable"
          @click="$emit('toggle-wishlist', course.id)"
          :class="['wishlist-btn', { 'is-wishlisted': isWishlisted }]"
        >
          {{ isWishlisted ? '❤️ Saved' : '🤍 Save' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  course: { type: Object, required: true },
  isWishlisted: { type: Boolean, default: false }
});
defineEmits(['toggle-wishlist']);
</script>

<style scoped>
.course-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s, box-shadow 0.2s;
  display: flex;
  flex-direction: column;
}
.course-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}
.sold-out-blur .course-image {
  filter: grayscale(60%);
  opacity: 0.7;
}
.image-container { position: relative; height: 200px; }
.course-image { width: 100%; height: 100%; object-fit: cover; }
.badge { position: absolute; padding: 6px 12px; border-radius: 20px; font-size: 0.75rem; font-weight: bold; text-transform: uppercase; }
.sold-out-badge { top: 12px; left: 12px; background-color: #e63946; color: white; }
.level-badge { top: 12px; right: 12px; background-color: #f1faee; color: #1d3557; }
.course-content { padding: 1.5rem; display: flex; flex-direction: column; flex-grow: 1; }
.course-title { margin: 0 0 0.5rem 0; font-size: 1.2rem; color: #1d3557; }
.course-chef { color: #4a5568; font-size: 0.9rem; margin-bottom: 1.5rem; }
.course-footer { margin-top: auto; display: flex; justify-content: space-between; align-items: center; }
.course-price { font-size: 1.25rem; font-weight: bold; color: #1d3557; }
.wishlist-btn { background: transparent; border: 2px solid #e63946; color: #e63946; padding: 6px 16px; border-radius: 20px; cursor: pointer; font-weight: 600; transition: all 0.2s; }
.wishlist-btn:hover:not(:disabled), .wishlist-btn.is-wishlisted { background: #e63946; color: white; }
.wishlist-btn:disabled { border-color: #cbd5e1; color: #94a3b8; cursor: not-allowed; }
</style>