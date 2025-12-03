<template>
  <div class="checkout">
    <h3>Checkout</h3>

    <form @submit.prevent="submit">
      <input v-model="name" placeholder="Name" required />
      <input v-model="phone" placeholder="Phone" required />

      <button :disabled="loading" type="submit">
        {{ loading ? "Processing..." : "Confirm Order" }}
      </button>
    </form>
  </div>
</template>

<script>
const BACKEND_URL = "http://localhost:3000";

export default {
  name: "Checkout",
  props: ["cart"],

  data() {
    return {
      name: "",
      phone: "",
      loading: false,
    };
  },

  methods: {
    validName() {
      return /^[A-Za-z ]+$/.test(this.name);
    },
    validPhone() {
      return /^[0-9]+$/.test(this.phone);
    },

    async submit() {
      if (!this.validName() || !this.validPhone()) {
        alert("Invalid name or phone");
        return;
      }

      this.loading = true;

      const order = {
        name: this.name,
        phone: this.phone,
        items: this.cart.map((i) => ({
          lessonId: i._id,
          qty: i.qty,
        })),
      };

      const res = await fetch("http://localhost:3000/orders", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(order)
      });


      if (!res.ok) {
        alert("Order failed");
      } else {
        this.$emit("success");
      }

      this.loading = false;
    },
  },
};
</script>
