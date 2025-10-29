checkout.vue:                                                                                                                                         <template>
  <div class="checkout">
    <h3>Checkout</h3>
    <form @submit.prevent="submit">
      <input v-model="name" placeholder="Enter your name" required />
      <input v-model="phone" placeholder="Enter your phone number" required />
      <button type="submit">Confirm</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Checkout',
  props: ['cart'],
  data() {
    return { name: '', phone: '' }
  },
  methods: {
    validName() {
      return /^[A-Za-z ]+$/.test(this.name)
    },
    validPhone() {
      return /^[0-9]+$/.test(this.phone)
    },
    submit() {
      if (!this.validName() || !this.validPhone()) {
        alert('Please enter a valid name and phone number.')
        return
      }
      this.$emit('success')
    }
  }
}
</script>
