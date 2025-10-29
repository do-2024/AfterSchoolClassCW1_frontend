<template>
  <div class="cart">
    <h2>Your Cart</h2>

    <p v-if="cart.length === 0">Cart is empty.</p>

    <table v-else>
      <tr>
        <th>Subject</th>
        <th>Qty</th>
        <th>Price</th>
        <th></th>
      </tr>
      <tr v-for="item in cart" :key="item.id">
        <td>{{ item.subject }}</td>
        <td>{{ item.qty }}</td>
        <td>£{{ item.price * item.qty }}</td>
        <td>
          <button @click="$emit('remove', item.id)">Remove</button>
        </td>
      </tr>
    </table>

    <p><strong>Total: £{{ total }}</strong></p>

    <!-- Checkout component -->
    <Checkout :cart="cart" @success="$emit('checkout-success')" />
  </div>
</template>

<script>
import Checkout from './Checkout.vue'

export default {
  name: 'Cart',
  components: { Checkout },
  props: ['cart'],
  computed: {
    total() {
      // Calculate the total price of all cart items
      return this.cart.reduce((sum, item) => sum + item.price * item.qty, 0)
    }
  }
}
</script>

<style scoped>
.cart {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.cart h2 {
  margin-bottom: 15px;
}

.cart table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 15px;
}

.cart th,
.cart td {
  border-bottom: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

button {
  background-color: #0078d4;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 6px 10px;
  cursor: pointer;
}

button:hover {
  background-color: #005fa3;
}
</style>