<template>
  <div class="header">
    <nav class="navbar navbar-light bg-light navbar-expand-lg">
      <div class="container">
        <router-link class="navbar-brand" to="/">Movies Manager</router-link>
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item active">
              <router-link :to="'/'" class="nav-link">Home</router-link>
            </li>
            <li v-if="authenticated" class="nav-item">
              <a class="nav-link" v-on:click="logout()" href="#">Logout</a>
            </li>
            <li v-if="!authenticated" class="nav-item">
              <router-link :to="'/login'" class="nav-link">Login</router-link>
            </li>
            <li v-if="!authenticated" class="nav-item">
              <router-link :to="'/register'" class="nav-link">Register</router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Movies",
  data() {
    return {
      authenticated: false
    };
  },
  beforeMount() {
    this.authenticated = localStorage.getItem("login") === "1";
  },
  watch: {
    $route() {
      this.authenticated = localStorage.getItem("login") === "1";
    }
  },

  methods: {
    logout() {
      const api = axios.create({
        withCredentials: true
      });
      api.get("http://localhost:8080/api/auth/logout").then(() => {
        localStorage.removeItem("login");
        this.authenticated = false;
        this.$router.replace({ name: "Login" });
      });
    },
    register() {
      this.$router.replace({ name: "Register" });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
