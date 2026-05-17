<template>
  <div id="app">
    <AnxLogin v-if="!isLoggedIn" @login-success="handleLoginSuccess" />
    <AdminPage v-else :currentUser="currentUser" @logout="handleLogout" />
  </div>
</template>

<script>
import AnxLogin from './components/Login.vue'
import AdminPage from './components/AdminPage.vue'

export default {
  name: 'App',
  components: { AnxLogin, AdminPage },
  data() {
    return {
      isLoggedIn: false,
      currentUser: ''
    }
  },
  created() {
    this.isLoggedIn = localStorage.getItem('anxju_logged_in') === 'true'
    this.currentUser = localStorage.getItem('anxju_current_user') || ''
  },
  methods: {
    handleLoginSuccess(payload) {
      this.isLoggedIn = true
      this.currentUser = payload?.username || localStorage.getItem('anxju_current_user') || ''
    },
    handleLogout() {
      localStorage.removeItem('anxju_logged_in')
      localStorage.removeItem('anxju_current_user')
      localStorage.removeItem('anxju_is_admin')
      this.isLoggedIn = false
      this.currentUser = ''
    }
  }
}
</script>

<style>
/* ====== 全局弹性布局设置 ====== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}

body {
  font-family: 'Inter', 'Segoe UI', 'PingFang SC', Roboto, 'Helvetica Neue', sans-serif;
  background: linear-gradient(145deg, #0a0f1e 0%, #0c1222 100%);
  color: #eef2ff;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}

/* ====== 滚动条美化 ====== */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: rgba(30, 42, 58, 0.3);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: rgba(59, 110, 255, 0.4);
  border-radius: 4px;
  transition: background 0.3s ease;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(59, 110, 255, 0.6);
}

/* ====== 通用工具类 ====== */
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.flex-between {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.flex-column {
  display: flex;
  flex-direction: column;
}

.flex-wrap {
  flex-wrap: wrap;
}

.gap-sm { gap: 8px; }
.gap-md { gap: 16px; }
.gap-lg { gap: 24px; }
.gap-xl { gap: 32px; }

/* ====== 响应式断点 ====== */
@media (max-width: 1400px) {
  html {
    font-size: 15px;
  }
}

@media (max-width: 1200px) {
  html {
    font-size: 14px;
  }
}

@media (max-width: 768px) {
  html {
    font-size: 13px;
  }
}

@media (max-width: 480px) {
  html {
    font-size: 12px;
  }
}
</style>