<template>
  <div class="app-container">
    <header class="header">
      <h1>After School Classes</h1>
      <div class="buttons">
        <button class="cart-btn" @click="toggleCart" :disabled="cart.length===0">
          <i class="fa-solid fa-cart-shopping"></i> Cart <span class="badge">{{ cartCount }}</span>
        </button>
        <button class="reset-btn" @click="resetData">Reset</button>
      </div>
    </header>

    <div v-if="!showCart">
      <div class="controls">
        <SearchBar v-model="searchQuery" placeholder="Search lessons..." />
        <SortControls :sort-by="sortBy" :order-asc="orderAsc" @updateSort="updateSort" />
      </div>
      <LessonList :lessons="filteredLessons" @add-to-cart="addToCart" />
    </div>

    <div v-else>
      <Cart :cart="cart" @remove="removeFromCart" @checkout-success="onCheckoutSuccess" />
    </div>
  </div>
</template>

<script>
import LessonList from './components/LessonList.vue'
import Cart from './components/Cart.vue'
import SortControls from './components/SortControls.vue'
import SearchBar from './components/SearchBar.vue'

export default {
  name: 'App',
  components: { LessonList, Cart, SortControls, SearchBar },
  data() {
    return {
      lessons: [
        { id: 1, subject: 'Maths', location: 'Hendon', price: 100, spaces: 5, icon: 'fa-solid fa-square-root-variable' },
        { id: 2, subject: 'English', location: 'Colindale', price: 80, spaces: 5, icon: 'fa-solid fa-book' },
        { id: 3, subject: 'Science', location: 'Brent Cross', price: 90, spaces: 5, icon: 'fa-solid fa-flask' },
        { id: 4, subject: 'Art', location: 'Golders Green', price: 70, spaces: 5, icon: 'fa-solid fa-palette' },
        { id: 5, subject: 'Coding', location: 'Hendon', price: 120, spaces: 5, icon: 'fa-solid fa-code' },
        { id: 6, subject: 'Music', location: 'Colindale', price: 85, spaces: 5, icon: 'fa-solid fa-music' },
        { id: 7, subject: 'Drama', location: 'Brent Cross', price: 75, spaces: 5, icon: 'fa-solid fa-theater-masks' },
        { id: 8, subject: 'PE', location: 'Golders Green', price: 60, spaces: 5, icon: 'fa-solid fa-basketball' },
        { id: 9, subject: 'Spanish', location: 'Hendon', price: 95, spaces: 5, icon: 'fa-solid fa-language' },
        { id: 10, subject: 'History', location: 'Colindale', price: 65, spaces: 5, icon: 'fa-solid fa-landmark' }
      ],
      cart: [],
      searchQuery: '',
      sortBy: 'subject',
      orderAsc: true,
      showCart: false
    }
  },
  computed: {
    filteredLessons() {
      const q = this.searchQuery.toLowerCase()
      let list = this.lessons.filter(l =>
        [l.subject, l.location, l.price, l.spaces]
          .some(v => String(v).toLowerCase().includes(q))
      )
      const key = this.sortBy
      list.sort((a, b) => {
        let A = a[key], B = b[key]
        if (typeof A === 'string') A = A.toLowerCase()
        if (typeof B === 'string') B = B.toLowerCase()
        return this.orderAsc ? (A > B ? 1 : -1) : (A > B ? -1 : 1)
      })
      return list
    },
    cartCount() {
      return this.cart.reduce((sum, item) => sum + item.qty, 0)
    }
  },
  methods: {
    addToCart(id) {
      const lesson = this.lessons.find(l => l.id === id)
      if (!lesson || lesson.spaces <= 0) return
      lesson.spaces--
      const existing = this.cart.find(c => c.id === id)
      existing ? existing.qty++ : this.cart.push({ ...lesson, qty: 1 })
    },
    removeFromCart(id) {
      const item = this.cart.find(c => c.id === id)
      const lesson = this.lessons.find(l => l.id === id)
      if (item && lesson) lesson.spaces += item.qty
      this.cart = this.cart.filter(c => c.id !== id)
    },
    toggleCart() { this.showCart = !this.showCart },
    resetData() { location.reload() },
    updateSort({ sortBy, orderAsc }) { this.sortBy = sortBy; this.orderAsc = orderAsc },
    onCheckoutSuccess() { this.cart = []; this.showCart = false }
  }
}
</script>
