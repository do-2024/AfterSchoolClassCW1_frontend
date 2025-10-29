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