<template>
  <div class="search-bar">
    <input
      type="text"
      placeholder="Search lessons..."
      v-model="query"
      @input="$emit('search', query)"
    />
  </div>
</template>


