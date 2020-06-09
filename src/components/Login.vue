<template>
  <div id="loginPage">
    <h3>Provide your credentials</h3>

    <form v-if="isLoginPage != false"
      @submit="checkLoginForm"
      novalidate="true"
    >
      <div>
        <input v-model="username" placeholder="Username">
      </div>
      <div>
        <input type="password" v-model="password" placeholder="Password">
      </div>
      <div >
        <button type="submit">Login</button>
        <button @click="startCreatingAccount">Create Account</button>
      </div>
    </form>

    <form v-else
      @submit="checkSignUpForm"
    >
      <div>
        <input v-model="username" placeholder="Username">
      </div>
      <div>
        <input type="password" v-model="password" placeholder="Password">
      </div>
      <div>
        <input type="password" v-model="password2" placeholder="Repeat password">
      </div>
      <div>
        <input v-model="email" type="email" placeholder="Email">
      </div>
      <button>Sign Up</button>
      <button @click="startLogin">Go back</button>
    </form>

    <div v-if="errors.length">
    <h5 class="err">Please correct the following error(s):</h5>
    <ul>
      <li class="err" v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
    </div>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      isLoginPage: null,
      username: null,
      password: null,
      password2: null,
      email: null,
      errors: []
    }
  },
  methods: {
    checkLoginForm: function (e) {
      e.preventDefault();

      this.errors = [];
      if (!this.username) {
        this.errors.push("Name required.");
      }
      if (!this.password) {
        this.errors.push("Password required.");
      }

      if (!this.errors.length) {
        this.login()
      }
    },
    checkSignUpForm: function (e) {
      e.preventDefault();

      this.errors = [];
      if (!this.username) {
        this.errors.push("Name required.");
      }
      if (!this.email) {
        this.errors.push('Email required.');
      }
      if (!this.password) {
        this.errors.push("Password required.");
      }
      else if (this.password.length < 6) {
        this.errors.push("Password must have 6+ characters.");
      }
      if (!this.password2) {
        this.errors.push('Password repeat required.');
      }
      if (this.password2 != this.password) {
        this.errors.push('Passwords are not the same.');
      }


      if (!this.errors.length) {
        this.createAccount()
      }
    },
    async login() {
      await axios.post("http://127.0.0.1:8000/auth/", {
        username: this.username,
        password: this.password
      })
      .then(resp => {this.$emit("logIn", this.username) && console.log(resp)})
      .catch(this.errors.push("Incorrect login data."))
    },
    startCreatingAccount() {
      this.isLoginPage = false
    },
    startLogin() {
      this.isLoginPage = true
    },
    async createAccount() {
      if (this.password != this.password2) {
        console.log("Bad password!")
      }
      var newUser = {
        "username": this.username,
        "password": this.password,
        "email": this.email
      }
      await axios.post("http://127.0.0.1:8000/users/", newUser)
      .then(({data}) => {console.log(data)})
      .catch(err => console.log(err));
      this.isLoginPage = true
    }
  }
}
</script>

<style scoped>
input {
  width: 10em;
}

ul, h5 {
  margin: 1rem;
}

.err{
  font-size: 50%;
  color: lightcoral;
}
</style>
