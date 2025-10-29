<template>
  <div class="sort-controls">
    <label>Sort by:</label>
    <select v-model="internalSortBy" @change="emitChange">
      <option value="subject">Subject</option>
      <option value="location">Location</option>
      <option value="price">Price</option>
      <option value="spaces">Spaces</option>
    </select>
    <button @click="toggleOrder">
      {{ internalOrderAsc ? '↑' : '↓' }}
    </button>
  </div>
</template>


<script>
export default {
  name: 'SortControls',
  props: ['sortBy', 'orderAsc'],
  data() {
    return {
      internalSortBy: this.sortBy,
      internalOrderAsc: this.orderAsc
    }
  },
  methods: {
    toggleOrder() {
      this.internalOrderAsc = !this.internalOrderAsc
      this.emitChange()
    },
    emitChange() {
      this.$emit('updateSort', {
        sortBy: this.internalSortBy,
        orderAsc: this.internalOrderAsc
      })
    }
  }
}
</script>
