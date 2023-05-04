<template>
  <div class="nav" v-if="loggedIn !== null">
    <div class="nav-bar">
      <div class="nav-left">
        <router-link to="/products" id="products-link">
          <h1>KEYCHRON</h1>
        </router-link>
      </div>

      <div class="nav-center">
        <router-link to="/products">Trang chủ</router-link>
        <router-link to="/orther">Lịch sử</router-link>
        <router-link to="/cart" id="cart-link">Giỏ hàng</router-link>
        <a v-if="loggedIn">{{ user.result.name }}</a>
        <a v-if="loggedIn" @click="logout">
          <fa icon="fa-solid fa-right-from-bracket" size="xl" />
        </a>
        <router-link v-if="!loggedIn" to="/login">Đăng nhập</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true,
    },
    updateUser: {
      type: Function,
      required: true,
    },
  },
  computed: {
    loggedIn() {
      return !!this.user;
    },
  },
  methods: {
    logout() {
      this.$router.push("/login");
      this.updateUser(null);
      localStorage.removeItem("user");
    },
  },
};
</script>

<style scoped>
/* styles */
</style>

<style scoped>
#products-link {
  text-align: center;
  display: block;
  color: black;
  font-size: 22px;
  text-decoration: none;
}

#products-link h1 {
  margin: 0;
}
#login-link button {
  margin-left: 16px;
}
.nav {
  background-color: rgb(111,111,111);
}
.nav-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 75px;
  max-width: 1200px;
  margin: auto;
}

.nav-left {
  margin-left: 16px;
}

.nav-center {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-left: 30px;
  margin-right: 30px;
  width: 50%;
}

.nav-center a {
  margin-left: 24px;
  margin-right: 24px;
  text-decoration: none;
  color: black;
  font-size: 18px;
}
</style>
