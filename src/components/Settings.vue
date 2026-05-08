<template>
  <div class="settings-panel">
    <div class="settings-header">
      <span class="header-icon">⚙️</span>
      <h3>账户设置</h3>
      <div class="header-decoration"></div>
    </div>
    
    <div class="settings-content">
      <div class="setting-item">
        <div class="setting-label">
          <span class="label-icon">👤</span>
          <label>账户名</label>
        </div>
        <div class="setting-input-group">
          <input v-model="accountName" type="text" placeholder="输入新账户名" />
          <button @click="updateAccountName" class="btn-action">修改</button>
        </div>
      </div>
      
      <div class="setting-item">
        <div class="setting-label">
          <span class="label-icon">🔒</span>
          <label>原密码</label>
        </div>
        <div class="setting-input-group">
          <input v-model="oldPassword" type="password" placeholder="输入原密码" />
        </div>
      </div>
      
      <div class="setting-item">
        <div class="setting-label">
          <span class="label-icon">🔑</span>
          <label>新密码</label>
        </div>
        <div class="setting-input-group">
          <input v-model="newPassword" type="password" placeholder="输入新密码" />
        </div>
      </div>
      
      <div class="setting-item">
        <div class="setting-label">
          <span class="label-icon">✓</span>
          <label>确认新密码</label>
        </div>
        <div class="setting-input-group">
          <input v-model="confirmPassword" type="password" placeholder="再次输入新密码" />
          <button @click="updatePassword" class="btn-action">修改</button>
        </div>
      </div>
      
      <div class="setting-item">
        <div class="setting-label">
          <span class="label-icon">📞</span>
          <label>紧急联系电话</label>
        </div>
        <div class="setting-input-group">
          <input v-model="emergencyPhone" type="tel" placeholder="输入新紧急联系电话" />
          <button @click="updateEmergencyPhone" class="btn-action">修改</button>
        </div>
      </div>
      
      <div class="setting-item info-item">
        <div class="setting-label">
          <span class="label-icon">ℹ️</span>
          <label>当前紧急联系电话（去敏）</label>
        </div>
        <div class="setting-value">
          <span class="value-badge">{{ desensitizedPhone }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AnxSettings',
  data() {
    return {
      accountName: '',
      oldPassword: '',
      newPassword: '',
      confirmPassword: '',
      emergencyPhone: '',
      currentEmergencyPhone: localStorage.getItem('anxju_emergency_phone') || '138****1234' // 示例默认值
    }
  },
  computed: {
    desensitizedPhone() {
      if (!this.currentEmergencyPhone) return '未设置'
      // 去敏：显示前3位和后4位，中间用*替换
      const phone = this.currentEmergencyPhone.replace(/\s/g, '')
      if (phone.length !== 11) return phone // 如果不是11位，直接返回
      return phone.substring(0, 3) + '****' + phone.substring(7)
    }
  },
  methods: {
    updateAccountName() {
      if (this.accountName.trim()) {
        // 更新账户名逻辑，这里可以保存到 localStorage 或发送到后端
        localStorage.setItem('anxju_account_name', this.accountName)
        alert('账户名已更新')
        this.accountName = ''
      } else {
        alert('请输入有效的账户名')
      }
    },
    updatePassword() {
      const storedPassword = localStorage.getItem('anxju_password') || '123456' // 默认密码
      if (this.oldPassword !== storedPassword) {
        alert('原密码错误')
        return
      }
      if (!this.newPassword.trim()) {
        alert('请输入新密码')
        return
      }
      if (this.newPassword !== this.confirmPassword) {
        alert('两次输入的新密码不一致')
        return
      }
      // 更新密码逻辑
      localStorage.setItem('anxju_password', this.newPassword)
      alert('密码已更新')
      this.oldPassword = ''
      this.newPassword = ''
      this.confirmPassword = ''
    },
    updateEmergencyPhone() {
      if (this.emergencyPhone.trim()) {
        // 更新紧急联系电话
        this.currentEmergencyPhone = this.emergencyPhone
        localStorage.setItem('anxju_emergency_phone', this.emergencyPhone)
        alert('紧急联系电话已更新')
        this.emergencyPhone = ''
      } else {
        alert('请输入有效的电话号码')
      }
    }
  }
}
</script>

<style scoped>
.settings-panel {
  padding: 32px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border-radius: 24px;
  color: #eef2ff;
  border: 1px solid rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(20px);
  box-shadow: 
    0 12px 32px rgba(0, 0, 0, 0.25),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

/* ===== 头部区域 ===== */
.settings-header {
  text-align: center;
  margin-bottom: 32px;
  position: relative;
  padding-bottom: 20px;
}

.header-icon {
  font-size: 2rem;
  display: block;
  margin-bottom: 12px;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
}

.settings-header h3 {
  color: #ffffff;
  font-size: 1.6rem;
  font-weight: 700;
  margin: 0;
  letter-spacing: 0.5px;
}

.header-decoration {
  width: 80px;
  height: 3px;
  background: linear-gradient(90deg, #3b6eff, #8b5cf6);
  border-radius: 2px;
  margin: 16px auto 0;
}

/* ===== 设置内容区域 ===== */
.settings-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.setting-item {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.setting-item:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateX(6px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}

.setting-item.info-item {
  background: rgba(59, 110, 255, 0.05);
  border-color: rgba(59, 110, 255, 0.15);
}

.setting-label {
  min-width: 160px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.label-icon {
  font-size: 1.3rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.setting-label label {
  color: #cbd5e6;
  font-weight: 600;
  font-size: 0.95rem;
  letter-spacing: 0.3px;
}

.setting-input-group {
  flex: 1;
  display: flex;
  gap: 12px;
  align-items: center;
}

.setting-input-group input {
  flex: 1;
  padding: 12px 16px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.05);
  color: #ffffff;
  font-size: 0.95rem;
  transition: all 0.3s;
  font-family: inherit;
}

.setting-input-group input:focus {
  outline: none;
  border-color: #3b6eff;
  box-shadow: 0 0 0 3px rgba(59, 110, 255, 0.15);
  background: rgba(255, 255, 255, 0.08);
}

.setting-input-group input::placeholder {
  color: rgba(234, 240, 255, 0.4);
}

.btn-action {
  padding: 12px 24px;
  background: linear-gradient(135deg, #3b6eff, #5c7cfa);
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(59, 110, 255, 0.3);
  white-space: nowrap;
  font-family: inherit;
}

.btn-action:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(59, 110, 255, 0.4);
  background: linear-gradient(135deg, #4b7eff, #6c8cfa);
}

.btn-action:active {
  transform: translateY(0);
}

.setting-value {
  flex: 1;
}

.value-badge {
  display: inline-block;
  padding: 12px 20px;
  background: rgba(59, 110, 255, 0.15);
  border: 1px solid rgba(59, 110, 255, 0.3);
  border-radius: 10px;
  color: #a8c7ff;
  font-weight: 600;
  font-size: 0.95rem;
  letter-spacing: 0.5px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .settings-panel {
    padding: 24px;
  }

  .setting-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }

  .setting-label {
    min-width: auto;
    width: 100%;
  }

  .setting-input-group {
    width: 100%;
    flex-direction: column;
  }

  .setting-input-group input,
  .btn-action {
    width: 100%;
  }
  
  .setting-value {
    width: 100%;
  }
  
  .value-badge {
    width: 100%;
    text-align: center;
  }
}
</style>