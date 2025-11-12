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

<script>
export default {
  name: 'SearchBar',
  data() {
    return {
      query: ''
    }
  }
}
</script>
