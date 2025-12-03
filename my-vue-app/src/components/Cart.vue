<template>
  <div class="cart">
    <h2>Your Cart</h2>

    <p v-if="cart.length === 0">Cart is empty.</p>

    <table v-else>
      <thead>
        <tr>
          <th>Subject</th>
          <th>Qty</th>
          <th>Price</th>
          <th></th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in cart" :key="item._id">
          <td>{{ item.topic }}</td>
          <td>{{ item.qty }}</td>
          <td>£{{ item.price * item.qty }}</td>
          <td>
            <button @click="$emit('remove', item._id)">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>

    <p><strong>Total: £{{ total }}</strong></p>

    <Checkout :cart="cart" @success="$emit('checkout-success')" />
  </div>
</template>

<script>
import Checkout from "./Checkout.vue";

export default {
  name: "Cart",
  components: { Checkout },
  props: ["cart"],

  computed: {
    total() {
      return this.cart.reduce(
        (sum, item) => sum + item.price * item.qty,
        0
      );
    },
  },
};
</script>
