<template>
  <div class="settings-panel">
    <h3>账户设置</h3>
    <div class="setting-item">
      <label>账户名：</label>
      <input v-model="accountName" type="text" placeholder="输入新账户名" />
      <button @click="updateAccountName">修改</button>
    </div>
    <div class="setting-item">
      <label>原密码：</label>
      <input v-model="oldPassword" type="password" placeholder="输入原密码" />
    </div>
    <div class="setting-item">
      <label>新密码：</label>
      <input v-model="newPassword" type="password" placeholder="输入新密码" />
    </div>
    <div class="setting-item">
      <label>确认新密码：</label>
      <input v-model="confirmPassword" type="password" placeholder="再次输入新密码" />
      <button @click="updatePassword">修改</button>
    </div>
    <div class="setting-item">
      <label>紧急联系电话：</label>
      <input v-model="emergencyPhone" type="tel" placeholder="输入新紧急联系电话" />
      <button @click="updateEmergencyPhone">修改</button>
    </div>
    <div class="setting-item">
      <label>当前紧急联系电话（去敏）：</label>
      <span>{{ desensitizedPhone }}</span>
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
  padding: 30px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 20px;
  color: #eef2ff;
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
}

.settings-panel h3 {
  color: #ffffff;
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 25px;
  text-align: center;
  position: relative;
}

.settings-panel h3::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: linear-gradient(90deg, #3b6eff, #8b5cf6);
  border-radius: 2px;
}

.setting-item {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
}

.setting-item:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateY(-2px);
}

.setting-item label {
  min-width: 130px;
  color: #cbd5e6;
  font-weight: 600;
  font-size: 0.9rem;
}

.setting-item input {
  flex: 1;
  padding: 10px 15px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.05);
  color: #ffffff;
  font-size: 0.9rem;
  transition: all 0.3s;
}

.setting-item input:focus {
  outline: none;
  border-color: #3b6eff;
  box-shadow: 0 0 0 3px rgba(59, 110, 255, 0.1);
  background: rgba(255, 255, 255, 0.08);
}

.setting-item button {
  padding: 10px 20px;
  background: linear-gradient(135deg, #3b6eff, #5c7cfa);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(59, 110, 255, 0.3);
}

.setting-item button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(59, 110, 255, 0.4);
}

.setting-item span {
  color: #a8c7ff;
  font-weight: 500;
  font-size: 0.9rem;
  padding: 10px 15px;
  background: rgba(59, 110, 255, 0.1);
  border-radius: 8px;
  border: 1px solid rgba(59, 110, 255, 0.2);
}

/* Responsive design */
@media (max-width: 768px) {
  .settings-panel {
    padding: 20px;
  }

  .setting-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }

  .setting-item label {
    min-width: auto;
    width: 100%;
  }

  .setting-item input,
  .setting-item span {
    width: 100%;
  }
}
</style>