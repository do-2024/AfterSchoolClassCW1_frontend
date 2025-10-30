<template>
  <div class="app-container">
    <header>
      <h1>After School Classes</h1>
      <div class="buttons">
        <button @click="toggleCart">{{ showCart ? 'Back to Lessons' : 'View Cart (' + cart.length + ')' }}</button>
        <button @click="reset">Reset</button>
      </div>
    </header>

    <div v-if="showCart">
      <Cart :cart="cart" @remove="removeFromCart" @checkout-success="onCheckoutSuccess" />
    </div>

    <div v-else>
      <SearchBar @search="searchQuery = $event" />
      <SortControls :sort-by="sortBy" :order-asc="orderAsc" @updateSort="updateSort" />
      <LessonList :lessons="filteredLessons" @add-to-cart="addToCart" />
    </div>
  </div>
</template>


