<template>
  <div class="checkout">
    <h3>Checkout</h3>

    <form @submit.prevent="submit">
      <input
        v-model="name"
        placeholder="Enter your name"
        required
      />
      <input
        v-model="phone"
        placeholder="Enter your phone number"
        required
      />
      <button type="submit">Confirm</button>
    </form>
  </div>
</template>