<template>
  <div id="app">
    <NavBar :user="user" :updateUser="updateUser" />
    <router-view @updateUser="updateUser" :user="user" />
    <Foodter />
  </div>
</template>
<script>
import NavBar from "./components/NavBar.vue";
import Foodter from "./components/Foodter.vue";
export default {
  name: "App",
  data() {
    return {
      user: null,
    };
  },
  created() {
    const userLocal = localStorage.getItem("user");
    if (userLocal != null) {
      this.user = JSON.parse(userLocal);
    }
    console.log(this.user);
  },
  methods: {
    updateUser(value) {
      if (value != null) {
        console.log("nó hoạt động lần 1");
        const userLocal = localStorage.getItem("user");
        if (userLocal != null) {
          this.user = JSON.parse(userLocal);
        }
        console.log(this.user);
      } else {
        console.log("nó hoạt động lần 2");
        this.user = value;
      }
    },
  },
  components: {
    NavBar,
    Foodter,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  font-family: Arial;
}
body {
  margin: 0;
}
#page-wrap {
  margin: auto;
  max-width: 800px;
  min-height: 100vh;
}
button {
  background-color: black;
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  outline: 0;
  padding: 16px;
}
</style>
