<template>
  <div class="app-container">
    <header>
      <h1>After School Classes</h1>

      <button @click="toggleCart">
        {{ showCart ? "Back to Lessons" : `View Cart (${cart.length})` }}
      </button>
    </header>

    <!-- CART VIEW -->
    <div v-if="showCart">
      <h2>Your Cart</h2>

      <p v-if="cart.length === 0">Cart is empty</p>

      <div v-for="item in cart" :key="item._id" class="lesson-card">
        <h3>{{ item.topic }}</h3>
        <p>Quantity: {{ item.qty }}</p>
        <p>£{{ item.price * item.qty }}</p>

        <button @click="removeFromCart(item._id)">Remove</button>
      </div>

      <h3>Total: £{{ total }}</h3>

      <div>
        <input v-model="order.name" placeholder="Your Name" />
        <input v-model="order.phone" placeholder="Phone Number" />

        <button @click="checkout" :disabled="cart.length === 0">
          Checkout
        </button>
      </div>
    </div>

    <!-- LESSONS VIEW -->
    <div v-else>
      <input
        v-model="searchQuery"
        placeholder="Search lessons"
      />

      <div>
        <div
          class="lesson-card"
          v-for="lesson in filteredLessons"
          :key="lesson._id"
        >
          <img
            :src="`http://localhost:3000/images/${lesson.image}`"
            width="170"
          />

          <h3>{{ lesson.topic }}</h3>
          <p>Location: {{ lesson.location }}</p>
          <p>Price: £{{ lesson.price }}</p>
          <p>Spaces left: {{ lesson.space }}</p>

          <button
            :disabled="lesson.space === 0"
            @click="addToCart(lesson)"
          >
            Add to Cart
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      lessons: [],
      cart: [],
      showCart: false,
      searchQuery: "",
      order: {
        name: "",
        phone: ""
      }
    };
  },

  computed: {
    filteredLessons() {
      return this.lessons.filter((l) =>
        l.topic.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },

    total() {
      return this.cart.reduce(
        (sum, item) => sum + item.price * item.qty,
        0
      );
    }
  },

  async created() {
    await this.loadLessons();
  },

  methods: {
    async loadLessons() {
      try {
        const res = await fetch("http://localhost:3000/lessons");
        this.lessons = await res.json();
      } catch (err) {
        console.error("FETCH ERROR:", err);
      }
    },

    toggleCart() {
      this.showCart = !this.showCart;
    },

    addToCart(lesson) {
      if (lesson.space === 0) return;

      lesson.space--;

      const found = this.cart.find(i => i._id === lesson._id);

      if (found) {
        found.qty++;
      } else {
        this.cart.push({ ...lesson, qty: 1 });
      }
    },

    removeFromCart(id) {
      const index = this.cart.findIndex(i => i._id === id);
      if (index === -1) return;

      const lesson = this.lessons.find(l => l._id === id);
      lesson.space += this.cart[index].qty;

      this.cart.splice(index, 1);
    },

    async checkout() {
      if (!this.order.name || !this.order.phone) {
        alert("Enter name and phone");
        return;
      }

      const payload = {
        name: this.order.name,
        phone: this.order.phone,
        items: this.cart.map(item => ({
          lessonId: item._id,
          qty: item.qty
        }))
      };

      try {
        await fetch("http://localhost:3000/orders", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload)
        });

        alert("Order placed successfully!");

        this.cart = [];
        this.order.name = "";
        this.order.phone = "";
        this.showCart = false;

        await this.loadLessons();

      } catch (err) {
        alert("Checkout failed");
      }
    }
  }
};
</script>
