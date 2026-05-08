<template>
  <div class="user-card glass-card">
    <div class="user-header">
      <div class="user-avatar-wrapper">
        <div class="user-avatar">👵</div>
        <div class="avatar-ring"></div>
      </div>
      <div class="user-basic-info">
        <div class="user-name">{{ userInfo.name }}</div>
        <div class="user-age">🎂 {{ userInfo.age }}岁</div>
      </div>
    </div>

    <div class="user-details">
      <div class="detail-item">
        <span class="detail-icon">🩺</span>
        <div class="detail-content">
          <div class="detail-label">身体状况</div>
          <div class="detail-value">{{ userInfo.healthCondition }}</div>
        </div>
      </div>
      <div class="detail-item warning">
        <span class="detail-icon">📌</span>
        <div class="detail-content">
          <div class="detail-label">注意事项</div>
          <div class="detail-value">跌倒高风险时段 00:00-06:00</div>
        </div>
      </div>
    </div>

    <div class="action-buttons">
      <button class="action-btn" @click="showUserDialog = true">
        <span class="btn-icon">👤</span>
        <span class="btn-text">更改使用者信息</span>
      </button>
      <button class="action-btn" @click="showECGDialog = true">
        <span class="btn-icon">❤️</span>
        <span class="btn-text">心电图设置</span>
      </button>
      <button class="action-btn" @click="showRouteDialog = true">
        <span class="btn-icon">🗺️</span>
        <span class="btn-text">路径图设置</span>
      </button>
    </div>

    <!-- 使用者信息修改弹窗 -->
    <div class="dialog-overlay" v-if="showUserDialog" @click="showUserDialog = false">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3><span class="header-icon">👤</span> 更改使用者信息</h3>
          <button class="close-btn" @click="showUserDialog = false">×</button>
        </div>
        <div class="dialog-body">
          <div class="form-group">
            <label>姓名：</label>
            <input v-model="userInfo.name" type="text" class="form-input" placeholder="请输入姓名" />
          </div>
          <div class="form-group">
            <label>年龄：</label>
            <input v-model.number="userInfo.age" type="number" class="form-input" min="1" max="150" placeholder="请输入年龄" />
          </div>
          <div class="form-group">
            <label>身体状况：</label>
            <textarea v-model="userInfo.healthCondition" class="form-textarea" rows="3" placeholder="请描述身体状况"></textarea>
          </div>
        </div>
        <div class="dialog-footer">
          <button class="btn-cancel" @click="showUserDialog = false">取消</button>
          <button class="btn-confirm" @click="saveUserInfo">保存</button>
        </div>
      </div>
    </div>

    <!-- 心电图设置弹窗 -->
    <div class="dialog-overlay" v-if="showECGDialog" @click="showECGDialog = false">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3><span class="header-icon">❤️</span> 心电图设置</h3>
          <button class="close-btn" @click="showECGDialog = false">×</button>
        </div>
        <div class="dialog-body">
          <div class="form-group">
            <label>背景颜色：</label>
            <select v-model="ecgSettings.bgColor" class="form-select">
              <option value="dark">深蓝色</option>
              <option value="black">黑色</option>
              <option value="white">白色</option>
              <option value="green">绿色（夜视模式）</option>
            </select>
          </div>
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="ecgSettings.showData" />
              <span>显示数值数据</span>
            </label>
          </div>
          <div class="form-group">
            <label>波形颜色：</label>
            <select v-model="ecgSettings.waveColor" class="form-select">
              <option value="lightblue">浅蓝色</option>
              <option value="green">绿色</option>
              <option value="red">红色</option>
              <option value="white">白色</option>
            </select>
          </div>
        </div>
        <div class="dialog-footer">
          <button class="btn-cancel" @click="showECGDialog = false">取消</button>
          <button class="btn-confirm" @click="saveECGSettings">保存</button>
          <button class="btn-export" @click="exportECGData">导出心电图数据</button>
        </div>
      </div>
    </div>

    <!-- 路线图设置弹窗 -->
    <div class="dialog-overlay" v-if="showRouteDialog" @click="showRouteDialog = false">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3><span class="header-icon">🗺️</span> 路径图设置</h3>
          <button class="close-btn" @click="showRouteDialog = false">×</button>
        </div>
        <div class="dialog-body">
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="routeSettings.showLabels" />
              <span>显示具体地名</span>
            </label>
          </div>
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="routeSettings.showRivers" />
              <span>显示河流</span>
            </label>
          </div>
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="routeSettings.showRoads" />
              <span>显示道路网格</span>
            </label>
          </div>
          <div class="form-group">
            <label>路线颜色：</label>
            <select v-model="routeSettings.routeColor" class="form-select">
              <option value="cyan">青绿色</option>
              <option value="blue">蓝色</option>
              <option value="red">红色</option>
              <option value="yellow">黄色</option>
            </select>
          </div>
        </div>
        <div class="dialog-footer">
          <button class="btn-cancel" @click="showRouteDialog = false">取消</button>
          <button class="btn-confirm" @click="saveRouteSettings">保存</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserCard',
  data() {
    return {
      showUserDialog: false,
      showECGDialog: false,
      showRouteDialog: false,
      userInfo: {
        name: '张德海',
        age: 78,
        healthCondition: '高血压、轻度认知障碍、夜间需关注如厕'
      },
      ecgSettings: {
        bgColor: 'dark',
        showData: false,
        waveColor: 'lightblue'
      },
      routeSettings: {
        showLabels: true,
        showRivers: true,
        showRoads: true,
        routeColor: 'cyan'
      }
    }
  },
  methods: {
    saveUserInfo() {
      this.$emit('update-user-info', this.userInfo)
      this.showUserDialog = false
    },
    saveECGSettings() {
      this.$emit('update-ecg-settings', this.ecgSettings)
      this.showECGDialog = false
    },
    saveRouteSettings() {
      this.$emit('update-route-settings', this.routeSettings)
      this.showRouteDialog = false
    },
    exportECGData() {
      this.$emit('export-ecg-data')
    }
  }
}
</script>

