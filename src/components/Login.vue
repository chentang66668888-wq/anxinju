<template>
  <div class="login-container">
    <div class="login-card">
      <div class="login-header">
        <div class="logo-wrapper">
          <div class="logo">安芯居</div>
          <div class="logo-ring"></div>
        </div>
        <h1 class="title">{{ isLoginMode ? '用户登录' : '用户注册' }}</h1>
        <p class="subtitle">家庭守护 · 安心陪伴</p>
      </div>

      <div class="login-body">
        <!-- 登录表单 -->
        <form v-if="isLoginMode" @submit.prevent="handleLogin" class="login-form">
          <div class="form-group">
            <label for="username">
              <span class="label-icon">👤</span>
              用户名
            </label>
            <input 
              id="username"
              v-model="loginForm.username" 
              type="text" 
              placeholder="请输入用户名"
              required
              class="form-input"
            />
          </div>
          
          <div class="form-group">
            <label for="password">
              <span class="label-icon">🔒</span>
              密码
            </label>
            <input 
              id="password"
              v-model="loginForm.password" 
              type="password" 
              placeholder="请输入密码"
              required
              class="form-input"
            />
          </div>

          <div v-if="errorMessage" class="error-message">
            <span class="error-icon">⚠️</span>
            {{ errorMessage }}
          </div>

          <button type="submit" class="btn-login">
            <span class="btn-icon">→</span>
            登 录
          </button>
        </form>

        <!-- 注册表单 -->
        <form v-else @submit.prevent="handleRegister" class="login-form">
          <div class="form-group">
            <label for="reg-username">
              <span class="label-icon">👤</span>
              用户名
            </label>
            <input 
              id="reg-username"
              v-model="registerForm.username" 
              type="text" 
              placeholder="请设置用户名（3-20位字符）"
              required
              minlength="3"
              maxlength="20"
              class="form-input"
            />
          </div>
          
          <div class="form-group">
            <label for="reg-password">
              <span class="label-icon">🔒</span>
              密码
            </label>
            <input 
              id="reg-password"
              v-model="registerForm.password" 
              type="password" 
              placeholder="请设置密码（6-20位字符）"
              required
              minlength="6"
              maxlength="20"
              class="form-input"
            />
          </div>

          <div class="form-group">
            <label for="reg-confirm-password">
              <span class="label-icon">✓</span>
              确认密码
            </label>
            <input 
              id="reg-confirm-password"
              v-model="registerForm.confirmPassword" 
              type="password" 
              placeholder="请再次输入密码"
              required
              class="form-input"
            />
          </div>

          <div v-if="errorMessage" class="error-message">
            <span class="error-icon">⚠️</span>
            {{ errorMessage }}
          </div>

          <div v-if="successMessage" class="success-message">
            <span class="success-icon">✓</span>
            {{ successMessage }}
          </div>

          <button type="submit" class="btn-register">
            <span class="btn-icon">+</span>
            注 册
          </button>
        </form>

        <div class="login-footer">
          <p v-if="isLoginMode">
            还没有账号？ 
            <a href="#" @click.prevent="switchMode">立即注册</a>
          </p>
          <p v-else>
            已有账号？ 
            <a href="#" @click.prevent="switchMode">返回登录</a>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AnxLogin',
  data() {
    return {
      isLoginMode: true,
      errorMessage: '',
      successMessage: '',
      loginForm: {
        username: '',
        password: ''
      },
      registerForm: {
        username: '',
        password: '',
        confirmPassword: ''
      },
      // 模拟用户数据库（本地数组）
      users: [
        { username: 'gly', password: '123456' },
        { username: 'admin', password: 'admin123' },
        { username: 'test', password: 'test123' }
      ]
    }
  },
  created() {
    // 从 localStorage 加载用户数据
    const savedUsers = localStorage.getItem('anxju_users')
    if (savedUsers) {
      this.users = JSON.parse(savedUsers)
    }

    // 确保管理员账户始终存在
    const adminUser = { username: 'gly', password: '123456' }
    if (!this.users.some(u => u.username === adminUser.username)) {
      this.users.unshift(adminUser)
      localStorage.setItem('anxju_users', JSON.stringify(this.users))
    }
    
    // 检查是否已经登录
    const isLoggedIn = localStorage.getItem('anxju_logged_in')
    if (isLoggedIn === 'true') {
      this.$emit('login-success', {
        username: localStorage.getItem('anxju_current_user') || '',
        isAdmin: localStorage.getItem('anxju_is_admin') === 'true'
      })
    }
  },
  methods: {
    switchMode() {
      this.isLoginMode = !this.isLoginMode
      this.errorMessage = ''
      this.successMessage = ''
      this.loginForm = { username: '', password: '' }
      this.registerForm = { username: '', password: '', confirmPassword: '' }
    },
    handleLogin() {
      this.errorMessage = ''
      
      // 查找匹配的用户
      const user = this.users.find(
        u => u.username === this.loginForm.username && 
             u.password === this.loginForm.password
      )

      if (user) {
        // 登录成功
        localStorage.setItem('anxju_logged_in', 'true')
        localStorage.setItem('anxju_current_user', this.loginForm.username)
        const isAdmin = this.loginForm.username === 'gly'
        localStorage.setItem('anxju_is_admin', isAdmin ? 'true' : 'false')
        this.$emit('login-success', { username: this.loginForm.username, isAdmin })
      } else {
        this.errorMessage = '用户名或密码错误，请重试'
      }
    },
    handleRegister() {
      this.errorMessage = ''
      this.successMessage = ''

      // 验证密码一致性
      if (this.registerForm.password !== this.registerForm.confirmPassword) {
        this.errorMessage = '两次输入的密码不一致'
        return
      }

      // 检查用户名是否已存在
      const existingUser = this.users.find(
        u => u.username === this.registerForm.username
      )

      if (existingUser) {
        this.errorMessage = '用户名已存在，请使用其他用户名'
        return
      }

      // 注册成功，添加新用户
      this.users.push({
        username: this.registerForm.username,
        password: this.registerForm.password
      })

      // 保存到 localStorage
      localStorage.setItem('anxju_users', JSON.stringify(this.users))

      this.successMessage = '注册成功！即将跳转到登录页面'
      
      // 1.5秒后自动切换到登录模式
      setTimeout(() => {
        this.switchMode()
        this.loginForm.username = this.registerForm.username
      }, 1500)
    }
  }
}
</script>

