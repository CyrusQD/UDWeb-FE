<template>
  <div id="page-wrap" v-if="product">
    <div id="img-wrap">
      <img v-bind:src="product.imageUrl" />
    </div>
    <div id="product-details">
      <h1>{{ product.name }}</h1>
      <h3 id="price">{{ product.price }}vnđ</h3>
      <p>Đánh giá: {{ product.averageRating }}</p>
      <button
        id="add-to-cart"
        v-if="!itemIsInCart && !showSuccessMessage"
        v-on:click="addToCart"
      >
        Thêm vào giỏ hàng
      </button>
      <button
        id="add-to-cart"
        class="green-button"
        v-if="!itemIsInCart && showSuccessMessage"
      >
        Thêm vào giỏ hàng thành công!
      </button>
      <button id="add-to-cart" class="grey-button" v-if="itemIsInCart">
        sản phẩm đã có trong giỏ hàng
      </button>
      <h4>Mô Tả</h4>
      <p>{{ product.description }}</p>
    </div>
  </div>
  <NotFound v-else />
</template>

<script>
import axios from "axios";
import NotFound from "./NotFound";

export default {
  name: "ProductDetailPage",
  components: {
    NotFound,
  },
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      product: {},
      cartItems: [],
      showSuccessMessage: false,
    };
  },
  computed: {
    itemIsInCart() {
      return this.cartItems.some((item) => item.id === this.product.id);
    },
  },
  methods: {
    async addToCart() {
      if (this.user == null) {
        window.alert("Vui lòng đăng nhập để thêm sản phẩm vào giỏ hàng!");
      } else {
        console.log(this.user.result.id);
        await axios.post(`/api/users/${this.user.result.id}/cart`, {
          productId: this.$route.params.id,
        });
        this.showSuccessMessage = true;
        setTimeout(() => {
          this.$router.push("/products");
        }, 1500);
      }
    },
  },
  async created() {
    const { data: product } = await axios.get(
      `/api/products/${this.$route.params.id}`
    );
    this.product = product;

    const { data: cartItems } = await axios.get(
      `/api/users/${this.user.result.id}/cart`
    );
    this.cartItems = cartItems;
  },
};
</script>

<style scoped>
#page-wrap {
  margin-top: 16px;
  padding: 16px;
  max-width: 600px;
}

#img-wrap {
  text-align: center;
  border: 1px solid rgb(211, 211, 211);
  border-radius: 20px;
}

img {
  width: 400px;
}

#product-details {
  padding: 16px;
  position: relative;
}

#add-to-cart {
  width: 100%;
}

#price {
  position: absolute;
  top: 24px;
  right: 16px;
}
</style>
