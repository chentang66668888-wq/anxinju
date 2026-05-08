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
  padding: 32px;
  background: linear-gradient(135deg, rgba(139, 92, 246, 0.1), rgba(59, 110, 255, 0.1));
  border-radius: 24px;
  color: #eef2ff;
  border: 1px solid rgba(139, 92, 246, 0.2);
  position: relative;
  overflow: hidden;
}
.settings-panel::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(139, 92, 246, 0.05), transparent);
  opacity: 0;
  transition: opacity 0.3s ease;
}
.settings-panel:hover::before {
  opacity: 1;
}
.settings-panel h3 {
  margin: 0 0 24px 0;
  color: #dfe8ff;
  font-size: 1.4rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  gap: 8px;
}
.setting-item {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 12px;
  border: 1px solid rgba(72, 120, 255, 0.15);
  transition: all 0.3s ease;
}
.setting-item:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(72, 120, 255, 0.3);
  transform: translateY(-1px);
}
.setting-item label {
  min-width: 140px;
  color: #9fb4ff;
  font-weight: 600;
  font-size: 0.9rem;
}
.setting-item input {
  flex: 1;
  padding: 10px 14px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(72, 120, 255, 0.3);
  border-radius: 8px;
  color: #eef2ff;
  font-size: 0.9rem;
  outline: none;
  transition: all 0.3s ease;
}
.setting-item input:focus {
  border-color: #3b6eff;
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 0 3px rgba(59, 110, 255, 0.1);
}
.setting-item button {
  padding: 10px 20px;
  background: linear-gradient(135deg, #3b6eff, #5b8eff);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 600;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(59, 110, 255, 0.3);
}
.setting-item button:hover {
  background: linear-gradient(135deg, #4b7eff, #6b9eff);
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(59, 110, 255, 0.4);
}
</style>