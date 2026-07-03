<script setup>
defineProps({
  stall: {
    type: Object,
    required: true
  }
})

defineEmits(['toggle-save'])
</script>

<template>
  <div :class="['stall-card', { 'saved-border': stall.isBookmarked }]">
    <div class="stall-image-frame">
      <img :src="stall.image" :alt="stall.name" class="stall-img" />
      <span class="stall-badge">{{ stall.type }}</span>
    </div>

    <div class="stall-info">
      <div class="stall-meta-row">
        <h4>{{ stall.name }}</h4>
        <span class="price-indicator">{{ stall.priceRange }}</span>
      </div>
      
      <p class="stall-desc">{{ stall.description }}</p>
      
      <div class="specialty-highlight">
        ⭐ <strong>Must Try:</strong> {{ stall.signatureDish }}
      </div>

      <button 
        @click="$emit('toggle-save', stall.id)" 
        :class="['trail-action-btn', { 'is-active-saved': stall.isBookmarked }]"
      >
        {{ stall.isBookmarked ? '❌ Remove from My Trail' : '✨ Add to My Trail' }}
      </button>
    </div>
  </div>
</template>