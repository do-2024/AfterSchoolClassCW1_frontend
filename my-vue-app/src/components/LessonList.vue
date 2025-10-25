<template>
  <div class="lesson-grid">
    <div v-for="lesson in lessons" :key="lesson.id" class="lesson-card">
      <i :class="lesson.icon" class="icon"></i>
      <h3>{{ lesson.subject }}</h3>
      <p>{{ lesson.location }}</p>
      <p>Price: £{{ lesson.price }}</p>
      <p>Spaces: {{ lesson.spaces }}</p>
      <button class="add-btn" :disabled="lesson.spaces===0" @click="$emit('add-to-cart', lesson.id)">
        Add to Cart
      </button>
    </div>
  </div>
</template>

<script>
export default { name: 'LessonList', props: { lessons: Array } }
</script>