<style scoped>
.user-card {
  padding: 28px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(20px);
  box-shadow: 
    0 12px 32px rgba(0, 0, 0, 0.25),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* ===== 用户头部区域 ===== */
.user-header {
  display: flex;
  align-items: center;
  gap: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.user-avatar-wrapper {
  position: relative;
  width: 80px;
  height: 80px;
}

.user-avatar {
  width: 80px;
  height: 80px;
  border-radius: 20px;
  background: linear-gradient(135deg, #3b6eff 0%, #8b5cf6 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  box-shadow: 0 8px 24px rgba(59, 110, 255, 0.4);
  position: relative;
  z-index: 2;
}

.avatar-ring {
  position: absolute;
  top: -4px;
  left: -4px;
  right: -4px;
  bottom: -4px;
  border-radius: 24px;
  border: 2px solid rgba(59, 110, 255, 0.3);
  animation: pulse-ring 2s infinite;
}

@keyframes pulse-ring {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.05);
    opacity: 0.6;
  }
}

.user-basic-info {
  flex: 1;
}

.user-name {
  font-size: 1.5rem;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 6px;
  letter-spacing: 0.5px;
}

.user-age {
  font-size: 0.95rem;
  color: rgba(234, 240, 255, 0.7);
  font-weight: 500;
}

/* ===== 用户详情区域 ===== */
.user-details {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.detail-item {
  display: flex;
  gap: 14px;
  padding: 16px;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 14px;
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s ease;
}

.detail-item:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateX(4px);
}

.detail-item.warning {
  background: rgba(255, 107, 107, 0.08);
  border-color: rgba(255, 107, 107, 0.2);
}

.detail-item.warning:hover {
  background: rgba(255, 107, 107, 0.12);
  border-color: rgba(255, 107, 107, 0.3);
}

.detail-icon {
  font-size: 1.5rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.detail-content {
  flex: 1;
}

.detail-label {
  font-size: 0.8rem;
  color: rgba(234, 240, 255, 0.6);
  font-weight: 600;
  margin-bottom: 6px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.detail-value {
  font-size: 0.95rem;
  color: #eef2ff;
  line-height: 1.6;
  font-weight: 500;
}

/* ===== 操作按钮区域 ===== */
.action-buttons {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.action-btn {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 18px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 14px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  font-family: inherit;
  color: #eef2ff;
  font-size: 0.9rem;
  font-weight: 600;
  position: relative;
  overflow: hidden;
}

.action-btn::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 3px;
  background: linear-gradient(180deg, #3b6eff 0%, #8b5cf6 100%);
  transform: scaleY(0);
  transition: transform 0.3s ease;
}

.action-btn:hover {
  background: rgba(59, 110, 255, 0.12);
  border-color: rgba(59, 110, 255, 0.3);
  transform: translateX(6px);
  box-shadow: 0 4px 16px rgba(59, 110, 255, 0.2);
}

.action-btn:hover::before {
  transform: scaleY(1);
}

.btn-icon {
  font-size: 1.3rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.btn-text {
  flex: 1;
  text-align: left;
}

/* ===== 弹窗样式 ===== */
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.75);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(6px);
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.dialog-content {
  background: linear-gradient(145deg, #0f1528 0%, #0a0f1e 100%);
  border: 1px solid rgba(72, 120, 255, 0.3);
  border-radius: 24px;
  padding: 28px;
  width: 90%;
  max-width: 480px;
  box-shadow: 
    0 24px 64px rgba(0, 0, 0, 0.6),
    0 0 40px rgba(59, 110, 255, 0.1);
  animation: slideUp 0.3s ease;
}

@keyframes slideUp {
  from {
    transform: translateY(20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  padding-bottom: 18px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.dialog-header h3 {
  color: #dfe8ff;
  font-size: 1.3rem;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: 700;
}

.header-icon {
  font-size: 1.4rem;
}

.close-btn {
  background: none;
  border: none;
  color: #9fb4ff;
  font-size: 2rem;
  cursor: pointer;
  padding: 0;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.2s;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  transform: rotate(90deg);
}

.dialog-body {
  margin-bottom: 24px;
}

.form-group {
  margin-bottom: 18px;
}

.form-group label {
  display: block;
  color: #9fb4ff;
  font-size: 0.9rem;
  margin-bottom: 8px;
  font-weight: 600;
}

.form-input,
.form-textarea,
.form-select {
  width: 100%;
  padding: 12px 14px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(72, 120, 255, 0.3);
  border-radius: 10px;
  color: #eef2ff;
  font-size: 0.95rem;
  outline: none;
  transition: all 0.3s;
  font-family: inherit;
}

.form-input:focus,
.form-textarea:focus,
.form-select:focus {
  border-color: #3b6eff;
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 0 3px rgba(59, 110, 255, 0.15);
}

.form-textarea {
  resize: vertical;
  min-height: 80px;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  padding: 10px;
  border-radius: 8px;
  transition: background 0.2s;
}

.checkbox-group label:hover {
  background: rgba(255, 255, 255, 0.03);
}

.checkbox-group input[type="checkbox"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #3b6eff;
}

.dialog-footer {
  display: flex;
  gap: 12px;
  justify-content: flex-end;
  padding-top: 18px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.btn-cancel,
.btn-confirm,
.btn-export {
  padding: 12px 24px;
  border-radius: 12px;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  border: none;
  font-family: inherit;
}

.btn-cancel {
  background: rgba(255, 255, 255, 0.05);
  color: #9fb4ff;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.btn-cancel:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

.btn-confirm {
  background: linear-gradient(135deg, #3b6eff, #5b8eff);
  color: white;
  box-shadow: 0 4px 12px rgba(59, 110, 255, 0.3);
}

.btn-confirm:hover {
  background: linear-gradient(135deg, #4b7eff, #6b9eff);
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(59, 110, 255, 0.4);
}

.btn-export {
  background: rgba(78, 205, 196, 0.2);
  color: #4ecdc4;
  border: 1px solid rgba(78, 205, 196, 0.4);
}

.btn-export:hover {
  background: rgba(78, 205, 196, 0.3);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(78, 205, 196, 0.2);
}

/* 响应式设计 */
@media (max-width: 768px) {
  .user-card {
    padding: 20px;
  }
  
  .user-header {
    flex-direction: column;
    text-align: center;
  }
  
  .action-btn {
    padding: 12px 16px;
  }
  
  .dialog-content {
    padding: 20px;
    max-width: 95%;
  }
  
  .dialog-footer {
    flex-direction: column;
  }
  
  .btn-cancel,
  .btn-confirm,
  .btn-export {
    width: 100%;
  }
}
</style>
