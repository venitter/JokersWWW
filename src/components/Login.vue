<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <input v-model="username" placeholder="Login" />
    </div>
    <div>
      <input type="password" v-model="password" placeholder="Password" />
    </div>
    <div class="button" @click="login">Login</div>
    <div>{{ error }}</div>
  </div>
</template>

<script>
import axios from "axios";
import router from "../router";
export default {
  name: "Login",
  data() {
    return {
      msg: "Jokers",
      username: "",
      password: "",
      error: "",
      loginAuth: false
    };
  },
  methods: {
    login: function() {
      let user = this.username;
      let pass = this.password;
      axios
        .get("http://35.156.131.239:3000/loginAttempt", {
          params: {
            login: user,
            password: pass
          }
        })
        .then(res => {
          if (res.data.loginAuth === false) {
            this.error = res.data.error;
          } else if (res.data.loginAuth === true) {
            // tu dane z serwera maja sie gdzies zapisywac
            localStorage.setItem("userData", JSON.stringify(res.data.userData));
            router.push("/home");
          }
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
