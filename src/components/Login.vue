<template>
  <div id="loginPage">
    <h3>Provide your credentials</h3>
    <div>
      <input v-model="username" placeholder="username">
    </div>
    <div>
      <input type="password" v-model="password" placeholder="password">
    </div>
    <button @click="login">Login</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      username: null,
      password: null
    }
  },
  methods: {
    async login() {
      await axios.post("http://127.0.0.1:8000/auth/", {
        username: this.username,
        password: this.password
      })
      .then(resp => {this.$emit("logIn", this.username) && console.log(resp)})
      .catch(err => console.log(err))
    }
  }
}
</script>

<style scoped>
input {
  width: 10em;
}
</style>