<style scoped>
.login-container {
  min-height: 100vh;
  background: linear-gradient(145deg, #0a0f1e 0%, #0c1222 50%, #0a0f1e 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  position: relative;
  overflow: hidden;
}

.login-container::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle at 30% 50%, rgba(59, 110, 255, 0.08) 0%, transparent 50%),
              radial-gradient(circle at 70% 50%, rgba(139, 92, 246, 0.08) 0%, transparent 50%);
  animation: rotate 30s linear infinite;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.login-card {
  background: rgba(18, 25, 45, 0.9);
  backdrop-filter: blur(24px);
  border-radius: 28px;
  border: 1px solid rgba(72, 120, 255, 0.25);
  box-shadow: 
    0 24px 64px rgba(0, 0, 0, 0.6),
    0 0 40px rgba(59, 110, 255, 0.1);
  width: 100%;
  max-width: 460px;
  overflow: hidden;
  position: relative;
  z-index: 1;
  animation: slideUp 0.5s ease;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.login-header {
  background: linear-gradient(135deg, rgba(59, 110, 255, 0.15), rgba(139, 92, 246, 0.15));
  padding: 48px 30px 36px;
  text-align: center;
  position: relative;
}

.logo-wrapper {
  position: relative;
  width: 90px;
  height: 90px;
  margin: 0 auto 24px;
}

.logo {
  width: 90px;
  height: 90px;
  background: linear-gradient(135deg, #3b6eff, #8b5cf6);
  border-radius: 22px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 1.6rem;
  font-weight: 700;
  box-shadow: 0 12px 32px rgba(59, 110, 255, 0.4);
  position: relative;
  z-index: 2;
}

.logo-ring {
  position: absolute;
  top: -6px;
  left: -6px;
  right: -6px;
  bottom: -6px;
  border-radius: 28px;
  border: 2px solid rgba(59, 110, 255, 0.3);
  animation: pulse-ring 2s infinite;
}

@keyframes pulse-ring {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.08);
    opacity: 0.6;
  }
}

.title {
  font-size: 2rem;
  color: #eef2ff;
  margin: 0 0 12px;
  font-weight: 700;
  letter-spacing: 1px;
}

.subtitle {
  font-size: 0.95rem;
  color: #9fb4ff;
  margin: 0;
  font-weight: 500;
}

.login-body {
  padding: 36px;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.form-group label {
  font-size: 0.9rem;
  color: #9fb4ff;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 8px;
}

.label-icon {
  font-size: 1.1rem;
}

.form-input {
  padding: 14px 18px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(72, 120, 255, 0.25);
  border-radius: 12px;
  color: #eef2ff;
  font-size: 1rem;
  outline: none;
  transition: all 0.3s;
  font-family: inherit;
}

.form-input:focus {
  border-color: #3b6eff;
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 0 4px rgba(59, 110, 255, 0.15);
}

.form-input::placeholder {
  color: #6c86a3;
}

.error-message {
  background: rgba(231, 76, 60, 0.12);
  border: 1px solid rgba(231, 76, 60, 0.3);
  color: #ff6b6b;
  padding: 14px 18px;
  border-radius: 10px;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 10px;
  animation: shake 0.4s ease;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

.error-icon {
  font-size: 1.2rem;
}

.success-message {
  background: rgba(46, 204, 113, 0.12);
  border: 1px solid rgba(46, 204, 113, 0.3);
  color: #2ecc71;
  padding: 14px 18px;
  border-radius: 10px;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 10px;
}

.success-icon {
  font-size: 1.2rem;
}

.btn-login,
.btn-register {
  padding: 16px;
  background: linear-gradient(135deg, #3b6eff, #5b8eff);
  border: none;
  border-radius: 12px;
  color: white;
  font-size: 1.05rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s;
  margin-top: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  box-shadow: 0 8px 24px rgba(59, 110, 255, 0.3);
  font-family: inherit;
  letter-spacing: 1px;
}

.btn-login:hover,
.btn-register:hover {
  background: linear-gradient(135deg, #4b7eff, #6b9eff);
  transform: translateY(-3px);
  box-shadow: 0 12px 28px rgba(59, 110, 255, 0.4);
}

.btn-login:active,
.btn-register:active {
  transform: translateY(-1px);
}

.btn-icon {
  font-size: 1.2rem;
  transition: transform 0.3s;
}

.btn-login:hover .btn-icon,
.btn-register:hover .btn-icon {
  transform: translateX(4px);
}

.login-footer {
  margin-top: 28px;
  text-align: center;
  padding-top: 24px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.login-footer p {
  margin: 0;
  color: #9fb4ff;
  font-size: 0.95rem;
}

.login-footer a {
  color: #3b6eff;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.2s;
  position: relative;
}

.login-footer a::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background: #3b6eff;
  transition: width 0.3s;
}

.login-footer a:hover {
  color: #5b8eff;
}

.login-footer a:hover::after {
  width: 100%;
}
</style>
