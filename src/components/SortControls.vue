<template>
  <div class="sort-controls">
    <label>Sort by:</label>

    <select v-model="localSortBy" @change="emitChange">
      <option value="topic">Subject</option>
      <option value="location">Location</option>
      <option value="price">Price</option>
      <option value="spaces">Spaces</option>
    </select>

    <button @click="toggleOrder">
      {{ localOrderAsc ? "↑" : "↓" }}
    </button>
  </div>
</template>

<script>
export default {
  name: "SortControls",
  props: ["sortBy", "orderAsc"],

  data() {
    return {
      localSortBy: this.sortBy,
      localOrderAsc: this.orderAsc,
    };
  },

  methods: {
    toggleOrder() {
      this.localOrderAsc = !this.localOrderAsc;
      this.emitChange();
    },

    emitChange() {
      this.$emit("updateSort", {
        sortBy: this.localSortBy,
        orderAsc: this.localOrderAsc,
      });
    },
  },
};
</script>
