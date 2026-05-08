<template>
  <div id="app">
    <AnxLogin v-if="!isLoggedIn" @login-success="handleLoginSuccess" />
    <Dashboard v-else @logout="handleLogout" />
  </div>
</template>

<script>
import AnxLogin from './components/Login.vue'
import Dashboard from './components/Dashboard.vue'

export default {
  name: 'App',
  components: { AnxLogin, Dashboard },
  data() {
    return {
      isLoggedIn: false
    }
  },
  created() {
    // 检查登录状态
    const loggedIn = localStorage.getItem('anxju_logged_in')
    this.isLoggedIn = loggedIn === 'true'
  },
  methods: {
    handleLoginSuccess() {
      this.isLoggedIn = true
    },
    handleLogout() {
      this.isLoggedIn = false
    }
  }
}
</script>

<style>
/* keep app minimal; dashboard styles are in Dashboard.vue */
html, body, #app { height: 100%; margin: 0; padding: 0; }
</style>