<template>
  <div>
    <div 
      class="lesson-card" 
      v-for="lesson in lessons" 
      :key="lesson._id"
    >
      <img 
        :src="`http://localhost:3000/images/${lesson.image}`" 
        width="160"
      />

      <h3>{{ lesson.topic }}</h3>

      <p>Location: {{ lesson.location }}</p>
      <p>Price: £{{ lesson.price }}</p>
      <p>Spaces left: {{ lesson.space }}</p>

      <button 
        :disabled="lesson.space === 0"
        @click="$emit('add-to-cart', lesson)"
      >
        Add to Cart
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["lessons"]
};
</script>
