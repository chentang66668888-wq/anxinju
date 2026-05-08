<template>
  <div class="login-container">
    <div class="login-card">
      <div class="login-header">
        <div class="logo">安芯居</div>
        <h1 class="title">{{ isLoginMode ? '用户登录' : '用户注册' }}</h1>
        <p class="subtitle">家庭守护 · 安心陪伴</p>
      </div>

      <div class="login-body">
        <!-- 登录表单 -->
        <form v-if="isLoginMode" @submit.prevent="handleLogin" class="login-form">
          <div class="form-group">
            <label for="username">用户名</label>
            <input 
              id="username"
              v-model="loginForm.username" 
              type="text" 
              placeholder="请输入用户名"
              required
            />
          </div>
          
          <div class="form-group">
            <label for="password">密码</label>
            <input 
              id="password"
              v-model="loginForm.password" 
              type="password" 
              placeholder="请输入密码"
              required
            />
          </div>

          <div v-if="errorMessage" class="error-message">
            {{ errorMessage }}
          </div>

          <button type="submit" class="btn-login">登 录</button>
        </form>

        <!-- 注册表单 -->
        <form v-else @submit.prevent="handleRegister" class="login-form">
          <div class="form-group">
            <label for="reg-username">用户名</label>
            <input 
              id="reg-username"
              v-model="registerForm.username" 
              type="text" 
              placeholder="请设置用户名（3-20位字符）"
              required
              minlength="3"
              maxlength="20"
            />
          </div>
          
          <div class="form-group">
            <label for="reg-password">密码</label>
            <input 
              id="reg-password"
              v-model="registerForm.password" 
              type="password" 
              placeholder="请设置密码（6-20位字符）"
              required
              minlength="6"
              maxlength="20"
            />
          </div>

          <div class="form-group">
            <label for="reg-confirm-password">确认密码</label>
            <input 
              id="reg-confirm-password"
              v-model="registerForm.confirmPassword" 
              type="password" 
              placeholder="请再次输入密码"
              required
            />
          </div>

          <div v-if="errorMessage" class="error-message">
            {{ errorMessage }}
          </div>

          <div v-if="successMessage" class="success-message">
            {{ successMessage }}
          </div>

          <button type="submit" class="btn-register">注 册</button>
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
    
    // 检查是否已经登录
    const isLoggedIn = localStorage.getItem('anxju_logged_in')
    if (isLoggedIn === 'true') {
      this.$emit('login-success')
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
        this.$emit('login-success')
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
}

.login-card {
  background: rgba(18, 25, 45, 0.85);
  backdrop-filter: blur(20px);
  border-radius: 24px;
  border: 1px solid rgba(72, 120, 255, 0.2);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  width: 100%;
  max-width: 440px;
  overflow: hidden;
}

.login-header {
  background: linear-gradient(135deg, rgba(59, 110, 255, 0.15), rgba(139, 92, 246, 0.15));
  padding: 40px 30px 30px;
  text-align: center;
}

.logo {
  width: 70px;
  height: 70px;
  margin: 0 auto 20px;
  background: linear-gradient(135deg, #3b6eff, #8b5cf6);
  border-radius: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 1.5rem;
  font-weight: 700;
  box-shadow: 0 8px 24px rgba(59, 110, 255, 0.3);
}

.title {
  font-size: 1.8rem;
  color: #eef2ff;
  margin: 0 0 10px;
  font-weight: 600;
}

.subtitle {
  font-size: 0.9rem;
  color: #9fb4ff;
  margin: 0;
}

.login-body {
  padding: 30px;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-group label {
  font-size: 0.9rem;
  color: #9fb4ff;
  font-weight: 500;
}

.form-group input {
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(72, 120, 255, 0.2);
  border-radius: 10px;
  color: #eef2ff;
  font-size: 0.95rem;
  outline: none;
  transition: all 0.2s;
}

.form-group input:focus {
  border-color: #3b6eff;
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 0 3px rgba(59, 110, 255, 0.1);
}

.form-group input::placeholder {
  color: #6c86a3;
}

.error-message {
  background: rgba(231, 76, 60, 0.1);
  border: 1px solid rgba(231, 76, 60, 0.3);
  color: #ff6b6b;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 0.85rem;
}

.success-message {
  background: rgba(46, 204, 113, 0.1);
  border: 1px solid rgba(46, 204, 113, 0.3);
  color: #2ecc71;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 0.85rem;
}

.btn-login,
.btn-register {
  padding: 14px;
  background: linear-gradient(135deg, #3b6eff, #5b8eff);
  border: none;
  border-radius: 10px;
  color: white;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
  margin-top: 10px;
}

.btn-login:hover,
.btn-register:hover {
  background: linear-gradient(135deg, #4b7eff, #6b9eff);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(59, 110, 255, 0.3);
}

.btn-login:active,
.btn-register:active {
  transform: translateY(0);
}

.login-footer {
  margin-top: 24px;
  text-align: center;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.login-footer p {
  margin: 0;
  color: #9fb4ff;
  font-size: 0.9rem;
}

.login-footer a {
  color: #3b6eff;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.2s;
}

.login-footer a:hover {
  color: #5b8eff;
  text-decoration: underline;
}
</style>