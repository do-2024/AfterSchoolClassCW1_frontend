<template>
  <div class="search-bar">
    <input
      type="text"
      placeholder="Search lessons..."
      v-model="query"
      @input="emitSearch"
    />
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  data() {
    return { query: "" };
  },
  methods: {
    emitSearch() {
      this.$emit("search", this.query);
    },
  },
};
</script>
