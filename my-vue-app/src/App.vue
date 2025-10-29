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


<script>
import LessonList from './components/LessonList.vue'
import SortControls from './components/SortControls.vue'
import SearchBar from './components/SearchBar.vue'
import Cart from './components/Cart.vue'

export default {
  components: { LessonList, SortControls, SearchBar, Cart },
  data() {
    return {
      lessons: [
        { id: 1, subject: 'Math', location: 'Hendon', price: 100, spaces: 5, icon: 'fa-solid fa-square-root-variable' },
        { id: 2, subject: 'Science', location: 'Colindale', price: 80, spaces: 5, icon: 'fa-solid fa-flask' },
        { id: 3, subject: 'English', location: 'Brent Cross', price: 90, spaces: 5, icon: 'fa-solid fa-book' },
        { id: 4, subject: 'Art', location: 'Hendon', price: 70, spaces: 5, icon: 'fa-solid fa-palette' },
        { id: 5, subject: 'Music', location: 'Colindale', price: 85, spaces: 5, icon: 'fa-solid fa-music' },
        { id: 6, subject: 'Dance', location: 'Mill Hill', price: 60, spaces: 5, icon: 'fa-solid fa-person-running' },
        { id: 7, subject: 'History', location: 'Hendon', price: 75, spaces: 5, icon: 'fa-solid fa-landmark' },
        { id: 8, subject: 'Drama', location: 'Brent Cross', price: 80, spaces: 5, icon: 'fa-solid fa-theater-masks' },
        { id: 9, subject: 'Computing', location: 'Mill Hill', price: 110, spaces: 5, icon: 'fa-solid fa-computer' },
        { id: 10, subject: 'Physics', location: 'Hendon', price: 120, spaces: 5, icon: 'fa-solid fa-atom' }
      ],
      sortBy: 'subject',
      orderAsc: true,
      searchQuery: '',
      cart: [],
      showCart: false
    }
  },
  computed: {
    filteredLessons() {
      let result = this.lessons.filter(l =>
        l.subject.toLowerCase().includes(this.searchQuery.toLowerCase())
      )
      result.sort((a, b) => {
        const fieldA = a[this.sortBy]
        const fieldB = b[this.sortBy]
        if (fieldA < fieldB) return this.orderAsc ? -1 : 1
        if (fieldA > fieldB) return this.orderAsc ? 1 : -1
        return 0
      })
      return result
    }
  },
  methods: {
    updateSort({ sortBy, orderAsc }) {
      this.sortBy = sortBy
      this.orderAsc = orderAsc
    },
    addToCart(id) {
      const lesson = this.lessons.find(l => l.id === id)
      if (lesson && lesson.spaces > 0) {
        lesson.spaces--
        const existing = this.cart.find(i => i.id === id)
        if (existing) existing.qty++
        else this.cart.push({ ...lesson, qty: 1 })
      }
    },
    removeFromCart(id) {
      const index = this.cart.findIndex(i => i.id === id)
      if (index > -1) {
        const lesson = this.lessons.find(l => l.id === id)
        lesson.spaces += this.cart[index].qty
        this.cart.splice(index, 1)
      }
    },
    toggleCart() {
      this.showCart = !this.showCart
    },
    onCheckoutSuccess() {
      alert('Checkout complete!')
      this.cart = []
      this.showCart = false
    },
    reset() {
      this.lessons.forEach(l => (l.spaces = 5))
      this.cart = []
      this.showCart = false
    }
  }
}
</script>

