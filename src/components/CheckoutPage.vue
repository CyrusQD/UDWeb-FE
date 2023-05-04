<template>
  <div id="page-wrap">
    <h1>Xác nhận đơn hàng</h1>
    <div class="check-out">
      <CheckoutList :cartItems="cartItems" :totalPrice="totalPrice" />
      <form v-on:submit.prevent="submitForm">
        <h3>Thông tin khách hàng</h3>
        <label for="name">Name: {{ this.user.result.name }}</label>
        <label for="email">Email: {{ this.user.result.email }}</label>
        <label for="address">Address:</label>
        <input type="text" id="address" name="address" required />
        <button type="submit" value="Submit">Submit</button>
      </form>
      <div v-if="showSuccessMessage">
        Order thành công!
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import CheckoutList from "@/components/CheckoutListItem.vue";
export default {
  name: "CheckoutPage",
  components: {
    CheckoutList,
  },
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      cartItems: [],
      showSuccessMessage: false,
    };
  },
  computed: {
    totalPrice() {
      const sum = this.cartItems.reduce(
        (sum, item) => sum + parseFloat(item.price),
        0
      );
      return sum > 999 ? sum / 1000 : sum;
    },
  },
  methods: {
    async submitForm(event) {
      event.preventDefault();
      const { address } = event.target.elements;
      const formData = {
        name: this.user.result.name,
        email: this.user.result.email,
        address: address.value,
        cartItems: this.cartItems,
        totalPrice: this.totalPrice,
      };
      console.log(formData);
      await axios.post(`/api/users/${this.user.result.id}/orthers`, formData);
      this.showSuccessMessage = true;
      setTimeout(() => {
        this.$router.push("/");
      }, 1500);
    },
  },
  async created() {
    try {
      const response = await axios.get(
        `/api/users/${this.user.result.id}/cart`
      );
      const cartItems = response.data;
      this.cartItems = cartItems;
    } catch (error) {
      console.error(error);
    }
  },
};
</script>
<style scoped>
.check-out {
  width: 60%;
  margin: auto;
}
h1 {
  border-bottom: 1px solid black;
  margin: 0;
  margin-top: 16px;
  padding: 16px;
}
h3 {
  margin: 0;
}
form {
  display: flex;
  flex-direction: column;
}

label {
  margin-top: 16px;
  margin-left: 40px;
}
#address {
  border: 1px solid gray;
}
input[type="text"],
input[type="email"] {
  padding: 8px;
  border-radius: 4px;
  border: none;
  margin-bottom: 16px;
}

button[type="submit"] {
  background-color: #000000;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
